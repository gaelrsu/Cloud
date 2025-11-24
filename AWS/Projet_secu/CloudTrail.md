# CloudTrail

### But :
Collecter les événements d’API (audit) pour détecter les actions suspectes et conserver une piste d’audit.

### Étapes (Console)

Ouvrir CloudTrail → Trails → Create trail.
Donner un nom

Check Apply trail to all regions pour capturer les événements multi-régions.

Sous Management events, activer Read/Write events selon le besoin (Read-only ou Read/Write).

Configurer une destination S3 :

Soit, choisir un bucket existant, soit create new S3 bucket (ex. aws-logs-<ton-id>).

Active l’option Enable log file validation si tu veux vérifier l’intégrité des logs.

(Optionnel A FAIRE) Activer l’envoi vers CloudWatch Logs pour faire des alarmes et des metric filters.

Crée le trail.
