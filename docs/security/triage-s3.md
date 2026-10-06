# Triage S3 — Juice Shop 20.1.1

Ces verdicts sont fondés sur la lecture du code de ce clone. Les positions de l'annexe S3 servent de repère ; le SARIF du run doit confirmer les lignes exactes avant remise.

| Fichier:ligne | Règle | CWE | Sévérité | Verdict | Exigence STRIDE | Action |
|---|---|---|---|---|---|---|
| `routes/login.ts:34` | `express-sequelize-injection` | CWE-89 | élevée | Vrai positif à corriger | #4 (divulgation des comptes) | Paramétrer la requête ; traiter séparément le hash MD5, qui est un second défaut. |
| `lib/insecurity.ts:150` (cause : clé déclarée en tête du fichier) | `juiceshop-hardcoded-private-key` | CWE-798 | élevée | Vrai positif à corriger | #2 | Sortir la clé du dépôt, injecter à l'exécution et prévoir sa rotation. La propagation de constante explique la ligne signalée. |
| `lib/insecurity.ts:41` | `juiceshop-weak-hash-md5` | CWE-327 | élevée | Vrai positif à corriger | #4 | Migrer vers Argon2id avec une stratégie compatible avec les comptes existants. |
| `routes/login.ts:64` | `generic-api-key` (Gitleaks) | CWE-798 | à qualifier | Risque accepté dans le laboratoire, sous réserve du contrôle de la valeur | aucune exigence dédiée | Lire la valeur décodée et confirmer qu'il s'agit d'un code de challenge sans usage externe ; si c'est un identifiant réel, le révoquer et changer ce verdict. Ne pas exclure par défaut. |

Le risque accepté concerne uniquement une éventuelle valeur de démonstration du jeu. Aucune empreinte Gitleaks n'est ignorée automatiquement : l'exclusion ne doit être ajoutée qu'après vérification du résultat réel et avec justification datée.
