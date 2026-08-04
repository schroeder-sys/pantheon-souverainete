# 🕸️ Hors-Série #01 : Constitution du Réseau Privé "Panthéon-Backbone"

> **⚠️ STATUT : EN RÉDACTION D'URGENCE.**
> *Ce guide est actuellement en cours d'écriture intensive par l'auteur.*
>
> **Pourquoi cette urgence ?**
> Comment ai-je pu concevoir une architecture "Zero-Trust" et "Souveraine" en oubliant la brique la plus évidente : **le réseau lui-même** ? 🤦‍♂️
> C'est l'erreur classique de l'architecte passionné : on veut poser les briques (Firewall, Image, Services) avant d'avoir coulé les fondations (le VLAN privé).
>
> **Ne vous inquiétez pas :** Ce guide arrivera dans les toutes prochaines heures/jours. Il sera le prérequis absolu pour le **HS-02 (Firewall)** et le **HS-03 (Image Gold)**.

[![Niveau](https://img.shields.io/badge/Niveau-Fondation-Critical-blue)](#)
[![Couche](https://img.shields.io/badge/Couche-0_Réseau-orange)](#)
[![Statut](https://img.shields.io/badge/Statut-En%20%C3%89criture-red)](#)

---

## 🚧 Ce qui vous attend dans ce guide (Bientôt !)

Pour vous faire patienter et vous préparer mentalement, voici ce que nous allons construire ensemble dès la finalisation de ce document :

### 1. La Philosophie du Réseau Isolée
Pourquoi un réseau privé virtuel (VLAN) est la condition *sine qua non* de la souveraineté.
*   **L'objectif :** Créer une autoroute privée (`192.168.10.0/24`) invisible depuis Internet, où vos 12 "Divinités" pourront communiquer librement sans jamais exposer leurs données à l'hébergeur ou aux scanners externes.
*   **La métaphore :** C'est creuser le tunnel souterrain reliant vos châteaux forts avant de construire les murs d'enceinte.

### 2. La Procédure OpenStack (Infomaniak)
Nous détaillerons pas à pas la création dans la console Horizon :
*   Création du **Réseau Privé** (Network).
*   Configuration du **Sous-Réseau** (Subnet) avec le CIDR `192.168.10.0/24`.
*   Activation du **Routeur** pour lier ce réseau privé à l'Internet public (pour permettre les sorties contrôlées via le Firewall).
*   *Note : Des captures d'écran précises seront incluses pour éviter toute erreur de clic.*

### 3. Vérification et Tests
Comment s'assurer que votre "tuyau" est étanche avant d'y brancher quoi que ce soit :
*   Tests de connectivité entre deux instances de test.
*   Vérification de l'isolement depuis l'extérieur.

---

## ⏳ En attendant, que pouvez-vous faire ?

Ne restez pas les bras croisés ! La publication imminente de ce guide ne vous empêche pas de préparer le terrain :

1.  **Lisez le HS-02 (Firewall) :** Vous pouvez déjà étudier la matrice de règles du **[HS-02 : Le Firewall Périmétrique](hs-02-security-group.md)**. Vous verrez qu'il fait référence au réseau `192.168.10.0/24` que nous allons créer ici. Comprendre *pourquoi* on filtre ce réseau vous aidera à mieux le construire.
2.  **Préparez votre Image Gold (HS-03) :** Le guide **[HS-03 : Création de l'Image Gold Debian](hs-03-gold-image.md)** est déjà disponible. Vous pouvez lire les prérequis système et préparer votre environnement de travail.
3.  **Consultez le README :** Revoyez l'architecture globale des **[12 Divinités](../README.md)** pour comprendre où ce réseau privé s'insère dans le grand plan.

---

## 🔔 Soyez notifié de la sortie

Ce guide est la priorité absolue numéro 1 du projet "Panthéon" cette semaine.

*   **Action recommandée :** **Starrez (⭐) le dépôt** sur GitLab/GitHub pour recevoir une notification dès que ce fichier sera mis à jour avec le contenu complet.
*   **Suivi :** Une fois publié, le badge "Statut" en haut de page passera du rouge (En Écriture) au vert (Validé).

> *"On ne construit pas le toit avant les murs, ni les murs avant les fondations. Je répare cette erreur logique dans les plus brefs délais pour que votre infrastructure repose sur du roc, et non sur du sable."*

---

## 🤝 Contribuer & Licence

Ce guide fait partie du projet "Le Panthéon de la Souveraineté".

*   💡 **Avez-vous une astuce** pour la configuration réseau sur OpenStack ? Partagez-la dans les issues en attendant la publication !

**Licence :** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

---

> *Document placeholder – Version 0.1 (En attente de rédaction).*