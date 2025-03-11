# Exercice 2 : Manipulations pratiques sur VM Linux

## Partie 1 : Gestion des utilisateurs

### Q.2.1.1 Sur le serveur, créer un compte pour ton usage personnel.
- Créez un compte utilisateur sur le serveur pour un usage personnel.
```bash
sudo adduser wilder
#Attribuer des droits sudo
sudo usermod -aG sudo monutilisateur
```

### Q.2.1.2 Quelles préconisations proposes-tu concernant ce compte ?
- Proposez des bonnes pratiques pour la gestion de ce compte, telles que l'utilisation de mots de passe forts, la gestion des groupes et des permissions.

## Partie 2 : Configuration de SSH

### Q.2.2.1 Désactiver complètement l'accès à distance de l'utilisateur root.

```bash
sudo nano /etc/ssh/sshd_config
PermitRootLogin no
```

### Q.2.2.2 Autoriser l'accès à distance à ton compte personnel uniquement.

```bash
AllowUsers wilder
```

### Q.2.2.3 Mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe.
- 1. Générer une clé SSH sur ton ordinateur local 
```bash
ssh-keygen -t rsa -b 4096
```
2. Copier la clé publique sur le serveur Ubuntu :

```bash
ssh-copy-id wilder@192.168.1.2
```
3. Désactiver l'authentification par mot de passe sur le serveur Ubuntu :
```bash
# Édite le fichier de configuration SSH :
sudo nano /etc/ssh/sshd_config

# Modifie ou ajoute ces lignes :
PasswordAuthentication no
ChallengeResponseAuthentication no
```
4. Redémarrer le service SSH :
```bash
sudo systemctl restart sshd
```
5. Tester la connexion :
```bash
ssh wilder@192.168.1.2
```
## Partie 3 : Analyse du stockage

### Q.2.3.1 Quels sont les systèmes de fichiers actuellement montés ?
- Lsblk
- Df -h
![lsblk](https://github.com/KAOUTARBAH/Checkpoint3_Blanc/blob/main/images/lsblk.png)
![df](https://github.com/KAOUTARBAH/Checkpoint3_Blanc/blob/main/images/df.png)


### Q.2.3.2 Quel type de système de stockage ils utilisent ?
- Indiquez le type de système de stockage (ext4, xfs, etc.) utilisé par chaque système de fichiers monté.

## Partie 4 : Sauvegardes

### Q.2.4.1 Expliquer succinctement les rôles respectifs des 3 composants bareos installés sur la VM.
- Expliquez brièvement les rôles des 3 composants principaux de **Bareos** (Directeur, Stockage, Client) installés sur la machine.

## Partie 5 : Filtrage et analyse réseau

### Q.2.5.1 Quelles sont actuellement les règles appliquées sur Netfilter ?
- Listez les règles actuellement appliquées par **Netfilter** (pare-feu Linux).

## Partie 6 : Analyse de logs

### Q.2.6.1 Lister les 10 derniers échecs de connexion ayant eu lieu sur le serveur en indiquant pour chacun :
- La date et l'heure de la tentative
- L'adresse IP de la machine ayant fait la tentative
