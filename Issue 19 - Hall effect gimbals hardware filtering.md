---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
---

https://gemini.google.com/app/5e5e40c2b231037b

## 1 Add op-amps
[[Op-Amp wiring diagram]]

- [x] vérifier le câblage ✅ 2026-06-21
- [x] allumer sans gimbal pour checker que ça brûle pas ✅ 2026-06-21
- [x] tester avec gimbals ✅ 2026-06-21

Les valeurs sont passées de 714 - 675 (39 de variance) à 561 - 418 (143 de variance).
Donc les amplificateurs opérationnels amplifient, avec un gain de 3.6.
Ce n'est pas assez car on n'utilise que 3.5% de la variance ADC du STM32 (0 - 4095).
De plus, les valeurs observées subissent un offset vers le bas.

Il faut augmenter le gain, et gérer l'offset pour éviter d'écraser les valeurs obtenues vers 0.
Augmentation du gain:
![[Op-Amp wiring diagram#2 Gain augmentation]] 

Comme je n'ai que 330R et 47R, je peux tester $R_1 = 3 \times 47R$ et $R_2 = 2 \times 330R$.
Si ça fonctionne, je commande un set de résistances d'au moins $1 k\Omega$.

Je vois une modification sur l'axe amplifié : la valeur passe à 2179 mais ne change pas quand je modifie la valeur de throttle.
Cela correspond environ à la moitié entre 0 et 4095.

___

Ce que j'observe maintenant, c'est une variation de seulement 23mV (alors que j'ai une amplitude de lecture de 3.3V). Avec les **AG01 NANO CNC metal gimbals**.

[Recommandations](https://chatgpt.com/c/6a4e8cf7-dae8-83ed-a78b-cc3f0b52d33c) :
I'd use a **two-stage architecture**, because it gives the best combination of stability and tuning:
1. **Precision reference at the center voltage** (around 560 mV), generated from a stable reference or DAC.
2. **Difference amplifier** with a gain of about **60–80**, so the stick uses roughly **0.2 V to 3.1 V** of the ADC range.
3. Feed that into the STM32 ADC.
4. In firmware:
    - calibrate center, min, and max on first power-up (and allow recalibration),
    - average **8–16 samples**,
    - apply a small deadband around center,
    - map to your CRSF or ELRS output,
    - optionally apply user-selectable expo curves.

This approach avoids the huge output offset you'd get from simply multiplying the absolute 560 mV signal, and it gives you a clean, repeatable center—something that's critical for FPV freestyle.

