# LOT 01 — Installation locale de PostgreSQL et PostGIS

- **Statut :** En cours
- **Objectif :** disposer d'une base de données locale reproductible pour le développement.
- **Prérequis :** Docker et Docker Compose opérationnels.
- **Périmètre :** environnement de développement local, pas production.

## 1. Versions vérifiées

| Élément                | Valeur constatée         |
| ---------------------- | ------------------------ |
| Image Docker           | `postgis/postgis:17-3.6` |
| PostgreSQL             | `17.11`                  |
| PostGIS                | `3.6.4`                  |
| Base                   | `vehicle_tracking`       |
| Utilisateur applicatif | `tracking_app`           |
| Port local par défaut  | `5432`                   |
| Volume logique Compose | `postgres_data`          |
| Volume Docker constaté | `infra_postgres_data`    |

Les versions indiquées correspondent aux vérifications réalisées pendant le
LOT 01. Le tag `17-3.6` peut recevoir des mises à jour : il ne garantit pas
une image strictement identique dans le temps.

## 2. Fichiers de configuration

- `infra/compose.yaml` : définition du service de base de données.
- `.env.example` : exemple de configuration sans secret réel.
- `.env` : configuration locale, à ne jamais commiter.
- `postgres_data` : nom logique du volume déclaré dans Compose.

Docker Compose préfixe normalement le volume par le nom du projet. Dans
l'environnement vérifié, son nom effectif est `infra_postgres_data`.

Le port est publié sur `127.0.0.1` uniquement. Si le port local `5432` est
déjà utilisé, modifier `POSTGRES_PORT` dans `.env`.

## 3. Préparer l'environnement

Créer `.env` uniquement s'il n'existe pas déjà :

```bash
cp .env.example .env
```

Remplacer ensuite le mot de passe d'exemple par un mot de passe local.
Ne jamais afficher ni commiter `.env`.

## 4. Démarrer la base

Depuis la racine du dépôt :

```bash
docker compose --env-file .env -f infra/compose.yaml up -d db
docker compose --env-file .env -f infra/compose.yaml ps
```

Attendre que le service `db` soit en état `healthy` avant d'effectuer les
vérifications suivantes.

## 5. Vérifier PostgreSQL et PostGIS

Afficher les versions :

```bash
docker compose --env-file .env -f infra/compose.yaml exec -T db \
  psql -U tracking_app -d vehicle_tracking \
  -c "SELECT version(), PostGIS_Full_Version();"
```

Vérifier les extensions installées :

```bash
docker compose --env-file .env -f infra/compose.yaml exec -T db \
  psql -U tracking_app -d vehicle_tracking \
  -c "SELECT extname, extversion FROM pg_extension ORDER BY extname;"
```

Vérifier une géométrie :

```bash
docker compose --env-file .env -f infra/compose.yaml exec -T db \
  psql -U tracking_app -d vehicle_tracking \
  -c "SELECT ST_AsText(ST_SetSRID(ST_MakePoint(15.2663, -4.7692), 4326)) AS position_test;"
```

Résultat attendu :

```text
POINT(15.2663 -4.7692)
```

Dans PostGIS, `ST_MakePoint(longitude, latitude)` reçoit la longitude en
premier. Le SRID 4326 correspond au système géographique WGS 84.

## 6. Vérifier la persistance

La persistance a été vérifiée avec une donnée de test :

```sql
SELECT id, marker FROM lot01_persistence_check;
```

Résultat constaté : une ligne avec le marqueur `persistence-ok`.

Pour une nouvelle vérification, créer une donnée de test distincte ou
réutiliser une donnée de test existante. Ne pas supprimer le volume pour
tester la persistance.

## 7. Arrêter et redémarrer

Arrêter le service sans supprimer les données :

```bash
docker compose --env-file .env -f infra/compose.yaml stop db
docker compose --env-file .env -f infra/compose.yaml start db
docker compose --env-file .env -f infra/compose.yaml ps
```

Un arrêt ou un redémarrage normal conserve le volume. Ne pas utiliser
`docker compose down -v` pour une opération courante : cette commande peut
supprimer les volumes déclarés par le projet.

## 8. Configuration et sécurité

- `.env` doit rester local et être exclu du contrôle de version.
- `.env.example` ne doit contenir aucun mot de passe réel.
- Le port PostgreSQL doit rester lié à `127.0.0.1` pour le développement local.
- Cette configuration n'est pas une configuration de production.
- Toute évolution des versions ou des paramètres doit être documentée.

## 9. État de validation

| Vérification                           | État                                      |
| -------------------------------------- | ----------------------------------------- |
| Configuration Compose                  | Réussie                                   |
| Démarrage de PostgreSQL                | Réussie — conteneur healthy               |
| Versions PostgreSQL et PostGIS         | Réussies                                  |
| Extensions PostGIS                     | Réussies                                  |
| Construction d'une géométrie SRID 4326 | Réussie                                   |
| Persistance des données                | Réussie                                   |
| Installation reproductible (`npm ci`)  | Réussie                                   |
| Lint de l'API                           | Réussi                                    |
| Tests unitaires de l'API               | Réussis                                   |
| Build de l'API                          | Réussi                                    |
| Tests E2E de l'API                      | Réussis                                   |
| Exécution du workflow GitHub Actions    | Réussie — exécution nº 1                  |

Les contrôles locaux listés ci-dessus ont réussi. La première exécution du
workflow GitHub Actions a également réussi sur le commit 065b04d.

Le LOT 01 ne doit être déclaré terminé qu'après vérification de l'ensemble des
livrables, de la procédure documentée et du résultat des contrôles distants.
