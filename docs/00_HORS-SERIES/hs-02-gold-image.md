# 🔐 Hors-Série #2 : Création de l'Image Gold Debian 13 (Panthéon-Socle)

> **⚠️ PRÉREQUIS ABSOLU.**
> Ce guide suppose que le Security Group `pantheon-gold-sg` (défini dans le [HS-01](hs-01-security-group.md)) est déjà créé et opérationnel.
>
> **Philosophie :** Aucune automatisation opaque. Chaque commande est exécutée, comprise et validée manuellement. L'objectif est de construire une référence stable, pas de lancer un script dont on ignore les effets de bord.
>
> **Public visé :** Ce guide accompagne les débutants motivés. Il ne mâche pas tout : il vous apprend à lire la documentation officielle et à comprendre *pourquoi* nous faisons ces choix spécifiques.

---

## 1. Initialisation de l'Instance de Construction

### 1.1 Création manuelle
Dans la console **Infomaniak Public Cloud** :
1.  Créez une instance nommée `test-gold-builder`.
2.  **Image :** Debian 13 (Trixie).
3.  **Réseaux :** Public **ET** Privé (indispensable pour tester et corriger le routage).
4.  **Security Group :** `pantheon-gold-sg` uniquement.
5.  **Clé SSH :** Votre clé publique.
    > 💡 **Recommandation de sécurité :** Générez une paire de clés SSH spécifique pour cette instance de construction. Ne réutilisez pas votre clé personnelle principale.

### 1.2 Mise à jour et Fuseau Horaire
Connectez-vous en SSH et lancez les mises à jour :
```bash
sudo apt update && sudo apt full-upgrade -y
sudo reboot
```
Une fois reconnecté, configurez le fuseau :
```bash
sudo dpkg-reconfigure tzdata
```
*   **Action :** Sélectionnez `Europe` puis `Paris` (ou votre ville de référence).
*   **Pourquoi ?** Une image Gold doit avoir un fuseau horaire défini pour éviter les prompts interactifs futurs et assurer la cohérence des logs entre tous les serveurs.

---

## 2. Correction du Routage (Point Critique)

> **🛑 LE PROBLÈME TECHNIQUE**
> Sur l'infrastructure OpenStack (Infomaniak), lorsqu'une instance possède deux interfaces (Publique + Privée), le système reçoit deux routes par défaut :
> 1.  Une via le DHCP sur l'interface publique (ex: `37.x.x.x`).
> 2.  Une route **statique** injectée par le provider sur l'interface privée (ex: `192.168.10.1`).
>
> **Conséquence :** Linux utilise souvent la route statique en priorité, envoyant tout le trafic Internet vers le réseau privé, qui n'a pas de sortie. Résultat : **Perte totale d'Internet**.
>
> **LA SOLUTION**
> Nous n'allons pas supprimer la route privée (ce qui couperait le réseau interne). Nous allons créer un service systemd **léger et transparent** qui s'exécute au boot pour supprimer la route par défaut indésirable sur le privé.
> *C'est la méthode recommandée par la documentation [systemd.network](https://www.freedesktop.org/software/systemd/man/latest/systemd.network.html#Routing) pour gérer les politiques de routage complexes.*

### 2.1 Création du service de correction
Créez le fichier de service :
```bash
sudo nano /etc/systemd/system/pantheon-route-fix.service
```

Collez-y ce contenu :
```ini
[Unit]
Description=Correctif Routage : Priorité Publique sur Privée
Documentation=https://www.freedesktop.org/software/systemd/man/latest/systemd.service.html
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
# Supprime la route par défaut via la gateway privée (si elle existe)
# On utilise '|| true' pour éviter l'échec du service si la route n'existe pas
ExecStart=/sbin/ip route del default via 192.168.10.1 dev enp4s0 || true
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```
*> **Note importante :** Vérifiez le nom de votre interface privée avec `ip -brief addr show`. Si ce n'est pas `enp4s0`, adaptez la ligne `ExecStart` en conséquence.*

### 2.2 Activation
```bash
sudo systemctl daemon-reload
sudo systemctl enable pantheon-route-fix.service
```
*Vérification immédiate : Lancez `sudo systemctl start pantheon-route-fix.service` puis `ip route show | grep default`. Vous ne devez voir qu'une seule ligne : la route publique.*

---

## 3. Firewall Local (UFW) : La Roue de Secours

> **POURQUOI UFW ?**
> Le Security Group (SG) est la première ligne de défense, mais il est externe à l'OS. UFW est la **défense en profondeur**. Si le SG est malencontreusement détaché, UFW maintient la politique "Deny All".
> *Référence : [Documentation officielle UFW](https://help.ubuntu.com/community/UFW)*

### 3.1 Installation et Politiques
```bash
sudo apt install ufw -y
sudo ufw --force reset
sudo ufw default deny incoming
sudo ufw default deny outgoing
sudo ufw default deny routed
```

### 3.2 Règles d'exception (À adapter avec VOS IPs)
```bash
# SSH : Remplacez <VOTRE_IP> par votre IP publique actuelle (trouvable sur whatismyip.com)
sudo ufw allow from <VOTRE_IP_V4>/32 to any port 22 proto tcp
sudo ufw allow from <VOTRE_IP_V6>/128 to any port 22 proto tcp

# Réseau Privé : Autorisation totale TCP/UDP pour le subnet interne
sudo ufw allow from 192.168.10.0/24 to any proto tcp
sudo ufw allow from 192.168.10.0/24 to any proto udp

# Sorties Vitales : DNS, HTTP, HTTPS, NTP
sudo ufw allow out to any port 53 proto udp
sudo ufw allow out to any port 53 proto tcp
sudo ufw allow out to any port 80 proto tcp
sudo ufw allow out to any port 443 proto tcp
sudo ufw allow out to any port 123 proto udp
```

### 3.3 Activation
```bash
sudo ufw --force enable
sudo ufw status verbose
```
*Vérifiez visuellement que les règles correspondent à vos attentes et que le statut est `active`.*

---

## 4. DNS Souverain (OpenResolv)

> **POURQUOI OPENRESOLV ?**
> Debian 13 (Trixie) utilise `openresolv` pour gérer `/etc/resolv.conf`, abandonnant `systemd-resolved` souvent source de conflits sur les serveurs headless.
> *Référence : [Debian Wiki - DNS Name Resolution](https://wiki.debian.org/NameResolution)*

### 4.1 Installation et Configuration
```bash
sudo apt install openresolv -y
sudo nano /etc/resolvconf.conf
```
Ajoutez les lignes suivantes pour utiliser **Quad9** (DNS sécurisé, utilisé temporairement en attendant le service AEGIS) :
```conf
resolv_conf=/etc/resolv.conf
name_servers="9.9.9.9 149.112.112.112"
name_servers_append="2620:fe::fe 2620:fe::9"
resolvconf_options="no_tuning"
```
Appliquez la configuration :
```bash
sudo resolvconf -u
```
Test de validation : `getent hosts infomaniak.com` (doit retourner une adresse IP immédiatement).

---

## 5. Sécurité : Fail2Ban & Mises à Jour

### 5.1 Fail2Ban (Protection SSH)
> **POURQUOI `banaction = ufw` ?**
> Par défaut, Fail2Ban utilise `iptables`. Comme nous utilisons UFW, il est crucial de configurer Fail2Ban pour qu'il ajoute ses règles **via** UFW. Cela garantit la cohérence des règles.
> *Référence : [Fail2Ban Documentation - Actions](https://fail2ban.readthedocs.io/en/latest/config.html)*

Installation :
```bash
sudo apt install fail2ban -y
```
Configuration :
```bash
sudo nano /etc/fail2ban/jail.local
```
Contenu minimaliste et robuste :
```ini
[DEFAULT]
bantime = 86400
maxretry = 3
banaction = ufw

[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 1
bantime = 3153600000
banaction = ufw
```
*Note : `bantime = 3153600000` équivaut à ~99 ans. Pour SSH, la tolérance est zéro : 1 échec = bannissement quasi-définitif.*

Activation et vérification :
```bash
sudo systemctl restart fail2ban
sudo systemctl enable fail2ban
sudo fail2ban-client status sshd
```

### 5.2 Mises à Jour Automatiques
> **POURQUOI LES CONFIGURER ?**
> Bien que le paquet soit installé par défaut sur Debian, sa configuration initiale est parfois trop conservative. Nous devons nous assurer que les correctifs de sécurité sont bien inclus et que le reboot automatique est planifié.
> *Référence : [Debian Wiki - UnattendedUpgrades](https://wiki.debian.org/UnattendedUpgrades)*

**Activation et configuration :**
```bash
sudo dpkg-reconfigure unattended-upgrades
```
*   **Action :** À l'invite, sélectionnez **`<Yes>`**.

**Configuration fine (Docker + Reboot auto) :**
Éditez le fichier de configuration :
```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```
Vérifiez et modifiez ces sections précises :
1.  **Origines :** Assurez-vous que les lignes `Debian-Security` sont décommentées. Ajoutez `"origin=Docker,codename=${distro_codename}";` pour inclure les mises à jour Docker.
2.  **Reboot automatique :**
    ```conf
    Unattended-Upgrade::Automatic-Reboot "true";
    Unattended-Upgrade::Automatic-Reboot-Time "04:00";
    ```
    *Argument : Un reboot est souvent requis après une mise à jour du noyau. Le planifier à 04:00 minimise l'impact.*

---

## 6. Préparation Docker (Dépôt Seul)

> **POURQUOI LE DÉPÔT SEULEMENT ?**
> Nous n'installons pas le moteur Docker dans l'image Gold pour garder l'image minimale. Cependant, 10 des 12 divinités utiliseront Docker. Nous ajoutons donc le dépôt maintenant pour faciliter les déploiements futurs.
>
> **🇬🇧 Note sur la documentation :** La procédure officielle est en anglais. Ne paniquez pas, voici les termes clés à repérer :
> *   *"Install using the apt repository"* → C'est la méthode à suivre.
> *   *"Set up the repository"* → Configurer le dépôt (ajouter l'adresse).
> *   *"Install Docker Engine"* → Installer le moteur (nous ne le ferons pas ici, juste le dépôt).
>
> *Référence : [Docker Official Documentation - Debian](https://docs.docker.com/engine/install/debian/)*

Suivez **exactement** les commandes de la section "Install using the apt repository" de la documentation officielle pour ajouter le dépôt et la clé GPG.

Vérification de l'origine (Crucial pour les MAJ auto) :
```bash
grep "^Origin:" /var/lib/apt/lists/download.docker.com_linux_debian_dists_*_InRelease
# Doit afficher : Origin: Docker
```

---

## 7. Validation Manuelle Avant Nettoyage

Ne passez pas à l'étape suivante tant que ces checks ne sont pas verts :
1.  `ip route show | grep default` : Une seule ligne (Publique).
2.  `sudo apt update` : Fonctionne sans erreur.
3.  `sudo ufw status verbose` : Statut `active`, règles cohérentes.
4.  `systemctl is-active fail2ban unattended-upgrades` : Retourne `active` pour les deux.

---

## 8. Sysprep (Nettoyage Final)

> **⚠️ ACTION REQUISE**
> Cette étape efface l'identité unique de la machine. Une fois éteinte, cette instance ne doit **jamais** être redémarrée. Elle sert uniquement à générer le Snapshot.

Exécutez manuellement chaque ligne pour comprendre ce qui est effacé :

```bash
# 1. Nettoyage des logs et caches
sudo journalctl --rotate && sudo journalctl --vacuum-time=1s
sudo rm -rf /var/log/*.gz /var/log/*.[0-9]
sudo apt clean

# 2. Réinitialisation de l'identité machine (Générera un nouvel ID au prochain boot)
sudo rm -f /etc/machine-id
sudo touch /etc/machine-id

# 3. Suppression des clés SSH hôtes (Générées à nouveau au premier boot)
sudo rm -f /etc/ssh/ssh_host_*

# 4. Suppression de votre clé utilisateur (Pour éviter qu'elle ne soit copiée partout)
sudo rm -f ~/.ssh/authorized_keys

# 5. Nettoyage de l'historique et des baux DHCP
history -c && rm -f ~/.bash_history
sudo rm -f /var/lib/dhcp/*

# 6. Arrêt immédiat
sudo sync && sudo shutdown -h now
```

### 8.1 Création du Snapshot
Dans la console Infomaniak :
1.  Attendez l'arrêt complet de l'instance (statut **"Shut Off"**).
2.  Allez dans **Compute** > **Images** (ou Snapshots).
3.  Créez un **Snapshot** de cette instance.
4.  Nommez-le : `pantheon-gold-d13-v1.0`.

---

## 9. Validation de l'Image (Obligatoire)

> **🛑 NE SAUTEZ PAS CETTE ÉTAPE.**

1.  Déployez une **nouvelle instance** nommée `test-validation` en utilisant le snapshot `pantheon-gold-d13-v1.0` comme source.
2.  Attachez le SG `pantheon-gold-sg` et une interface privée.
3.  Connectez-vous en SSH.
4.  Vérifiez **sans rien installer** :
    *   Le routage est-il corrigé automatiquement ? (`ip route show`)
    *   Internet fonctionne-t-il ? (`sudo apt update`)
    *   Le réseau privé est-il présent ? (`ip addr show`)

Si tout est vert, l'image est validée. Vous pouvez détruire l'instance `test-validation`.
Votre socle est maintenant prêt pour le déploiement de **VULCAIN**.

---

> *"La souveraineté numérique ne se bâtit pas sur du sable. Elle commence par une image système saine, reproductible et sécurisée."*