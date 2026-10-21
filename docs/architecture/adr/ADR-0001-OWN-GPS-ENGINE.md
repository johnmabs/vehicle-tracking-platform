# ADR-0001 — Développer notre propre moteur GPS

- **Statut :** Accepté pour le projet pédagogique
- **Date :** 2026-10-20
- **Décideurs :** Mainteneur du projet
- **Contexte :** projet de portfolio et d'apprentissage

## Contexte

La plateforme doit recevoir des positions provenant d'une application mobile et, à terme, de traceurs GPS physiques. Le projet vise à apprendre les protocoles, l'ingestion, la normalisation, la persistance géospatiale et la diffusion temps réel. Réutiliser entièrement un serveur GPS existant réduirait la quantité de travail sur le moteur lui-même.

## Décision

Développer un moteur GPS maison. Commencer par un point d'entrée d'ingestion HTTP simple et testable, puis ajouter des adaptateurs de protocoles matériels de manière incrémentale.

Le moteur doit séparer :
- les transports réseau ;
- les adaptateurs de protocole ;
- la validation et la normalisation ;
- le domaine et la persistance ;
- les événements et la diffusion temps réel.

## Conséquences positives

- Apprentissage approfondi du traitement de données GPS et des protocoles.
- Contrôle de l'architecture et des règles de validation.
- Projet de portfolio démontrant des compétences backend, réseau, géospatiales et de test.
- Possibilité de documenter chaque protocole pris en charge.

## Conséquences négatives et risques

- Coût de développement et de maintenance supérieur à l'intégration d'un moteur existant.
- Chaque protocole matériel demande de l'étude, des tests et parfois du matériel réel.
- La sécurité, les limites réseau, les acquittements, les messages malformés et la résilience doivent être conçus et testés.
- La plateforme ne sera pas compatible avec tous les traceurs dès le départ.

## Alternatives considérées

1. **Intégrer Traccar** : écarté pour le moteur principal, car l'apprentissage du moteur GPS est un objectif explicite. Une comparaison ou une intégration de référence pourra être envisagée séparément.
2. **Construire immédiatement un moteur complet multi-protocoles** : écarté, car trop large pour un premier incrément.
3. **Commencer par un point d'entrée HTTP normalisé** : retenu comme première étape d'apprentissage, sans prétendre que HTTP représente tous les protocoles des traceurs.

## Conditions de réévaluation

Réexaminer cette décision si le projet doit prendre en charge rapidement de nombreux modèles matériels en production, si les exigences de disponibilité deviennent élevées ou si la maintenance des protocoles dépasse les objectifs pédagogiques.
