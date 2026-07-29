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
    *   **Réseau Privé :** Guide HS-02 en finalisation urgente (oubli de la v1.0 réparé !).
    *   **VULCAIN :** En cours de déploiement (Priorité n°1). Sera configuré pour utiliser le DoT vers AEGIS.
    *   **AEGIS :** Architecture DoT/DoH définie, prête au déploiement.
    *   **Backups (ARK) :** Architecture multi-DC (Suisse, Allemagne, Finlande) validée dans les specs, en cours de déploiement.
*   🛑 **AVERTISSEMENT :** Ne déployez rien en production sans avoir validé chaque étape dans un environnement de test. Le non-respect des phases de test se fait à **vos risques et périls**.

### 4. Limitation de Responsabilité
Projet éducatif et personnel. L'auteur ne garantit pas l'absence de bugs. La sécurité de vos données et la stratégie de backup (ARK) sont de votre **entière responsabilité**.

---

## 📜 Philosophie & Objectifs

Démontrer qu'il est possible de construire une infrastructure **redondante, chiffrée de bout en bout et indépendante**, sans dépendre des GAFAM.

**Les 3 Piliers Fondamentaux (v1.1) :**
1.  **Souveraineté & Redondance :** Hébergement principal en **Suisse** (Infomaniak) avec réplication des backups en **Allemagne** et **Finlande** (Hetzner). Vos données survivent à la perte d'un pays entier.
2.  **Security by Design (Chiffrement des Serveurs) :**
    *   **Actuel (v1.1) :** **Les serveurs eux-mêmes** chiffrent leurs requêtes DNS via le protocole **DoT (DNS-over-TLS)** vers AEGIS. Que ce soit pour une mise à jour (`apt`) ou pour joindre un service interne, aucune requête ne sort en clair du serveur.
    *   **Règle de Flux :** Tout flux sortant non essentiel est strictement bloqué par le firewall **OU** filtré au niveau DNS.
    *   **Futur (Réflexion mTLS) :** Implémentation potentielle d'une authentification mutuelle par certificats entre les services (Zero-Trust avancé).
3.  **Ordre Logique de Déploiement :** **Réseau Privé** → **Porte d'Entrée (VULCAIN)** → Socle DNS (AEGIS) → Services Métiers.

---

## 🤝 Les Tiers de Confiance Assumés

La souveraineté n'est pas l'isolement, c'est le choix maîtrisé de ses dépendances.

| Tiers | Rôle | Pourquoi ce choix ? | Souveraineté |
|:---|:---|:---|:---|
| **Infomaniak** 🇨🇭 | Hébergeur Principal | Données en Suisse (hors Cloud Act/UE). DPA signé. Écologie réelle. | ✅ **Totale** |
| **Hetzner** 🇩🇪 🇫🇮 | Hébergeur Secondaire (Backup) | Redondance géographique stricte : Stockage froid répliqué sur **deux** DC distincts (Allemagne & Finlande). | ✅ **Européen** |
| **CrowdSec** 🇫🇷 | Intelligence de Menaces | Partage communautaire d'IP malveillantes. Protection temps réel. | ✅ **Européenne** |
| **Certum** 🇵🇱 | Certificats SSL & S/MIME | Autorité de certification européenne, conforme **eIDAS**. Remplace Let's Encrypt pour une confiance souveraine. | ✅ **Européenne** |
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
| **04** | **JANUS** 🚪 | Identité & SSO | Bitwarden + Authentik (SSO)* | 🇺🇸/🇺🇸* | 📋 *Audit à faire* |
| **05** | **ARK** 🗄️ | Backups Multi-DC | Restic + Swift (CH) + S3 (DE/FI) | 🇩🇪/🇨🇭/🇫🇮 | ✅ **Validé** |
| **06** | **HERMÈS** 📧 | Email | *(En attente)* | - | ⚪ **Vierge** |
| **07** | **MIDAS** 💰 | Finances | *(En attente)* | - | ⚪ **Vierge** |
| **08** | **DIANE** 🏹 | Cartes & NTP | Valhalla + Photon | 🇩🇪 | ✅ **Validé** |
| **09** | **VERITAS** 🔍 | Recherche | SearXNG | 🇫🇷 | ✅ **Validé** |
| **10** | **HÉPHAÏSTOS** 🔨 | Forge Git | Forgejo | 🌍 | ✅ **Validé** |
| **11** | **MNÉMOSYNE** ☁️ | Cloud & Fichiers | Nextcloud Hub | 🇩🇪 | ✅ **Validé** |
| **12** | **PROMÉTHÉE** 🔥 | IA Locale | Ollama + Mixtral | 🇫🇷 | ✅ **Validé** |

*\*Note sur JANUS : Authentik (US) est en cours d'audit pour vérifier l'absence de télémétrie. Plan B : Authelia.*

> ### 🖼️ Schéma d'Architecture (À venir)
> *Un schéma détaillé illustrant les flux chiffrés (DoT), la topologie multi-DC (CH/DE/FI) et l'interconnexion des instances sera ajouté ici dès la finalisation du déploiement de VULCAIN. La priorité est donnée à l'exactitude technique plutôt qu'à l'illustration prématurée.*

> ### 🔍 Focus Évolution : Du DoT au mTLS (Réflexion en cours)
> *   **Version 1.1 (DoT) :** Imaginez que chaque serveur possède son propre téléphone sécurisé. Quand VULCAIN a besoin de connaître l'adresse d'un autre service, il appelle AEGIS via une ligne chiffrée (DoT). Personne, pas même l'hébergeur, ne peut entendre quelle adresse est demandée.
> *   **Réflexion Future (mTLS) :** Nous étudions l'ajout d'une "carte d'identité" numérique (certificat) pour chaque service. Avec le **mTLS**, deux services ne peuvent communiquer que s'ils se reconnaissent mutuellement via ces certificats.
>     *   *Note technique :* Si le mTLS est mis en place, le flux de données est déjà tellement sécurisé que la résolution DNS pourrait théoriquement rester en clair (car le contenu est protégé par le tunnel mTLS), bien que nous puissions choisir de cumuler les deux pour une sécurité maximale.
>     *   Cette évolution est encore à l'étude.

---

### 🧩 Philosophie "Lego" : Modularité & Universalité

Imaginez ce projet comme un set **Lego Star Wars** complexe :
*   🚀 **Modularité :** Chaque brique (Divinité) est fonctionnelle seule.
*   🏗️ **Ordre de construction :** L'ordre est critique pour éviter de devoir démonter/remonter.
*   🏗️ **Hors-Séries (Spécifique Cloud) :** Les guides de socle (Réseau, Firewall, Image) sont conçus spécifiquement pour le **Cloud Public** (OpenStack/Infomaniak).
*   🌍 **Universalité des Divinités :** Une fois le socle en place, les guides d'installation des services sont compatibles **Cloud Public**, **VPS** et **Local** (Raspberry Pi).

**Le principe clé (v1.1) :**
1.  **Socle Réseau (HS-02) :** Le fondamental (oublié en v1.0, réparé maintenant !).
2.  **Porte (VULCAIN) :** Reverse Proxy et WAF.
3.  **Socle DNS (AEGIS) :** Chiffrement des requêtes serveurs (DoT).
4.  **Services :** Déployés et connectés.

> **Conseil :** Ne brûlez pas les étapes. Cette version 1.1 impose cet ordre logique pour garantir la stabilité.

---

## 🛡️ Modèle de Sécurité

Cette version marque un saut qualitatif en matière de sécurité :
*   **Chiffrement Interne Systématique :** Adoption de DoT/DoH pour *tous* les flux DNS des serveurs, éliminant le risque d'écoute même sur le réseau privé de l'hébergeur.
*   **Vers du mTLS ?** Étude active pour étendre le chiffrement et l'authentification mutuelle à l'ensemble des flux applicatifs internes via une AC privée.
*   **Hygiène Numérique Stricte :** Abandon immédiat de tout outil client (SSH, gestion) ne respectant pas une charte de confidentialité stricte.
*   **Redondance des Backups (ARK) :** Stratégie 3-2-1 avec copie externe chez un second hébergeur européen sur **deux sites distincts** (Allemagne & Finlande).
*   **Zero-Trust :** Isolation stricte des instances et authentification forte via JANUS.

---

## 📚 Documentation & Guides de Déploiement

Ce dépôt propose une **méthodologie durable**. Pas de scripts "magiques", mais des guides basés sur la doc officielle.

### 🗂️ Structure de la Documentation

La documentation est organisée de manière modulaire dans le dossier [`docs/`](docs/) :

*   📂 **[00_HORS-SERIES](docs/00_HORS-SERIES/)** : Guides transverses indispensables (Spécifiques Cloud Public).
    *   ✅ **Disponible :** [HS-01 : Le Firewall Périmétrique "Panthéon-Gold"](docs/00_HORS-SERIES/hs-01-security-group.md)
    *   🚧 **EN URGENCE :** [HS-02 : Constitution du Réseau Privé](docs/00_HORS-SERIES/hs-02-reseau-prive-pantheon.md) *(Note de l'auteur : Comment ai-je pu oublier le réseau dans une infra Zero-Trust ? C'est l'évidence même qui est souvent invisible. Le guide arrive en urgence, mais en attendant, vous pouvez déjà avancer sur la Gold Image !)*
    *   ✅ **Disponible :** [HS-03 : Création de l'Image Gold Debian](docs/00_HORS-SERIES/hs-03-gold-image.md)
*   📂 **[01_DIVINITES](docs/01_DIVINITES/)** : Guides de déploiement spécifiques à chaque service (Compatibles Cloud, VPS & Local).
*   📂 **[99_COMMUN](docs/99_COMMUN/)** : Ressources partagées, schémas d'architecture et snippets de configuration.

---

### 🚀 Par où commencer ? (Quick Start)

Le projet étant en construction, voici comment vous pouvez déjà vous impliquer :

1.  **Lisez ce README** pour comprendre l'architecture v1.1 et la logique de déploiement.
2.  **Déployez le socle de sécurité :**
    *   📄 **[Hors-Série #1 : Le Firewall Périmétrique](docs/00_HORS-SERIES/hs-01-security-group.md)** (Prérequis absolu).
    *   📄 **[Hors-Série #3 : L'Image Gold Debian](docs/00_HORS-SERIES/hs-03-gold-image.md)** (Nouveau ! Créez votre image de référence).
    *   ⏳ **À venir très vite :** [Hors-Série #2 : Le Réseau Privé](docs/00_HORS-SERIES/hs-02-reseau-prive-pantheon.md) *(L'auteur répare son oubli en urgence !)*
3.  **Surveillez les prochaines publications :**
    *   🔥 **Guide VULCAIN** (Déploiement du Proxy et WAF) – En rédaction.
    *   🛡️ **Guide AEGIS** (Socle DNS et VPN) – À venir.

> 💡 **Conseil :** **Starrez (⭐) ce dépôt** pour être notifié des mises à jour et de la publication des Hors-Séries dans les prochains jours.

🛑 **AVERTISSEMENT :** Ne déployez rien en production tant que les guides ne sont pas marqués "PROD".

---

## 💰 Modèle Économique & Hébergement

La v1.1 intègre un modèle de redondance professionnelle renforcé.

*   **Hébergeur Principal :** Infomaniak (**Suisse**). Pour les instances de production.
*   **Hébergeur Secondaire (Stockage Backup) :** Hetzner. Réplication des backups froids sur **deux datacenters distincts** : **Allemagne** et **Finlande**. Cela assure une redondance géographique sur **3 Datacenters distincts** (1x Suisse, 1x Allemagne, 1x Finlande).
*   **Sécurité des Données (ARK) :** Toutes les sauvegardes sont **chiffrées côté client (AES-256)** via Restic *avant* leur envoi. Ni Infomaniak ni Hetzner ne peuvent accéder au contenu de vos données.
*   **Coût Cible :** 200€~300€/mois.
    *   Ce budget couvre l'ensemble des 12 services en production, avec une allocation de ressources confortable et la réplication des sauvegardes sur ces trois datacenters distincts.

*   **Soutien au projet :** Si ce projet vous est utile, vous pouvez soutenir son développement via les liens d'affiliation ci-dessous (sans surcoût pour vous) :
    *   ☁️ **[Infomaniak Public Cloud](https://www.infomaniak.com/fr/hebergement/public-cloud?utm_term=68ceeeb369ce1)** – Pour le socle principal.
    *   🖥️ **[Infomaniak VPS](https://www.infomaniak.com/fr/hebergement/vps-cloud?utm_term=68ceeeb369ce1)** – Pour les instances légères.
    *   📧 **[kSuite (incluant kDrive & Euria)](https://www.infomaniak.com/fr/ksuite?utm_term=68ceeeb369ce1)** – Pour la suite collaborative.
    *   ℹ️ **[Site Officiel Infomaniak (Lien non affilié)](https://infomaniak.com/fr)**

> *Un clin d'œil sincère à **Infomaniak** : ce projet n'aurait jamais vu le jour sans un hébergeur capable de cocher simultanément les cases "Souveraineté Suisse", "Écologie réelle" et "Prix accessibles". Merci de rendre la liberté numérique économiquement viable.*

---

## 🚀 Roadmap (v1.1 - Construction)

- [x] **Phase 0 : Conception & Outils** – Mise en place de l'environnement de rédaction (Debian, VSCodium) et finalisation des specs VULCAIN.
- [x] **Publication Hors-Série #1** – Guide du Firewall Périmétrique (Gold Security Group).
- [ ] **Publication Hors-Série #2 (Urgent)** – Guide du Réseau Privé. *(Priorité Immédiate)*
- [x] **Publication Hors-Série #3** – Guide de l'Image Gold Debian.
- [ ] **Phase 1 : Porte d'Entrée (VULCAIN)** – Déploiement du Proxy et du WAF.
- [ ] **Phase 2 : Socle DNS (AEGIS)** – Déploiement de l'instance avec DoT/DoH natif.
- [ ] **Phase 3 : Observabilité (ARGUS)** – Mise en place du monitoring centralisé.
- [ ] **Phase 4 : Identité & Données (JANUS, MNÉMOSYNE, ARK)** – Déploiement du SSO, du Cloud et configuration de la redondance des backups (Infomaniak + Hetzner DE/FI).
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