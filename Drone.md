---
tags:
  - drone
journal-start-date: 2025-12-21
---
Conception d'un drone FPV.
## Description du système
Le système est composé de trois sous-systèmes communicants :
- véhicule autonome contraint (drone)
- lien de contrôle en temps réel (RC)
- chaîne de réception humaine (lunettes FPV/écran)
## Domaines techniques
Ingénierie système :
- définition des exigences (latence, masse, autonomie, sécurité ?)
- découpage en sous-systèmes
- interfaces (hardware / software / protocoles)
- gestion des risques

Mécanique / physique :
- Dynamique du drone (6 DOF)
- Contrainte de masse, vibrations
- Choix du châssis

Electronque :
- Alimentation
- MCU / SoC
- IMU, ESC, moteurs
- Schémas, PCB

Logiciel embarqué :
- Bare-metal ou RTOS
- Divers capteurs
- Boucles temps réel
- Sécurité / watchdogs

Commande & estimation :
- Exploitation IMU
- Filtres (complémentaire, Kalman)
- PIB / contrôle d'attitude
- Stabilisation

Télécommunications :
- RC (latence, robustesse)
- Vidéo FPV
- Protocole, erreures, synchronisation

Outillage & qualité :
- Logs
- Simulation
- Tests unitaires / HIL
- Documentation
## Grandes étapes du projet
### [[Etape 0 - Cadrage]]
Objectif : éviter un projet flou
- [x] Scope V1 ✅ 2025-12-27
- [x] Contraintes chiffrées ✅ 2025-12-27
- [x] From scratch vs off-the-shelf ✅ 2025-12-27
- [ ] [[Architecture RemoteController]]
- [ ] Choix du hardware
### [[Etape 1 - Comprendre le drone (sans voler)]]
Objectif : comprendre la physique et le contrôle, sans faire du FPV

A faire : 
- Simuler un drone (Python)
- Implémenter :
	- Modèle dynamique simplifié
	- PID d'attitude
- Tester avec données simulées

-> automatique, mathématiques, modélisation
### Etape 2 - Plateforme embarquée minimale
Objectif : faire tourner du code sur du vrai hardware, sans drone.

Setup typique :
- MCU (ESP32)
- IMU (MPU6050, ICM20948)
- Carte sur breadboard

A implémenter :
- Driver IMU
- Scheduler temps réel
- Filtrage orientation
- Logs série

-> pas de moteurs, pas de RC, pas de FPV
### Etape 3 - Boucle de contrôle moteur (au sol)
Objectif : maîtriser les commandes temps réel

A faire :
- ESC + moteurs
- PWM / DSHOT
- Boucle PID stable
- Gestion des fréquences (1kHz typiquement)
### Etape 4 - Vol stabilisé minimal
Objectif : le drone tient tout seul

Fonctionnalités :
- Stabilisation attitude uniquement
- Pas de position
- Pas de GPS
- Pas de FPV

-> "développement du flight controller"
### Etape 5 - RC custom
Objectif : compétences en télécommunications et protocoles

Options :
- RC simple (UART / RF)
- Latence mesurée
- CRC, retransmission
- Failsafe

\+ mon propre protocole ?
\+ chiffrement léger ?
### Etape 6 - FPV (séparé du contrôle)
Objectif : ne jamais mélanger contrôle et vidéo

Deux choix :
- Analogique (simple, robuste)
- Numérique (compression, latence)
### Etape 7 - Industrialisation et qualité
Objectif : transformer un prototype en projet professionnel
- PCB custom
- Tests HIL
- Documentation
- Démo vidéo
- Repo Git structuré
## Gestion de la complexité
Principe fondamental : un seul nouveau problème à la fois

Incrémentation verticale : ne pas faire toute l'électronique, puis tout le logiciel, etc. Faire un système complet mais simple, puis enrichir chaque couche.

Toujours savoir justifier mes choix.

