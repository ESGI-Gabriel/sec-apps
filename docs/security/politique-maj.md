# Politique de mise à jour des dépendances — exercice Juice Shop

## Cadence

- Correctifs : revue hebdomadaire, après CI et tests de non-régression.
- Versions mineures : revue mensuelle, avec test du parcours d'achat.
- Versions majeures : planification par lot, analyse de compatibilité et validation métier.
- Vulnérabilité critique activement exploitée : qualification immédiate de l'exposition, correctif ou mesure compensatoire prioritaire.

## Qui décide

| Changement | Propose | Valide | Trace |
|---|---|---|---|
| Patch/minor | Responsable technique | Revue de code | PR, résultat CI, SBOM |
| Majeur | Responsable technique | Responsable métier | ADR ou ticket avec essais de compatibilité |
| Acceptation de risque | Responsable technique | Responsable métier | Justification, échéance et date de revue |

## Lien MCO–MCS

Le MCO vérifie que le service reste fonctionnel. Le MCS ajoute la veille des avis, la mise à jour des dépendances et la vérification des images livrées. Les deux sont planifiés dans la maintenance régulière ; un service qui démarre n'est pas nécessairement corrigé.

## Cas étudié

Le support S4 documente la chaîne `juice-shop -> pdfkit@0.11.0 -> crypto-js@3.3.0`, liée à `GHSA-xwcq-pm8m-c4vf` / `CVE-2023-46233`. Dans son relevé, la correction proposée passe par `pdfkit@0.20.2`, une montée majeure. Décision d'exercice : ouvrir une tâche de migration, tester la génération des PDF et choisir une mesure compensatoire si la migration ne passe pas. Le propriétaire métier valide tout report avec une date de revue. Les versions et avis doivent être relus sur le rapport courant avant de décider en production.
