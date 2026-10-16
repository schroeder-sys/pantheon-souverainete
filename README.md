> 🏠 **Développé sur :** [GitLab](https://gitlab.com/schroeder-sys/pantheon-souverainete) · 🪞 **Miroir :** [GitHub](https://github.com/schroeder-sys/pantheon-souverainete) · 🌐 **Site :** [pantheon-souverainete.eu](https://pantheon-souverainete.eu)

# 🏛️ Le Panthéon de la Souveraineté

> **Une infrastructure cloud 100% souveraine, redondante et sécurisée, conçue pour s'affranchir totalement des GAFAM.**
> *Version 2.0.0-alpha — Refonte structurelle intégrale. On repart de zéro.*

[![Version](https://img.shields.io/badge/Version-2.0.0--alpha-orange)](#)
[![Status](https://img.shields.io/badge/Status-Refonte%20Totale-orange)](#)
[![License](https://img.shields.io/badge/License-CC%20BY--SA%204.0-green)](LICENSE)
[![Stack](https://img.shields.io/badge/Stack-100%25%20Open%20Source-orange)](#)
[![Site](https://img.shields.io/badge/Site-Vitrine%20En%20Ligne-success)](https://pantheon-souverainete.eu)

---

## 📋 Table des Matières

1.  [Un mot sur le silence](#-un-mot-sur-le-silence-4-mois)
2.  [Avertissements & Méthodologie](#-avertissements--méthodologie)
3.  [Architecture (12 Divinités)](#-architecture-les-12-divinités)
4.  [Documentation & Guides](#-documentation--guides-de-déploiement)
5.  [Pré-requis & Contacts](#-pré-requis--contacts)
6.  [Roadmap](#-roadmap-v200-alpha)
7.  [Licences](#-licences)

---

## 🙏 Un mot sur le silence (2 mois)

**Je vous dois des excuses.** Le projet n'a pas avancé publiquement ces 2 derniers mois.

**La vérité :** ce n'était pas un abandon. J'ai travaillé **en sous-marin** sur une refonte structurelle complète. La V1.x avait des fondations bancales, j'ai pris la décision de **tout reprendre depuis page blanche** plutôt que de bricoler des correctifs.

**Preuve concrète que le projet est vivant :** le site vitrine est en ligne ([pantheon-souverainete.eu](https://pantheon-souverainete.eu)).

**Leçon apprise :** je ne promets plus de dates chiffrées. Les guides arrivent "quand ce sera prêt".

---

## ⚠️ Avertissements & Méthodologie

### Versionning & Historique

> **Note importante :** Les versions antérieures (1.0.0, 1.1.0) étaient des alpha avant l'adoption formelle du suffixe. **Aucune version stable n'a jamais été publiée** — tout est en alpha.

| Version | Statut | Description |
|---------|--------|-------------|
| **v1.x** | ❌ Dépassé | Architecture initiale, guides publiés au fur et à mesure |
| **v2.0.0-alpha** | 🚧 En cours | **Refonte structurelle intégrale**. On repart de zéro. |

**Hébergement & Souveraineté :**
*   **Principal :** Infomaniak (**Suisse**) — Données en Suisse, DPA signé, écologie réelle
*   **Backups :** Hetzner (**Allemagne** & **Finlande**) — Stockage froid répliqué sur 2 DC
*   **Règle d'or :** Aucune donnée ne dort sur un seul site.

### Puis-je utiliser la V1 en attendant la V2 ?

**Techniquement :** Oui, les guides HS-02 (Firewall) et HS-03 (Image Gold) sont techniquement valides.

**Dans la pratique :** ❌ **Non, ce n'est pas recommandé.**

**Pourquoi ?**
*   Seulement 2 Hors-Séries sur 3 ont été publiés en V1 (HS-01 manquant)
*   La sécurité a été **repensée entièrement** pour la V2
*   De nouveaux outils de sécurité seront intégrés (ex: **Lynis** pour l'audit de durcissement)
*   **Les guides V1 sont donc dépassés dans la pratique**

**Recommandation :** Attendez la publication des guides V2 pour une expérience optimale.

### Profil de l'Auteur & Transparence

Je ne suis **pas un professionnel de l'IT** diplômé. Ce projet est le fruit d'une passion pour l'administration système, construite en **auto-formation sur ~5 ans**.

*   **Risque d'erreur :** Je m'appuie sur les documentations officielles, mais je ne suis pas exempt d'erreurs.
*   **Assistance par IA :** La documentation et l'architecture bénéficient d'une assistance IA, mais **chaque commande est vérifiée humainement**.
*   **Posture Éthique :** Abandon immédiat de tout outil dont la politique de confidentialité est douteuse.

> > **📒 Ce projet est un journal de bord technique partagé.**
>
> *C'est mon infrastructure personnelle, construite sur mon temps libre et pour mes propres besoins. Je la documente et je la partage parce que j'espère que ça pourra aider d'autres personnes dans leur démarche de souveraineté numérique.*
>
> *Les guides avancent à mon rythme — quand j'ai du temps disponible. Si le contenu vous aide ou vous inspire, j'en suis sincèrement content. Si le rythme de publication ou certains choix techniques ne vous correspondent pas, je le comprends tout à fait : chacun a ses contraintes, ses priorités et sa propre vision.*
>
> *L'objectif reste simple : transmettre ce que j'apprends, à ma manière, sans prétention.*

### Limitation de Responsabilité

Projet éducatif et personnel. Les guides sont fournis **« tels quels » (as is)**, sans garantie explicite ou implicite.

*   **Absence de Garantie d'Exactitude :** L'auteur ne garantit pas l'absence d'erreurs ou d'incompatibilités futures.
*   **Sécurité & Failles :** Aucune architecture n'est « exempte de failles ». Les guides reflètent le meilleur de nos connaissances à leur date de rédaction.
*   **Responsabilité de Déploiement :** Il est impératif de tester toute procédure dans un **environnement isolé** avant production. L'administrateur assume l'entière responsabilité de la sécurité de ses données.

---

## 🗺️ Architecture : Les 12 Divinités

> **⚠️ Important V2.0 :** Toutes les divinités sont en cours de **révision/réintégration** dans la nouvelle architecture.

| Ordre | Divinité | Rôle | Stack | Statut V2 |
|:---:|:---|:---|:---|:---:|
| **01** | **VULCAIN** 🔥 | Reverse Proxy & WAF | Traefik + CrowdSec + GateKeeper | 🚧 En Refonte |
| **02** | **AEGIS** 🛡️ | DNS Souverain & VPN | WireGuard + AdGuard + Unbound + Amnezia | 🚧 En Refonte |
| **03** | **ARGUS** 👁️ | Monitoring | Grafana + Prometheus | 🔁 En Révision |
| **04** | **JANUS** 🚪 | Identité & SSO | Bitwarden + Authentik | ✅ Validé |
| **05** | **ARK** 🗄️ | Backups Multi-DC | Restic + Swift/S3 | 🔁 En Révision |
| **06** | **HERMÈS** 📧 | Email | *(En attente)* | ⚪ Vierge |
| **07** | **MIDAS** 💰 | Finances | *(En attente)* | ⚪ Vierge |
| **08** | **DIANE** 🏹 | Cartes & NTP | Valhalla + Photon | 🔁 En Révision |
| **09** | **VERITAS** 🔍 | Recherche | SearXNG | 🔁 En Révision |
| **10** | **HÉPHAÏSTOS** 🔨 | Forge Git | Forgejo | 🔁 En Révision |
| **11** | **MNÉMOSYNE** ☁️ | Cloud & Fichiers | Nextcloud Hub | 🔁 En Révision |
| **12** | **PROMÉTHÉE** 🔥 | IA Locale | Ollama + Mixtral | 🔁 En Révision |

**Changements majeurs V2 :**
*   **VULCAIN :** GateKeeper (OpenSource) remplace ALTCHA
*   **AEGIS :** Amnezia by default (obfuscation anti-DPI)
*   **JANUS :** Authentik validé (audit labo : scan trafic + analyse policy)

---

## 📚 Documentation & Guides de Déploiement

### Structure de la Documentation

```
docs/
├── 00_HORS-SERIES/     # Guides de socle (3 variantes par guide)
├── 01_DIVINITES/       # Guides de déploiement (tous environnements)
└── 99_COMMUN/          # Ressources partagées
```

### Les 3 Variantes d'Environnement

> **📋 Chaque Hors-Série existe en 3 variantes :**

| Environnement | Pour qui ? | Avantages | Contraintes |
|---------------|------------|-----------|-------------|
| **☁️ Cloud** | Ceux qui veulent l'infra complète, budget assumé | Full features, réseau privé natif, sécurité maximale | ~300€/mois |
| **🖥️ VPS/Dédié** | Budget limité, test, ou hybride | Moins cher, flexible | Réseau privé dépend du provider |
| **🏠 HomeLab** | Puristes, données ne quittent jamais le domicile | Souveraineté totale, coût matériel uniquement | 0 accès internet (sauf MAJ), segment réseau dédié |

**Ordre de publication :** Cloud → VPS → HomeLab (du plus complet au plus spécifique)

### Ordre de Déploiement

**L'ordre à suivre (quand les guides seront publiés) :**

```
HS-01 (Réseau) → HS-02 (Firewall) → HS-03 (Image Gold) 
    → VULCAIN → AEGIS → Service Métier → ARGUS → ARK
```

> **🤡 Note de l'auteur sur la V2 :**
> *Vous vous demandez pourquoi on repart de zéro après avoir publié des guides ? Bonne question.*
> *En bon architecte passionné, j'ai appris en faisant. La V1 avait des fondations bancales, j'aurais pu bricoler des correctifs… j'ai choisi de tout reprendre proprement.*
> *C'est comme découvrir qu'on a construit sa maison sur du sable : soit on consolide en urgence, soit on coule de vraies fondations et on recommence. J'ai choisi l'option 2.*
> *Rassurez-vous : la V2 sera cohérente, complète et dans le bon ordre. Promis. (Cette fois.)*

---

## 🤝 Soutien au Projet (Liens d'Affiliation)

Ce projet est développé sur **mon temps libre**, sans financement extérieur. Si vous souhaitez soutenir la démarche sans coût supplémentaire pour vous, utilisez les liens d'affiliation ci-dessous pour vos souscriptions Infomaniak.

| Service | Lien |
|---------|------|
| **☁️ Public Cloud** (300€ offerts) | [Voir l'offre](https://www.infomaniak.com/fr/hebergement/public-cloud?utm_term=6acca8df963ee) |
| **🖥️ VPS Cloud** | [Voir l'offre](https://www.infomaniak.com/fr/hebergement/vps-cloud?utm_term=6acca8df963ee) |
| **🖥️ VPS Lite** | [Voir l'offre](https://www.infomaniak.com/fr/hebergement/vps-lite?utm_term=6acca8df963ee) |
| **💾 NAS Synology** | [Voir l'offre](https://www.infomaniak.com/goto/fr/hosting.synology?utm_term=6acca8df963ee) |
| **🏠 Accueil Infomaniak** | [Voir le site](https://www.infomaniak.com/goto/fr/home?utm_term=6acca8df963ee) |

> **ℹ️ Transparence :** Ces liens sont des liens d'affiliation. Ils ne génèrent aucun surcoût pour vous, mais me permettent de toucher une petite commission qui aide à financer le développement et la documentation de ce projet.

---

## 🛠️ Pré-requis & Contacts

### Compétences Requises

| Compétence | Niveau | Pourquoi ? |
|------------|--------|------------|
| **Ligne de commande (Bash)** | Intermédiaire | Tous les guides sont en CLI |
| **Docker / Conteneurs** | Débutant+ | La majorité des services sont conteneurisés |
| **Réseau (IP, DNS, VLAN)** | Intermédiaire | Comprendre les flux, les sous-réseaux |
| **SSH / Clés** | Débutant | Connexion aux serveurs |
| **Linux (Debian)** | Intermédiaire | Administration système de base |

**Pour apprendre :** 🎯 [SadServers](https://sadservers.com/) — Scénarios d'administration système à résoudre

### Environnement de Test Recommandé

**☁️ Infomaniak Public Cloud — 300€ de crédit offerts sur 3 mois**

Idéal pour tester l'infrastructure sans toucher au portefeuille. Le crédit couvre largement un déploiement complet de test.

*   **Lien :** [Infomaniak Public Cloud](https://www.infomaniak.com/fr/hebergement/public-cloud?utm_term=6acca8df963ee)
*   **Configuration recommandée :** 2 vCPU, 4 Go RAM, 40 Go SSD
*   **1 service métier :** Nextcloud OU Forgejo OU SearXNG (au choix)

**Autres options Infomaniak :**
*   🖥️ **[VPS Cloud](https://www.infomaniak.com/fr/hebergement/vps-cloud?utm_term=6acca8df963ee)** — Pour des besoins plus importants
*   🖥️ **[VPS Lite](https://www.infomaniak.com/fr/hebergement/vps-lite?utm_term=6acca8df963ee)** — Pour petits budgets et tests
*   💾 **[NAS Synology](https://www.infomaniak.com/goto/fr/hosting.synology?utm_term=6acca8df963ee)** — Alternative clé en main

> **💡 Astuce :** Le coût cible de 200-300€/mois correspond à la **structure TOTALE en production**. Vous pouvez commencer avec **un seul service métier**, ce qui divise la facture par 6.

### Contacts

| Sujet | Contact |
|-------|---------|
| **📧 Questions sur le projet** | [contact@pantheon-souverainete.eu](mailto:contact@pantheon-souverainete.eu) |
| **💼 Audit / Intégration pro** | [contact@schroeder-sys.fr](mailto:contact@schroeder-sys.fr) |
| **🐛 Bugs / Issues** | [GitLab](https://gitlab.com/schroeder-sys/pantheon-souverainete/-/issues) ou [GitHub](https://github.com/schroeder-sys/pantheon-souverainete/issues) |

### Contribuer

Le projet est **100% en français** pour l'instant.

*   **📝 Traduction** — Proposez une traduction des guides dans votre langue
*   **🐛 Signalement d'erreur** — Ouvrez une Issue (je fais de mon mieux pour répondre)
*   **💡 Amélioration** — Proposez via les Issues

> **⏳ En attendant les guides V2 :** La meilleure façon de contribuer est de **proposer des traductions**.

---

## 🚀 Roadmap (V2.0.0-alpha)

### ✅ Livré
- [x] **Site Vitrine** — En ligne
- [x] **Refonte Structurelle** — Architecture V2 définie
- [x] **Authentik (JANUS)** — Audit terminé, validé ✅

### 🚧 En Cours
- [ ] **Schémas d'Architecture** — Conception des diagrammes
- [ ] **Guide VULCAIN V2** — Intégration GateKeeper
- [ ] **Guide AEGIS V2** — Intégration Amnezia
- [ ] **Réécriture HS-01/02/03** — Nouveaux guides avec sécurité renforcée

### ⏳ À Venir
- [ ] **ARGUS** — Monitoring
- [ ] **ARK** — Backups multi-DC
- [ ] **DIANE, VERITAS, HÉPHAÏSTOS, MNÉMOSYNE, PROMÉTHÉE** — Guides à mettre à jour
- [ ] **HERMÈS (Email)** — En attente
- [ ] **MIDAS (Finances)** — En attente

---

## 📜 Licences

| Élément | Licence |
|---------|---------|
| **Contenu (Guides)** | CC BY-SA 4.0 |
| **Code Source du Site Vitrine** | EUPL-1.2 |
| **Logiciels cités** | Licence propre à chaque logiciel |

---

> *« La souveraineté numérique ne se bâtit pas sur du sable. Elle commence par un firewall verrouillé, avant même la première ligne de code. »*

---

**Auteur :** Christopher Schroëder — Auto-entrepreneur (Schroeder-Sys)  
**Contact pro :** [schroeder-sys.fr](https://schroeder-sys.fr)  
**Dernière mise à jour :** 16 octobre 2026 — Version 2.0.0-alpha
