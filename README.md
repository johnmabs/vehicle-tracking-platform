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

**Phase actuelle : LOT 01 — Initialisation technique.**

Le LOT 00 a établi le périmètre produit, les orientations architecturales et la feuille de route. Le LOT 01 vise à préparer un environnement de développement reproductible et un premier socle applicatif.

L'API, le moteur GPS et la persistance applicative restent à implémenter et à vérifier. Les fonctionnalités prévues dans la feuille de route ne doivent pas être considérées comme disponibles tant qu'elles n'ont pas été réalisées et validées.

## Prérequis de développement

L'environnement de référence utilise :

| Outil          | Version de référence                                |
| -------------- | --------------------------------------------------- |
| Node.js        | 24.22.1                                             |
| npm            | Version fournie avec l'installation Node.js retenue |
| Git            | Version récente                                     |
| Docker         | Version compatible avec Docker Compose              |
| Docker Compose | Plugin `docker compose`                             |

Node.js est fixé par le fichier `.nvmrc`. Si `nvm` est installé, active la version du projet avec :

```bash
nvm install
nvm use
```

Vérifie ensuite l'environnement :

```bash
node --version
npm --version
git --version
docker --version
docker compose version
docker info
```

Le fichier `.nvmrc` doit correspondre à la version Node.js de référence documentée ci-dessus.

Sous Windows avec WSL 2, Docker Desktop doit être démarré et l'intégration de la distribution WSL activée.

Les commandes d'installation des dépendances et de lancement de l'application seront ajoutées lorsque le squelette applicatif et ses scripts auront été créés.

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
7. Toute fonctionnalité annoncée comme terminée doit avoir été vérifiée.

## Statut des décisions

Les choix du cadrage initial constituent la proposition de départ. Les décisions importantes seront confirmées ou modifiées explicitement dans les ADR.

Les versions et dépendances réellement utilisées devront rester cohérentes avec les fichiers de configuration et les contrôles automatisés du projet.
