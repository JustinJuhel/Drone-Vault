---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
---
Modèle utilisé : ***MCP6002***

## 1 **Voltage Follower** configuration
Also called *Unity-Gain Buffer*.
Wiring diagram:
![[MCP6002.webp|542]]
Wiring:

| PIN  | 1     | 2         | 3      | 4   | 5      | 6         | 7     | 8   |
| ---- | ----- | --------- | ------ | --- | ------ | --------- | ----- | --- |
| WIRE | STM32 | MCP PIN 1 | Gimbal | GND | Gimbal | MCP PIN 7 | STM32 | 3V3 |

## 2 Gain augmentation
The formula for this amplifier's gain is:
$$
\boxed{
G = 1 + \frac{R_{2}}{R_{1}}
}
$$

![[Pasted image 20260621144848.jpg|745]]

To meet the $3.3V$, we can try $R_{2} = 10 k\Omega$ and $R_{1} = 2.2 k\Omega$ for a gain of $5.54$.

**Rule :** for general-purpose op-amps, keep your feedback resistors between $1k\Omega$ and $100k\Omega$ to keep current draw negligible.