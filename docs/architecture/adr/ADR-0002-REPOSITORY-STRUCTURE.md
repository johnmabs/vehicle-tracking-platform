# ADR-0002 — Structure du dépôt

- **Statut :** Acceptée
- **Date :** 2026-10-21

## Contexte

La plateforme doit accueillir progressivement une API de tracking GPS,
une interface web cartographique et une application mobile.

La structure initiale doit rester simple, tout en permettant ces évolutions
sans réorganisation majeure du dépôt.

## Décision

Adopter une structure organisée autour des applications et de l'infrastructure :

- `apps/api/` : API backend NestJS et ingestion des positions GPS.
- `infra/` : configuration de l'infrastructure locale, notamment Docker Compose.
- `docs/` : documentation produit, architecture et apprentissage.

Les répertoires `apps/web/`, `apps/mobile/` et `packages/` seront créés
lorsqu'un besoin concret le justifiera.

Chaque application pourra disposer de ses propres dépendances et scripts.
L'éventuelle orchestration commune au niveau racine sera décidée si elle
devient utile.

## Conséquences

### Avantages

- Séparation claire entre applications et infrastructure.
- Démarrage limité au périmètre nécessaire pour le MVP.
- Possibilité d'ajouter progressivement les interfaces web et mobile.
- Documentation conservée à son emplacement actuel.

### Contraintes

- Les commandes de développement de l'API seront initialement exécutées
  depuis `apps/api/`.
- Les conventions de partage de code et d'orchestration entre applications
  devront être définies lorsque plusieurs applications existeront.

## Hors périmètre

Cette décision ne fixe pas encore :

- le choix d'un gestionnaire de monorepo ;
- le partage de types entre applications ;
- l'organisation détaillée des modules NestJS ;
- la stratégie de déploiement en production.
