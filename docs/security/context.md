# Contexte de sécurité - fork Juice Shop

## 1. Contexte métier

L'app est un site de commerce électronique permettant à une entreprise de vendre différents produits, notamment des produits alimentaires, des vêtements etc. S'adresse aux clients de la boutique et traite des données sensibles, telles que leurs données personnelles, leurs adresses de livraison et leurs informations de paiement
Une indisponibilité du service empêcherait les clients de passer commande et par conséquent une perte de CA pour le commerçant
Une fuite de données exposerait les clients à une divulgation de leurs informations personnelles et pourrait entraîner pour l’entreprise des sanctions liées au RGPD ainsi qu’une perte de confiance

## 2. Biens essentiels

| # | Bien essentiel | Pourquoi il a de la valeur métier | Biens supports qui le portent |
|---|---|---|---|
| BE1 | Comptes clients | Ils contiennent les données personnelles des clients et permettent leur identification sur le site | Base de données SQLite, modèles Sequelize, serveur Express et jetons JWT |
| BE2 | Commandes et historique d’achats | Ils représentent les ventes réalisées et contribuent directement au chiffre d’affaires | Base de données SQLite, modèles Sequelize et routes Express |
| BE3 | Catalogue de produits | Il présente les produits disponibles à la vente et permet aux clients de passer commande | Base de données SQLite, modèle `Product`, serveur Express et fichiers des images |

## 3. Sources de risque

Des cyberattaquants peuvent chercher à voler les données personnelles ou bancaires des clients afin de les revendre et ils peuvent également modifier les produits ou passer des fausses commandes

## 4. Événements redoutés

| # | Événement redouté (fait + impact) | Bien essentiel touché | Gravité (1 à 4) | Justification de la gravité |
|---|---|---|---|---|
| ER1 |Divulgation des données personnelles et bancaires des clients, entraînant des fraudes et une perte de confiance |Comptes clients |4 |Les données sensibles de tous les clients peuvent être exposées et l’entreprise risque des sanctions liées au RGPD |
| ER2 |Passage de fausses commandes, entraînant des pertes financières pour l’entreprise |Commandes et historique d’achats |4 |L’entreprise peut préparer ou expédier des commandes frauduleuses, ce qui entraîne des coûts et fausse le chiffre d’affaires

## 5. Suivi

https://github.com/ESGI-Gabriel/sec-apps/actions/runs/34238020978