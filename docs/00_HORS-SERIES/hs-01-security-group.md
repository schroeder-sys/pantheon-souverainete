# 🛡️ Introduction Vitale : Le Firewall Périmétrique "Panthéon-Gold"

> **⚠️ LIRE AVANT TOUTE CHOSE.**
> Ce document définit la fondation sécuritaire de **toutes** les instances du projet. Il est le prérequis absolu avant la création de votre Image Gold et le déploiement de la moindre "Divinité". Sans cette configuration, aucune instance ne sera considérée comme conforme au standard "Panthéon".

[![Niveau](https://img.shields.io/badge/Niveau-Vital-Critical-red)](#)
[![Couche](https://img.shields.io/badge/Couche-1_Périmétrique-orange)](#)
[![Compatibilité](https://img.shields.io/badge/Compat-OpenStack_&_Providers-green)](#)

---

## ⚠️ Pourquoi ce guide est "Vital" ?

Dans l'architecture "Panthéon de la Souveraineté", nous n'avons pas droit à l'erreur :
1.  **Pas de Console de Secours (KVM/Rescue) :** Par sécurité, le root n'a pas de mot de passe et la clé SSH privée reste chez vous. Si vous vous enfermez dehors, l'instance est perdue à jamais.
2.  **Zero-Trust par Défaut :** Nous partons du principe que tout est dangereux sauf ce qui est explicitement autorisé.
3.  **Modularité :** Ce firewall est la brique de base ("Lego") qui sera réutilisée à l'identique pour vos 12 Divinités.

> **📚 Liens avec les autres guides :**
> *   **Ce guide (Firewall) :** À faire en **PREMIER**. Il crée le "moule" de sécurité (`pantheon-gold-sg`).
> *   **Guide Suivant (À venir) : "Hors-Série : Création de l'Image Gold Debian"** : Une fois ce firewall créé, vous l'utiliserez pour construire votre image système de référence.
> *   **Guides Divinités (AEGIS, VULCAIN, etc.) :** Ils viendront **après**, en ajoutant simplement des options à cette base.

---

## 1. Philosophie : Séparation des Rôles

Pour garantir sécurité ET accès, nous séparons strictement les couches :

| Couche | Outil | Gestion | Politique IP | Risque de Lock-out |
|:---|:---|:---|:---|:---|
| **1. Périmétrique** | Security Group (Provider) | Interface Web (Horizon) | **1 IP Unique** (Max Sécurité) | **Faible** : Corigeable via le Web. |
| **2. Interne** | UFW (Debian) | Ligne de commande (SSH) | **2 IPs** (Domicile + AEGIS) | **Critique** : Perte totale si erreur. |

> **🛑 RÈGLE D'OR :**
> La redondance des IPs (Domicile + AEGIS) est **OBLIGATOIRE** sur la couche interne (UFW, voir guide dédié).
> Sur la couche périmétrique (ce guide), **une seule IP suffit** (votre IP Domicile actuelle), car vous pourrez toujours la modifier via l'interface web de l'hébergeur si besoin.

---

## 2. La Matrice de Règles (Standard Panthéon)

Vous allez créer un Security Group nommé `pantheon-gold-sg`. Il doit contenir **exclusivement** les règles suivantes.

### 🧠 Comprendre avant de copier : Public vs Privé

> **💡 Analogie de la Forteresse :**
> Imaginez que vous construisez une forteresse.
> *   **Deny All (Politique par défaut) :** Par défaut, **toutes les portes sont murées**. Personne n'entre, personne ne sort. C'est la sécurité absolue.
> *   **Nos Règles :** Nous allons ensuite percer **uniquement** les trous strictement nécessaires (une petite porte pour le gardien SSH, un tunnel pour vos autres châteaux).
> *   **Si vous laissez les portes par défaut ouvertes**, la forteresse est inutile. N'ayez pas peur de tout bloquer au début !

> **🌍 Réseau Public vs Réseau Privé :**
> *   **IP Publique (Votre IP Domicile) :** C'est votre adresse sur Internet (ex: `82.123.45.67`). C'est elle que vous mettrez dans la ligne **SSH**.
>     *   *Comment la trouver ?* Allez sur **[whatismyipaddress.com](https://whatismyipaddress.com/)**. Notez l'IPv4 et l'IPv6 affichées en gros. Ce sont vos `<IP_UNIQUE>`.
> *   **IP Privée (Réseau Interne) :** C'est une adresse virtuelle qui n'existe que *entre* vos serveurs chez l'hébergeur (ex: `192.168.10.x`).
>     *   *Attention :* Dans la ligne **Réseau Privé**, ne mettez **PAS** votre IP personnelle. Laissez exactement `192.168.10.0/24`. C'est l'adresse de tout votre réseau interne virtuel, pas la vôtre.

### A. Flux Entrants (Ingress)
*Politique par défaut : **DENY ALL** (Tout bloquer)*

| Ordre | Service | Protocole | Port | Source IPv4 | Source IPv6 | Statut |
|:---:|:---|:---|:---|:---|:---|:---:|
| **01** | **SSH** | TCP | `22` | `<VOTRE_IP_V4>/32` | `<VOTRE_IP_V6>/128` | ✅ **Obligatoire** |
| **02** | **Privé** | TCP | `1-65535` | `192.168.10.0/24` | *N/A* | ✅ **Obligatoire** |
| **03** | **Privé** | UDP | `1-65535` | `192.168.10.0/24` | *N/A* | ✅ **Obligatoire** |
| **04** | **ICMP** | ICMP | `All` | `<VOTRE_IP_V4>/32` | `<VOTRE_IP_V6>/128` | ℹ️ **Recommandé** |

> **ℹ️ Notes :**
> *   **IP Source :** Remplacez par votre IP publique actuelle (celle trouvée sur whatsmyip.com).
> *   **Réseau Privé :** Seul l'IPv4 est utilisé pour le subnet privé (`192.168.10.0/24`). C'est suffisant pour la communication inter-services.

### B. Flux Sortants (Egress)
*Politique par défaut : **DENY ALL** (Tout bloquer)*

C'est ici que se joue la souveraineté : on empêche les fuites de données et les connexions malveillantes sortantes.

| Ordre | Service | Protocole | Port | Destination | Statut |
|:---:|:---|:---|:---|:---|:---:|
| **01** | **DNS** | UDP/TCP | `53` | `0.0.0.0/0` & `::/0` | ✅ **Vital** |
| **02** | **HTTP/HTTPS** | TCP | `80`, `443` | `0.0.0.0/0` & `::/0` | ✅ **Vital** |
| **03** | **NTP** | UDP | `123` | `0.0.0.0/0` & `::/0` | ✅ **Vital** |
| **04** | **ICMP** | All | `All` | `0.0.0.0/0` & `::/0` | ℹ️ **Debug** |
| **05** | **TOUT AUTRE** | - | - | **BLOQUÉ** | 🛑 **Sécurité** |

---

## 3. Procédure de Déploiement (Workflow Idéal)

Suivez cet ordre chronologique. Ne brûlez pas les étapes.

### Étape 1 : Création du Security Group `pantheon-gold-sg`
*C'est la fondation. À faire une seule fois par projet.*

1.  Connectez-vous à votre interface **OpenStack Horizon** (Infomaniak Public Cloud).
2.  Allez dans **Network** > **Security Groups**.
3.  Cliquez sur **+ Create Security Group**.
    *   **Name :** `pantheon-gold-sg`
    *   **Description :** `SOCLE PANTHÉON : SSH Admin + Réseau Privé + Sorties Vitales (Deny All)`
4.  **⚠️ CRUCIAL :** Assurez-vous que le groupe est **VIDE**. Supprimez toute règle par défaut si elle existe. Nous voulons un "Deny All" total avant d'ajouter nos règles.
5.  Cliquez sur **Manage Rules** et ajoutez les **8 règles** définies dans la matrice ci-dessus (4 en Entrée, 4 en Sortie).
    *   *Attention à bien spécifier vos IPs personnelles (trouvées sur whatsmyip.com) pour le SSH.*

### Étape 2 : Création de l'Image Gold (Guide Dédié)
*Une fois le groupe créé, il servira de base pour l'image système.*

1.  Suivez le guide **"Hors-Série : Création de l'Image Gold Debian"** (à venir).
2.  Vous y créerez une instance de test en lui attachant **immédiatement** le groupe `pantheon-gold-sg`.
3.  Vous y configurerez le système (Docker, UFW, Hardening) puis vous ferez un **Snapshot**.
4.  Ce snapshot deviendra votre "Gold Image" pour tous les futurs déploiements.

### Étape 3 : Déploiement des Divinités
*Quand vous lancerez AEGIS, JANUS, etc., vous utiliserez cette Image Gold.*

*   **Cas Standard (Mode Équipe) :** Pour 90% des services (JANUS, MNÉMOSYNE, ARGUS...), attachez **UNIQUEMENT** le groupe `pantheon-gold-sg`. Ils seront invisibles depuis Internet.
*   **Cas Exposé (Mode Solo ou VULCAIN) :** Si un service doit être accessible depuis le web (ex: VULCAIN, ou MNÉMOSYNE déployé seul), vous devrez créer un **second** Security Group avec les règles d'ouverture Web (TCP 80/443).
    *   *Référence :* Consultez le guide **`/docs/VULCAIN.md`** pour la matrice de règles d'exposition Web standardisée.
    *   Attachez ce second groupe **en plus** du `pantheon-gold-sg`.

---

## 4. Tableau Récapitulatif des Combinaisons

| Divinité | Mode | Groupe 1 (Socle) | Groupe 2 (Extension) | Source Règles Groupe 2 |
|:---|:---|:---|:---|:---|
| **AEGIS** 🛡️ | Solo | `pantheon-gold-sg` | `sg-aegis` | Guide `/docs/AEGIS.md` |
| **VULCAIN** 🔥 | Équipe | `pantheon-gold-sg` | `sg-vulcain` | Guide `/docs/VULCAIN.md` |
| **JANUS** 🚪 | Équipe | `pantheon-gold-sg` | *(Aucun)* | - |
| **MNÉMOSYNE** ☁️ | Équipe | `pantheon-gold-sg` | *(Aucun)* | - |
| **MNÉMOSYNE** ☁️ | **Solo** | `pantheon-gold-sg` | `sg-web-expose` | Guide `/docs/VULCAIN.md` |
| **ARGUS** 👁️ | Équipe/Solo | `pantheon-gold-sg` | *(Aucun)* | - |

---

## 5. Validation & Tests

Une fois votre instance de test (Étape 2) déployée avec ce groupe, connectez-vous en SSH et vérifiez :

### ✅ Tests de Fonctionnement
```bash
sudo apt update       # Doit passer (DNS + HTTP/S)
timedatectl status    # Doit être synchronisé (NTP)
ping -c 4 1.1.1.1     # Doit répondre (ICMP)
```

### ❌ Tests de Sécurité
```bash
# Depuis l'extérieur (ex: 4G), testez un port non ouvert
nc -vz -w 2 <IP_PUBLIQUE> 6667  # Doit échouer (Timeout)
```

---

## 🤝 Contribuer & Licence

Ce guide fait partie du projet "Le Panthéon de la Souveraineté".

*   🐛 **Signaler une erreur** dans la matrice de ports.
*   💡 **Proposer un protocole sortant** supplémentaire justifié.

**Licence :** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

---

> *"La souveraineté numérique ne se bâtit pas sur du sable. Elle commence par un firewall verrouillé, avant même la première ligne de code."*