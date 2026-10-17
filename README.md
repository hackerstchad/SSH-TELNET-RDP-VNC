# Protocoles d'Accès à Distance : SSH, Telnet, RDP, VNC

**Auteur :** hackers_tchad  
**Date de rédaction :** 2026  
**Version :** 1.0  
**Objectif :** Guide avancé, exhaustif et pratique des protocoles d'accès à distance les plus utilisés dans l'administration système, le réseau, la cybersécurité, le support technique et le domaine du hacking éthique.

---

## Table des matières

1. [Introduction générale](#1-introduction-générale)
2. [SSH (Secure Shell)](#2-ssh-secure-shell)
3. [Telnet](#3-telnet)
4. [RDP (Remote Desktop Protocol)](#4-rdp-remote-desktop-protocol)
5. [VNC (Virtual Network Computing)](#5-vnc-virtual-network-computing)
6. [Comparatif des protocoles](#6-comparatif-des-protocoles)
7. [Architecture et modèles OSI/TCP-IP](#7-architecture-et-modèles-ositcp-ip)
8. [Commandes de vérification et diagnostics](#8-commandes-de-vérification-et-diagnostics)
9. [Outils et applications utilisées](#9-outils-et-applications-utilisées)
10. [Bonnes pratiques et sécurité](#10-bonnes-pratiques-et-sécurité)
11. [Ressources, livres et liens](#11-ressources-livres-et-liens)
12. [Annexes](#12-annexes)

---

## 1. Introduction générale

L'accès à distance désigne la capacité d'un utilisateur ou d'un administrateur à se connecter à un système informatique, un serveur, un poste de travail ou un équipement réseau situé physiquement à un autre endroit, via un réseau (local ou Internet). Cette capacité est essentielle à l'administration système, au support technique, à la gestion des infrastructures, à la télémaintenance, au télétravail, à l'enseignement à distance, à la cybersécurité et à de nombreuses autres disciplines informatiques.

### 1.1 Pourquoi les protocoles d'accès à distance sont-ils indispensables ?

- **Centralisation de l'administration :** Un seul administrateur peut gérer des centaines de serveurs répartis dans le monde entier.
- **Réduction des coûts :** Moins de déplacements, moins d'interventions sur site.
- **Rapidité d'intervention :** Résolution des incidents en temps réel.
- **Flexibilité du travail :** Télétravail, assistance technique à distance.
- **Formation et démonstration :** Partage d'écran, sessions de formation.
- **Tests de sécurité :** Les pentesters utilisent ces protocoles pour évaluer la sécurité des systèmes.

### 1.2 Historique et évolution

L'accès à distance est né avec les premiers réseaux informatiques. Dans les années 1960 et 1970, les terminaux se connectaient aux mainframes via des lignes série. Avec l'émergence d'Internet et des réseaux TCP/IP, de nouveaux protocoles ont vu le jour : Telnet dans les années 1970, SSH en 1995, RDP en 1996 avec Windows NT 4.0 Terminal Server Edition, et VNC en 1999 à l'Université de Cambridge.

### 1.3 Classification des protocoles

- **Protocoles en ligne de commande :** Telnet, SSH.
- **Protocoles graphiques :** RDP, VNC.
- **Protocoles sécurisés :** SSH, RDP (avec chiffrement).
- **Protocoles non sécurisés :** Telnet, VNC (sauf versions chiffrées).

### 1.4 Enjeux de sécurité

L'accès à distance est une cible privilégiée des attaquants. Les enjeux principaux sont :

- **Confidentialité :** Empêcher l'interception des données.
- **Intégrité :** Garantir que les données ne sont pas altérées.
- **Authentification :** S'assurer de l'identité des utilisateurs.
- **Disponibilité :** Maintenir l'accès tout en se protégeant contre les attaques par déni de service.
- **Traçabilité :** Enregistrer les sessions pour l'audit et la forensic.

---

## 2. SSH (Secure Shell)

### 2.1 Définition

SSH, abréviation de Secure Shell, est un protocole réseau sécurisé permettant d'ouvrir une session interactive sur un serveur distant et d'exécuter des commandes à distance. Il fournit également des mécanismes pour le transfert de fichiers sécurisé (SCP, SFTP), le tunneling, le transfert de port et l'exécution de commandes distantes sans interaction.

### 2.2 Historique et créateurs

SSH a été créé en 1995 par Tatu Ylönen, un chercheur finlandais de l'Université d'Helsinki. La première version, SSH-1, a été développée pour remplacer Telnet et les outils rlogin/rsh, qui transmettaient les informations en clair. En 1996, une version améliorée, SSH-2, a été proposée par l'IETF. Aujourd'hui, OpenSSH, développé par le projet OpenBSD, est l'implémentation la plus répandue.

### 2.3 Objectifs de SSH

- Remplacer les protocoles non sécurisés (Telnet, rlogin, rsh, ftp).
- Chiffrer toutes les communications entre le client et le serveur.
- Authentifier fortement les utilisateurs et les serveurs.
- Permettre l'administration sécurisée des serveurs.
- Offrir des fonctionnalités avancées : tunneling, transfert de fichiers, proxy SOCKS, etc.

### 2.4 Fonctionnement de SSH

SSH fonctionne selon un modèle client-serveur. Le client SSH initie une connexion vers le serveur SSH. Les échanges se déroulent en plusieurs phases :

1. **Négociation de version :** Le client et le serveur se mettent d'accord sur la version du protocole (SSH-1 ou SSH-2).
2. **Échange de clés :** Les deux parties génèrent une clé de session symétrique via un algorithme de type Diffie-Hellman.
3. **Authentification du serveur :** Le serveur présente son empreinte de clé hôte (host key) au client.
4. **Authentification du client :** Le client s'authentifie par mot de passe, clé publique/privée, Kerberos ou autre méthode.
5. **Session sécurisée :** Les données échangées sont chiffrées et intègres.

### 2.5 Algorithmes de chiffrement utilisés par SSH

- **Échange de clés :** Diffie-Hellman (DH), Elliptic Curve Diffie-Hellman (ECDH), Curve25519.
- **Chiffrement symétrique :** AES, ChaCha20, 3DES, Blowfish.
- **Hachage :** SHA-256, SHA-512.
- **Signatures :** RSA, ECDSA, Ed25519.

### 2.6 Ports et protocoles réseau

- **Port par défaut :** TCP/22.
- **Transport :** TCP.
- **Couche OSI :** Application (couche 7).

### 2.7 Installation et configuration de SSH

#### 2.7.1 Sous Linux (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install openssh-server openssh-client
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```

#### 2.7.2 Sous Linux (CentOS/RHEL/Rocky)

```bash
sudo dnf install openssh-server openssh-clients
sudo systemctl enable sshd
sudo systemctl start sshd
sudo systemctl status sshd
```

#### 2.7.3 Sous Windows

Depuis Windows 10 et Windows Server 2019, OpenSSH est disponible en option :

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType 'Automatic'
```

#### 2.7.4 Fichier de configuration du serveur

Le fichier principal de configuration est `/etc/ssh/sshd_config` (Linux) ou `C:\ProgramData\ssh\sshd_config` (Windows).

Exemple de configuration sécurisée :

```
Port 22
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
AllowUsers admin backup
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2
Protocol 2
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org
X11Forwarding no
PermitEmptyPasswords no
Banner /etc/ssh/banner
```

#### 2.7.5 Fichier de configuration du client

Le fichier de configuration client est `~/.ssh/config`.

Exemple :

```
Host serveur-prod
    HostName 192.168.1.10
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_rsa_prod
    ForwardAgent no
    ServerAliveInterval 60
```

### 2.8 Utilisation de SSH

#### 2.8.1 Connexion simple

```bash
ssh utilisateur@serveur
ssh -p 2222 utilisateur@serveur
```

#### 2.8.2 Exécution de commandes distantes

```bash
ssh utilisateur@serveur "ls -la /var/log"
ssh utilisateur@serveur "sudo systemctl restart apache2"
```

#### 2.8.3 Transfert de fichiers avec SCP

```bash
scp fichier.txt utilisateur@serveur:/chemin/destination/
scp -r dossier utilisateur@serveur:/chemin/destination/
scp utilisateur@serveur:/chemin/fichier.txt ./local/
```

#### 2.8.4 Transfert de fichiers avec SFTP

```bash
sftp utilisateur@serveur
sftp -P 2222 utilisateur@serveur
```

Commandes SFTP courantes : `ls`, `cd`, `get`, `put`, `mget`, `mput`, `pwd`, `lpwd`, `bye`.

#### 2.8.5 Génération de clés SSH

```bash
ssh-keygen -t ed25519 -C "commentaire"
ssh-keygen -t rsa -b 4096 -C "commentaire"
ssh-keygen -t ecdsa -b 521 -C "commentaire"
```

#### 2.8.6 Copie de clé publique

```bash
ssh-copy-id utilisateur@serveur
ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 2222 utilisateur@serveur
```

#### 2.8.7 Tunneling et redirection de ports

- **Redirection locale (local forwarding) :**

```bash
ssh -L 8080:localhost:80 utilisateur@serveur
```

- **Redirection distante (remote forwarding) :**

```bash
ssh -R 9090:localhost:3000 utilisateur@serveur
```

- **Proxy SOCKS dynamique :**

```bash
ssh -D 1080 utilisateur@serveur
```

#### 2.8.8 Exécution d'applications graphiques (X11 forwarding)

```bash
ssh -X utilisateur@serveur
ssh -Y utilisateur@serveur
```

### 2.9 Architecture de SSH

SSH repose sur une architecture en trois couches :

1. **Couche transport :** Gère le chiffrement, l'intégrité et la compression.
2. **Couche authentification :** Gère l'authentification de l'utilisateur.
3. **Couche connexion :** Gère les canaux logiques (session, transfert de fichiers, tunneling, etc.).

### 2.10 Commandes de vérification et diagnostics SSH

```bash
ssh -v utilisateur@serveur
ssh -vv utilisateur@serveur
ssh -vvv utilisateur@serveur
ssh-keygen -lf /etc/ssh/ssh_host_rsa_key.pub
ssh-keyscan serveur
ss -tlnp | grep ssh
systemctl status sshd
journalctl -u sshd -f
cat /var/log/auth.log | grep sshd
ssh -G utilisateur@serveur
```

### 2.11 Outils et applications basés sur SSH

- **OpenSSH :** Implémentation de référence.
- **PuTTY :** Client SSH pour Windows.
- **MobaXterm :** Terminal avancé pour Windows.
- **Termius :** Client multiplateforme.
- **WinSCP :** Client SFTP/SCP graphique.
- **FileZilla :** Client SFTP graphique.
- **Cyberduck :** Client SFTP pour macOS et Windows.
- **sshpass :** Automatisation de l'authentification par mot de passe.
- **sshuttle :** VPN simple basé sur SSH.
- **autossh :** Maintien permanent des tunnels SSH.
- **mosh :** Shell mobile, alternative à SSH pour les connexions instables.

### 2.12 Cas d'utilisation de SSH

- Administration de serveurs Linux/Unix.
- Déploiement automatisé via Ansible, Chef, Puppet, SaltStack.
- Transfert sécurisé de fichiers.
- Accès sécurisé à GitHub, GitLab, Bitbucket.
- Tunneling pour sécuriser des protocoles non chiffrés.
- Accès à des bases de données distantes.
- Gestion d'équipements réseau (routeurs, switchs, pare-feu).
- Exécution de commandes à distance dans des scripts.
- Création de VPN légers.

### 2.13 Sécurité avancée SSH

- Désactiver l'authentification par mot de passe.
- Utiliser des clés Ed25519 ou RSA 4096 bits.
- Protéger les clés privées par une passphrase forte.
- Utiliser ssh-agent et ssh-add.
- Restreindre les utilisateurs et les adresses IP autorisées.
- Changer le port par défaut ou limiter l'accès via un pare-feu.
- Utiliser fail2ban pour bloquer les tentatives de brute-force.
- Activer l'authentification à deux facteurs (2FA) avec Google Authenticator ou YubiKey.
- Auditer régulièrement les journaux d'authentification.
- Mettre à jour OpenSSH régulièrement.

### 2.14 Exemple de configuration fail2ban pour SSH

```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
findtime = 600
```

### 2.15 Exemple d'authentification à deux facteurs

```bash
sudo apt install libpam-google-authenticator
google-authenticator
```

Modifier `/etc/pam.d/sshd` pour ajouter :

```
auth required pam_google_authenticator.so
```

Modifier `/etc/ssh/sshd_config` :

```
ChallengeResponseAuthentication yes
AuthenticationMethods publickey,keyboard-interactive
```

---

## 3. Telnet

### 3.1 Définition

Telnet (Teletype Network) est un protocole réseau ancien permettant d'ouvrir une session interactive en ligne de commande sur un hôte distant. Il a été conçu pour fonctionner sur des réseaux de confiance. Telnet transmet toutes les données, y compris les noms d'utilisateur et les mots de passe, en clair.

### 3.2 Historique et créateurs

Telnet a été développé en 1969 par l'Internet Engineering Task Force (IETF), sous l'impulsion des premiers travaux sur ARPANET. Il est défini dans la RFC 15, puis normalisé dans la RFC 854 en 1983. Telnet est l'un des plus anciens protocoles Internet encore utilisé, bien que de manière très limitée.

### 3.3 Objectifs de Telnet

- Fournir un terminal virtuel sur un hôte distant.
- Permettre l'administration à distance des équipements réseau.
- Offrir un protocole simple et léger pour les environnements contrôlés.
- Servir de base pour d'autres protocoles (par exemple, SMTP, POP3, HTTP peuvent être testés via Telnet).

### 3.4 Fonctionnement de Telnet

Telnet utilise une architecture client-serveur. Le client Telnet se connecte au serveur Telnet sur le port TCP/23. Les caractères tapés par l'utilisateur sont envoyés au serveur, qui exécute les commandes et renvoie les résultats. Telnet est basé sur le modèle NVT (Network Virtual Terminal), qui définit un ensemble standard de caractères et de commandes.

### 3.5 Ports et protocoles réseau

- **Port par défaut :** TCP/23.
- **Transport :** TCP.
- **Couche OSI :** Application (couche 7).

### 3.6 Installation et configuration de Telnet

#### 3.6.1 Sous Linux (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install telnetd telnet
sudo systemctl enable inetd
sudo systemctl start inetd
```

#### 3.6.2 Sous Linux (CentOS/RHEL/Rocky)

```bash
sudo dnf install telnet telnet-server
sudo systemctl enable telnet.socket
sudo systemctl start telnet.socket
```

#### 3.6.3 Configuration du serveur Telnet

Le serveur Telnet est généralement géré par xinetd ou systemd. Le fichier de configuration peut se trouver dans `/etc/inetd.conf` ou `/etc/xinetd.d/telnet`.

Exemple de configuration xinetd :

```
service telnet
{
    flags           = REUSE
    socket_type     = stream
    wait            = no
    user            = root
    server          = /usr/sbin/in.telnetd
    log_on_failure  += USERID
    disable         = no
}
```

### 3.7 Utilisation de Telnet

#### 3.7.1 Connexion simple

```bash
telnet serveur
telnet serveur 23
```

#### 3.7.2 Test de connectivité sur un port

Telnet est souvent utilisé pour vérifier si un port TCP est ouvert :

```bash
telnet serveur 80
telnet serveur 443
telnet serveur 25
telnet serveur 110
```

#### 3.7.3 Test manuel de protocoles

- **Test SMTP :**

```bash
telnet serveur-smtp 25
HELO exemple.com
MAIL FROM:<expediteur@exemple.com>
RCPT TO:<destinataire@exemple.com>
DATA
Sujet: Test

Ceci est un test.
.
QUIT
```

- **Test HTTP :**

```bash
telnet serveur-web 80
GET / HTTP/1.1
Host: serveur-web

```

### 3.8 Architecture de Telnet

Telnet repose sur le modèle NVT (Network Virtual Terminal) :

- Le client transforme les frappes clavier locales en une séquence NVT standard.
- Le serveur interprète ces séquences et renvoie les résultats.
- Telnet supporte des options de négociation (echo, type de terminal, mode de transmission, etc.).

### 3.9 Commandes de vérification et diagnostics Telnet

```bash
telnet serveur 23
netstat -tlnp | grep telnet
systemctl status telnet.socket
nmap -p 23 serveur
nc -vz serveur 23
```

### 3.10 Outils et applications Telnet

- **telnet :** Client en ligne de commande.
- **PuTTY :** Supporte Telnet en plus de SSH.
- **HyperTerminal :** Ancien terminal Windows.
- **Tera Term :** Émulateur de terminal pour Windows.
- **SecureCRT :** Terminal commercial supportant Telnet.

### 3.11 Cas d'utilisation de Telnet

- Administration d'anciens équipements réseau (routeurs, switchs, modems).
- Tests de connectivité sur des ports TCP.
- Diagnostic de protocoles en clair (SMTP, POP3, HTTP, FTP).
- Environnements de laboratoire et de formation.
- Débogage d'applications réseau.

### 3.12 Pourquoi éviter Telnet en production ?

Telnet ne chiffre rien. Un attaquant positionné sur le réseau peut intercepter :

- Les noms d'utilisateur.
- Les mots de passe.
- Les commandes tapées.
- Les résultats renvoyés.

Pour toute utilisation sur un réseau non entièrement contrôlé, il faut privilégier SSH.

### 3.13 Migration de Telnet vers SSH

1. Activer SSH sur le serveur.
2. Configurer l'authentification par clé.
3. Désactiver Telnet.
4. Bloquer le port TCP/23 au pare-feu.
5. Former les utilisateurs à SSH.
6. Mettre à jour la documentation.

### 3.14 Sécurité et Telnet

- **Ne jamais utiliser Telnet sur Internet.**
- **Limiter Telnet aux réseaux isolés et contrôlés.**
- **Utiliser Telnet uniquement pour des tests ou du diagnostic.**
- **Surveiller les journaux si Telnet est activé temporairement.**
- **Préférer netcat (nc) ou nmap pour les tests de ports.**

---

## 4. RDP (Remote Desktop Protocol)

### 4.1 Définition

RDP, ou Remote Desktop Protocol, est un protocole propriétaire développé par Microsoft pour fournir une interface graphique à distance à un utilisateur connecté à un autre ordinateur via un réseau. RDP permet d'afficher le bureau distant, d'interagir avec les applications, de transférer des fichiers, d'imprimer à distance, et bien plus.

### 4.2 Historique et créateurs

RDP a été développé par Microsoft. La première version publique a été incluse avec Windows NT 4.0 Terminal Server Edition en 1996. RDP est basé sur le protocole T.120 de l'ITU-T, initialement conçu pour la téléconférence. Les versions successives ont apporté des améliorations en termes de sécurité, de performance et de fonctionnalités.

### 4.3 Objectifs de RDP

- Permettre le télétravail et le support technique à distance.
- Centraliser l'exécution des applications sur des serveurs.
- Faciliter l'administration graphique des serveurs Windows.
- Réduire les coûts matériels grâce aux bureaux virtuels.
- Offrir une expérience utilisateur riche (audio, vidéo, impression, USB).

### 4.4 Fonctionnement de RDP

RDP fonctionne selon un modèle client-serveur. Le serveur RDP (hôte de session Bureau à distance) écoute sur le port TCP/3389. Le client RDP (Bureau à distance) initie la connexion. Les étapes principales sont :

1. **Négociation des capacités :** Résolution, profondeur de couleur, redirections.
2. **Authentification :** Le client fournit ses identifiants (mot de passe, carte à puce, NLA).
3. **Établissement de la session :** Création de la session utilisateur ou reconnexion.
4. **Transport des données graphiques :** Le serveur compresse et envoie les mises à jour d'écran.
5. **Transport des entrées :** Le client envoie les frappes clavier, les mouvements de souris et les événements audio.

### 4.5 Ports et protocoles réseau

- **Port par défaut :** TCP/3389 et UDP/3389.
- **Transport :** TCP et UDP.
- **Couche OSI :** Application (couche 7).
- **Autres ports :** TCP/3390, TCP/3391 pour les configurations personnalisées.

### 4.6 Versions de RDP

| Version | Année | Système d'exploitation | Améliorations |
|---------|-------|------------------------|---------------|
| RDP 4.0 | 1996 | Windows NT 4.0 TSE | Première version |
| RDP 5.0 | 2001 | Windows 2000 Server | Amélioration des performances |
| RDP 5.1 | 2001 | Windows XP | Support 24 bits |
| RDP 5.2 | 2003 | Windows Server 2003 | Compression améliorée |
| RDP 6.0 | 2006 | Windows Vista | Aéro Glass, redirection multimedia |
| RDP 7.0 | 2009 | Windows 7 / Server 2008 R2 | Multimedia, bidirectionnel |
| RDP 8.0 | 2012 | Windows 8 / Server 2012 | UDP, RemoteFX |
| RDP 8.1 | 2013 | Windows 8.1 | Optimisations |
| RDP 10.0 | 2015 | Windows 10 | H.264/AVC, AVC444 |
| RDP 10.7 | 2021 | Windows 11 / Server 2022 | AV1, performances |

### 4.7 Installation et configuration de RDP

#### 4.7.1 Activer RDP sur Windows 10/11

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -value 0
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```

Graphiquement : Paramètres > Système > Bureau à distance > Activer le Bureau à distance.

#### 4.7.2 Activer RDP sur Windows Server

```powershell
Install-WindowsFeature -Name RDS-RD-Server -IncludeManagementTools
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -value 0
```

#### 4.7.3 Changer le port RDP

```powershell
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "PortNumber" -value 3390
New-NetFirewallRule -DisplayName 'RDP Custom' -Direction Inbound -LocalPort 3390 -Protocol TCP -Action Allow
```

#### 4.7.4 Configuration via l'Éditeur de stratégie de groupe

- `gpedit.msc` sur les éditions Pro/Entreprise.
- `Configuration ordinateur > Stratégies > Modèles d'administration > Composants Windows > Services Bureau à distance > Hôte de session Bureau à distance > Sécurité`.
- Activer NLA (Network Level Authentication).
- Exiger TLS pour les connexions.

### 4.8 Utilisation de RDP

#### 4.8.1 Connexion depuis Windows

```cmd
mstsc
mstsc /v:serveur
mstsc /v:serveur:3390 /f
```

#### 4.8.2 Connexion depuis Linux

```bash
xfreerdp /v:serveur /u:utilisateur /p:motdepasse
rdesktop serveur -u utilisateur -p motdepasse
remmina
```

#### 4.8.3 Connexion depuis macOS

Utiliser Microsoft Remote Desktop depuis le Mac App Store.

#### 4.8.4 Connexion depuis Android/iOS

Applications officielles : Microsoft Remote Desktop.

### 4.9 Architecture de RDP

RDP repose sur plusieurs composants :

- **TermService :** Service hôte de session Bureau à distance.
- **RDP-Tcp :** Pile de transport.
- **TermDD :** Pilote de périphérique de bureau à distance.
- **RDPWSX :** Gestionnaire de session.
- ** canal dynamique :** Pour les redirections USB, audio, imprimante.
- **Multitransport :** Utilisation de TCP et UDP pour optimiser la latence.

### 4.10 Commandes de vérification et diagnostics RDP

```powershell
Get-Service TermService
Test-NetConnection -ComputerName serveur -Port 3389
qwinsta
quser
netstat -an | findstr 3389
Get-Process *rdp*
Get-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections"
```

Sous Linux :

```bash
nmap -p 3389 serveur
nc -vz serveur 3389
xfreerdp /v:serveur /u:utilisateur /p:motdepasse /cert-ignore
```

### 4.11 Outils et applications RDP

- **Bureau à distance Microsoft (mstsc) :** Client natif Windows.
- **FreeRDP :** Client/serveur open source.
- **Remmina :** Client graphique Linux.
- **rdesktop :** Client RDP pour Linux.
- **Microsoft Remote Desktop :** Applications mobiles.
- **Royal TS :** Gestionnaire de connexions commerciales.
- **mRemoteNG :** Gestionnaire de connexions open source.
- **Devolutions Remote Desktop Manager :** Solution commerciale.
- **Apache Guacamole :** Passerelle HTML5 sans client.

### 4.12 Cas d'utilisation de RDP

- Télétravail et accès au poste de bureau.
- Administration graphique des serveurs Windows.
- Support technique à distance.
- Déploiement de bureaux virtuels (VDI).
- Hébergement d'applications centralisées (RDS).
- Formation et démonstration.
- Tests d'applications Windows depuis d'autres systèmes.

### 4.13 Sécurité avancée RDP

- Activer Network Level Authentication (NLA).
- Exiger l'authentification à deux facteurs (2FA).
- Restreindre l'accès par adresse IP via un pare-feu.
- Utiliser un VPN ou une passerelle RDS avant d'exposer RDP.
- Changer le port par défaut (obscurité, non suffisante seule).
- Activer le chiffrement TLS/SSL.
- Désactiver les comptes invités et restreindre les utilisateurs.
- Appliquer les correctifs de sécurité régulièrement.
- Surveiller les connexions et les échecs d'authentification.
- Utiliser des certificats valides pour l'authentification du serveur.
- Activer les stratégies de verrouillage de compte.

### 4.14 Fail2ban pour RDP

Sous Windows, utiliser des outils comme **IPBan** ou **RdpGuard** pour bloquer les tentatives de brute-force.

### 4.15 Passerelle RDS et Web Access

- **RDS Gateway :** Point d'entrée sécurisé pour les connexions RDP via HTTPS.
- **RDS Web Access :** Portail web pour lancer des applications et des bureaux.
- **RDS Broker :** Gestion de la répartition de charge et de la reconnexion.

---

## 5. VNC (Virtual Network Computing)

### 5.1 Définition

VNC, ou Virtual Network Computing, est un système de partage d'écran graphique utilisant le protocole RFB (Remote Framebuffer) pour contrôler à distance un autre ordinateur. VNC est indépendant du système d'exploitation : un client VNC peut se connecter à un serveur VNC quel que soit le système.

### 5.2 Historique et créateurs

VNC a été développé en 1998 à l'Université de Cambridge par l'équipe du Olivetti & Oracle Research Lab, dirigée par Tristan Richardson, Quentin Stafford-Fraser et Andy Harter. Il est devenu open source en 1999. Le protocole RFB est désormais maintenu par RealVNC.

### 5.3 Objectifs de VNC

- Fournir un accès graphique à distance multiplateforme.
- Permettre le support technique et la formation.
- Offrir une solution légère de partage d'écran.
- Fonctionner indépendamment du système d'exploitation.
- Servir d'outil de visualisation et de contrôle à distance.

### 5.4 Fonctionnement de VNC

VNC utilise le protocole RFB (Remote Framebuffer). Le serveur VNC capture l'affichage de l'écran distant, compresse les images et les envoie au client. Le client envoie les événements clavier et souris au serveur. Le protocole est asymétrique : le client est passif pour l'affichage et actif pour les entrées.

### 5.5 Ports et protocoles réseau

- **Port par défaut :** TCP/5900 + N (N = numéro d'affichage).
- **Port HTTP/Java :** TCP/5800 + N.
- **Transport :** TCP.
- **Couche OSI :** Application (couche 7).

### 5.6 Versions et implémentations de VNC

| Implémentation | Caractéristiques |
|----------------|------------------|
| RealVNC | Version commerciale et gratuite, fonctions avancées |
| TightVNC | Compression optimisée, performances sur réseaux lents |
| TigerVNC | Dérivé de RealVNC, performances modernes |
| UltraVNC | Fonctions Windows avancées, transfert de fichiers |
| x11vnc | Serveur VNC pour le serveur X11 existant |
| vino/vinagre | Intégration GNOME |
| KRDC/KRFB | Intégration KDE |
| Apple Remote Desktop | Basé sur VNC pour macOS |
| noVNC | Client VNC dans un navigateur HTML5 |

### 5.7 Installation et configuration de VNC

#### 5.7.1 Sous Linux avec TightVNC

```bash
sudo apt update
sudo apt install tightvncserver
vncpasswd
vncserver :1 -geometry 1920x1080 -depth 24
```

#### 5.7.2 Sous Linux avec TigerVNC

```bash
sudo apt install tigervnc-standalone-server tigervnc-viewer
vncpasswd
vncserver :1 -geometry 1920x1080
```

#### 5.7.3 Sous Windows

Installer RealVNC, TightVNC ou UltraVNC. Activer le serveur VNC et définir un mot de passe.

#### 5.7.4 Sous macOS

Activer le partage d'écran dans Préférences Système > Partage > Partage d'écran.

#### 5.7.5 Configuration du serveur VNC

Fichier de configuration : `~/.vnc/xstartup`

Exemple pour un bureau XFCE :

```bash
#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &
```

Donner les droits d'exécution :

```bash
chmod +x ~/.vnc/xstartup
```

### 5.8 Utilisation de VNC

#### 5.8.1 Connexion avec un client VNC

```bash
vncviewer serveur:1
vncviewer serveur::5901
```

#### 5.8.2 Connexion sécurisée via un tunnel SSH

```bash
ssh -L 5901:localhost:5901 utilisateur@serveur
vncviewer localhost:1
```

#### 5.8.3 Connexion depuis un navigateur

Accéder à `http://serveur:5801` si le serveur VNC intègre un serveur HTTP/Java.

### 5.9 Architecture de VNC

VNC repose sur le protocole RFB :

- **Framebuffer :** Représentation pixel de l'écran distant.
- **Encodages :** ZRLE, Hextile, Raw, Tight, etc.
- **Mises à jour :** Le serveur envoie les rectangles modifiés de l'écran.
- **Entrées :** Le client envoie les événements clavier et souris.
- **Négociation :** Choix des encodages, de la résolution et de la profondeur.

### 5.10 Commandes de vérification et diagnostics VNC

```bash
vncserver -list
vncserver -kill :1
netstat -tlnp | grep vnc
ss -tlnp | grep 5900
nmap -p 5900-5905 serveur
nc -vz serveur 5901
ps aux | grep vnc
```

### 5.11 Outils et applications VNC

- **RealVNC :** Solution complète.
- **TightVNC :** Légère et performante.
- **TigerVNC :** Pour les environnements modernes.
- **UltraVNC :** Riche en fonctionnalités Windows.
- **noVNC :** Client HTML5.
- **Remmina :** Client multi-protocoles Linux.
- **Chicken of the VNC / TigerVNC Viewer :** Clients macOS.
- **VNC Viewer :** Client officiel multiplateforme.

### 5.12 Cas d'utilisation de VNC

- Support technique à distance multiplateforme.
- Accès à des bureaux Linux distants.
- Surveillance d'affichages industriels.
- Kiosques et bornes interactives.
- Formation et démonstration.
- Accès à distance aux machines virtuelles.
- Administration de serveurs sans interface graphique native.

### 5.13 Sécurité avancée VNC

- Toujours utiliser VNC à travers un tunnel SSH ou un VPN.
- Choisir un mot de passe VNC long et complexe.
- Limiter l'accès aux adresses IP de confiance.
- Désactiver l'accès web Java si inutile.
- Préférer les versions supportant le chiffrement TLS (RealVNC Cloud, etc.).
- Mettre à jour régulièrement le serveur VNC.
- Surveiller les connexions et les échecs.

### 5.14 Exemple de tunnel SSH pour VNC

```bash
ssh -L 5901:localhost:5901 -N -f utilisateur@serveur
vncviewer localhost:5901
```

---

## 6. Comparatif des protocoles

| Critère | SSH | Telnet | RDP | VNC |
|---------|-----|--------|-----|-----|
| Type d'interface | Ligne de commande | Ligne de commande | Graphique | Graphique |
| Chiffrement | Oui (fort) | Non | Oui (variable) | Optionnel |
| Port par défaut | TCP/22 | TCP/23 | TCP/3389 | TCP/5900 |
| Authentification | Clé, mot de passe, 2FA | Mot de passe en clair | Mot de passe, NLA, 2FA | Mot de passe VNC |
| Performance | Très rapide | Très rapide | Bonne sur réseaux rapides | Variable |
| Multiplateforme | Oui | Oui | Client oui, serveur principalement Windows | Oui |
| Cas principal | Administration sécurisée | Tests, réseaux isolés | Bureaux Windows | Partage d'écran |
| Sécurité | Excellente | Faible | Bonne si configuré correctement | Moyenne |

---

## 7. Architecture et modèles OSI/TCP-IP

### 7.1 Modèle OSI

| Couche | Fonction | Protocoles concernés |
|--------|----------|----------------------|
| 7 - Application | Accès aux applications réseau | SSH, Telnet, RDP, VNC |
| 6 - Présentation | Formatage, chiffrement | TLS, RDP, SSH |
| 5 - Session | Gestion des sessions | SSH, RDP |
| 4 - Transport | Fiabilité des flux | TCP, UDP |
| 3 - Réseau | Adressage et routage | IP |
| 2 - Liaison | Accès au médium | Ethernet, Wi-Fi |
| 1 - Physique | Transmission des bits | Câbles, ondes |

### 7.2 Modèle TCP/IP

| Couche | Protocoles |
|--------|------------|
| Application | SSH, Telnet, RDP, VNC, HTTP, FTP, SMTP |
| Transport | TCP, UDP |
| Internet | IP, ICMP, ARP |
| Accès réseau | Ethernet, Wi-Fi |

### 7.3 Encapsulation des données

Lors d'une connexion à distance, les données sont encapsulées à chaque couche : données applicatives > segment TCP/UDP > paquet IP > trame Ethernet. Le chiffrement intervient généralement au niveau de la couche application ou présentation.

---

## 8. Commandes de vérification et diagnostics

### 8.1 Généralités réseau

```bash
ping serveur
traceroute serveur
mtr serveur
nslookup serveur
dig serveur
ip a
ip r
ss -tlnp
netstat -tlnp
nmap -sV -p 22,23,3389,5900 serveur
nc -vz serveur 22
nc -vz serveur 23
nc -vz serveur 3389
nc -vz serveur 5900
```

### 8.2 Diagnostics SSH

```bash
ssh -v utilisateur@serveur
ssh -vvv utilisateur@serveur
ssh-keygen -lf ~/.ssh/id_rsa.pub
ssh-keyscan -H serveur >> ~/.ssh/known_hosts
ssh -G utilisateur@serveur
cat ~/.ssh/known_hosts
```

### 8.3 Diagnostics Telnet

```bash
telnet serveur 23
telnet serveur 80
```

### 8.4 Diagnostics RDP

```powershell
Test-NetConnection -ComputerName serveur -Port 3389
Get-Service TermService
qwinsta
quser
```

### 8.5 Diagnostics VNC

```bash
vncserver -list
nmap -p 5900-5905 serveur
nc -vz serveur 5901
```

### 8.6 Outils de capture et analyse

```bash
tcpdump -i any port 22
tcpdump -i any port 23
tcpdump -i any port 3389
tcpdump -i any port 5900
wireshark
```

---

## 9. Outils et applications utilisées

### 9.1 Clients et serveurs

| Outil | Type | Protocoles supportés | Plateformes |
|-------|------|----------------------|-------------|
| OpenSSH | Client/Serveur | SSH, SFTP, SCP | Linux, macOS, Windows |
| PuTTY | Client | SSH, Telnet, Rlogin, Serial | Windows, Linux |
| MobaXterm | Client | SSH, RDP, VNC, SFTP, X11 | Windows |
| Termius | Client | SSH, SFTP | Multiplateforme |
| WinSCP | Client | SFTP, SCP, FTP | Windows |
| FileZilla | Client | SFTP, FTP | Multiplateforme |
| Remmina | Client | SSH, RDP, VNC | Linux |
| FreeRDP | Client/Serveur | RDP | Multiplateforme |
| mstsc | Client | RDP | Windows |
| RealVNC | Client/Serveur | VNC | Multiplateforme |
| TightVNC | Client/Serveur | VNC | Multiplateforme |
| TigerVNC | Client/Serveur | VNC | Multiplateforme |
| noVNC | Client | VNC | Navigateur |
| Apache Guacamole | Passerelle | SSH, RDP, VNC | Serveur web |

### 9.2 Outils de sécurité

- **fail2ban :** Protection contre les attaques par force brute.
- **IPBan / RdpGuard :** Protection RDP sous Windows.
- **Google Authenticator / YubiKey :** 2FA.
- **Nmap :** Scan de ports.
- **Wireshark :** Analyse de trafic.
- **tcpdump :** Capture réseau en ligne de commande.
- **Metasploit :** Tests d'intrusion.
- **Hydra / Medusa :** Tests de mot de passe.

---

## 10. Bonnes pratiques et sécurité

### 10.1 Généralités

- **Chiffrer toujours les accès à distance.**
- **Utiliser l'authentification forte.**
- **Limiter l'exposition à Internet.**
- **Surveiller les journaux d'accès.**
- **Appliquer les correctifs de sécurité.**
- **Segmenter le réseau.**
- **Utiliser des VPN pour les accès externes.**
- **Former les utilisateurs aux risques.**

### 10.2 Checklist SSH

- [ ] Désactiver root login.
- [ ] Désactiver l'authentification par mot de passe.
- [ ] Utiliser des clés fortes (Ed25519/RSA 4096).
- [ ] Restreindre les utilisateurs.
- [ ] Activer fail2ban.
- [ ] Mettre à jour OpenSSH.
- [ ] Activer le 2FA.

### 10.3 Checklist RDP

- [ ] Activer NLA.
- [ ] Limiter l'accès par IP.
- [ ] Utiliser un VPN ou une passerelle RDS.
- [ ] Activer le 2FA.
- [ ] Changer le port par défaut.
- [ ] Surveiller les connexions.

### 10.4 Checklist VNC

- [ ] Utiliser un tunnel SSH ou un VPN.
- [ ] Choisir un mot de passe fort.
- [ ] Limiter les adresses IP.
- [ ] Désactiver l'accès web si inutile.
- [ ] Préférer les versions chiffrées.

---

## 11. Ressources, livres et liens

### 11.1 Livres recommandés

- **SSH, The Secure Shell :** The Definitive Guide - Daniel J. Barrett, Richard E. Silverman, Robert G. Byrnes.
- **The Practice of System and Network Administration -** Thomas A. Limoncelli, Christina J. Hogan, Strata R. Chalup.
- **Windows Internals -** Mark Russinovich, David Solomon, Alex Ionescu.
- **Hacking :** The Art of Exploitation - Jon Erickson.
- **The Web Application Hacker's Handbook -** Dafydd Stuttard, Marcus Pinto.
- **Network Security Assessment -** Chris McNab.
- **Linux Administration -** Tom Adelstein, Bill Lubanovic.
- **Mastering Windows Server -** Mark Minasi.

### 11.2 RFCs et documentation officielle

- RFC 4251 - The Secure Shell (SSH) Protocol Architecture
- RFC 4252 - SSH Authentication Protocol
- RFC 4253 - SSH Transport Layer Protocol
- RFC 4254 - SSH Connection Protocol
- RFC 854 - Telnet Protocol Specification
- RFC 855 - Telnet Option Specifications
- Microsoft Docs : Remote Desktop Protocol
- RealVNC Documentation

### 11.3 Liens utiles

- OpenSSH : https://www.openssh.com
- PuTTY : https://www.putty.org
- FreeRDP : https://www.freerdp.com
- RealVNC : https://www.realvnc.com
- TightVNC : https://www.tightvnc.com
- TigerVNC : https://tigervnc.org
- noVNC : https://novnc.com
- Apache Guacamole : https://guacamole.apache.org
- Microsoft Remote Desktop : https://docs.microsoft.com/fr-fr/windows-server/remote/remote-desktop-services/welcome-to-rds
- Nmap : https://nmap.org
- Wireshark : https://www.wireshark.org
- Fail2ban : https://www.fail2ban.org

### 11.4 Communautés et auteurs

- **Auteur du document :** hackers_tchad
- **Communautés :** Stack Overflow, Server Fault, Reddit r/sysadmin, Reddit r/netsec
- **Cours en ligne :** Cisco Networking Academy, Cybrary, Hack The Box, TryHackMe

---

## 12. Annexes

### 12.1 Glossaire

- **2FA (Two-Factor Authentication) :** Authentification à deux facteurs.
- **AES (Advanced Encryption Standard) :** Standard de chiffrement symétrique.
- **Clé publique/privée :** Paire de clés utilisée pour le chiffrement asymétrique.
- **DPI (Dots Per Inch) :** Résolution d'affichage.
- **Framebuffer :** Mémoire contenant l'image affichée à l'écran.
- **NLA (Network Level Authentication) :** Authentification au niveau réseau.
- **NVT (Network Virtual Terminal) :** Terminal virtuel réseau.
- **RFB (Remote Framebuffer) :** Protocole utilisé par VNC.
- **RDS (Remote Desktop Services) :** Services Bureau à distance Microsoft.
- **TLS (Transport Layer Security) :** Protocole de sécurisation des communications.
- **VDI (Virtual Desktop Infrastructure) :** Infrastructure de bureaux virtuels.
- **VPN (Virtual Private Network) :** Réseau privé virtuel.

### 12.2 Tableau récapitulatif des ports

| Protocole | Port par défaut | Transport | Sécurité |
|-----------|-----------------|-----------|----------|
| SSH | 22 | TCP | Forte |
| Telnet | 23 | TCP | Aucune |
| RDP | 3389 | TCP/UDP | Moyenne à forte |
| VNC | 5900 | TCP | Optionnelle |

### 12.3 Schéma d'architecture simplifié

```
[Client] ---(Internet/Réseau LAN)--- [Pare-feu/VPN] --- [Serveur SSH/Telnet/RDP/VNC]
```

### 12.4 Exemples de scripts d'automatisation

#### 12.4.1 Script SSH pour exécuter une commande sur plusieurs serveurs

```bash
#!/bin/bash
SERVEURS="srv1 srv2 srv3"
for srv in $SERVEURS; do
    echo "=== $srv ==="
    ssh utilisateur@$srv "uptime"
done
```

#### 12.4.2 Script de vérification des ports

```bash
#!/bin/bash
PORTS="22 23 3389 5900"
for port in $PORTS; do
    nc -vz serveur $port 2>&1
done
```

### 12.5 Notes sur la conformité et la législation

L'utilisation des protocoles d'accès à distance doit respecter la législation en vigueur, notamment en matière de protection des données, de cybersécurité et d'accès aux systèmes d'information. Tout accès non autorisé à un système informatique est illégal. Ce document est destiné à un usage éducatif et professionnel dans un cadre légal.

### 12.6 Remerciements

Ce guide a été rédigé par **hackers_tchad** en 2026 pour accompagner les administrateurs système, les techniciens réseau, les professionnels de la cybersécurité et tous ceux qui souhaitent maîtriser les protocoles d'accès à distance.

---

*Fin du document.*
