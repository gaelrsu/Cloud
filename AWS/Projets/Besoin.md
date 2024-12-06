# Projet : "Détection et réponse aux menaces dans le Cloud AWS"

## Objectif
Créer une architecture Cloud sécurisée et résiliente, capable de détecter, alerter et répondre aux menaces en utilisant des outils AWS, Terraform pour l’automatisation de l’infrastructure, et GitLab pour le pipeline CI/CD.

---

## Étapes du projet

### 1. Mise en place de l’infrastructure sécurisée avec Terraform
- Déployer un environnement AWS sécurisé :
  - VPC avec sous-réseaux publics/privés.
  - Instances EC2 avec rôles IAM restreints et des groupes de sécurité bien définis.
  - Intégrer un service de base de données RDS (chiffré avec KMS).
  - Activer AWS Config pour surveiller les ressources et s'assurer qu'elles respectent les bonnes pratiques.

---

### 2. Intégration des services de surveillance et d’alerte
- **CloudTrail** :
  - Activer pour collecter les journaux d’activité API.
  - Automatiser l'activation via Terraform.
- **CloudWatch** :
  - Configurer des alarmes pour des activités inhabituelles (ex. tentatives de connexion échouées sur EC2).
- **GuardDuty** :
  - Activer et surveiller les menaces (ex. trafic suspect, exfiltration de données).
- **Wazuh ou OSSEC** :
  - Déployer un agent sur les instances EC2 pour la détection d’intrusions.

---

### 3. Automatisation de la réponse avec des règles Lambda
- Configurer des AWS Lambda déclenchées par des alarmes CloudWatch pour répondre automatiquement :
  - Isoler une instance compromise (par modification automatique de ses groupes de sécurité).
  - Révoquer des clés IAM exposées.
  - Alerter via SNS (ex. e-mails ou SMS).

---

### 4. Sécurisation des pipelines CI/CD avec GitLab
- Créer un pipeline CI/CD sécurisé qui :
  - Analyse le code pour les vulnérabilités (ex. avec Snyk ou Trivy).
  - Effectue des tests de sécurité automatisés (ex. scans de conteneurs).
  - Déploie uniquement si les tests sont réussis.
- Restreindre les accès aux secrets dans les pipelines avec AWS Secrets Manager.

---

### 5. Rapport et tableau de bord de sécurité
- Intégrer AWS QuickSight ou un tableau de bord ELK pour :
  - Visualiser les menaces détectées (logs de GuardDuty, CloudTrail, etc.).
  - Générer des rapports hebdomadaires sur les incidents de sécurité.

---

## Technologies utilisées
- **Cloud** : AWS (VPC, CloudTrail, GuardDuty, CloudWatch, Lambda, SNS).
- **Infrastructure as Code** : Terraform.
- **CI/CD** : GitLab.
- **Surveillance** : Wazuh/OSSEC, AWS Config.
- **Analyse et Reporting** : AWS QuickSight ou ELK Stack.

---

## Livrables
- Code Terraform pour l’infrastructure.
- Pipeline GitLab CI/CD avec intégration des tests de sécurité.
- Tableau de bord pour le suivi des menaces.
- Documentation sur la gestion des incidents et la réponse automatisée.
