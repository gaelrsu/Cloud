# Sécurisation d’un compte AWS

Objectif : Configurer un compte AWS en appliquant : MFA, gestion des utilisateurs IAM, audit via CloudTrail, et alerting via CloudWatch + SNS.

-------------------------------------
## Étape 1 — Activer l'authentification multifactorielle (MFA)

Étapes détaillées (Console)

Ouvrir la console AWS et se connecter en tant que root.

Clique sur le nom (en haut à droite) → 'My Security Credentials'.

Dans la section Multi-Factor Authentication (MFA), cliquer sur 'Activate MFA'.

Choisir 'Virtual MFA device' (recommandé) puis Continue.

Ouvrir l'application MFA et scanner le QR code affiché.

Saisir deux codes consécutifs fournis par l'application pour terminer l'association.

Vérifier que le statut 'Assigned' apparaît.


## Étapes (AWS CLI — avancé)

Pour un utilisateur IAM, on peut en cli :
### Créer un appareil MFA virtuel (ex. pour user 'alice')
aws iam create-virtual-mfa-device --virtual-mfa-device-name alice-mfa \
--outfile /tmp/alice-mfa-qr.png


### Associer l'appareil (deux codes successifs fournis par l'app MFA)
aws iam enable-mfa-device --user-name moi \
--serial-number arn:aws:iam::123456789012:mfa/moi-mfa \
--authentication-code1 123456 --authentication-code2 456789

# Utilisateur IAM 
## Étape 2 — Créer un utilisateur IAM avec permissions minimales

Étapes (Console)
Ouvrir IAM → Users → Add user.

Cocher 'Programmatic access' pour les clés (CLI/SDK) et AWS Management Console access si besoin d’accès console.

Dans Set permissions, choisir l’une des options :

Attach existing policies directly → sélectionner ReadOnlyAccess pour commencer

Add user to group → crée un groupe et attache la policy désirée.

Finir la création et télécharger le fichier CSV contenant l’Access Key ID et Secret Access Key si accès programmatique.
