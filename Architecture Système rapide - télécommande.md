---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - drone/V1/RC
journal-start-date: 2025-12-27
---
## 1 Etude électronique
[ESP32-WROOM-32 datasheet](https://documentation.espressif.com/esp32-wroom-32_datasheet_en.pdf)
L'ESP32 possède plusieurs pins. Je vais y connecter mes [joysticks](https://betafpv.com/products/literadio-transmitter-nano-gimbal-for-literadio-3-and-2-se) qui ont chacun deux axes (et chaque axe correspond à 3 câbles dont la terre).

Le joystick est composé de deux potentiomètres. Chaque axe est donc associé à une tension (signal analogique). Pour récupérer une valeur numérique sur l'ESP32, il faut connecter mes câbles à une pin ayant une fonction ADC[^1]. Chaque axe du joystick correspond à trois câbles :
1. GND : ground (en général noir) -> pin GND de l'ESP32
2. VCC : power supply (en général rouge) -> pin 3.3V de l'ESP32 pour fournir une référence pour la mesure de tension
3. VR : signal -> c'est le signal que l'on souhaite récupérer en le connectant à la pin GPIO de l'ESP32

[^1]: Analog-to-Digital Converter

Les Gimbals prennent une tension en entrée de 3.3V. The ESP32 ADC returns 0-4095 (12-bit resolution).

[[Branchement des Joysticks Gimbals sur l'ESP32]]

[[Suivi de projet - télécommande]]

___
Correspondance axes <--> grandeur physique

| Joystick | Axis      | Physical value |
| -------- | --------- | -------------- |
| Left     | Y (vert)  | Throttle       |
| Left     | X (horiz) | Yaw            |
| Right    | Y (vert)  | Pitch          |
| Right    | X (horiz) | Roll           |
___
