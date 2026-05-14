---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - drone/V1/RC
journal-start-date: 2026-02-22
---
- [d] Soudage des câbles du premier joystick
- [d] Code qui print la position X et Y du joystick [[Branchement des Joysticks Gimbals sur l'ESP32]]
- [d] Repérage des pins à utiliser [[Mapping des GPIO ESP32]]
- [d] Soudage du deuxième joystick
- [d] Fixation des deux joysticks
- [d] Encodage pour communication avec PC (tests sur LiftOff) [[Bridge RC-LiftOff]] ✅ 2026-02-26
- [d] Mise en place d'une [[Pipeline for raw data|Pipeline]] pour le traitement des données qui sortent des capteurs ✅ 2026-03-01
- [d] git init avec cette version de base ✅ 2026-03-01
- [ ] use the "interrupt" approach regarding the main loop iterations (https://gemini.google.com/app/5d9e2c403df59915?pli=1)
- [ ] signal processing studies to stabilize signal [[Signal Stabilization]]
- [ ] [[RC Firmware Architecture]]

Pour utiliser le switch avec une LED il faut un resistor.
[[RC - Liste de course]]


ST-LINK command pipe to flash firmware and see output through `display_curves`:
```shell
cargo run --release --quiet 2>/dev/null | /home/justin/projects/6_diplay_curve/env/bin/python ~/projects/6_diplay_curve/main.py
```
___
[[Screen Wiring]]
[[Arm-disarm Switch Wiring]]
[[Arm-Disarm Switch Test Bench]]
[[Rotary Encoder Wiring]]

___
```shell
# Build
cargo objcopy --release -- -O binary firmware.bin
# Press & Hold BOOT, press RESET, release BOOT (to put the STM32 in flash mode)
# Flash & Leave (to start the STM32)
dfu-util -a 0 -s 0x08000000:leave -D firmware.bin
```
___
STM32F401 Flash & Serial Cheatsheet

  Build

  cargo build --release
  cargo objcopy --release -- -O binary target/firmware.bin

  Flash (DFU mode)

  1. Enter DFU mode: hold BOOT0, press RESET, release BOOT0
  2. Verify detection: dfu-util -l
  3. Flash:
  dfu-util -a 0 -s 0x08000000:leave -D target/firmware.bin

  Read Serial output

  4. Unplug/replug the board (normal boot, don't hold BOOT0)
  picocom /dev/ttyACM0 -b 115200
  5. Exit picocom: Ctrl-A then Ctrl-X

  One-liner: build + flash

  cargo objcopy --release -- -O binary target/firmware.bin && dfu-util -a 0 -s 0x08000000:leave -D target/firmware.bin


   1 ---
   2 name: STM32F401RC board details
   3 description: User's STM32 board is an STM32F401RCT6 (256K flash, 64K RAM) with blue LED on PC13, USB-C connector, DFU bootloader
   4 type: project
   5 ---
   6
   7 Board has STM32F401RCT6 chip: 256K flash, 64K RAM. Blue LED on PC13, active-low. USB-C goes to USB OTG (PA11/PA12), no built-in USB-to-UART bridge. Uses DFU
      bootloader for flashing. Flash with: `cargo objcopy --release -- -O binary target/firmware.bin && dfu-util -a 0 -s 0x08000000:leave -D target/firmware.bin`
     . HSE crystal likely 25 MHz.
   8
   9 **Why:** Wrong memory.x (96K RAM instead of 64K) caused the bootloader to reject firmware as corrupt — initial SP pointed to nonexistent RAM.
  10
  11 **How to apply:** Always use `LENGTH = 64K` for RAM and `LENGTH = 256K` for flash in memory.x for this board.