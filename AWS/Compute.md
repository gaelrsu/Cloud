# Compute
## les différents services de calcul
### EC2
Fournit une capacité de calcul sécurisée et redimensionnable, utilisé pour metter en service des serveurs virtuels (appelé EC2) qui gèrent les besoins informatique.
### Lambda
Service de calcul permettant d'exécuter du code sans avoir à mettre en service ni à gérer des serveurs. 
### ECS 
Système de gestion de conteneurs aide à la mise en service de nouveaux conteneurs et à gérer les instances EC2.
### EKS
Permet de démarrer et exécuter les applis Kubernetes dans les cloud AWS ou sur site.
### Fargate
Moteur de calcul sans serveur, pour les conteneurs (pour ECS et EKS). Il alloue la bonne quantité de calcul.
### Elastic Beantalk
Après lui avoir donner du code, il effectue automatiquement les étapes de déploiement. (Mise en service, répartition des charges, mise à l'chelle, surveillance)
______________________________________________________________________________________
/!\ Un EC2 avec un "store volume" ne gardera pas ses données en cas de reboot contrairement à un EBS
## Les différentes familles
Famille T \
Famille M \
Famille C \
Famille P \
Famille R \
Ex : t3.large => t-> nom de la famille / 3 -> numéro de la génération / large -> taille de l'instace
### Catégories d'instances
Polyvalentes (assurent l'équilibre entre les ressources de calcul, la mémoire et les ressources réseau ex: serveurs web) : A1, M4, M5, T2, T3 \
Optimisées pour le calcul (charges de travail de traitement par lots, transcodage de fichiers multimédias autres applis à forte intensité de calcul) : C4, C5 \
Optimisées en mémoire (fournir des performances rapides pour les charges de travail traitant des grands ensembles de données en mémoire) : R4, R5, X1, Z1 \
Calcul accéléré (Coprocesseur, execute certaines fonctions telles que les calculs des nombres à virgule flottante, traitement graphique..) : F1, G3, G4, P2, P3 \
Optimisée pour le stockage (optimisées pour fournir aux applications des dizaines de milliers d'opérations aléatiores à faible temps de latence) : D2, H1, I3 \


## Exemple
```
1. Dans la zone Search de la Console de gestion AWS, saisissez EC2, puis cliquez sur Enter.
2. Dans les résultats de recherche, sélectionnez EC2.
3. Dans la section Launch instance, cliquez sur Launch instance
4. Dans le volet Name and tags, dans la zone de texte Name, saisissez Web-Server
puis cliquez sur Enter.
5. Cliquez sur le lien Add additional tags.
6. Dans la liste déroulante Resource types, Instances est sélectionné par défaut. Laissez
Instances sélectionné et sélectionnez Volumes.

```
