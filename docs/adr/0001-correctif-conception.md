# ADR 0001 — Contrôle d'appartenance du panier et configuration HTTP

Date : 2026-10-05. Statut : proposé, validation de la CI et du navigateur en attente.

## Contexte

La ligne STRIDE #1 du threat model décrit un client authentifié lisant un panier tiers par `/rest/basket/:id`. Le middleware JWT authentifie le client mais ne vérifie pas le propriétaire de l'objet. Cela ouvre l'événement redouté ER1 (divulgation) et peut contribuer à ER2 (commande frauduleuse). La revue du serveur montre aussi `cors()` sans restriction et seulement deux protections Helmet ; ZAP vise notamment les en-têtes CSP et CORS.

## Décision

Charger le panier par `id` **et** `UserId` dans la même requête Sequelize. Refuser un utilisateur non authentifié et un panier non possédé. Restreindre CORS à `server.baseUrl`, ajouter CSP, HSTS et `Referrer-Policy` dans Express. Le workflow DAST doit scanner le code reconstruit depuis ce fork.

## Options écartées

- Masquer le panier tiers dans Angular : une requête HTTP fabriquée contourne l'interface.
- Comparer uniquement `req.params.id` au `bid` du jeton : cela couple l'autorisation à une donnée potentiellement ancienne et ne vérifie pas l'appartenance effective en base.
- Ignorer toutes les alertes ZAP : cela transformerait un job vert en absence de contrôle.

## Conséquences et risque résiduel

Le challenge pédagogique d'accès au panier ne pourra plus être résolu sur ce fork durci ; le Refactoring Safety Net et les tests du challenge doivent être évalués explicitement. La CSP garde `'unsafe-inline'` dans `script-src` à cause du script inline et du chargement de styles produits par le frontend actuel. Un contenu injecté pourrait donc encore utiliser certaines formes de script inline : risque résiduel accepté pour cet exercice le 2026-10-05, à revoir après suppression du code inline et validation navigateur. Les alertes ZAP ignorées dans `.zap/curso-rules.tsv` restent des décisions à réviser sur le rapport réel, pas une preuve d'absence de défaut.
