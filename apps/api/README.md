# API — Vehicle Tracking Platform

Backend de la plateforme de suivi GPS de véhicules, développé avec NestJS et TypeScript.

## État actuel

L'API constitue le socle applicatif du projet. Elle expose actuellement un endpoint de santé permettant de vérifier que le serveur HTTP répond correctement.

| Méthode | Endpoint  | Description              |
| ------- | --------- | ------------------------ |
| `GET`   | `/health` | Vérifie que l'API répond |

Réponse attendue :

```json
{
  "status": "ok"
}
```

**Limites actuelles :**

- L'API ne se connecte pas encore à PostgreSQL.
- L'endpoint `/health` ne vérifie pas l'état de la base de données.
- La réception et la normalisation des positions GPS ne sont pas encore implémentées.
- Aucune interface d'administration ou de suivi des véhicules n'est exposée par cette API à ce stade.

## Prérequis

- Node.js dans la version définie par le fichier `.nvmrc` à la racine du dépôt.
- npm fourni avec l'installation Node.js retenue.
- Git pour récupérer et gérer le code source.

Pour l'installation complète de l'environnement, notamment PostgreSQL/PostGIS et Docker Compose, consultez le [README principal](../../README.md).

## Installation

Depuis la racine du dépôt :

```bash
cd apps/api
npm ci
```

La commande `npm ci` installe les dépendances à partir du fichier `package-lock.json` et convient à une installation reproductible.

## Démarrage

### Mode développement

```bash
npm run start:dev
```

Le serveur utilise le port `3000` par défaut avec la configuration actuelle de NestJS.

Vérifiez le fonctionnement de l'API :

```bash
curl http://localhost:3000/health
```

Résultat attendu :

```json
{ "status": "ok" }
```

Le serveur peut être arrêté avec `Ctrl+C`.

### Autres modes

```bash
# Démarrage standard
npm run start

# Mode débogage
npm run start:debug

# Compilation
npm run build

# Exécution du build
npm run start:prod
```

Le mode production nécessite une compilation préalable. Les paramètres de déploiement en production restent à définir dans le cadre d'un lot dédié.

## Tests et contrôles qualité

Exécutez les commandes suivantes depuis `apps/api`.

### Lint

```bash
npm run lint
```

Analyse le code TypeScript de `src/` et `test/` avec Oxlint.

### Tests unitaires

```bash
npm test -- --runInBand
```

Exécute les tests unitaires avec Jest, en série.

### Tests de bout en bout

```bash
npm run test:e2e -- --runInBand
```

Exécute les tests E2E configurés dans `test/jest-e2e.json`. Le test actuel vérifie que `GET /health` renvoie HTTP 200 et le statut attendu.

### Build

```bash
npm run build
```

Compile l'application NestJS.

### Couverture des tests

```bash
npm run test:cov
```

Génère le rapport de couverture configuré avec Jest.

### Ordre recommandé de vérification

```bash
npm ci
npm run lint
npm test -- --runInBand
npm run build
npm run test:e2e -- --runInBand
```

Ces contrôles sont également exécutés par le workflow GitHub Actions défini dans `.github/workflows/ci.yml`.

## Organisation du code

```text
apps/api/
├── src/
│   ├── app.module.ts
│   ├── app.controller.ts
│   ├── app.service.ts
│   └── main.ts
├── test/
│   ├── app.e2e-spec.ts
│   └── jest-e2e.json
├── package.json
├── package-lock.json
├── nest-cli.json
├── tsconfig.json
└── tsconfig.build.json
```

### Responsabilités principales

- **`app.module.ts`** : module racine qui assemble les contrôleurs et les fournisseurs de services.
- **`app.controller.ts`** : expose les routes HTTP. Le contrôleur actuel déclare `GET /health`.
- **`app.service.ts`** : contient la logique associée à l'endpoint de santé.
- **`main.ts`** : point d'entrée de l'application NestJS.
- **`test/app.e2e-spec.ts`** : vérifie le comportement HTTP de l'endpoint de santé.

La structure évoluera avec l'ajout des modules métier, de la persistance et de l'ingestion GPS.

## Architecture cible

Le backend doit progressivement prendre en charge :

1. La gestion des organisations, utilisateurs, flottes et véhicules.
2. L'enregistrement des appareils GPS et de leurs identifiants.
3. La réception des positions provenant de l'application mobile et des traceurs physiques.
4. La validation et la normalisation des données GPS.
5. La persistance des positions avec PostgreSQL/PostGIS.
6. La consultation et la diffusion des positions aux clients autorisés.

Ces éléments décrivent la cible du projet et non des fonctionnalités déjà disponibles.

## Conventions de développement

- Respecter la version Node.js définie à la racine.
- Utiliser `npm ci` pour les installations reproductibles.
- Ajouter des tests aux nouveaux comportements métier.
- Vérifier le lint, les tests et le build avant de valider une modification.
- Ne jamais committer de secrets, de jetons ou de données de localisation personnelles.
- Documenter les décisions d'architecture importantes dans les ADR du dépôt.

## Documentation complémentaire

- [README principal et environnement local](../../README.md)
- [LOT 01 — Installation locale de PostgreSQL et PostGIS](../../docs/learning/LOT-01-DATABASE-SETUP.md)
- [Cadrage technique](../../docs/architecture/TECHNICAL_SCOPE.md)
- [ADR-0001 — Développer notre propre moteur GPS](../../docs/architecture/adr/ADR-0001-OWN-GPS-ENGINE.md)
- [Feuille de route](../../docs/ROADMAP.md)

---

Ce document décrit l'état connu de l'API. Il doit être actualisé lorsque de nouveaux endpoints, modules ou mécanismes de persistance sont implémentés et vérifiés.
