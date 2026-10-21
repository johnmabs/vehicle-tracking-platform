# LOT 00 — Cadrage et fondations documentaires

- **Statut :** Terminé
- **Objectif :** établir une base produit et technique commune avant toute implémentation.
- **Prérequis :** aucun
- **Livrable :** documentation cohérente, décisions initiales et procédure de validation du lot.

## Objectifs pédagogiques

À la fin du lot, être capable d'expliquer :

- le problème métier et les utilisateurs visés ;
- la différence entre une source GPS, un protocole, un transport et le moteur de tracking ;
- pourquoi l'heure de mesure diffère de l'heure de réception ;
- pourquoi le moteur est séparé des adaptateurs de protocoles ;
- les principaux risques de sécurité et de vie privée ;
- l'ordre des incréments et leurs dépendances.

## Étapes de travail

### Étape 00.1 — Créer la structure documentaire

- [x] Créer le dépôt Git et la branche de travail initiale.
- [x] Ajouter `README.md`.
- [x] Créer `docs/README.md`.
- [x] Créer les répertoires `docs/product/`, `docs/architecture/`, `docs/architecture/adr/` et `docs/learning/`.
- [x] Ajouter `docs/ROADMAP.md`.

**Vérification :** les fichiers existent, les liens relatifs fonctionnent et aucun fichier généré ou secret n'est commité.

### Étape 00.2 — Définir le produit

- [x] Décrire la vision et les utilisateurs.
- [x] Séparer le MVP, les fonctions ultérieures et le hors-périmètre.
- [x] Définir les principes métier concernant les positions, les appareils et les organisations.
- [x] Documenter les contraintes de sécurité et de vie privée.

**Vérification :** chaque fonctionnalité du MVP est formulée de manière vérifiable et les fonctions hors périmètre sont explicites.

### Étape 00.3 — Définir l'architecture cible

- [x] Documenter la stack proposée et les raisons des choix.
- [x] Décrire le flux d'une position depuis la source jusqu'à la carte.
- [x] Définir la frontière entre transport, adaptateur, domaine et persistance.
- [x] Proposer le modèle de données initial.
- [x] Documenter les risques et les décisions reportées.

**Vérification :** l'architecture couvre les deux sources prévues sans imposer de développer tous les protocoles dès le premier incrément.

### Étape 00.4 — Enregistrer la décision sur le moteur GPS

- [x] Ajouter `docs/architecture/adr/ADR-0001-OWN-GPS-ENGINE.md`.
- [x] Expliquer les alternatives et conséquences.
- [x] Définir les conditions qui justifieraient de réévaluer la décision.

**Vérification :** la décision de développer un moteur maison est explicite et son coût est reconnu.

### Étape 00.5 — Relecture et validation

- [x] Vérifier la cohérence entre le README, le périmètre, l'architecture et la feuille de route.
- [x] Vérifier tous les liens Markdown internes.
- [x] Vérifier l'orthographe, les noms de fichiers et la cohérence des termes.
- [x] Vérifier que les choix non confirmés sont marqués comme propositions ou décisions reportées.
- [x] Ajouter une note de validation et les éventuels écarts dans ce fichier.

**Vérification :** aucun choix différé n'est présenté comme une fonctionnalité déjà implémentée.

## Critères d'acceptation du LOT 00

- [x] Le projet possède une vision et un périmètre MVP compréhensibles.
- [x] Les utilisateurs et les responsabilités principales sont décrits.
- [x] Les sources mobile et traceur physique sont prises en compte.
- [x] Le choix d'un moteur GPS maison est enregistré dans un ADR.
- [x] L'architecture sépare ingestion, adaptateurs, domaine, persistance et diffusion.
- [x] Les risques de sécurité, de vie privée, de connectivité et de données hors ordre sont documentés.
- [x] La feuille de route liste les lots, livrables et dépendances.
- [x] Le README permet de découvrir le projet et sa documentation.
- [x] Les liens internes sont valides et la structure documentaire est cohérente.
- [ ] Les commits du lot sont atomiques et le dépôt ne contient ni secrets ni données GPS personnelles.

## Commits atomiques suggérés

Les noms sont des propositions ; ne pas créer de commit avant que le changement correspondant ait été vérifié.

1. `docs: add project overview and documentation structure`
2. `docs: define product scope and MVP acceptance goals`
3. `docs: document GPS engine architecture and technical risks`
4. `docs: record decision to build a custom GPS engine`
5. `docs: add LOT 00 execution checklist`

Il est acceptable de regrouper ou de scinder ces commits si cela améliore leur cohérence, mais chaque commit doit rester compréhensible et réversible.

## Compte rendu de fin de lot

- **Date de validation :** 2026-10-21
- **Résultat :** Validé
- **Vérifications effectuées :**
  - Cohérence générale des documents relue.
  - 5 liens Markdown internes vérifiés ; aucune cible introuvable.
  - Historique Git examiné.
- **Écarts ou décisions reportées :**
  - Versions et dépendances techniques à confirmer au LOT 01.
  - Premier modèle de traceur et protocole matériel à sélectionner ultérieurement.
  - Politique détaillée de rétention à définir avant la mise en production.
- **Commit(s) de référence :** à renseigner après le commit final.

## Définition de terminé

Le LOT 00 n'est terminé que lorsque tous les critères obligatoires sont cochés, que les documents ont été relus et que le compte rendu de fin de lot est rempli. La simple création des fichiers ne suffit pas.
