---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
journal-start-date: 2025-12-27
tags:
  - drone/V1
---
La V1 n'est pas un drone FPV, c'est une plateforme volante minimale.

Objectif de la VA : **Le drone peut décoller, rester stable quelques secondes, répondre aux commandes et se poser sans se détruire.** Pas de notion de performance, précision, figures, longue autonomie.
## 1 Ce que la V1 fait

| Struct. Méca                              | Calculateur              | Capteurs                                      | Contrôle                                                                                   | Radio                         | Sécurité minimale                    |
| ----------------------------------------- | ------------------------ | --------------------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------- | ------------------------------------ |
| Quad X                                    | 1 [[MCU]]                | [[IMU]] 6 axes<br>(gyroscope + accéléromètre) | Stabilisation angle uniquement : roll + pitch stabilisés, yaw en rate libre (ou très lent) | RC très simple                | Coupure moteurs immédiate            |
| 5" très bridé<br>(sinon grosse nervosité) | Bare-metal               | Pas de magnétomètre                           | Pas de mode Acro, pas de modes multiples                                                   | 4 axes + arm/disarm           | Timeout RC => moteurs off            |
| Châssis simple, rigide                    | Boucle principale simple |                                               | Comme le mode LEVEL de Liftoff                                                             | Fréquence modeste (50-100 Hz) | Batterie surveillée (seuil grossier) |
## 2 Ce que la V1 ne fait pas
- FPV
- GPS
- Acro
- Télémétrie temps réel
- Optimisation des perfs
- Longs vols
- Récupération après perte RC
## 3 Segmentation en objectifs intermédiaires

| V0.1 - Propulsion                      | V0.2 - Capteur                | V0.3 - Stabilisation                                             | V0.4 - Commande                 | V1.0 - Vol contrôlé            |
| -------------------------------------- | ----------------------------- | ---------------------------------------------------------------- | ------------------------------- | ------------------------------ |
| **faire tourner les moteurs**          | **IMU fiable**                | **boucle de stabilisation minimale**                             | **RC + sécurité**               | **vol court, contrôlé, à vue** |
| MCU -> ESC -> moteurs                  | Lecture gyro + acc            | Calcul angle pitch/roll                                          | Réception commandes             | Hélices                        |
| Commande PWM fixe                      | Calibration offsets           | Filtre complémentaire                                            | Mapping sticks -> consignes     | Décollage doux                 |
| Test moteur un par un                  | Filtrage simple (LPF)         | PID très mou                                                     | Timeout RC -> moteurs OFF       | Hover 2-5 secondes             |
|                                        | Envoi des valeurs en série    | Mixage moteurs                                                   | Interrupteur ARM                | Atterrissage                   |
| **chaque moteur répond immédiatement** | **valeurs stables à l'arrêt** | **le drone résiste quand on l'incline à la main (sans hélices)** | **moteurs coupés dès perte RC** |                                |
## 4 Simplifications
- Un seul mode de vol : ANGLE
- Fréquences modestes :
	- IMU : 500 Hz
	- PID : 250 Hz
	- RC : 50 Hz
- Pas de tuning fin : PID "safe et mou" -> drone un peu paresseux = bon signe
- Logs simples -> log texte UART pour le debug
## 5 From scratch vs off-the-shelf
Structure mécanique : off-the-shelf

Calculateur (MCU + firmware) : from scratch total
- Choix du MCU
- Init hardware
- Boucle temps réel
- Drivers maison
- Architecture logicelle claire

Capteurs (IMU) : from scratch maîtrisé.
- Dans la V1, on fait from scratch :
	- Lecture brute
	- Calibration
	- Filtrage
	- Estimation d'attitude
- On garde pour la suite :
	- Driver I2C/SPI "bit banging exotique"
	- Fusion multi-capteurs avancée
	- EKF lourd

Contrôle (stabilisation) : from scratch, scope contrôlé.
On se limite à 1 mode de contrôle et 1 modèle mental. Pour la V1 :
- Mode ANGLE uniquement
- Roll + pitch stabilisés
- Yaw rate simple
- PID classiques

Pas de LQR, de MPC, d'optimalité.

Radio : hybride. En V1 :
- PHY + protocoles simples
- Pas de performance extrême
- Latence "acceptable"


| From scratch           | Off-the-shelf       |
| ---------------------- | ------------------- |
| Encodage des commandes | Transceiver         |
| Protocole applicatif   | Stack RF bas niveau |
| Failsafe logique       | Drivers radio       |
