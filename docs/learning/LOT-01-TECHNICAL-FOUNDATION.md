# LOT 01 — Initialisation technique

- **Statut :** En cours
- **Objectif :** préparer un environnement de développement reproductible et un socle applicatif vérifiable.
- **Prérequis :** LOT 00 validé ; Node.js 24.22.1 ; npm ; Git ; Docker et Docker Compose opérationnels.
- **Livrable :** structure du projet, API minimale, configuration PostgreSQL/PostGIS, contrôles automatisés et procédure de démarrage.

## Objectifs pédagogiques

À la fin du lot, être capable d'expliquer :

- le rôle de Node.js, npm et NestJS ;
- l'organisation d'un backend modulaire ;
- la différence entre configuration et code applicatif ;
- le rôle de PostgreSQL et de PostGIS ;
- le fonctionnement de Docker Compose et des volumes ;
- la différence entre un contrôle de santé applicatif et un contrôle de disponibilité de la base ;
- le rôle des tests automatisés et de l'intégration continue.

## Étapes de travail

### Étape 01.1 — Stabiliser l'environnement

- [x] Fixer Node.js avec `.nvmrc`.
- [x] Documenter les prérequis de développement dans `README.md`.
- [x] Vérifier les versions de Node.js, npm, Git, Docker et Docker Compose.
- [x] Vérifier le fonctionnement du moteur Docker avec `hello-world`.

**Vérification :** les outils sont accessibles depuis WSL 2 et les commandes de contrôle fonctionnent.

### Étape 01.2 — Définir l'organisation du dépôt

- [x] Choisir une structure de dépôt adaptée aux applications prévues.
- [x] Définir l'emplacement de l'API, du futur frontend et de l'application mobile.
- [ ] Définir les règles de nommage et de configuration.
- [x] Documenter les décisions structurantes dans `ADR-0002-REPOSITORY-STRUCTURE.md`.

**Décision enregistrée :** la structure initiale prévoit `apps/api/` pour l'API et `infra/` pour l'infrastructure. Les répertoires web, mobile et packages seront créés lorsqu'ils seront nécessaires.

**Vérification :** structure et responsabilités documentées ; les conventions détaillées de nommage et de configuration restent à définir.

### Étape 01.3 — Initialiser l'API

- [x] Initialiser une application NestJS avec TypeScript dans `apps/api/`.
- [x] Vérifier les scripts de développement, de compilation et de test générés.
- [x] Ajouter l'endpoint `GET /health`, qui retourne `{"status":"ok"}`.
- [x] Ajouter un test unitaire et un test E2E pour cet endpoint.
- [x] Vérifier la compilation, le lint et les tests.
- [x] Vérifier le démarrage réel et la réponse HTTP de l'endpoint.

**Vérification :** compilation réussie, lint sans avertissement ni erreur, test unitaire et test E2E réussis ; requête HTTP réelle à `GET /health` validée avec le statut `200 OK`.

**Limite actuelle :** ce contrôle confirme que l'API répond ; il ne vérifie pas encore la disponibilité de PostgreSQL.

### Étape 01.4 — Préparer PostgreSQL et PostGIS

- [x] Choisir et fixer une image PostgreSQL compatible avec PostGIS.
- [x] Définir les services Docker Compose nécessaires au développement.
- [x] Configurer les identifiants par variables d'environnement et fournir un exemple sans secret.
- [x] Définir un volume persistant et un contrôle de santé de la base.
- [x] Vérifier que l'extension PostGIS est disponible.
- [x] Documenter les commandes de démarrage et d'arrêt.

**Vérification :** la base démarre, conserve ses données lors d'un redémarrage et permet de vérifier la disponibilité de PostGIS.

### Étape 01.5 — Ajouter les contrôles automatisés

- [ ] Définir le formatage et le lint.
- [x] Configurer les tests unitaires et d'intégration appropriés au socle initial.
- [x] Ajouter une intégration continue pour les vérifications essentielles.
- [ ] Vérifier que les erreurs de compilation ou les tests en échec bloquent la validation.

**Vérification :** les contrôles peuvent être exécutés avec des commandes documentées et échouent lorsqu'une régression est introduite.

### Étape 01.6 — Vérifier la reproductibilité

- [ ] Documenter l'installation des dépendances.
- [ ] Documenter le démarrage et l'arrêt de tous les services.
- [ ] Vérifier la procédure depuis un environnement propre.
- [ ] Vérifier l'absence de secrets et de données GPS personnelles dans les fichiers suivis.
- [ ] Compléter le compte rendu de fin de lot.

**Vérification :** une personne disposant des prérequis peut démarrer le socle en suivant uniquement la documentation du dépôt.

## Critères d'acceptation

- [ ] Les versions et prérequis sont documentés.
- [ ] La structure du dépôt est cohérente avec la feuille de route.
- [ ] L'API démarre et son endpoint de santé répond.
- [ ] Les tests de l'API passent.
- [ ] PostgreSQL démarre avec Docker Compose.
- [ ] PostGIS est installé et vérifiable.
- [ ] La configuration ne contient aucun secret commité.
- [ ] Les contrôles de formatage, de lint et de compilation passent.
- [ ] L'intégration continue exécute les vérifications prévues.
- [ ] La procédure de démarrage a été testée.
- [ ] Les changements sont enregistrés dans des commits atomiques.

## Décisions à enregistrer

Les décisions suivantes doivent être prises avant ou pendant l'étape concernée :

- organisation du dépôt ;
- version et configuration de NestJS ;
- version de PostgreSQL et image PostGIS ;
- stratégie d'accès à la base et outil de migration ;
- stratégie de configuration et de gestion des secrets ;
- commandes de test et règles de validation continue.

Les choix retenus doivent être documentés dans les fichiers du projet et, si leur impact architectural le justifie, dans un ADR.

## Compte rendu de fin de lot

- **Date de validation :** à renseigner après validation finale
- **Résultat :** en attente de validation
- **Vérifications effectuées :** à compléter au fil du lot
- **Écarts ou décisions reportées :** à compléter
- **Commit(s) de référence :** à compléter après validation

## Définition de terminé

Le LOT 01 est terminé uniquement lorsque tous les critères obligatoires sont vérifiés, que la procédure de démarrage est reproductible et que le compte rendu de fin de lot est renseigné.
