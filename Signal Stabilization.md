---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - drone/V1/RC
journal-start-date: 2026-03-01
---

The signal instability seen in the simulator can have two main sources :

**Input Conditioning (ADC & Filtering)**
- Topics: Oversampling, Moving Average Filters, *PT1/PT3 (Low-pass)* Filters.
- Betaflight uses *PT3 based RC smoothing*. A 3rd order filter provides a much smoother response without the "rubbery" feel of high latency.
- **EdgeTX ADC Filter:** Study the [EdgeTX source code](https://github.com/EdgeTX/edgetx) regarding their `ADC Filter` implementation. They use a specific "hysteresis" logic to ignore tiny fluctuations in the ADC value while maintaining response to intentional movement.

**Temporal Processing (Jitter & Synchronization)**
Even a clean signal will cause "stuttering" if the time between packets varies. This is called **Jitter**.
- Topics: *Interrupt Service Routines* (ISRs), *Real-Time Operating Systems* (RTOS) and *MixerSync*.
- Study the [ExpressLRS architecture](https://www.expresslrs.org/). They use a technique where the transmitter synchronizes its internal processing loop exactly with the RF chip’s transmission window. This ensures that every packet sent contains the absolute freshest data, eliminating "beat frequencies" between your internal clock and your radio link.

- [d] check if I use the right baudrate in the [[Bridge RC-LiftOff|Python Bridge]] ✅ 2026-03-01

https://gemini.google.com/app/cc083a7a308de299

Currently I have a good stabilization (no more jitter) but it seems like the curve is weird. I need to have a better estimation of this curve. I feel like the center zone is very unsensitive. I move my stick on a large zone without any reaction from the drone.
- [d] I need to get a plot of the processed data in time (easy with Python) ✅ 2026-03-07

**Addressing the "dip" problem**
https://gemini.google.com/app/a3e1ec41a7287ea7
I see the signal go down then up to the right value, or down then down to the right value. The problem is likely hardware-related.

I see the same behaviour (and even harder) when I stop using any software algorithm (no more oversampling, no more PT3 filter). In conclusion, the problem comes from *hardware*. This can be due to several phenomena :


- solution 1 : updgrade to Hall-effect gimbals : https://fr.aliexpress.com/item/1005007982794308.html?spm=a2g0o.productlist.main.21.4b91RURKRURKL8&algo_pvid=b562a6ae-be8a-4b7e-be55-34248d1f3f02&pdp_ext_f=%7B%22order%22%3A%2211%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005007982794308%7C_p_origin_prod%3A#nav-specification

The problem was reduced by using shorter jumper wires and isolating the gimbals power and the ESP32 power. Before, the gimbals were powered by the ESP32's power pin, which power comes from USB, which is more unstable than the dedicated power supply.

**Currently 2026-03-08** the flight is very unstable when I want to try and make the drone change its trajectory. It feels like I have to push the axis all the way to an extreme to make the drone react.

*To try:* the "buffering" solution (**MOST RECOMMENDED**):
Add an **Op-Amp (TL072 or LM358)** in a **Voltage Follower** configuration between the gimbal and the ESP32.
- *(bad way)* The Op-Amp has near-infinite input impedance (won't "drain" the gimbal) and near-zero output impedance (will "force" the ESP32 ADC to see the correct voltage instantly). This usually deletes the "dip" entirely. **WARNING** these solutions need a different power supply and if done bad, can damage the ESP32 (too high voltage for ex.)
- *(better way)* a **Rail-to-Rail Input/Output (RRIO) Op-Amp** that works cleanly at 3.3V. **MCP6002** or **LMV358** (the low-voltage version of the LM358). I would then need *two MCP6002*.

I need one op-amp channel for every axis (X and Y). But, I'll likely need only one chip per gimbal.


Other way: The "Low-Pass" Hardware Hack

If you don't want to use an Op-Amp, try an **RC Filter** as close to the ESP32 pin as possible:
1. Place a **$1k\Omega$ resistor** in series with the signal wire.
2. Place a **$10\mu F$ electrolytic capacitor** (plus a $0.1\mu F$ ceramic one) between the ESP32 signal pin and GND.

_Note:_ The **location** matters. They must be at the ESP32 pins, not at the gimbal side.




**Do I actually need another microprocessor?**
Solutions to address the ESP32's bad ADC :
- Use an external ADC
	- ADS1115 (16-bit, communicates via I2C)
	- MCP3204 / MCP3208 (12-bit, communicates via SPI)
- Hardware low-pass filter before the ADC (currently I have a software filter in the ESP32)
- Clean power delivery (already in use)
- Almost all RC in the market uses a STM32 microcontroller (like the STM32F4 series)
	- Superior ADC: highly accurate, low noise floor
	- Stable power (compared to the ESP32 if the WiFi module is active)
	- Hardware Timers: They have advanced hardware timers perfect for generating PWM/PPM signals or handling high-speed serial links (like ExpressLRS) without interrupting the main code loop

2026-03-11: so I bought an STM32 and op-amp, and 0.1µF capacitors.

**Adding op-amps MCP6002**
Wiring diagram:
![[MCP6002.webp|402]]
Wiring:

| PIN  | 1     | 2         | 3      | 4   | 5      | 6         | 7     | 8   |
| ---- | ----- | --------- | ------ | --- | ------ | --------- | ----- | --- |
| WIRE | STM32 | MCP PIN 1 | Gimbal | GND | Gimbal | MCP PIN 7 | STM32 | 3V3 |
[[Mapping des GPIO STM32]]