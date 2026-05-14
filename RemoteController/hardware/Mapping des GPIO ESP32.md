---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - drone/V1/RC
journal-start-date: 2026-02-22
---
valeur Rust -> Pin physique
GPIO4 -> VP
GPIO12 -> D33

Pour les pins qui ont un ADC (Analogic to Digital Converter) :

| Pin Physique | D32    | D33    | D34      | D35    |
| ------------ | ------ | ------ | -------- | ------ |
| Valeur Rust  | GPIO32 | GPIO33 | GPIO34   | GPIO35 |
| Gdeur phys.  | Pitch  | Roll   | Throttle | Yaw    |
