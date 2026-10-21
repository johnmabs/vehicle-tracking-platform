# Vehicle Tracking Platform

Plateforme de suivi de véhicules conçue comme projet de portfolio et d'apprentissage en ingénierie logicielle.

## Vision

Construire un système de tracking GPS de bout en bout, capable de recevoir des positions depuis :
- une application mobile utilisant le GPS du téléphone ;
- des traceurs GPS physiques communiquant avec le serveur via des protocoles documentés.

Le projet développe son propre moteur GPS. Il ne dépend pas d'un serveur de tracking tiers pour recevoir et normaliser les positions.

## Objectifs pédagogiques

- Comprendre l'ingestion de données GPS et les protocoles réseau.
- Concevoir un backend modulaire, testable et sécurisé.
- Modéliser et interroger des données géospatiales.
- Diffuser les positions en temps réel.
- Construire une interface cartographique et une application mobile.
- Automatiser les tests, le déploiement et la documentation.

## État du projet

**Phase actuelle : LOT 00 — Cadrage et fondations documentaires.**

Aucun moteur GPS ni service applicatif n'est encore considéré comme implémenté. Le développement commence après validation des critères d'acceptation du LOT 00.

## Documentation

- [Périmètre produit](docs/product/PRODUCT_SCOPE.md)
- [Cadrage technique](docs/architecture/TECHNICAL_SCOPE.md)
- [ADR-0001 — Développer notre moteur GPS](docs/architecture/adr/ADR-0001-OWN-GPS-ENGINE.md)
- [Feuille de route](docs/ROADMAP.md)
- [LOT 00 — Fondations](docs/learning/LOT-00-FOUNDATION.md)

## Principes de travail

1. Un lot à la fois ; aucun lot n'est déclaré terminé sans vérification de ses critères d'acceptation.
2. Des commits atomiques, nommés selon Conventional Commits.
3. Des tests automatisés pour les comportements applicatifs dès qu'ils existent.
4. Des décisions d'architecture documentées dans des ADR.
5. Aucune donnée de localisation réelle ou personnelle dans les fixtures ou dépôts publics.
6. Ne jamais exposer de secrets, identifiants de traceurs ou jetons dans les journaux.

## Statut des décisions

Les choix de ce cadrage constituent la proposition initiale. Les décisions importantes seront confirmées ou modifiées explicitement dans les ADR.
