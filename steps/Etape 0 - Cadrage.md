---
tags:
  - drone/V1
journal-start-date: 2025-12-22
aliases:
  - Scope V1
---
Objectif : éviter un projet flou

Livrables :
- Document d'architecture système
- Diagramme bloc
- Choix des technologies
- Roadmap incrémentale

## [[Scope V1]]
- [x] Définir ce que la V1 fait et ne fait pas 📅 2025-12-26 ✅ 2025-12-28

Exemple :
- Drone quad X
- Stabilisation attitude uniquement (roll/pitch/yaw)
- IMU 6 axes (gyro + accel)
- RC basique
- Pas de GPS
- Pas de vision
- FPV séparé du contrôle

Livrable : 1 page objectifs V1 / non-objectifs
## Contraintes chiffrées
- [x] identifier les contraintes chiffrées 📅 2025-12-26 ✅ 2025-12-28

| Element                   | Valeur cible |
| ------------------------- | ------------ |
| Fréquence boucle contrôle | 500-1000 Hz  |
| Latence RC max            | < 20 ms      |
| Fréquence IMU             | \>= 1 kHz    |
| MCU                       | \>= 200 MHz  |
| RAM                       | \>= 256 KB   |
| Flash                     | \>= 1 MB     |
Cela permet d'éliminer la majorité des mauvais choix.
## Décider de ce que je fais from scratch vs off-the-shelf
- [x] Décider de ce que je fais from scratch vs off-the-shelf pour la V1 📅 2025-12-26 ✅ 2025-12-28
Exemple :

| Sous-système   | Choix recommandé |
| -------------- | ---------------- |
| Flight control | From scratch     |
| IMU driver     | From scratch     |
| RC protocole   | From scratch     |
| FPV vidéo      | Off-the-shelf    |
| ESC moteurs    | Off-the-shelf    |
### [[Architecture Système rapide - télécommande]]
- [x] architecture système rapide 📅 2026-01-02 ✅ 2026-01-27
Diagramme bloc système -> fige les interfaces.
### [[Choix du Hardware]]
- [x] choix du hardware 📅 2025-01-02 ✅ 2026-01-27
MCU, IMU, ESC & moteurs, RC, FPV
