# Périmètre produit

## 1. Vision

Fournir une plateforme de suivi GPS permettant à une organisation de gérer ses véhicules et de consulter leurs positions et déplacements depuis une interface web.

Le projet sert à la fois de projet de portfolio et de parcours d'apprentissage. La première version doit démontrer un flux de données complet, de la source GPS jusqu'à la carte, sans prétendre offrir immédiatement toutes les fonctions d'un produit commercial mature.

## 2. Utilisateurs cibles

- **Administrateur de plateforme** : gère les organisations et les paramètres globaux.
- **Gestionnaire de flotte** : gère les véhicules, associe les appareils et consulte les trajets de son organisation.
- **Opérateur** : consulte la carte, l'état des véhicules et les alertes selon ses permissions.
- **Appareil GPS** : source technique authentifiée de positions ; ce n'est pas un utilisateur humain.

Les rôles précis et leurs permissions seront affinés avant l'implémentation de l'authentification.

## 3. Fonctionnalités prévues

### MVP

1. Créer une organisation et gérer ses véhicules.
2. Enregistrer un appareil et l'associer à un véhicule.
3. Recevoir des positions GPS de test via un point d'entrée documenté.
4. Valider et normaliser les positions.
5. Persister les positions avec leur heure de mesure et leur heure de réception.
6. Consulter la dernière position connue d'un véhicule.
7. Afficher les véhicules et leurs dernières positions sur une carte.
8. Afficher clairement si une position est ancienne ou si un appareil n'a plus communiqué.
9. Fournir une suite de tests automatisés et une procédure de lancement local reproductible.

### Après le MVP

- Flux de positions en temps réel via WebSocket.
- Historique des trajets et lecture d'un trajet.
- Calcul de distances estimées, arrêts et événements.
- Géorepérage : entrée/sortie d'une zone.
- Alertes configurables.
- Application mobile transmettant la position en arrière-plan, sous réserve des autorisations et contraintes du système d'exploitation.
- Adaptateurs pour des modèles de traceurs physiques et leurs protocoles.
- Rapports, exports et outils d'administration.
- Supervision, sauvegardes, politiques de rétention et durcissement de production.

## 4. Hors périmètre initial

- Commandes à distance du véhicule, immobilisation moteur ou contrôle d'équipements.
- Garantie d'une précision GPS ou d'une disponibilité réseau absolue.
- Optimisation avancée de tournées.
- Facturation commerciale et intégration de paiement.
- Prise en charge de tous les modèles de traceurs dès la première version.
- Application iOS native avant validation de l'architecture et du besoin.

## 5. Principes métier

- Une position représente une observation à un instant donné ; elle ne doit pas être confondue avec la dernière position reçue.
- L'heure de mesure fournie par la source et l'heure de réception serveur sont stockées séparément.
- Une position invalide ne doit pas modifier la dernière position valide d'un véhicule.
- Les données d'une organisation ne doivent pas être accessibles à une autre organisation.
- Un appareil doit être authentifié et associé à un périmètre d'accès explicite.
- Les doublons, messages retardés et positions hors ordre doivent être traités de manière déterministe.
- Une absence de message signifie « aucune donnée récente reçue », pas nécessairement « véhicule arrêté ».

## 6. Contraintes de sécurité et de vie privée

- Utiliser TLS pour les communications en production.
- Ne jamais intégrer de secret permanent dans le code source de l'application mobile.
- Pouvoir révoquer les identifiants d'un appareil.
- Limiter les permissions et l'accès aux données de localisation.
- Définir une politique de rétention et de suppression avant une mise en production réelle.
- Informer les personnes concernées et respecter les règles applicables au lieu d'exploitation.
- Éviter les coordonnées réelles dans les dépôts publics, captures d'écran et jeux de données de démonstration.

## 7. Indicateurs de réussite du MVP

- Une position de test valide est acceptée et stockée.
- Une position invalide est rejetée sans corrompre l'état du véhicule.
- L'API distingue l'heure de mesure de l'heure de réception.
- La dernière position connue est restituée et affichée sur la carte.
- Les tests automatisés passent dans un environnement reproductible.
- Les instructions de démarrage et de test sont suffisantes pour qu'un autre développeur puisse suivre le README.
