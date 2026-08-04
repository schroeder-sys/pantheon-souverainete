# 🛡️ Hors-Série #02 : Le Firewall Périmétrique "Panthéon-Gold"

> **⚠️ PRÉREQUIS ABSOLU : HORS-SÉRIE #01 (RÉSEAU PRIVÉ).**
> Ce guide suppose que vous avez **déjà créé et configuré votre Réseau Privé** (VLAN `192.168.10.0/24`) selon les instructions du **[HS-01 : Constitution du Réseau Privé](hs-01-reseau-prive-pantheon.md)**.
> *Pourquoi ?* Un firewall sans réseau à filtrer est comme un garde du corps sans maison à protéger. Nous allons ici verrouiller les accès à ce réseau que vous venez de créer.

[![Niveau](https://img.shields.io/badge/Niveau-Vital-Critical-red)](#)
[![Couche](https://img.shields.io/badge/Couche-2_Filtrage-orange)](#)
[![Compatibilité](https://img.shields.io/badge/Compat-OpenStack_&_Providers-green)](#)

---

## ⚠️ Pourquoi ce guide est "Vital" ?

Dans l'architecture "Panthéon de la Souveraineté", le Security Group (SG) est votre **première et dernière ligne de défense physique**.
1.  **Pas de Console de Secours (KVM/Rescue) :** Par sécurité, le root n'a pas de mot de passe. Si vous vous enfermez dehors (mauvaise règle SSH), l'instance est perdue à jamais.
2.  **Zero-Trust par Défaut :** Nous partons du principe que **tout le monde est hostile**, sauf ce que nous autorisons explicitement.
3.  **Protection contre les Fuites :** Ce n'est pas seulement pour empêcher les intrusions, c'est surtout pour empêcher vos serveurs (s'ils sont compromis) de "téléphoner" à des pirates (botnets, C&C).

> **📚 Liens avec les autres guides :**
> *   **HS-01 (Réseau Privé) :** À faire **AVANT**. Crée le terrain.
> *   **Ce guide (Firewall) :** À faire **MAINTENANT**. Construit les murs et la porte blindée.
> *   **HS-03 (Image Gold) :** À faire **APRÈS**. Vous utiliserez ce firewall pour sécuriser la création de votre image système.

---

## 1. Philosophie : Séparation des Rôles & Menaces

Pour garantir sécurité ET accès, nous séparons strictement les couches de défense. Comprendre la **menace** permet de comprendre la **règle**.

| Couche | Outil | Gestion | Menace Principale | Notre Parade | Risque de Lock-out |
|:---|:---|:---|:---|:---|:---|
| **1. Périmétrique** | Security Group (Provider) | Interface Web (Horizon) | **Scans de ports & Intrusions externes.** | **Deny All** par défaut. Seule votre IP entre. | **Faible** : Corigeable via le Web en 2 min. |
| **2. Interne** | UFW (Debian) | Ligne de commande (SSH) | **Compromission d'un service voisin.** | **Deny All** + Redondance IPs (Domicile + AEGIS). | **Critique** : Perte totale si erreur de commande. |

> **🛑 RÈGLE D'OR : L'IP DOMICILE N'EST QUE TEMPORAIRE**
> *   **Phase de Démarrage :** Nous utilisons votre IP domicile publique pour autoriser le SSH (Port 22) depuis Internet. C'est la béquille indispensable pour configurer le serveur avant l'installation du VPN.
> *   **Phase de Production (Après AEGIS) :** Cette règle SSH publique devra être **MODIFIÉE** ou **SUPPRIMÉE**.
>     *   Soit en la restreignant à l'IP publique d'AEGIS (Transition).
>     *   Soit en la supprimant totalement pour ne passer que par le réseau privé (Sécurité Max).
> *   **Objectif Final :** Zéro port de gestion (SSH) ouvert sur une IP publique personnelle.

---

## 2. La Matrice de Règles (Standard Panthéon v1.1)

Vous allez créer (ou mettre à jour) un Security Group nommé `pantheon-gold-sg`.
**Politique de base :** **DENY ALL** (Tout bloquer, en entrée comme en sortie) avant d'ajouter les règles ci-dessous.

### 🧠 Comprendre avant de copier : Public vs Privé

> **🌍 Réseau Public vs Réseau Privé :**
> *   **IP Publique (`<VOTRE_IP>`)** : C'est votre adresse réelle sur Internet. Elle change si vous redémarrez votre box. C'est la **seule** adresse autorisée à toucher au port SSH *pour l'instant*.
> *   **IP Privée (`192.168.10.0/24`)** : C'est le réseau interne virtuel créé dans le **HS-01**. Seul ce réseau a le droit de communiquer librement entre les serveurs (pour le futur DNS DoT, les backups, etc.).

### A. Flux Entrants (Ingress) - "Qui a le droit d'entrer ?"

| Ordre | Service | Protocole | Port | Source Autorisée | Pourquoi ? (Menace vs Parade) | Statut |
|:---:|:---|:---|:---|:---|:---|:---:|
| **01** | **SSH** | TCP | `22` | `<VOTRE_IP_V4>/32` <br> `<VOTRE_IP_V6>/128` | **Menace :** Bots scannant le port 22 en permanence.<br>**Parade :** On n'ouvre la porte qu'à votre clé et votre IP précise. | ✅ **Vital (Temp.)** |
| **02** | **Privé** | TCP | `1-65535` | `192.168.10.0/24` | **Menace :** Écoute interne si un serveur est compromis.<br>**Parade :** Seul le réseau de confiance (vos autres serveurs) peut parler en TCP. | ✅ **Vital** |
| **03** | **Privé** | UDP | `1-65535` | `192.168.10.0/24` | **Menace :** Fuite de données via UDP.<br>**Parade :** Seul le réseau de confiance peut parler en UDP (nécessaire pour le futur DNS DoT vers AEGIS). | ✅ **Vital** |
| **04** | **ICMP** | ICMP | `All` | `<VOTRE_IP_V4>/32` <br> `<VOTRE_IP_V6>/128` | **Menace :** Cartographie du réseau par des pirates.<br>**Parade :** Vous seul pouvez "pinger" vos serveurs pour vérifier qu'ils sont vivants. | ℹ️ **Confort** |

> **⚠️ POINT CRITIQUE SUR LE DNS (PORT 53) :**
> Vous ne voyez **aucune règle d'entrée DNS (Port 53) ouverte sur `0.0.0.0/0`**. C'est volontaire !
> *   **Pourquoi ?** Dans l'architecture v1.1 (AEGIS), le DNS est un service **interne**. Seul le serveur AEGIS (situé sur le réseau privé `192.168.10.x`) a le droit de recevoir des requêtes DNS.
> *   **Sécurité :** Cela empêche tout serveur externe d'utiliser le vôtre comme "Open Resolver" pour des attaques DDoS. Le DNS est strictement réservé au réseau privé (règle 03 UDP).

### B. Flux Sortants (Egress) - "Où a-t-on le droit d'aller ?"

C'est ici que se joue la vraie souveraineté. On empêche les fuites de données et les connexions malveillantes sortantes.

| Ordre | Service | Protocole | Port | Destination | Pourquoi ? (Menace vs Parade) | Statut |
|:---:|:---|:---|:---|:---|:---|:---:|
| **01** | **DNS** | UDP/TCP | `53` | `0.0.0.0/0` & `::/0` | **Nécessité :** Permet au serveur de résoudre des noms (ex: `apt update`) **avant** qu'AEGIS ne soit en place. <br>*Note : Une fois AEGIS déployé, ce flux sera restreint au réseau privé.* | ⚠️ **Temporaire** |
| **02** | **HTTP/HTTPS** | TCP | `80`, `443` | `0.0.0.0/0` & `::/0` | **Nécessité :** Indispensable pour les mises à jour (`apt`), télécharger Docker, ou contacter les API externes (Let's Encrypt). | ✅ **Permanent** |
| **03** | **NTP** | UDP | `123` | `0.0.0.0/0` & `::/0` | **Nécessité :** Synchronisation de l'heure. Une heure fausse = échec des certificats SSL et logs inexploitables. | ✅ **Permanent** |
| **04** | **ICMP** | All | `All` | *AUCUNE* | **Sécurité :** Un serveur n'a **jamais** besoin d'initier des pings vers l'extérieur. Si un malware veut pinguer une cible, il sera bloqué ici. | 🛑 **Bloqué** |
| **05** | **TOUT AUTRE** | - | - | **BLOQUÉ** | **Sécurité :** Tout le reste (FTP, SMTP, jeux, P2P) est interdit par défaut. | 🛑 **Sécurité** |

> **ℹ️ Note sur le DNS Temporaire :**
> La règle de sortie DNS (Ordre 01) est ouverte largement **uniquement** pour la phase d'installation.
> *   **Évolution v1.1 :** Une fois le serveur **AEGIS** déployé, nous modifierons cette règle (ou celle d'UFW en interne) pour forcer tout le DNS à passer exclusivement par l'IP privée d'AEGIS via le protocole **DoT** (DNS-over-TLS).

---

## 3. Procédure de Déploiement (Workflow Idéal)

Suivez cet ordre chronologique. Ne brûlez pas les étapes.

### Étape 1 : Vérification du Réseau (HS-01)
Assurez-vous que votre réseau privé `192.168.10.0/24` est créé et fonctionnel (voir **[HS-01](hs-01-reseau-prive-pantheon.md)**).

### Étape 2 : Création du Security Group `pantheon-gold-sg`
1.  Connectez-vous à votre interface **OpenStack Horizon** (Infomaniak Public Cloud).
2.  Allez dans **Network** > **Security Groups**.
3.  Cliquez sur **+ Create Security Group**.
    *   **Name :** `pantheon-gold-sg`
    *   **Description :** `SOCLE PANTHÉON v1.1 : SSH Admin + Réseau Privé + Sorties Vitales (Deny All)`
4.  **⚠️ CRUCIAL :** Assurez-vous que le groupe est **VIDE**. Supprimez toute règle par défaut si elle existe. Nous voulons un "Deny All" total.
5.  Cliquez sur **Manage Rules** et ajoutez les **7 règles** définies dans la matrice ci-dessus (4 en Entrée, 3 en Sortie).
    *   *Attention à bien spécifier vos IPs personnelles (trouvées sur whatsmyip.com) pour le SSH.*
    *   *Vérifiez que le DNS en entrée n'est PAS ouvert sur 0.0.0.0/0.*

### Étape 3 : Création de l'Image Gold (Guide Dédié)
*Une fois ce firewall créé, il servira de base pour l'image système.*

1.  Suivez le guide **[HS-03 : Création de l'Image Gold Debian](hs-03-gold-image.md)**.
2.  Vous y créerez une instance de test en lui attachant **immédiatement** le groupe `pantheon-gold-sg`.
3.  Vous y configurerez le système (Docker, UFW, Hardening) puis vous ferez un **Snapshot**.

### Étape 4 : Déploiement des Divinités
*Quand vous lancerez AEGIS, VULCAIN, etc., vous utiliserez cette Image Gold.*

*   **Cas Standard (Mode Équipe) :** Pour 90% des services (JANUS, MNÉMOSYNE, ARGUS...), attachez **UNIQUEMENT** le groupe `pantheon-gold-sg`. Ils seront invisibles depuis Internet.
*   **Cas Exposé (Mode Solo ou VULCAIN) :** Si un service doit être accessible depuis le web, vous devrez créer un **second** Security Group avec les règles d'ouverture Web (TCP 80/443).
    *   *Référence :* Consultez le futur guide dans le dossier `01_DIVINITES/`.

---

## 4. Tableau Récapitulatif des Combinaisons

| Divinité | Mode | Groupe 1 (Socle) | Groupe 2 (Extension) | Source Règles Groupe 2 |
|:---|:---|:---|:---|:---|
| **AEGIS** 🛡️ | Solo | `pantheon-gold-sg` | `sg-aegis` | Guide `01_DIVINITES/aegis.md` (À venir) |
| **VULCAIN** 🔥 | Équipe | `pantheon-gold-sg` | `sg-vulcain` | Guide `01_DIVINITES/vulcain.md` (À venir) |
| **JANUS** 🚪 | Équipe | `pantheon-gold-sg` | *(Aucun)* | - |
| **MNÉMOSYNE** ☁️ | Équipe | `pantheon-gold-sg` | *(Aucun)* | - |
| **MNÉMOSYNE** ☁️ | **Solo** | `pantheon-gold-sg` | `sg-web-expose` | Guide `01_DIVINITES/vulcain.md` (À venir) |
| **ARGUS** 👁️ | Équipe/Solo | `pantheon-gold-sg` | *(Aucun)* | - |

---

## 5. Validation & Tests

Une fois votre instance de test déployée avec ce groupe, connectez-vous en SSH et vérifiez :

### ✅ Tests de Fonctionnement
```bash
sudo apt update       # Doit passer (DNS + HTTP/S sortants autorisés)
timedatectl status    # Doit être synchronisé (NTP sortant autorisé)
ping -c 4 1.1.1.1     # Doit répondre (ICMP entrant depuis votre IP autorisé)
```

### ❌ Tests de Sécurité
```bash
# Depuis l'extérieur (ex: 4G), testez un port non ouvert
nc -vz -w 2 <IP_PUBLIQUE> 6667  # Doit échouer (Timeout)
# Testez le port DNS en entrée (ne doit pas répondre depuis l'extérieur)
nc -vz -u -w 2 <IP_PUBLIQUE> 53 # Doit échouer (Timeout)
```

---

## 6. Maintenance : Évolution vers la Sécurité Maximale

Une fois votre serveur **AEGIS** (VPN) déployé et fonctionnel, vous n'avez plus besoin de laisser votre IP domicile accéder directement au port 22. C'est le moment de durcir. Deux méthodes s'offrent à vous.

### 🏫 École 1 : La Transition Douce (Recommandée pour débuter)
*Principe : On garde l'accès SSH via l'IP publique, mais on change la "clé" d'autorisation.*

Au lieu d'autoriser votre IP domicile, vous autorisez **l'IP Publique de votre serveur AEGIS**.
*   **Avantage :** Vous continuez à vous connecter en SSH en utilisant l'IP publique de vos serveurs. Rien ne change dans votre client SSH.
*   **Sécurité :** Seule la machine AEGIS a le droit de toucher au port 22.

**Procédure :**
1.  Notez l'**IP Publique** de votre serveur AEGIS.
2.  Allez dans **Security Groups** > `pantheon-gold-sg` > **Manage Rules**.
3.  **Supprimez** l'ancienne règle SSH (`<VOTRE_IP_DOMICILE>`).
4.  **Créez une nouvelle règle** : `Ingress | TCP | 22 | <IP_PUBLIQUE_D_AEGIS>/32`.

---

### 🛡️ École 2 : La Sécurité Maximale (Recommandée pour les experts)
*Principe : On coupe totalement l'accès SSH public. Le SSH ne transite plus que dans le tunnel.*

On supprime **toute** règle SSH (Port 22) dans le Security Group.
*   **Pourquoi ça marche ?** Une fois dans le tunnel VPN, votre trafic SSH est considéré comme du trafic "Privé". Il est donc automatiquement autorisé par la règle `Privé TCP 1-65535`.
*   **Avantage :** Surface d'attaque publique nulle.
*   **Contrainte :** Vous devez modifier votre client SSH pour qu'il se connecte à l'**IP PRIVÉE** du serveur.

**Procédure :**
1.  **Test Préalable (CRUCIAL) :** Activez votre VPN et tentez `ssh user@192.168.10.X` (IP privée). Si ça marche, continuez.
2.  **L'Action Irréversible :** Supprimez la règle SSH (Port 22) dans le Security Group. **Ne la remplacez par rien.**
3.  **Mise à jour de votre client SSH :**
    *   Vous devez désormais vous connecter à l'IP privée.
    *   💡 **Astuce Outil :** Nous utilisons **Tabby.sh** (Open Source) pour sa gestion moderne des profils, mais tout client (PuTTY, OpenSSH) fonctionne.
    *   Dans **Tabby** : Settings > Profiles > Changez le **Host** par l'IP privée (`192.168.10.X`).
    *   *Bonus :* Dupliquez le profil pour garder une version "Public" (secours) et une "Privée" (quotidien).

---

## 🤝 Contribuer & Licence

Ce guide fait partie du projet "Le Panthéon de la Souveraineté". Les outils mentionnés (Tabby, Debian, Infomaniak) le sont pour leur adéquation avec nos valeurs (Open Source, Éthique, Souveraineté), mais restent remplaçables par des équivalents respectant ces critères.

*   🐛 **Signaler une erreur** dans la matrice de ports.
*   💡 **Proposer un protocole sortant** supplémentaire justifié.

**Licence :** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

---

> *"La souveraineté numérique ne se bâtit pas sur du sable. Elle commence par un firewall verrouillé, avant même la première ligne de code. Ce guide est vos murs ; le HS-01 est vos fondations."*