---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - drone/V1/RC
journal-start-date: 2026-03-17
---

Resources :
- Gimbal pins (4)
- Switch pins (2 for now)
- Screen pins (not considered for now)

I want to split the code between a part that handles the hardware and another part that handles [[Signal Stabilization|signal processing]].

![[RC Firmware Architecture.excalidraw]]

Modules :
- `HardwareContext`: in contact with the hardware.
	- Reads all pins and deduces user input.
	- Changes the system mode with the switch 1
	- Changes the fly mode with switch 2
	- Reads raw axes signal with minimum smoothing (zero algorithms if the STM32 is good enough)
- `SystemState`: represents the RC state
	- Calibration
		- With no screen we just store the historical minimum/maximum.
		- With a screen we will be able to send instructions to the user in order to get a center too, and a dead-zone around the center.
	- Standby: does nothing until the throttle is all the way down AND we have a calibration (anyways being "all the way down" makes no sense without a calibration). If this case is met, then the system goes to Flying mode.
	- Flying: the flying mode is the one told by `HardwareContext` by reading the pin for switch 2. (When flying, there is no way to go back to calibration. Else if we miss-switch, the drone falls down.)
- `Filter`: this module will be used only in Flying mode and will take the raw axes signal in input and output the filtered signals. The algorithms should be different if the user is in Angle vs. Acro mode.

NB: the differences between Angle and Acro mode are actually located both in the RC and in the drone firmware.
- The RC will be less sensible in Angle mode
- The drone will consider signal from its IMU to stay stable. So in the real product, the RC will be able to send the Flying mode to the drone, alongside with the axes data.

Improvements :
- `Radio` module (two-ways communication with the drone. We want to be able to know the battery level and the drone distance)
- `UiFeedback` module that would control LED, screen, sound?

I will need to have a "Serial" mode where I just output acro values to the computer.

![[rc-firmware-architecture.drawio]]
