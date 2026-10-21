# Feuille de route

L'ordre ci-dessous est indicatif. Chaque lot doit satisfaire ses critères d'acceptation avant le lot suivant. Les lots peuvent être réordonnés si une dépendance technique le justifie, en documentant la décision.

| Lot | Objectif | Livrable principal | Dépend de |
|---|---|---|---|
| LOT 00 | Cadrage et documentation | Vision, architecture, ADR, plan d'exécution | Aucun |
| LOT 01 | Initialisation technique | Monorepo, API minimale, configuration, migrations PostGIS, CI | LOT 00 |
| LOT 02 | Domaine et persistance | Organisations, véhicules, appareils, positions, règles métier testées | LOT 01 |
| LOT 03 | Ingestion HTTP de test | Contrat d'ingestion, validation, idempotence, persistance | LOT 02 |
| LOT 04 | API de consultation | Authentification, autorisation, véhicules et dernière position | LOT 02–03 |
| LOT 05 | Interface cartographique | Carte, liste des véhicules, statut de fraîcheur des positions | LOT 04 |
| LOT 06 | Diffusion temps réel | WebSocket autorisé et mise à jour de la carte | LOT 03–05 |
| LOT 07 | Historique et trajets | Requêtes géospatiales, filtres temporels, restitution des trajets | LOT 03–04 |
| LOT 08 | Premier protocole matériel | Adaptateur pour un modèle et un protocole choisis | LOT 03 |
| LOT 09 | Application mobile | Collecte GPS, permissions, envoi sécurisé, gestion hors ligne | LOT 03–04 |
| LOT 10 | Géorepérage et alertes | Zones, événements d'entrée/sortie, règles et notifications | LOT 06–07 |
| LOT 11 | Durcissement et déploiement | Observabilité, sauvegardes, sécurité, charge et documentation de déploiement | Lots précédents |

## Règles de livraison

- Chaque lot dispose d'un fichier `docs/learning/LOT-XX-*.md`.
- Les tâches sont exécutées dans l'ordre et cochées uniquement après vérification.
- Chaque changement cohérent est livré par un commit atomique.
- Les résultats des tests, décisions et écarts sont consignés dans le fichier du lot.
- Un lot est terminé uniquement lorsque tous ses critères obligatoires sont satisfaits.
