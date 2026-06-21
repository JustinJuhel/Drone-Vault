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
