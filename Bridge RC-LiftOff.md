---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - drone/V1/RC
journal-start-date: 2026-02-24
aliases:
  - Python Bridge
---

Comme je suis sur Linux, je peux utiliser le sous-système `uinput` du système qui permet de créer un *joystick virtuel* que LiftOff prendra pour un vrai joystick.

**Architecture :**
1. **Microcontrôleur :** l'ESP32 envoie des valeurs numériques via Serial (`/dev/ttyUSB0`)
2. **Le bridge (Python) :** un script sur le PC qui lit la serial data et convertit les floats en integer steps, et les envoie au joystick virtuel
3. **OS Linux/Steam :** voit le joystick virtuel et envoie les données des axes à LiftOff