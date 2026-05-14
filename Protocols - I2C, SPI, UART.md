---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - embedded-software
journal-start-date: 2026-01-11
---

[Analog devices](https://www.analog.com/en/index.html) : great resource (such as Texas Instruments)
## 1 SPI
One MCU (or chip) (**master**) coordinates communication flow between multiple other chips or MCUs *on the bus* (**slaves**).

Each slave needs a GPIO pin to know if it's the one the master is talking to. By default, all pins are high and if the master want to talk to a slave, it puts its pin to low and keeps all other pins high.

 Benefits: we need a pin for each slave on the bus
Drawbacks : pretty quick, flexible
## 2 I²C
Master-slave type protocol, like SPI. One MCU coordinates all communication between all chips on the bus.

But it differs to SPI in one key factor : it only uses **two wires** for everything : *SCL* and *SDA* (in serial). It is an **addressed protocol** -> every device has an address. The address is put in the messages so that the slaves know if they are being talked to. The address is 7 bits long (128 possibilities).

It has a messaging protocol, unlike SPI. 

Benefits : we only use two lines and can talk to "as many" slaves as we want, in direct contrast with SPI
Drawbacks : it's a bit slow because the message contain the addresses, the acknowledgement, etc.
## 3 UART
One very key difference with the two previous protocols, is that it is not technically a master-slave protocol.

Two UARTs[^1] can communicate with each other using UART. Each one has a TX line and a RX line. We can't use multiple UART because each UART would connect its TX to two other UARTs and there would be congestion on this line. **With UART, we only consider two MCUs.**

[^1]: Here, we refer to MCUs as UARTs.

The mode of transmission is in the form of a (serial) packet. A **packer** consists of a start bit, data frame a parity bit, and stop bits.

There is **no clock line** in UART. Instead, we use a **baudrate**. It is a pre-agreed-upon rate of transmission of the information. It is very popular for debug stuff. A UART communication shows up as a TTY port on Linux, which we can read from an write on.

[AVR Baud Rate Tables](https://cache.amobbs.com/bbs_upload782111/files_22/ourdev_508497.html) helps to choose the right baudrate depending on the application and hardware.

Benefits : if we have only two MCUs communicating, or we want to use it for debug, then we don't need the complexity of SPI or I²C. We can also add our protocol on top of it, or use for example a checksum to detect errors more efficiently than the parity bits.
Drawbacks :