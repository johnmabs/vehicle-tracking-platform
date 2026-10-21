# Cadrage technique

## 1. Principes d'architecture

- **Moteur GPS maison** : le serveur reçoit les messages des appareils et implémente lui-même les adaptateurs, la validation et la normalisation.
- **Monolithe modulaire au départ** : ne pas distribuer prématurément les composants en microservices.
- **Frontières explicites** entre protocoles d'entrée, domaine, persistance et diffusion temps réel.
- **Évolution incrémentale** : commencer par une ingestion HTTP de test, puis ajouter des protocoles de traceurs et les transports nécessaires.
- **Testabilité** : les règles métier ne dépendent ni du réseau, ni du framework HTTP, ni de la base de données.
- **Observabilité sans fuite de données** : métriques et journaux structurés, sans secrets ni coordonnées précises inutiles.

## 2. Stack proposée

| Domaine | Choix initial | Raison |
|---|---|---|
| Langage principal | TypeScript | Cohérence entre API et interfaces, typage et outillage |
| Backend | NestJS | Modules, injection de dépendances, testabilité |
| Base de données | PostgreSQL | Persistance relationnelle fiable |
| Données géospatiales | PostGIS | Types et requêtes géographiques |
| Accès aux données | SQL explicite ou couche compatible avec les besoins PostGIS | Contrôle des requêtes géospatiales et migrations |
| Interface web | React + TypeScript | Tableau de bord |
| Carte | Leaflet | Intégration cartographique web |
| Application mobile | React Native, à confirmer au lot mobile | Réutilisation de TypeScript ; contraintes de localisation arrière-plan à tester sur appareil réel |
| Temps réel | WebSocket | Diffusion des nouvelles positions aux clients autorisés |
| Tests | Jest ou outillage compatible NestJS, tests d'intégration et E2E | Vérifier domaine, API et parcours |
| Environnement local | Docker Compose | Base de données et services reproductibles |
| CI | GitHub Actions | Vérifications automatiques sur les pull requests |

La stack est une proposition de départ. Les dépendances exactes et versions seront figées lors du LOT 01, avec vérification de compatibilité.

## 3. Vue logique

```text
Application mobile ── HTTPS ─────────────┐
                                         │
Traceur GPS ── TCP/UDP ou protocole ──> Ingestion / adaptateurs
                                         │
                                         v
                              Décodage et validation
                                         │
                                         v
                               Modèle de position normalisé
                                         │
                              ┌──────────┴──────────┐
                              v                     v
                      PostgreSQL/PostGIS      Événements métier
                              │                     │
                              v                     v
                       API de consultation     WebSocket autorisé
                              │                     │
                              └──────────┬──────────┘
                                         v
                                Interface web / carte
```

Cette vue représente des responsabilités logiques, pas nécessairement des processus séparés.

## 4. Architecture interne du moteur GPS

### 4.1 Adaptateurs de protocole

Chaque famille de traceurs doit disposer d'un adaptateur isolé qui sait :
- recevoir les octets ou messages selon le transport ;
- décoder le format du constructeur ;
- extraire l'identifiant d'appareil, les coordonnées, l'heure et les attributs disponibles ;
- traiter les acquittements exigés par le protocole ;
- produire un message normalisé ou une erreur de décodage contrôlée.

Les protocoles TCP, UDP et HTTP ont des contraintes différentes. Ils ne doivent pas être artificiellement regroupés dans un seul gestionnaire réseau. Le support d'un protocole physique sera ajouté seulement après le choix d'un modèle de traceur et l'étude de sa documentation.

### 4.2 Modèle de position normalisé

Le modèle devra au minimum représenter :
- l'identifiant interne de l'appareil ;
- l'identifiant du véhicule associé, lorsque disponible ;
- latitude et longitude validées ;
- l'heure de mesure de la source ;
- l'heure de réception par le serveur ;
- la précision GPS et la vitesse, si fournies ;
- le cap, l'altitude et les attributs complémentaires, s'ils sont disponibles ;
- des métadonnées de provenance utiles au diagnostic, sans secrets.

Les unités seront explicites et uniformes. Les coordonnées doivent respecter les bornes géographiques. Le traitement des dates invalides, valeurs manquantes, doublons et messages hors ordre sera défini par des tests.

### 4.3 Pipeline de traitement

1. Recevoir un message.
2. Identifier et authentifier l'appareil selon le protocole.
3. Décoder le message.
4. Normaliser et valider les données.
5. Appliquer les règles d'idempotence et d'ordre temporel.
6. Enregistrer la position et mettre à jour la dernière position connue de manière cohérente.
7. Publier un événement interne exploitable pour le temps réel et les alertes.
8. Mesurer les erreurs, délais et volumes sans journaliser de secrets.

La stratégie de mise à jour atomique, de déduplication et de gestion des erreurs sera détaillée au moment de l'implémentation.

## 5. Modèle de données initial

Entités pressenties :
- `Organization`
- `User`
- `Membership` ou relation utilisateur-organisation
- `Vehicle`
- `Device`
- `Position`
- `Geofence` (lot ultérieur)
- `Alert` (lot ultérieur)

Relations principales :
- une organisation possède plusieurs véhicules ;
- un véhicule peut avoir un ou plusieurs appareils au cours du temps, avec une règle explicite sur l'association active ;
- un appareil émet plusieurs positions ;
- chaque requête d'un utilisateur est autorisée dans le contexte de son organisation.

La cardinalité exacte, les contraintes d'unicité et les index seront validés dans le LOT 01.

## 6. API et temps réel

- API REST pour la gestion des organisations, véhicules, appareils et la consultation des positions.
- Point d'entrée d'ingestion HTTP pour le développement et les sources compatibles.
- Serveurs TCP/UDP séparés ou adaptateurs dédiés pour les protocoles matériels qui l'exigent.
- WebSocket pour diffuser les mises à jour aux clients autorisés.
- Authentification et autorisation distinctes pour les utilisateurs humains et les appareils.
- Versionnement des contrats d'API et documentation OpenAPI lorsque l'API sera créée.

## 7. Découpage du dépôt envisagé

```text
vehicle-tracking-platform/
├── apps/
│   ├── api/                 # Backend et moteur d'ingestion initial
│   ├── web/                 # Tableau de bord
│   └── mobile/              # Application mobile, dans un lot ultérieur
├── packages/
│   └── contracts/           # Contrats partagés réellement stables
├── docs/
│   ├── architecture/
│   │   └── adr/
│   ├── learning/
│   └── product/
├── infra/
├── README.md
└── ...
```

Ce découpage est une cible, pas une obligation de créer tous les dossiers dès le LOT 00. Ne créer que les éléments utiles à chaque étape.

## 8. Risques techniques

| Risque | Réponse prévue |
|---|---|
| Protocoles de traceurs propriétaires ou mal documentés | Sélectionner un modèle précis, isoler l'adaptateur, utiliser des captures synthétiques et des tests de protocole |
| Réseau intermittent | Accepter les données retardées et distinguer heure de mesure et réception |
| Positions dupliquées ou hors ordre | Définir une clé de déduplication et une règle temporelle testée |
| Volume important de positions | Indexer, mesurer, puis envisager partitionnement et traitements asynchrones sur données réelles de charge |
| Fuite de localisation entre clients | Tests d'isolation multi-organisation et autorisation côté serveur |
| Application mobile suspendue par le système | Tester les permissions et le fonctionnement en arrière-plan sur appareils réels |
| Coût des cartes et de l'hébergement | Vérifier les conditions des fournisseurs et mesurer les besoins avant production |

## 9. Décisions reportées

- Premier modèle de traceur physique à prendre en charge.
- Premier protocole matériel.
- Choix précis de l'ORM ou de l'accès SQL.
- Politique de partitionnement de l'historique.
- Fournisseur de tuiles cartographiques pour la production.
- Politique détaillée de rétention.
- Choix de la stratégie de déploiement en production.

Ces décisions ne bloquent pas le cadrage documentaire, mais devront être résolues avant les lots qui en dépendent.
