# Exercice 1 : Manipulations pratiques sur VM Windows

## Partie 1 : Gestion des utilisateurs

### Q.1.1.1 Créer l'utilisateur Lionel Lemarchand avec les mêmes attributs que Kelly Rhameur.
- Créez un nouvel utilisateur **Lionel Lemarchand** dans Active Directory avec les mêmes attributs que **Kelly Rhameur** (nom complet, identifiant, groupes, etc.).

- Ajouter l'utilisateur : **Kelly Rhameur**
- Ouvrez le Gestionnaire de serveur sur un serveur Windows et allez dans Outils > Utilisateurs et ordinateurs Active Directory.
- Utilisateurs >  Nouveau > Utilisateur.
- Allez dans l'onglet Membre de, puis cliquez sur Ajouter pour ajouter Kelly Rhameur à des groupes comme Utilisateurs.

# Récupérer les attributs de Kelly Rhameur en utilisant le SamAccountName exact
- code en powershel attributs.sh
```sh
$kelly = Get-ADUser -Identity "Kelly.Rhameur" -Properties *

# Vérifiez si l'utilisateur Kelly a été récupéré correctement
if ($kelly) {
    # Créer l'utilisateur Lionel Lemarchand avec les mêmes attributs
    New-ADUser -SamAccountName "LionelLemarchand" `
        -UserPrincipalName "$($kelly.UserPrincipalName.Replace('Kelly.Rhameur', 'Lionel.Lemarchand'))" `
        -GivenName "Lionel" `
        -Surname "Lemarchand" `
        -Name "Lionel Lemarchand" `
        -DisplayName "Lionel Lemarchand" `
        -Department $kelly.Department `
        -Title $kelly.Title `
        -EmailAddress $(if ($kelly.EmailAddress) { $kelly.EmailAddress.Replace('Kelly.Rhameur', 'Lionel.Lemarchand') } else { "lionel.lemarchand@domaine.com" }) `
        -AccountPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force) `
        -Enabled $true `
        -PassThru

    # Ajouter l'utilisateur au groupe "Utilisateurs"
    Add-ADGroupMember -Identity "Utilisateurs" -Members "LionelLemarchand"
} else {
    Write-Host "L'utilisateur Kelly Rhameur n'a pas été trouvé."
}

```




### Q.1.1.2 Créer une OU DeactivatedUsers et déplacer le compte désactivé de Kelly Rhameur dedans.
- Créez une nouvelle **OU** (Organizational Unit) nommée **DeactivatedUsers**.
- Déplacez le compte **Kelly Rhameur** dans cette nouvelle **OU**.

### Q.1.1.3 Modifier le groupe de l'OU dans laquelle était Kelly Rhameur.
- Modifiez les groupes associés à l'ancienne **OU** de Kelly Rhameur pour mettre à jour les membres ou groupes de sécurité.

### Q.1.1.4 Archiver le dossier de Kelly Rhameur et créer celui de Lionel Lemarchand.
- Archivez le dossier personnel de **Kelly Rhameur**.
- Créez un nouveau dossier pour **Lionel Lemarchand**.

## Partie 2 : Restriction utilisateurs

### Q.1.2.1 Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.
- Appliquez une restriction de connexion horaire à l'utilisateur **Gabriel Ghul** pour limiter les connexions du lundi au vendredi, entre 7h et 17h.

### Q.1.2.2 Bloquer sa connexion au seul ordinateur CLIENT01.
- Configurez une restriction pour bloquer la connexion de **Gabriel Ghul** uniquement à l'ordinateur **CLIENT01**.

### Q.1.2.3 Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers.
- Mettez en place une politique de mot de passe stricte pour les utilisateurs dans l'**OU LabUsers** afin de renforcer la sécurité des comptes.

## Partie 3 : Lecteurs réseaux

### Q.1.3.1 Créer une GPO Drive-Mount qui monte les lecteurs E: et F: sur les clients.
- Créez une **GPO (Group Policy Object)** nommée **Drive-Mount** pour monter les lecteurs **E:** et **F:** sur les postes clients via la stratégie de groupe.
