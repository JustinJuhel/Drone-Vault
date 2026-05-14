---
number headings: first-level 2, start-at 1, max 6, 1.1, auto, contents ^toc
tags:
  - embedded-software
journal-start-date: 2026-01-11
aliases:
  - Règles UDEV
  - règles UDEV
---

Les règles UDEV sont utilisées au moment où les périphériques associés à des bus [[Protocols - I2C, SPI, UART|I²C, SPI ou UART]] apparaissent dans l'espace utilisateur. On peut les voir comme l'interface entre le kernel (drivers + bus) et les applications.

Le kernel Linux implémente les [[Protocols - I2C, SPI, UART|protocoles]], gère les drivers, détecte les périphériques. Les règles UDEV observent les événements du noyau, crée les fichiers dans `/dev`, applique permissions, noms, liens symboliques, peut déclencher des scripts.

En fonction du type de bus, les règles UDEV ont des interactions différentes :
- I²C : permissions, liens symboliques stables, accès aux capteurs depuis l'interface utilisateur
- SPI : nommage cohérent des périphériques, gestion des droits d'accès, liens symboliques
- UART : différencier plusieurs adaptateurs identiques, créer des noms stables, régler permissions et groupes

Chaîne complète d'événements :
```
Boot ou hotplug
   ↓
Device Tree / driver noyau
   ↓
Création sysfs (/sys/class/...)
   ↓
Uevent kernel
   ↓
Règle UDEV
   ↓
Création /dev + permissions + symlink
   ↓
Application utilisateur
```
