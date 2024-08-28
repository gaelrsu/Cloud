# RDS
## MEP
Créer un moteur de recherche de données (ex: Mysql) => Créer le VPC => Créer l'instance RDS => Relier l'application (Mysql) au RDS => travailler avec la BDD => Monitorer la BDD
### process
RDS => Databases -> Create Database => choisir 'Standard' ou 'Easy' => Entrer l'application désirée => Choisir un template => choisir type de dispo : \
-> Cluster 1 Primaire 2 secondaires \
-> Instance 1 Primaire 1 standby \
-> Single instance no standby \
=> Entrer Nom de la BDD, un user et MDP => choisir la classe : \
-> Standard = objectif générale (m1, m3, m4, m5, m5d, m5g) \
=> choisir le type de stockage : SSd, IOPS (utilisé pour une base de donnée "intense I/O"), magnetique (non recommandé) \
=> choisir si utilisation d'un EC2 et un VPC  => choisir un sous-réseau \
=> choisir port par défaut ou en saisir un, entrer un mdp ppour la bdd si besoin ou IAM/Kerberos option => Create database


