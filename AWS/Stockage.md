
## Les différents services de stockage

EBS : Amazon Elastic Block Store qui est un service de stockage évolutif pour les EC2. Offre une grande disponibilité (99,999%) \
-> Basé sur des SSD -> Joignent les instances EC2 -> Application \
EFS : Amazon Elastic File System est un système de fichier élastique pour partager des données (sans service ni gestion de stockage) \
-> EC2/Conteneur/AWS Lambda/serveurs -> test et optimise -> transfert les données -> partage et protège + les données des fichiers \
S3 : Amazon Simple Storage Service est un service de stockage d'objets sous forme de compartiments (objet=fichier ou métadonnée qui décrit le fichier). \ 
A X utilisations possible : Lacs de données, sites web, app mobiles, sauvegarde / resto ... \
-> S3 -> Transfert de données -> Stock les données -> utilise et analyse les données 
- S3 Standard pour les données actives
- S3 One zone pour les données rarement utilisées, peuvent être facilement recréées en cas de perte 
- S3 Glacier utilisé pour l'archivage des données, récupérable en quelques heures
- S3 Intelligent si on ne connais pas le schéma d'accès des données ou si elles sont imprévisibles, permet d'éviter les impacts sur la perf et réduit les frais/couts
