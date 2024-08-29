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
# Lancement de l'instance EC2
1. Dans la zone Search de la Console de gestion AWS, saisissez EC2, puis cliquez sur Enter.
2. Dans les résultats de recherche, sélectionnez EC2.
3. Dans la section Launch instance, cliquez sur Launch instance
4. Dans le volet Name and tags, dans la zone de texte Name, saisissez Web-Server
puis cliquez sur Enter.
5. Cliquez sur le lien Add additional tags.
6. Dans la liste déroulante Resource types, Instances est sélectionné par défaut. Laissez
Instances sélectionné et sélectionnez Volumes.
# CHOISIR UNE AMI
7. Localisez la section Application and OS Images (Amazon Machine Image). Elle se situe
juste sous la section Name and tags. Dans la zone de recherche, saisissez Windows Server
2019 Base, puis cliquez sur Enter.
8. En regard de Microsoft Windows Server 2019 Base, cliquez sur Select.
# CHOISIR UN TYPE D'INSTANCE
9. Dans la section Instance type, conservez le type d'instance par défaut, t2.micro.
# CONFIGURER UNE PAIRE DE CLÉS
10. Dans la section Key pair (login), dans la liste déroulante Key pair name - required,
choisissez Proceed without a key pair (not recommended).
# CONFIGURER LES PARAMÈTRES RÉSEAU
11. Dans la section Network settings, cliquez sur Edit.
12. Dans la liste déroulante VPC - required, choisissez Lab VPC.
13. Pour Firewall (security groups), choisissez Select existing security group.
14. Depuis Common security groups, choisissez Web Server security group.
# CONFIGURER LES DÉTAILS AVANCÉS
15. Développez la section Advanced details.
16. Pour IAM instance profile, choisissez le nom de rôle qui commence par LabStack.
17. Dans la liste déroulante Termination protection, choisissez Enable
18. Copiez les commandes suivantes et choisissez la zone de texte User data. Ensuite, cliquez
sur Paste.
```
```powershell
<powershell>
# Installing web server
Install-WindowsFeature -name Web-Server -IncludeManagementTools
# Getting website code
wget https://us-east-1-tcprod.s3.amazonaws.com/courses/CUR-TF-100-
EDCOMP/v1.0.4.prod-ef70397c/01-Lab-ec2/scripts/code.zip -outfile
"C:\Users\Administrator\Downloads\code.zip"
# Unzipping website code
Add-Type -AssemblyName System.IO.Compression.FileSystem
function Unzip
{
param([string]$zipfile, [string]$outpath)
[System.IO.Compression.ZipFile]::ExtractToDirectory($zipfile, $outpath)
}
Unzip "C:\Users\Administrator\Downloads\code.zip" "C:\inetpub\"
# Setting Administrator password
$Secure_String_Pwd = ConvertTo-SecureString "P@ssW0rD!" -AsPlainText -Force
$UserAccount = Get-LocalUser -Name "Administrator"
$UserAccount | Set-LocalUser -Password $Secure_String_Pwd
</powershell>
```
```
Le script effectue les opérations suivantes :
• Installe un serveur web Microsoft IIS (Internet Information Services)
• Crée un site web simple
• Définit le mot de passe de l'administrateur
# LANCER UNE INSTANCE EC2
19. Dans la section Summary, cliquez sur Launch instance.
20. Choisissez View all instances.
21. En regard de votre Web-Server, cochez la case . Cette action affichera l'onglet Details.
Vérifiez l'onglet Details qui affiche des informations sur votre instance.
22. Sélectionnez l'onglet Security et vérifiez les informations mises à votre disposition.
23. Sélectionnez l'onglet Networking et vérifiez les informations mises à votre disposition.
# Surveillance de votre instance
24. Sélectionnez l'onglet Status and alarms. Vérifiez les informations mises à votre
disposition.
25. Sélectionnez l'onglet Monitoring.
26. En haut de la page, choisissez la liste déroulante Actions. Choisissez Monitor and
troubleshoot Get system log.
27. Dans le journal Système, vérifiez les messages dans la sortie.
28. Cliquez sur Cancel pour revenir au tableau de bord Amazon EC2.
29. Votre Web-Server étant sélectionné, choisissez la liste déroulante Actions, puis Monitor
and troubleshoot Get instance screenshot. (Cette option vous montre ce à quoi ressemblerait votre console d'instance EC2 si un écran lui étaitassocié. Comme il s'agit d'une instance Windows, la capture d'écran montre un écran de connexion verrouillé)
30. En bas de la page, cliquez sur Cancel
# Mise à jour de votre groupe de sécurité et accès au serveur web
31. Dans le volet de navigation de gauche, cliquez sur Security Groups.
32. En regard de Web Server security group, cochez la case.
33. Sélectionnez l'onglet Inbound rules.
34. Cliquez sur Edit inbound rules, puis sur Add rule et configurez les options suivantes :
• Type : choisissez HTTP.
• Source : choisissez Anywhere-IPv4.
35. Cliquez sur Save rules.
# Connexion à votre instance à l'aide d'AWS Systems Manager Fleet Manager
36. Cherchez Systems Manager, puis cliquez sur Enter.
37. Cliquez sur Systems Manager.
38. Dans le volet de navigation de gauche, cliquez sur Fleet Manager.
39. Sous Managed nodes, sélectionnez votre instance EC2 de Web-Server.
40. Dans la liste déroulante des actions de nœud, choisissez Connect, puis Connect with
Remote Desktop.
41. Saisissez comme Username : Administrator
42. Saisissez comme Password : P@ssW0rD!
43. Cliquez sur Connect.
44. Pour vous déconnecter de votre instance de Web-Server, cliquez sur Action, puis sur End
session.
45. Dans la fenêtre contextuelle, cliquez à nouveau sur End session.
# Redimensionnement de votre instance
46. Dans la Console de gestion AWS, cherchez EC2, puis cliquez sur Enter. Ensuite, cliquez sur
EC2.
47. Dans la Console de gestion EC2, dans le volet de navigation de gauche, cliquez sur
Instances.
48. Cochez la case en regard de votre instance de Web-Server. En haut de la page, choisissez
la liste déroulante Instance state et choisissez Stop instance.
49. Dans la fenêtre contextuelle Stop instance, cliquez sur Stop.
Votre instance effectue un arrêt normal, puis cesse de s'exécuter.
50. Attendez que Instance State affiche Stopped.
51. Cochez la case en regard de votre Web-Server. Dans la liste déroulante Actions ,
sélectionnez Instance settings Change instance type, puis configurez l'option suivante :
• Instance type : sélectionnez t2.nano.
52. Cliquez sur Apply.
53. En regard de votre Web-Server, cochez la case .
54. Dans la liste déroulante Instance state, choisissez Start instance.
# Test de la protection contre la résiliation
55. Cochez la case en regard de votre instance de Web-Server. Dans la liste déroulante
Instance state, choisissez Terminate instance.
56. Cliquez sur Terminate pour voir ce qui se passe si vous essayez de résilier l'instance.
Si vous souhaitez réellement résilier l'instance, vous devez désactiver la protection contre la
résiliation.
57. Dans la liste déroulante Actions, choisissez Instance settings, puis Change termination
protection.
58. La case Enable sera cochée. Décochez-la pour désactiver.
59. Cliquez sur Save.
60. Maintenant, essayez à nouveau de résilier l'instance. Dans la liste déroulante Instance
state, choisissez Terminate instance.
61. L'état de l'instance passe alors à Résilié. Cliquez sur Terminate
```
