---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - drone/V1/RC
journal-start-date: 2026-02-20
---

Throttle Gimbal :
- Câble noir : GND
- Câble rouge : 3.3V
- Câble jaune : pin 6

1. retrouver le projet qui fonctionne sur l'ESP32
2. modifer pour print ce qui arrive en tension sur la pin 6

[[Mapping des GPIO ESP32]]

Caractéristiques du gimbal :
- résistance (entre GND et signal): 1-4 $k\Omega$
- inductace: None. The gimbals do not have a built-in smoothing capacitor
- voltage: 