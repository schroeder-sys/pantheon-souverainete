> 🏠 **Développé sur :** [GitLab](https://gitlab.com/schroeder-sys/pantheon-souverainete) · 🪞 **Miroir :** [GitHub](https://github.com/schroeder-sys/pantheon-souverainete) · 🌐 **Site :** [pantheon-souverainete.eu](https://pantheon-souverainete.eu)

# 🏛️ Le Panthéon de la Souveraineté

> **Une infrastructure cloud 100% souveraine, redondante et sécurisée, conçue pour s'affranchir totalement des GAFAM.**
> *Version 2.1.0-alpha — Refonte structurelle intégrale. On repart de zéro.*

[![Version](https://img.shields.io/badge/Version-2.1.0--alpha-orange)](#)
[![Status](https://img.shields.io/badge/Status-Refonte%20Totale-orange)](#)
[![License](https://img.shields.io/badge/License-CC%20BY--SA%204.0-green)](LICENSE)
[![Stack](https://img.shields.io/badge/Stack-100%25%20Open%20Source-orange)](#)
[![Site](https://img.shields.io/badge/Site-Vitrine%20En%20Ligne-success)](https://pantheon-souverainete.eu)

---

## 📋 Table des Matières

1.  [Un mot sur le silence](#-un-mot-sur-le-silence-2-mois)
2.  [Philosophie & Méthodologie](#-philosophie--méthodologie)
3.  [Architecture (12 Divinités)](#-architecture-les-12-divinités)
4.  [Tiers de Confiance & Dépendances](#-tiers-de-confiance--dépendances)
5.  [Liens d'Affiliation](#-liens-daffiliation-infomaniak)
6.  [Documentation & Guides](#-documentation--guides-de-déploiement)
7.  [Pré-requis & Contacts](#-pré-requis--contacts)
8.  [Roadmap](#-roadmap-v210-alpha)
9.  [Licences](#-licences)

---

## 🙏 Un mot sur le silence (2 mois)

**Je vous dois des excuses.** Le projet n'a pas avancé publiquement ces 2 derniers mois.

**La vérité :** ce n'était pas un abandon. J'ai travaillé **en sous-marin** sur une refonte structurelle complète. La V1.x avait des fondations bancales, j'ai pris la décision de **tout reprendre depuis page blanche** plutôt que de bricoler des correctifs.

**Preuve concrète que le projet est vivant :** le site vitrine est en ligne ([pantheon-souverainete.eu](https://pantheon-souverainete.eu)).

**Leçon apprise :** je ne promets plus de dates chiffrées. Les guides arrivent "quand ce sera prêt".

---

## 🧠 Philosophie & Méthodologie

### Versionning & Historique

> **Note importante :** Les versions antérieures (1.0.0, 1.1.0) étaient des alpha avant l'adoption formelle du suffixe. **Aucune version stable n'a jamais été publiée** — tout est en alpha.

| Version | Statut | Description |
|---------|--------|-------------|
| **v1.x** | ❌ Dépassé | Architecture initiale, guides publiés au fur et à mesure |
| **v2.1.0-alpha** | 🚧 En cours | **Refonte structurelle intégrale**. On repart de zéro. |

### 🤔 Philosophie sur les Forks

**Question récurrente :** *"Pourquoi tu utilises [X] plutôt que son fork [Y] qui est plus populaire dans la communauté self-hosted ?"*

**Ma réponse :** Je limite volontairement l'usage de forks. Un fork crée un **décalage structurel** dans les mises à jour par rapport au projet source, ce qui peut introduire des failles de sécurité ou des incompatibilités.

**Je ne privilégie un fork que s'il apporte un gain SIGNIFICATIF en :**
- ✅ **Sécurité** (code audité, vulnérabilités corrigées plus rapidement)
- ✅ **Confidentialité** (télemétrie supprimée, fonctionnalités privacy-first)

**Dans tous les autres cas : je reste sur le projet source.**

#### Exemple 1 : Debian > Ubuntu

| Critère | Debian | Ubuntu |
|---------|--------|--------|
| **Origine** | Source | Fork de Debian |
| **Stabilité** | ✅ **Rock-solid** (tests rigoureux) | ⚠️ Parfois des régressions |
| **Patchs sécurité** | ✅ **Directs** (équipe Debian Security) | ⚠️ Délai (Canonical doit merger) |
| **Philosophie** | Communautaire, sobre | Commerciale, "user-friendly" |

**Pourquoi Debian ?**
> *"On ne peut pas se permettre d'attendre un patch sur le système lui-même. Question de sécurité et de stabilité."*

---

#### Exemple 2 : AmneziaWG > WireGuard (kernel)

| Critère | WireGuard | AmneziaWG |
|---------|-----------|-----------|
| **Origine** | Protocole source | **Fork du kernel WireGuard** |
| **Obfuscation** | ❌ Non | ✅ **Native (anti-DPI)** |
| **Confidentialité** | Excellente | ✅ **Encore meilleure** |
| **Collaboration** | — | ✅ **Travaille avec l'équipe WireGuard** |

**Pourquoi AmneziaWG ?**
> *"Fork assumé car gain significatif en confidentialité (obfuscation anti-DPI). De plus, l'équipe Amnezia travaille étroitement avec WireGuard — ce n'est pas un fork 'sauvage'."*

---

#### Exemple 3 : Bitwarden > Vaultwarden

| Critère | Bitwarden (officiel) | Vaultwarden (fork) |
|---------|---------------------|-------------------|
| **Origine** | Projet source | Fork Rust (léger) |
| **Clients** | ✅ **Officiels** | ⚠️ **Utilise les clients Bitwarden** |
| **Compatibilité** | ✅ **Garantie** | ⚠️ **Fenêtres d'incompatibilité** |
| **MAJ synchronisées** | ✅ Oui | ❌ Non (décalage possible) |

**Pourquoi Bitwarden et pas Vaultwarden ?**
> *"Vaultwarden lui-même indique qu'il peut exister des fenêtres d'incompatibilité où le serveur est inaccessible aux clients officiels Bitwarden. Outre la sécurité, il y a un vrai sujet de compatibilité. Je préfère la version officielle."*

---

**En résumé :**
- ✅ **Debian** (source) > Ubuntu (fork) — Stabilité & sécurité système
- ✅ **AmneziaWG** (fork) > WireGuard — Gain confidentialité (obfuscation)
- ✅ **Bitwarden** (source) > Vaultwarden (fork) — Compatibilité & sécurité

---

### Hébergement & Souveraineté

*   **Principal :** Infomaniak (**Suisse**) — Données en Suisse, DPA signé, écologie réelle
*   **Backups :** Hetzner (**Allemagne** & **Finlande**) — Stockage froid répliqué sur 2 DC
*   **Règle d'or :** Aucune donnée ne dort sur un seul site.

### Puis-je utiliser la V1 en attendant la V2.1 ?

**Techniquement :** Oui, les guides HS-02 (Firewall) et HS-03 (Image Gold) sont techniquement valides.

**Dans la pratique :** ❌ **Non, ce n'est pas recommandé.**

**Pourquoi ?**
*   Seulement 2 Hors-Séries sur 3 ont été publiés en V1 (HS-01 manquant)
*   La sécurité a été **repensée entièrement** pour la V2.1
*   De nouveaux outils de sécurité seront intégrés (ex: **Lynis** pour l'audit de durcissement)
*   **Les guides V1 sont donc dépassés dans la pratique**

**Recommandation :** Attendez la publication des guides V2.1 pour une expérience optimale.

### Profil de l'Auteur & Transparence

Je ne suis **pas un professionnel de l'IT** diplômé. Ce projet est le fruit d'une passion pour l'administration système, construite en **auto-formation sur ~5 ans**.

*   **Risque d'erreur :** Je m'appuie sur les documentations officielles, mais je ne suis pas exempt d'erreurs.
*   **Assistance par IA :** La documentation et l'architecture bénéficient d'une assistance IA, mais **chaque commande est vérifiée humainement**.
*   **Posture Éthique :** Abandon immédiat de tout outil dont la politique de confidentialité est douteuse.

> **📒 Ce projet est un journal de bord technique partagé.**
>
> *C'est mon infrastructure personnelle, construite sur mon temps libre et pour mes propres besoins. Je la documente et je la partage parce que j'espère que ça pourra aider d'autres personnes dans leur démarche de souveraineté numérique.*
>
> *Les guides avancent à mon rythme — quand j'ai du temps disponible. Si le contenu vous aide ou vous inspire, j'en suis sincèrement content. Si le rythme de publication ou certains choix techniques ne vous correspondent pas, je le comprends tout à fait : chacun a ses contraintes, ses priorités et sa propre vision.*
>
> *L'objectif reste simple : transmettre ce que j'apprends, à ma manière, sans prétention.*

### Limitation de Responsabilité

Projet éducatif et personnel. Les guides sont fournis **« tels quels » (as is)**, sans garantie explicite ou implicite.

*   **Absence de Garantie d'Exactitude :** L'auteur ne garantit pas l'absence d'erreurs ou d'incompatibilités futures.
*   **Sécurité & Failles :** Aucune architecture n'est « exempte de failles ». Les guides reflètent le meilleur de mes connaissances à leur date de rédaction.
*   **Responsabilité de Déploiement :** Il est impératif de tester toute procédure dans un **environnement isolé** avant production. L'administrateur assume l'entière responsabilité de la sécurité de ses données.

---

## 🗺️ Architecture : Les 12 Divinités

> **⚠️ Important V2.1 :** Toutes les divinités sont en cours de **révision/réintégration** dans la nouvelle architecture.

| Ordre | Divinité | Rôle | Stack | Statut V2.1 |
|:---:|:---|:---|:---|:---:|
| **01** | **VULCAIN** 🔥 | Reverse Proxy & WAF | Traefik + CrowdSec + GateKeeper | 🚧 En Refonte |
| **02** | **AEGIS** 🛡️ | DNS Souverain & VPN | WireGuard + AdGuard + Unbound + Amnezia | 🚧 En Refonte |
| **03** | **ARGUS** 👁️ | Monitoring | Grafana + Prometheus | 🔁 En Révision |
| **04** | **JANUS** 🚪 | Identité & SSO | Bitwarden + Authentik | ✅ Audit Validé |
| **05** | **ARK** 🗄️ | Backups Multi-DC | Restic (+ BorgBackup à l'étude) | 🔁 En Révision |
| **06** | **HERMÈS** 📧 | Email Souverain | Postfix + Dovecot + SimpleLogin (reco) | ⚪ Vierge |
| **07** | **MIDAS** 💰 | Finances | — | ⚪ Vierge |
| **08** | **DIANE** 🗺️ | Cartographie | Valhalla + OSM | 🔁 En Révision |
| **09** | **VERITAS** 🔍 | Recherche | SearXNG | 🔁 En Révision |
| **10** | **HÉPHAÏSTOS** 🔨 | Forge Git | Forgejo | 🔁 En Révision |
| **11** | **MNÉMOSYNE** 🧠 | Cloud & IA | NextCloud + Ollama + Pangolin (à l'étude) | 🔁 En Révision |
| **12** | **PROMÉTHÉE** 🤖 | IA Locale | Ollama + Mixtral | 🔁 En Révision |

**Légende :**
- ✅ **Validé** — Stack figée et choisie, en attente de rédaction du guide
- 🚧 **En Refonte** — Brique validée, travail de rédaction en cours
- 🔁 **En Révision** — Compatibilité projet à vérifier, choix techniques à confirmer
- ⚪ **Vierge** — Pas encore commencé

---

## 🤝 Tiers de Confiance & Dépendances

> **Note de l'auteur :** 🤡 *J'étais tellement concentré sur la philosophie des forks, l'architecture V2.1 et les recherches pour améliorer le projet... que j'en ai oublié les bases : mentionner les tiers avec qui je partage des données. Comme d'habitude, quoi. C'est maintenant corrigé ci-dessous.*

### 🇪🇺 Tiers de Confiance (Europe)

| Tiers | Rôle | Données partagées | Localisation | Pourquoi ? |
|-------|------|-------------------|--------------|------------|
| **👮 CrowdSec** | Sécurité collaborative | IPs malveillantes (anonymisées) | 🇫🇷 France | Sécurité par la communauté, DPA signé |
| **🇮🇹 Actalis** | Certificats SSL | Email de contact | 🇮🇹 Italie | Autorité européenne, conforme eIDAS, gratuit via ACME |
| **🇵🇱 Certum** | Certificats S/MIME | Identité + Email | 🇵🇱 Pologne | Autorité européenne pour HERMÈS (futur) |
| **🇨🇭 Infomaniak** | Hébergement principal | Données + facturation | 🇨🇭 Suisse | Souveraineté, écologie, DPA signé, 300€ de crédit offert (3 mois) |
| **🇩🇪 Hetzner** | Backups multi-DC | Données chiffrées | 🇩🇪🇫🇮 Allemagne/Finlande | Redondance géographique, stockage froid |

### ⚠️ Dépendances Techniques (Structurelles)

| Tiers | Rôle | Données partagées | Localisation | Pourquoi ? | Solution à terme |
|-------|------|-------------------|--------------|------------|------------------|
| **🐳 Docker Hub** | Registry d'images | Email (compte gratuit) | 🇺🇸 USA | **Rate limiting** — Docker Hub est le registry de référence. Presque toutes les images y sont hébergées. Le pull anonyme est limité, d'où la nécessité d'un compte. | **Moindre mal :** HERMÈS + Alias mail pour isoler (ne supprime pas la dépendance, mais la cloisonne) |
| **🔒 Bitwarden** | Gestion mots de passe | Email (clé de licence) | 🇺🇸 USA | **Obligation technique** — Installation ID & Key requises pour self-hosting (même pour Vaultwarden !). Région EU disponible. | **Moindre mal :** HERMÈS + Alias mail pour isoler (ne supprime pas la dépendance, mais la cloisonne) |

> **💡 Mise en pratique (exemple réel) :**
> ```
> catchall@pantheon-souverainete.eu
> ├── docker@pantheon-souverainete.eu     → Docker Hub
> ├── github@pantheon-souverainete.eu     → GitHub
> ├── gitlab@pantheon-souverainete.eu     → GitLab
> ├── bitwarden@pantheon-souverainete.eu  → Bitwarden (futur)
> ├── contact@pantheon-souverainete.eu    → Contact public
> └── ... (tous les alias que tu veux)
> ```
> **Avantage :** Si un service fuit ou spam, tu sais lequel et tu révoques l'alias sans impacter les autres. **Aujourd'hui :** Catchall Infomaniak. **Demain :** HERMÈS + alias auto-gérés.

> **🎯 Objectif :** Ces dépendances sont **structurelles et inhérentes** au projet (Docker Hub = registry de référence, Bitwarden = clé de licence requise). **Podman** peut remplacer Docker Engine, mais pas Docker Hub. **HERMÈS** (service email souverain) permettra d'utiliser des alias pour **minimiser l'impact** (isoler chaque dépendance) sans pouvoir les supprimer totalement.

### 💡 Recommandation : SimpleLogin

> **SimpleLogin** ([simplelogin.io](https://simplelogin.io)) est un service d'alias email que je **recommande** (sans l'avoir personnellement testé) pour les raisons suivantes :
> - ✅ **Open Source** — Code auditable
> - ✅ **Entreprise française** (détenue par 🇨🇭 ProtonAG)
> - ✅ **Gratuit** — Version gratuite généreuse
> - ✅ **Premium abordable** — ~36€/an
> - 🔗 **Aucun lien d'affiliation** — Je ne touche aucune commission
>
> **Usage :** Alternative aux alias `+tag` de Bitwarden/ProtonMail. Ex: `random@sl.schroeder-sys.fr` au lieu de `mon+netflix@gmail.com`.

---

## 🔗 Liens d'Affiliation Infomaniak

> **Philosophie :** Au-delà du revenu passif pour financer le projet, ces liens sont des **raccourcis pratiques** qui redirigent directement vers les services que j'utilise et recommande. **Aucun surcoût pour vous.**

| Service | Lien | Usage |
|---------|------|-------|
| **🏠 Infomaniak — Accueil** | [infomaniak.com/goto/fr/home](https://www.infomaniak.com/goto/fr/home?utm_term=6acca8df963ee) | Page d'accueil générale |
| **☁️ Public Cloud** | [infomaniak.com/fr/hebergement/public-cloud](https://www.infomaniak.com/fr/hebergement/public-cloud?utm_term=6acca8df963ee) | **300€ offerts sur 3 mois** — Idéal pour tester |
| **🖥️ VPS Cloud** | [infomaniak.com/fr/hebergement/vps-cloud](https://www.infomaniak.com/fr/hebergement/vps-cloud?utm_term=6acca8df963ee) | VPS classiques |
| **💚 VPS Lite** | [infomaniak.com/fr/hebergement/vps-lite](https://www.infomaniak.com/fr/hebergement/vps-lite?utm_term=6acca8df963ee) | VPS entrée de gamme |
| **💾 NAS Synology** | [infomaniak.com/goto/fr/hosting.synology](https://www.infomaniak.com/goto/fr/hosting.synology?utm_term=6acca8df963ee) | NAS hébergés |
| **📦 myKsuite+** | [infomaniak.com/fr/ksuite/myksuite](https://www.infomaniak.com/fr/ksuite/myksuite?utm_term=6acca8df963ee) | Suite collaborative (Euria) |
| **🤖 AI Tools** | [infomaniak.com/fr/hebergement/llm-api](https://www.infomaniak.com/fr/hebergement/llm-api?utm_term=6acca8df963ee) | API IA (Qwen, Kimi, etc.) |

---

## 📚 Documentation & Guides de Déploiement

### Structure de la Documentation (V2.1)

La documentation est organisée dans le dossier [`docs/`](docs/) :

*   🏗️ **Hors-Séries (3 variantes) :** Chaque guide de socle (Réseau, Firewall, Image) existe en version **Cloud**, **VPS/Dédié** et **HomeLab**.
    *   🔁 **À Réécrire :** HS-01 : Constitution du Réseau Privé (Cloud uniquement)
    *   🔁 **Obsolète :** HS-02 : Le Firewall Périmétrique (sécurité repensée)
    *   🔁 **Obsolète :** HS-03 : Création de l'Image Gold Debian (nouveaux outils)

*   ⚔️ **Divinités (à venir) :** Guides dédiés à chaque service (VULCAIN, AEGIS, ARGUS, etc.)

> **📋 Note sur les variantes d'environnement :**
> *Chaque Hors-Série existe en **3 variantes** selon votre infrastructure :*
> *   **☁️ Cloud** — **Recommandé**. Full features, infrastructure professionnelle, budget assumé (~300€/mois). C'est la référence.
> *   **🖥️ VPS/Dédié** — **Compatible**. Alternative budget ou "test qui termine en prod". Réseau privé dépend du provider.
> *   **🏠 HomeLab** — **Souveraineté TOTALE**. Les données ne quittent JAMAIS votre domicile/association. 0 accès internet sauf MAJ (via AdGuard + Unbound). Segment réseau dédié.
> 
> **Ordre de publication :** Cloud → VPS → HomeLab (du plus complet au plus spécifique)

### État des Livrables

| Composant | Statut | Notes |
|-----------|--------|-------|
| **🌐 Site Vitrine** | ✅ En ligne | [pantheon-souverainete.eu](https://pantheon-souverainete.eu) |
| **📄 README V2.1** | ✅ Finalisé | Refonte complète, honnêteté radicale |
| **🏗️ Architecture V2.1** | ✅ Définie | 12 divinités, 3 environnements, stack validée |
| **🎨 Schémas** | ⚪ À faire | draw.io + Mermaid (Cloud / VPS / HomeLab) |
| **📚 HS-01 (Réseau)** | 🚧 En cours | Cloud uniquement |
| **📚 HS-02 (Firewall)** | 🔁 Obsolète (V1) | À réécrire avec sécurité V2.1 |
| **📚 HS-03 (Gold Image)** | 🔁 Obsolète (V1) | 3 guides séparés (Cloud / VPS / HomeLab) |

---

## 🛠️ Pré-requis & Contacts

### Compétences Requises

| Niveau | Compétence | Pourquoi ? | Ressources |
|--------|------------|------------|------------|
| **Débutant** | Ligne de commande Bash | Navigation, édition de fichiers | [SadServers](https://sadservers.com/) |
| **Intermédiaire** | Réseaux (IP, DNS, VLAN) | Comprendre les flux | [OpenClassrooms](https://openclassrooms.com/) |
| **Intermédiaire** | Docker/Podman | Conteneurisation | [Documentation Docker](https://docs.docker.com/) |
| **Avancé** | Sécurité (firewall, SSH) | Durcissement système | [Debian Security Guide](https://www.debian.org/doc/manuals/securing-debian-manual/) |

### Environnement de Travail (Auteur)

| Outil | Usage | Pourquoi ? |
|-------|-------|------------|
| **🖥️ VSCodium** | Rédaction & Scripting | VS Code sans télémétrie Microsoft |
| **🔑 Tabby.sh** | Gestionnaire SSH | Open Source, multi-plateforme, plugins |
| **🐧 Debian 13 (Trixie)** | OS Laptop + KDE | Dernière stable, souveraineté, pas de télémétrie |

### Environnement de Test Recommandé

*   **Cloud :** [Infomaniak Public Cloud](https://www.infomaniak.com/fr/hebergement/public-cloud?utm_term=6acca8df963ee) — **300€ de crédit offerts sur 3 mois** (idéal pour tester sans toucher au portefeuille)
*   **Coût cible :** ~50€/mois (test) vs ~300€/mois (production complète)

### Contacts

| Sujet | Canal | Réponse |
|-------|-------|---------|
| **🐛 Signalement d'erreur** | Ouvrez une **Issue** sur GitLab ou GitHub (je fais de mon mieux pour répondre) | Variable |
| **❓ Questions sur le projet** | Email : [contact@pantheon-souverainete.eu](mailto:contact@pantheon-souverainete.eu) | Quand je peux |
| **💼 Contact pro / audit** | Email : [contact@schroeder-sys.fr](mailto:contact@schroeder-sys.fr) | Sous 48h |

---

## 📅 Roadmap V2.1.0-alpha

### Cycle de Version

| Version | Critères de Fin | Statut |
|---------|-----------------|--------|
| **Alpha** | Tous les guides publiés + testés indépendamment (labo) | 🚧 En cours |
| **Bêta** | Structure complète testée + guides mis à jour avec dernières connaissances | ⏳ À venir |
| **Stable** | Retour communauté intégré + aucun bug critique signalé | ⏳ À venir |

### Phases de Déploiement

| Phase | Période | Version | Durée | Heures | Statut |
|-------|---------|---------|-------|--------|--------|
| **Phase 1** | Q4 2026 | `2.1.0-alpha` | 3 mois | 80-100h | 🚧 En cours |
| **Phase 2** | Q1 2027 | `2.1.0-alpha` | 3 mois | 120-150h | ⏳ À venir |
| **Phase 3** | Q2 2027 | `2.1.0-alpha` | 3 mois | 100-120h | ⏳ À venir |
| **Phase 4** | Q3 2027 | `2.1.0-alpha` | 3 mois | 100-120h | ⏳ À venir |
| **Phase 5** | Q4 2027 | `2.1.0-beta` | 3 mois | 80-100h | ⏳ À venir |
| **Phase 6** | Q1 2028 | `2.1.0` ✅ | 3 mois | 60-80h | ⏳ À venir |
| **Phase 7** | Q2 2028 | `2.1.0` | 3 mois | 40-60h | ⏳ À venir |
| **Phase 8** | Q3 2028+ | `2.x.0`+ | Variable | Variable | ⏳ À venir |

**Jalons de version :**
- **Fin Alpha** (Phase 4) → Tous les guides publiés + testés indépendamment
- **Fin Bêta** (Phase 5) → Structure complète testée + guides mis à jour
- **Stable** (Phase 6) → Retours communauté intégrés + aucun bug critique

---

## 📜 Licences

*   **Code source (infrastructure) :** [EUPL-1.2](LICENSE) — European Union Public Licence
*   **Contenus textuels (guides, articles) :** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) — Partage dans les mêmes conditions
*   **Site vitrine :** EUPL-1.2 (code) + CC BY-SA 4.0 (contenus)

---

> **Dernière mise à jour :** 20 octobre 2026  
> **Version :** 2.1.0-alpha  
> **Auteur :** Christopher Schroëder (Schroeder-Sys)  
> **Assistance IA :** Euria, Qwen3.5, Kimi-K2.6
