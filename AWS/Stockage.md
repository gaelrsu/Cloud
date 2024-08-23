
## Les différents services de stockage

EBS : Amazon Elastic Block Store qui est un service de stockage évolutif pour les EC2. Offre une grande disponibilité (99,999%) \
-> Basé sur des SSD -> Joignent les instances EC2 -> Application \
\
EFS : Amazon Elastic File System est un système de fichier élastique pour partager des données (sans service ni gestion de stockage) \
-> EC2/Conteneur/AWS Lambda/serveurs -> test et optimise -> transfert les données -> partage et protège + les données des fichiers \
\
S3 : Amazon Simple Storage Service est un service de stockage d'objets sous forme de compartiments (objet=fichier ou métadonnée qui décrit le fichier). \ 
A X utilisations possible : Lacs de données, sites web, app mobiles, sauvegarde / resto ... \
-> S3 -> Transfert de données -> Stock les données -> utilise et analyse les données 
- S3 Standard pour les données actives \
  ex : Sites web (sans contenu statique) // Stockage des fichiers type journaux avec besoin d'accès // Installeur d'application, fichiers de conf
- S3 One zone pour les données rarement utilisées, peuvent être facilement recréées en cas de perte \
  ex : stockage hors site des fichiers de sauvegarde sur site
- S3 Glacier utilisé pour l'archivage des données, récupérable en quelques heures \
  ex : Sauvegarde, archive qui doit être conservé pendant une longue durée (souvent à des fins de conformité)
- S3 Intelligent si on ne connais pas le schéma d'accès des données ou si elles sont imprévisibles, permet d'éviter les impacts sur la perf et réduit les frais/couts \
  ex : charge de travail non prévisible, inconnue ou à évolution rapide

## MEP
### Créer le Bucket
  Dans la barre de recherche : S3 -> "Créer un bucket" -> donner un nom -> choisir "Listes ACL activées" -> Gestion des versions de compartiment "Activer" -> Ajouter les Balises clé : "Departement" Valeur: "Marketing" -> Créer
### Configurer un site web sur S3
  Dans les configurations du S3, aller dans "Propriétés" puis défiler jusqu'à "Static website hosting " -> "Modifier" -> "Activer" -> entrer un document d'index et d'erreur -> sauvegarder \
  Toujours dans "Propriétés" cliquer sur le lien tout en bas pour accéder à la page
### Chargement du contenu 
  Vous pouvez charger les codes sources directement depuis la page du S3 -> dans le S3 -> Objets -> charger -> ajouter les fichiers -> Charger
### Activer de l'accès 
  Selectionner les 3 objets du S3 -> Actions -> Rendre public à l'aide de la liste ACL -> Rendre public 
### Partager un objet en utilisant une URL présignée avec une durée
  Choisir l'objet -> Actions -> Partager avec une URL présignée -> choisir la durée -> Créer une URL présignée -> en haut copier l'URL présignée 
### politique de compartiment
  Dans Votre bucket -> Autorisations -> Stratégie de compartiment -> ex pour empecher la suppression : 
  ```
{
    "Version": "2012-10-17",
    "Id": "MyBucketPolicy",
    "Statement": [
        {
            "Sid": "BucketPutDelete",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "s3:DeleteObject",
            "Resource": [
                "arn:aws:s3:::<bucket-name>/index.html",
                "arn:aws:s3:::<bucket-name>/script.js",
                "arn:aws:s3:::<bucket-name>/style.css"
# bucket-name à changer
        ]
        }
    ]
}
```
 Enregistrer la modification 
 ### Mise à jour du site web
  charger le nouveau fichier  -> Choisir l'objet -> Actions -> "Rendre public en utilisant ACL" -> Rendre public 
### versions de fichiers 
  Dans les objets cocher l'option en haut "Afficher les Versions"






















   
