> 🏠 **Développé sur :** [GitLab](https://gitlab.com/schroeder-sys/pantheon-souverainete) | 🪞 **Miroir automatisé sur :** [GitHub](https://github.com/schroeder-sys/pantheon-souverainete)

# 🏛️ Le Panthéon de la Souveraineté

> **Une infrastructure cloud 100% souveraine, redondante et sécurisée, conçue pour s'affranchir totalement des GAFAM.**
> *Version 1.1 : Socle réseau privé, chiffrement des requêtes DNS des serveurs (DoT) et architecture de backup multi-datacenters (Suisse, Allemagne, Finlande).*

[![Version](https://img.shields.io/badge/Version-1.1%20Current-blue)](#)
[![Status](https://img.shields.io/badge/Status-En%20Construction-orange)](#)
[![License](https://img.shields.io/badge/License-CC%20BY--SA%204.0-green)](#)
[![Stack](https://img.shields.io/badge/Stack-100%25%20Open%20Source-orange)](#)
[![Privacy](https://img.shields.io/badge/Privacy-Server%20DoT%20Ready-red)](#)

---

## ⚠️ Avertissements Importants & Méthodologie

### 1. Contexte du Projet & Versionning
Ce projet naît d'un besoin de souveraineté totale et suit une évolution itérative :
*   **Contexte :** Une infrastructure hébergée principalement en **Suisse** (Infomaniak), dont les données sont répliquées de manière chiffrée sur deux sites secondaires en **Allemagne** et **Finlande** (Hetzner). Aucune donnée ne dort sur un seul site.
*   **v1.0 (Fondations) :** Définition du Réseau Privé et du Firewall Périmétrique (HS-01 & HS-02). *Base acquise.* Les serveurs communiquent en réseau privé, mais leurs requêtes de résolution (DNS) peuvent encore transiter en clair.
*   **v1.1 (Actuelle - Souveraineté des Serveurs) :** Intégration native du **DoT (DNS-over-TLS)**. Désormais, **chaque serveur** (comme VULCAIN) chiffre ses propres requêtes DNS (mises à jour, résolution de services) vers le socle AEGIS. Plus aucun serveur ne parle en clair au DNS, même en réseau privé.
*   **En Réflexion (mTLS) :** Étude d'un durcissement majeur via **mTLS (Mutual TLS)** pour authentifier les services entre eux. Cette piste est encore à l'étude et n'a pas encore de numéro de version attribué.

### 2. Profil de l'Auteur & Transparence
Je ne suis **pas un professionnel de l'IT** diplômé. Ce projet est le fruit d'une passion pour l'administration système, construite en auto-formation ces 5 dernières années.
*   **Risque d'erreur :** Bien que je m'appuie scrupuleusement sur les documentations officielles, je ne suis pas exempt d'erreurs de compréhension ou d'interprétation.
*   **Assistance par IA (Dualité EURIA & PROMÉTHÉE) :**
    *   **Actuellement :** **EURIA (Infomaniak)** assure à la fois la rédaction de la documentation et l'assistance technique pour les commandes et l'architecture.
    *   **Futur (Projet PROMÉTHÉE) :** Une IA locale dédiée prendra le relais sur les aspects purement techniques et l'automatisation sensible, tandis qu'Euria se concentrera sur la documentation et la méthodologie.
    *   *Note :* Chaque commande est vérifiée humainement.
*   **Posture Éthique :** Abandon immédiat de tout outil dont la politique de confidentialité est douteuse.

### 3. État du Projet & Mise en Garde
Ce projet est en **phase de construction active (v1.1)**.
*   **Documentation :** Les guides sont rédigés au fur et à mesure du déploiement réel.
*   **Statut des services :**
    *   **HS-01 (Réseau Privé) :** 🚧 **FINALISATION (J-4)**. Le réseau est déjà opérationnel en production, le guide écrit arrive sous 96h.
    *   **HS-02 (Firewall) :** ✅ **VALIDÉ**. Matrice de règles v1.1 corrigée (DNS fermé en entrée, ICMP sortant bloqué).
    *   **HS-03 (Image Gold) :** ✅ **VALIDÉ**. Prêt pour la création du snapshot de référence.
    *   **VULCAIN :** En cours de déploiement (Priorité n°1). Sera configuré pour utiliser le DoT vers AEGIS.
    *   **AEGIS :** Architecture DoT/DoH définie, prête au déploiement.
    *   **Backups (ARK) :** Architecture multi-DC (Suisse, Allemagne, Finlande) validée dans les specs, en cours de déploiement.
*   🛑 **AVERTISSEMENT :** Ne déployez rien en production sans avoir validé chaque étape dans un environnement de test.

### 4. Limitation de Responsabilité
Projet éducatif et personnel. La sécurité de vos données et la stratégie de backup sont de votre **entière responsabilité**.

---

## 📜 Philosophie & Objectifs

Démontrer qu'il est possible de construire une infrastructure **redondante, chiffrée de bout en bout et indépendante**.

**Les 3 Piliers Fondamentaux (v1.1) :**
1.  **Souveraineté & Redondance :** Hébergement principal en **Suisse** (Infomaniak) avec réplication des backups en **Allemagne** et **Finlande** (Hetzner).
2.  **Security by Design (Chiffrement des Serveurs) :**
    *   **Actuel (v1.1) :** **Les serveurs eux-mêmes** chiffrent leurs requêtes DNS via le protocole **DoT (DNS-over-TLS)** vers AEGIS.
    *   **Règle de Flux :** Tout flux sortant non essentiel est strictement bloqué par le firewall **OU** filtré au niveau DNS.
    *   **Futur (Réflexion mTLS) :** Implémentation potentielle d'une authentification mutuelle par certificats.
3.  **Ordre Logique de Déploiement :** **Réseau Privé** → **Porte d'Entrée (VULCAIN)** → Socle DNS (AEGIS) → Services Métiers.

---

## 🤝 Les Tiers de Confiance Assumés

| Tiers | Rôle | Pourquoi ce choix ? | Souveraineté |
|:---|:---|:---|:---|
| **Infomaniak** 🇨🇭 | Hébergeur Principal | Données en Suisse. DPA signé. Écologie réelle. | ✅ **Totale** |
| **Hetzner** 🇩🇪 🇫🇮 | Hébergeur Secondaire (Backup) | Redondance stricte : Stockage froid répliqué sur **deux** DC (Allemagne & Finlande). | ✅ **Européen** |
| **CrowdSec** 🇫🇷 | Intelligence de Menaces | Partage communautaire d'IP malveillantes. | ✅ **Européenne** |
| **Certum** 🇵🇱 | Certificats SSL & S/MIME | Autorité européenne, conforme **eIDAS**. Remplace Let's Encrypt. | ✅ **Européenne** |
| **Debian** 🌍 | Système d'exploitation | Base universelle, stable, communautaire. | ✅ **Standard Ouvert** |
| **Docker** 🇺🇸 | Conteneurisation | Standard de facto. Utilisation sans télémétrie active. | ✅ **Standard** |

---

## 🗺️ Architecture du Projet (Les 12 Divinités)

| Ordre | Divinité | Rôle | Stack Technologique Validée | Origine | Statut |
|:---:|:---|:---|:---|:---:|:---:|
| **01** | **VULCAIN** 🔥 | Reverse Proxy & WAF | Traefik + CrowdSec + ALTCHA (PoW) | 🇫🇷/🇦🇹 | 🚧 **En Cours** |
| **02** | **AEGIS** 🛡️ | **DNS Souverain & VPN** | WireGuard + AdGuard (DoH/DoQ) + Unbound (DoT + DNSSEC) | 🇺🇸/🇨🇾/🇳🇱 | ⏳ **À Venir** |
| **03** | **ARGUS** 👁️ | Monitoring | Grafana + Prometheus + Node Exporter | 🇸🇪/🇺🇸 | ⏳ **À Venir** |
| **04** | **JANUS** 🚪 | Identité & SSO | Bitwarden + Authentik (SSO)* | 🇺🇸/🇺🇸* | 📋 *Audit à faire* |
| **05** | **ARK** 🗄️ | Backups Multi-DC | Restic + Swift (CH) + S3 (DE/FI) | 🇩🇪/🇨🇭/🇫🇮 | ✅ **Validé** |
| **06** | **HERMÈS** 📧 | Email | *(En attente)* | - | ⚪ **Vierge** |
| **07** | **MIDAS** 💰 | Finances | *(En attente)* | - | ⚪ **Vierge** |
| **08** | **DIANE** 🏹 | Cartes & NTP | Valhalla + Photon | 🇩🇪 | ✅ **Validé** |
| **09** | **VERITAS** 🔍 | Recherche | SearXNG | 🇫🇷 | ✅ **Validé** |
| **10** | **HÉPHAÏSTOS** 🔨 | Forge Git | Forgejo | 🌍 | ✅ **Validé** |
| **11** | **MNÉMOSYNE** ☁️ | Cloud & Fichiers | Nextcloud Hub | 🇩🇪 | ✅ **Validé** |
| **12** | **PROMÉTHÉE** 🔥 | IA Locale | Ollama + Mixtral | 🇫🇷 | ✅ **Validé** |

> ### 🔍 Focus Évolution : Du DoT au mTLS
> *   **Version 1.1 (DoT) :** Chaque serveur chiffre ses requêtes DNS vers AEGIS.
> *   **Réflexion Future (mTLS) :** Authentification mutuelle par certificats pour un Zero-Trust total.

---

### 🧩 Philosophie "Lego" : Modularité & Universalité

*   🏗️ **Hors-Séries (Spécifique Cloud) :** Les guides de socle (Réseau, Firewall, Image) sont conçus spécifiquement pour le **Cloud Public**.
*   🌍 **Universalité des Divinités :** Une fois le socle en place, les guides d'installation sont compatibles **Cloud Public**, **VPS** et **Local**.

**L'ordre de construction (v1.1) :**
1.  **Socle Réseau (HS-01) :** La fondation invisible (enfin à sa place !).
2.  **Firewall (HS-02) :** Les murs et la porte blindée.
3.  **Image Gold (HS-03) :** Le moule système sécurisé.
4.  **Services :** Déployés et connectés.

> **🤡 Note de l'auteur sur le réordonnancement :**
> *Vous avez peut-être remarqué que le "Hors-Série #1" a mis du temps à arriver alors que le "#2" était déjà là. Non, ce n'est pas une faille de sécurité, c'est une fonctionnalité expérimentale appelée **"Déploiement par la fenêtre"**.*
> *En bon architecte passionné, j'ai commencé par peindre les volets (Firewall) et choisir les meubles (Image Gold) avant de couler les fondations (Réseau). C'est l'évidence même qui est souvent invisible... jusqu'à ce qu'on réalise qu'on ne peut pas poser les murs. 🤦‍♂️*
> *Rassurez-vous : le réseau était déjà monté en coulisses ! Le guide HS-01 arrive juste pour expliquer comment j'ai coulé ces fondations que j'avais posées en pilote automatique.*

---

## 📚 Documentation & Guides de Déploiement

### 🗂️ Structure de la Documentation

La documentation est organisée dans le dossier [`docs/`](docs/) :

*   📂 **[00_HORS-SERIES](docs/00_HORS-SERIES/)** : Guides transverses **Spécifiques Cloud Public**.
    *   ⏳ **A venir sous peu :** [HS-01 : Constitution du Réseau Privé](docs/00_HORS-SERIES/hs-01-reseau-prive-pantheon.md) *(La vraie fondation !)*
    *   ✅ **Disponible :** [HS-02 : Le Firewall Périmétrique](docs/00_HORS-SERIES/hs-02-security-group.md) *(Les murs de la forteresse)*
    *   ✅ **Disponible :** [HS-03 : Création de l'Image Gold Debian](docs/00_HORS-SERIES/hs-03-gold-image.md) *(Le moule système)*
    
    > **🔄 Note sur le "Chasse-Croisée" des Hors-Séries :**
    > *Vous avez peut-être remarqué que les numéros ont bougé ? C'est normal (et assumé !).*
    > *Initialement, le Firewall était le HS-01 et le Réseau le HS-02 (oublié). Nous nous sommes rendu compte qu'il est difficile de filtrer un réseau qui n'existe pas encore ! 🤦‍♂️*
    > *Nous avons donc procédé à un **remaniement complet** pour respecter l'ordre logique de déploiement :*
    > *1. Réseau (HS-01) → 2. Firewall (HS-02) → 3. Image (HS-03).*
    > *Si vous aviez suivi l'ancienne numérotation, toutes nos excuses ! C'est désormais corrigé et cohérent.*

*   📂 **[01_DIVINITES](docs/01_DIVINITES/)** : Guides de déploiement spécifiques (Compatibles Cloud, VPS & Local).
*   📂 **[99_COMMUN](docs/99_COMMUN/)** : Ressources partagées.

---

### 🚀 Par où commencer ? (Quick Start)

1.  **Lisez ce README** pour comprendre l'architecture v1.1.
2.  **Déployez le socle de sécurité (Dans l'ordre !):**
    *   📄 **[Hors-Série #1 : Le Réseau Privé](docs/00_HORS-SERIES/hs-01-reseau-prive-pantheon.md)** (La fondation).
    *   📄 **[Hors-Série #2 : Le Firewall Périmétrique](docs/00_HORS-SERIES/hs-02-security-group.md)** (La sécurité).
    *   📄 **[Hors-Série #3 : L'Image Gold Debian](docs/00_HORS-SERIES/hs-03-gold-image.md)** (Le système).
3.  **Surveillez les prochaines publications :**
    *   🔥 **Guide VULCAIN** – En rédaction.
    *   🛡️ **Guide AEGIS** – À venir.

> 💡 **Conseil :** **Starrez (⭐) ce dépôt** pour être notifié des mises à jour.

---

## 💰 Modèle Économique & Hébergement

*   **Hébergeur Principal :** Infomaniak (**Suisse**).
*   **Hébergeur Secondaire (Backup) :** Hetzner (**Allemagne** & **Finlande**).
*   **Redondance :** 3 Datacenters distincts. Backups chiffrés (AES-256).
*   **Coût Cible :** 200€~300€/mois.

*   **Soutien au projet :**
    *   ☁️ **[Infomaniak Public Cloud](https://www.infomaniak.com/fr/hebergement/public-cloud?utm_term=68ceeeb369ce1)**
    *   🖥️ **[Infomaniak VPS](https://www.infomaniak.com/fr/hebergement/vps-cloud?utm_term=68ceeeb369ce1)**
    *   📧 **[kSuite (incluant kDrive & Euria)](https://www.infomaniak.com/fr/ksuite?utm_term=68ceeeb369ce1)**
    *   ℹ️ **[Site Officiel Infomaniak](https://infomaniak.com/fr)**

---

## 🚀 Roadmap (v1.1 - Construction)

- [x] **Phase 0 : Conception & Outils**
- [x] **Réorganisation des Hors-Séries** – Correction de l'ordre logique (Réseau → Firewall → Image).
- [x] **Publication HS-02 & HS-03** – Firewall et Image Gold.
- [ ] **Publication HS-01** – Réseau Privé (La fondation manquante).
- [ ] **Phase 1 : VULCAIN** – Déploiement du Proxy.
- [ ] **Phase 2 : AEGIS** – Déploiement du socle DNS.
- [ ] **Phase 3 : ARK & Données** – Backups multi-DC.
- [ ] **Réflexion mTLS** – Étude d'implémentation.

---

## 🤝 Contribuer & Licence

**Licences :**
*   **Contenu (Guides) :** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
*   **Logiciels cités :** Chaque divinité respecte la licence de son logiciel constitutif.

---

> *"La souveraineté numérique n'est pas un produit que l'on achète, c'est une architecture que l'on bâtit. Parfois, il faut déplacer les briques pour que l'édifice tienne debout."*