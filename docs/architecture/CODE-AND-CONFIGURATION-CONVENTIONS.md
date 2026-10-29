# Conventions de code et de configuration

## 1. Objectif

Ce document définit les conventions de base applicables au code et à la
configuration de Vehicle Tracking Platform. Il doit évoluer avec les besoins
réels du projet, sans anticiper inutilement les fonctionnalités futures.

## 2. Nommage et organisation du code

### TypeScript et NestJS

- Utiliser `PascalCase` pour les classes, les interfaces et les types.
- Utiliser `camelCase` pour les variables, les propriétés et les méthodes.
- Utiliser les suffixes NestJS adaptés au rôle du fichier :
  `.controller.ts`, `.service.ts`, `.module.ts` et `.spec.ts`.
- Nommer les tests de bout en bout avec le suffixe `.e2e-spec.ts`.
- Conserver les noms de fichiers en minuscules, avec des mots séparés par
  des points selon les conventions NestJS.
- Utiliser l'injection de dépendances NestJS plutôt que d'instancier
  manuellement les services applicatifs dans les contrôleurs.
- Garder les contrôleurs centrés sur le traitement HTTP et déléguer la logique
  applicative aux services ou aux composants appropriés.

### Qualité et formatage

- Utiliser Prettier pour le formatage du code TypeScript.
- Conserver la configuration actuelle : guillemets simples et virgules finales.
- Utiliser Oxlint pour l'analyse statique, avec les règles définies dans
  `apps/api/.oxlintrc.json`.
- Exécuter les vérifications de lint, de formatage, de compilation et de tests
  avant de considérer un changement comme validé.

## 3. Configuration par environnement

- Placer les paramètres variables dans des variables d'environnement plutôt
  que de les coder en dur dans la logique applicative.
- Réserver `.env` à la configuration locale et ne jamais le versionner.
- Maintenir `.env.example` comme modèle partageable, avec des valeurs
  d'exemple qui ne sont pas des secrets réels.
- Ne jamais committer de mots de passe, jetons, clés privées ou identifiants
  sensibles.
- Documenter les nouvelles variables d'environnement dans le modèle adapté
  et dans la documentation du composant concerné.
- Valider explicitement les variables obligatoires au démarrage du composant
  qui les utilise.
- Fournir des valeurs par défaut uniquement lorsqu'elles sont sûres et
  pertinentes.

## 4. Configuration actuelle

- Le fichier `.nvmrc` fixe la version Node.js de référence du projet.
- `infra/compose.yaml` utilise les variables `POSTGRES_DB`,
  `POSTGRES_USER` et `POSTGRES_PASSWORD` pour configurer PostgreSQL.
- `POSTGRES_PORT` permet de configurer le port local exposé, avec une valeur
  par défaut définie dans Compose.
- L'API utilise actuellement la variable `PORT`, avec `3000` comme valeur
  par défaut.
- La configuration PostgreSQL de l'API elle-même n'est pas encore définie :
  elle devra être ajoutée lorsque l'intégration applicative à la base sera
  implémentée.

## 5. Évolution des conventions

Toute nouvelle convention doit répondre à un besoin identifié, rester cohérente
avec l'architecture et être accompagnée des modifications de configuration
ou des contrôles automatisés nécessaires.
