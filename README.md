> 🏠 **Développé sur :** [GitLab](https://gitlab.com/schroeder-sys/pantheon-souverainete) | 🪞 **Miroir automatisé sur :** [GitHub](https://github.com/schroeder-sys/pantheon-souverainete)

# 🏛️ Le Panthéon de la Souveraineté

> **Une infrastructure cloud 100% souveraine, redondante et sécurisée, conçue pour s'affranchir totalement des GAFAM.**
> *Version 1.0 : Architecture fondée sur le chiffrement interne (DoT/DoH) et la redondance multi-datacenters.*

[![Version](https://img.shields.io/badge/Version-1.0%20Official-blue)](#)
[![Status](https://img.shields.io/badge/Status-En%20Construction-orange)](#)
[![License](https://img.shields.io/badge/License-CC%20BY--SA%204.0-green)](#)
[![Stack](https://img.shields.io/badge/Stack-100%25%20Open%20Source-orange)](#)
[![Privacy](https://img.shields.io/badge/Privacy-DoT%2FDoH%20Internal-red)](#)

---

## ⚠️ Avertissements Importants & Méthodologie

### 1. Contexte du Projet (Démarrage à Froid)
Ce projet démarre officiellement avec cette version 1.0. Il intègre dès la conception les retours d'expérience d'une phase préparatoire privée, marquée par :
*   **Une reconstruction totale (Rebuild) :** Pour garantir un ordre de déploiement logique et éviter les dettes techniques (ex: commencer par le Proxy avant le DNS pour faciliter les reconfigurations).
*   **Un durcissement sécurité natif :** Implémentation systématique du chiffrement DNS (DoT/DoH) même en réseau privé.
*   **Une hygiène numérique stricte :** Utilisation exclusive de logiciels libres audités et abandon de tout outil client (SSH, etc.) dont la politique de confidentialité est jugée insuffisante.
*   **Une base saine :** Déploiement sur **Debian** (pur) avec régénération complète des clés de sécurité.

### 2. Profil de l'Auteur & Transparence
Je ne suis **pas un professionnel de l'IT** diplômé. Ce projet est le fruit d'une passion pour l'administration système, construite en auto-formation ces 5 dernières années.
*   **Risque d'erreur :** Bien que je m'appuie scrupuleusement sur les documentations officielles, je ne suis pas exempt d'erreurs de compréhension ou d'interprétation.
*   **Assistance par IA (EURIA) :** Ce projet est réalisé avec l'assistance d'**EURIA** (IA d'Infomaniak) pour la rédaction et la structuration. Chaque commande est vérifiée humainement.
    *   *Futur :* Le projet **PROMÉTHÉE** (IA locale) remplacera EURIA à terme pour une autonomie totale.
*   **Posture Éthique :** Abandon immédiat de tout outil dont la politique de confidentialité est douteuse.

### 3. État du Projet & Mise en Garde
Ce projet est en **phase de construction active**.
*   **Documentation :** Les guides sont rédigés au fur et à mesure du déploiement réel.
*   **Statut des services :**
    *   **VULCAIN :** En cours de déploiement (Priorité n°1).
    *   **AEGIS :** En attente de déploiement (Nouvelle archi DNS).
    *   **Backups (ARK) :** Architecture multi-DC (Infomaniak + Hetzner) en conception.
*   🛑 **AVERTISSEMENT :** Ne déployez rien en production sans avoir validé chaque étape dans un environnement de test. Le non-respect des phases de test se fait à **vos risques et périls**.

### 4. Limitation de Responsabilité
Projet éducatif et personnel. L'auteur ne garantit pas l'absence de bugs. La sécurité de vos données et la stratégie de backup (ARK) sont de votre **entière responsabilité**.

---

## 📜 Philosophie & Objectifs

Démontrer qu'il est possible de construire une infrastructure **redondante, chiffrée de bout en bout et indépendante**, sans dépendre des GAFAM.

**Les 3 Piliers Fondamentaux :**
1.  **Souveraineté & Redondance :** Hébergement principal en Suisse (Infomaniak) avec réplication des backups sur un second Datacenter indépendant (Hetzner).
2.  **Security by Design (Chiffrement Interne) :**
    *   Aucun flux DNS en clair, même en réseau privé.
    *   Utilisation systématique de **DoT (DNS-over-TLS)** pour les communications inter-instances.
    *   Utilisation de **DoH/DoQ (DNS-over-HTTPS/QUIC)** pour les clients VPN.
3.  **Ordre Logique de Déploiement :** Respect strict de la chaîne de construction : **Porte d'Entrée (VULCAIN)** → Socle DNS (AEGIS) → Services Métiers.

---

## 🤝 Les Tiers de Confiance Assumés

La souveraineté n'est pas l'isolement, c'est le choix maîtrisé de ses dépendances.

| Tiers | Rôle | Pourquoi ce choix ? | Souveraineté |
|:---|:---|:---|:---|
| **Infomaniak** 🇨🇭 | Hébergeur Principal | Données en Suisse (hors Cloud Act/UE). DPA signé. Écologie réelle. | ✅ **Totale** |
| **Hetzner** 🇩🇪 | Hébergeur Secondaire (Backup) | Redondance géographique (DC distinct). Stockage objet froid pour ARK. | ✅ **Européen** |
| **CrowdSec** 🇫🇷 | Intelligence de Menaces | Partage communautaire d'IP malveillantes. Protection temps réel. | ✅ **Européenne** |
| **Let's Encrypt** 🌍 | Certificats SSL | Standard mondial pour le chiffrement TLS 1.3. | ✅ **Standard Ouvert** |
| **Debian** 🌍 | Système d'exploitation | Base universelle, stable, communautaire et auditée. | ✅ **Standard Ouvert** |
| **Docker** 🇺🇸 | Conteneurisation | Standard de facto. Utilisation avec DPA public et sans télémétrie active. | ✅ **Standard** |

> **Règle d'Or :** Tout flux sortant non essentiel (télémétrie, mises à jour auto non contrôlées) est **strictement bloqué** par firewall.

---

## 🗺️ Architecture du Projet (Les 12 Divinités)

| Ordre | Divinité | Rôle | Stack Technologique Validée | Origine | Statut |
|:---:|:---|:---|:---|:---:|:---:|
| **01** | **VULCAIN** 🔥 | Reverse Proxy & WAF | Traefik + CrowdSec + ALTCHA (PoW) | 🇫🇷/🇦🇹 | 🚧 **En Cours** |
| **02** | **AEGIS** 🛡️ | **DNS Souverain & VPN** | WireGuard + AdGuard (DoH/DoQ) + Unbound (DoT + DNSSEC) | 🇺🇸/🇨🇾/🇳🇱 | ⏳ **À Venir** |
| **03** | **ARGUS** 👁️ | Monitoring | Grafana + Prometheus + Node Exporter | 🇸🇪/🇺🇸 | ⏳ **À Venir** |
| **04** | **JANUS** 🚪 | Identité & SSO | Bitwarden (Officiel) + Authentik (SSO)* | 🇺🇸/🇺🇸* | 📋 *Audit à faire* |
| **05** | **ARK** 🗄️ | Backups Multi-DC | Restic + Swift (Infomaniak) + S3 (Hetzner) | 🇩🇪/🇨🇭/🇩🇪 | ✅ **Validé** |
| **06** | **HERMÈS** 📧 | Email | *(En attente)* | - | ⚪ **Vierge** |
| **07** | **MIDAS** 💰 | Finances | *(En attente)* | - | ⚪ **Vierge** |
| **08** | **DIANE** 🏹 | Cartes & NTP | Valhalla + Photon (rayon < 500Km) | 🇩🇪 | ✅ **Validé** |
| **09** | **VERITAS** 🔍 | Recherche | SearXNG | 🇫🇷 | ✅ **Validé** |
| **10** | **HÉPHAÏSTOS** 🔨 | Forge Git | Forgejo (Fork communautaire) | 🌍 | ✅ **Validé** |
| **11** | **MNÉMOSYNE** ☁️ | Cloud & Fichiers | Nextcloud Hub (Éditeur natif) | 🇩🇪 | ✅ **Validé** |
| **12** | **PROMÉTHÉE** 🔥 | IA Locale | Ollama + Mixtral (Mistral AI) | 🇫🇷 | ✅ **Validé** |

*\*Note sur JANUS : Authentik (US) est en cours d'audit pour vérifier l'absence de télémétrie. Plan B : Authelia.*

> ### 🖼️ Schéma d'Architecture (À venir)
> *Un schéma détaillé illustrant les flux chiffrés (DoT/DoH), la topologie multi-DC (CH/DE/FI) et l'interconnexion des instances sera ajouté ici dès la finalisation du déploiement de VULCAIN. La priorité est donnée à l'exactitude technique plutôt qu'à l'illustration prématurée.*

**🔍 Focus sur la stratégie de déploiement (VULCAIN First) :**
Contrairement à une intuition première, **VULCAIN** est déployé avant **AEGIS**.
*   **Pourquoi ?** Il est techniquement plus simple et moins risqué de modifier la configuration DNS des instances (quelques lignes) que de devoir recréer des conteneurs Proxy, régénérer des certificats SSL et reconfigurer les routages si le DNS changeait après coup.
*   **Résultat :** VULCAIN fournit une plateforme stable (HTTPS, WAF) dès le jour 1. AEGIS vient ensuite se "brancher" dessus comme un service sécurisé parmi d'autres.

---

### 🧩 Philosophie "Lego" : Modularité & Ordre

Imaginez ce projet comme un set **Lego Star Wars** complexe :
*   🚀 **Modularité :** Chaque brique (Divinité) est fonctionnelle seule.
*   🏗️ **Ordre de construction :** L'ordre est critique pour éviter de devoir démonter/remonter.

**Le principe clé (v1.0) :**
1.  **Porte (VULCAIN) :** Reverse Proxy et WAF. On construit la route et la sécurité périmétrique en premier.
2.  **Socle (AEGIS) :** DNS et Réseau sécurisés. On connecte le résolveur DNS chiffré une fois la route prête.
3.  **Services (Les autres) :** Déployés et connectés au fur et à mesure.

> **Conseil :** Ne brûlez pas les étapes. Cette version 1.0 impose cet ordre logique pour garantir la stabilité et vous éviter de devoir recommencer (comme je l'ai fait en phase privée 😉).

---

## 🛡️ Modèle de Sécurité

Cette version marque un saut qualitatif en matière de sécurité :
*   **Chiffrement Interne Systématique :** Adoption de DoT/DoH pour *tous* les flux DNS, éliminant le risque d'écoute même sur le réseau privé de l'hébergeur.
*   **Hygiène Numérique Stricte :** Abandon immédiat de tout outil client (SSH, gestion) ne respectant pas une charte de confidentialité stricte. Rotation des clés SSH lors du déploiement initial.
*   **Redondance des Backups (ARK) :** Stratégie 3-2-1 avec copie externe chez un second hébergeur européen (Hetzner) pour parer à la perte totale d'un datacenter.
*   **Zero-Trust :** Isolation stricte des instances et authentification forte via JANUS.

---

## 📚 Documentation & Guides de Déploiement

Ce dépôt propose une **méthodologie durable**. Pas de scripts "magiques", mais des guides basés sur la doc officielle.
Chaque guide inclura :
1.  Prérequis & Dimensionnement.
2.  Architecture Réseau (Flux chiffrés DoT/DoH).
3.  Procédure d'Installation & Hardening.
4.  Maintenance & Backup (ARK).

### 🗂️ Structure de la Documentation

La documentation est organisée de manière modulaire dans le dossier [`docs/`](docs/) :

*   📂 **[00_HORS-SERIES](docs/00_HORS-SERIES/)** : Guides transverses indispensables (Firewall, Image Système, Sécurité).
    *   ✅ **Disponible :** [HS-01 : Le Firewall Périmétrique "Panthéon-Gold"](docs/00_HORS-SERIES/hs-01-security-group.md)
    *   ⏳ *À venir :* HS-02 : Création de l'Image Gold Debian.
*   📂 **[01_DIVINITES](docs/01_DIVINITES/)** : Guides de déploiement spécifiques à chaque service (Vulcain, Aegis, etc.).
*   📂 **[99_COMMUN](docs/99_COMMUN/)** : Ressources partagées, schémas d'architecture et snippets de configuration.

---

### 🚀 Par où commencer ? (Quick Start)

Le projet étant en construction, voici comment vous pouvez déjà vous impliquer :

1.  **Lisez ce README** pour comprendre l'architecture v1.0 et la logique de déploiement.
2.  **Étudiez le premier guide officiel :**
    *   📄 **[Hors-Série #1 : Le Firewall Périmétrique](docs/00_HORS-SERIES/hs-01-security-group.md)** (Déjà en ligne). C'est le prérequis absolu avant toute création d'instance.
3.  **Surveillez les prochaines publications :**
    *   🔐 **Hors-Série #2 : La Gold Image Debian** (Guide de durcissement de l'OS de base).
    *   🔥 **Guide VULCAIN** (Déploiement du Proxy et WAF).
4.  **Ne déployez rien** en production tant que les guides ne sont pas marqués "PROD".

> 💡 **Conseil :** Starrez (⭐) ce dépôt pour être notifié des mises à jour et de la publication des Hors-Séries dans les prochains jours.

---

## 💰 Modèle Économique & Hébergement

La v1.0 intègre un modèle de redondance professionnelle.

*   **Hébergeur Principal :** Infomaniak (🇨🇭 Suisse). Pour les instances de production (Souveraineté, DPA, Écologie).
*   **Hébergeur Secondaire (Stockage Backup) :** Hetzner (🇩🇪 Allemagne & 🇫🇮 Finlande). Uniquement pour le stockage froid des backups (ARK), assurant une redondance géographique sur **3 Datacenters distincts** (1x Suisse, 1x Allemagne, 1x Finlande).
*   **Sécurité des Données (ARK) :** Toutes les sauvegardes sont **chiffrées côté client (AES-256)** via Restic *avant* leur envoi. Ni Infomaniak ni Hetzner ne peuvent accéder au contenu de vos données.
*   **Coût Cible :** 200€~300€/mois.
    *   Ce budget couvre l'ensemble des 12 services en production, avec une allocation de ressources confortable et la réplication des sauvegardes sur ces trois datacenters distincts.
    *   *Note : Ce coût est une estimation "Production Redondante". Une version "Solo" sans redondance externe coûterait environ le tiers.*
*   **Soutien au projet :** Si ce projet vous est utile, vous pouvez soutenir son développement via les liens d'affiliation ci-dessous (sans surcoût pour vous) :
    *   ☁️ **[Infomaniak Public Cloud](https://www.infomaniak.com/fr/hebergement/public-cloud?utm_term=68ceeeb369ce1)** – Pour le socle principal.
    *   🖥️ **[Infomaniak VPS](https://www.infomaniak.com/fr/hebergement/vps-cloud?utm_term=68ceeeb369ce1)** – Pour les instances légères.
    *   📧 **[kSuite (incluant kDrive & Euria)](https://www.infomaniak.com/fr/ksuite?utm_term=68ceeeb369ce1)** – Pour la suite collaborative.
    *   ℹ️ **[Site Officiel Infomaniak (Lien non affilié)](https://infomaniak.com/fr)**

> *Un clin d'œil sincère à **Infomaniak** : ce projet n'aurait jamais vu le jour sans un hébergeur capable de cocher simultanément les cases "Souveraineté Suisse", "Écologie réelle" et "Prix accessibles". Merci de rendre la liberté numérique économiquement viable.*

---

## 🚀 Roadmap (v1.0 - Construction)

- [x] **Phase 0 : Conception & Outils** – Mise en place de l'environnement de rédaction (Debian, VSCodium) et finalisation des specs VULCAIN.
- [x] **Publication Hors-Série #1** – Guide du Firewall Périmétrique (Gold Security Group).
- [ ] **Phase 1 : Porte d'Entrée (VULCAIN)** – Déploiement du Proxy et du WAF. *(Priorité Actuelle)*
- [ ] **Phase 2 : Socle DNS (AEGIS)** – Déploiement de l'instance avec DoT/DoH natif et connexion à VULCAIN.
- [ ] **Phase 3 : Observabilité (ARGUS)** – Mise en place du monitoring centralisé.
- [ ] **Phase 4 : Identité & Données (JANUS, MNÉMOSYNE, ARK)** – Déploiement du SSO, du Cloud et configuration de la redondance des backups (Infomaniak + Hetzner).
- [ ] **Phase 5 : Services Utilisateurs** – VERITAS, DIANE, HÉPHAÏSTOS.
- [ ] **Phase 6 : Avancé** – HERMÈS, MIDAS, PROMÉTHÉE.

---

## 🤝 Contribuer & Licence

Projet éducatif en construction ouverte.

**Comment contribuer ?**
*   🐛 **Signaler un bug** ou une erreur dans un guide (ouvrez une "Issue").
*   🌍 **Traduire** un guide dans votre langue maternelle.
*   📝 **Partager** votre retour d'expérience (REX) après avoir déployé une brique.
*   💡 **Proposer** une optimisation de sécurité ou de performance.

**Licences :**
*   **Contenu (Guides) :** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) - Libre partage et adaptation avec citation.
*   **Logiciels cités :** Chaque divinité respecte la licence de son logiciel constitutif (majoritairement MIT, Apache 2.0, AGPL, GPL).

---

> *"La souveraineté numérique n'est pas un produit que l'on achète, c'est une architecture que l'on bâtit. C'est le refus de la dépendance et la conquête quotidienne de sa propre liberté."*