# XMotion All In One Controller V3 — Documentación técnica

**Fabricante:** JSumo · **Compatibilidad:** Arduino (bootloader Leonardo)

---

## Especificaciones principales

| Característica | Valor |
|---|---|
| Microcontrolador | ATMEGA 32U4 (bootloader Arduino Leonardo) |
| Dimensiones | 80 × 30 × 7 mm |
| Peso | 13 g |
| Voltaje de entrada | 7–24 V (la tabla comparativa del fabricante indica hasta 28 V) |
| Regulador | Conmutado (switching), salida 5 V para sensores (≈600 mA disponibles; capacidad del regulador ~1600 mA sin calentamiento) |
| Drivers de motor | 2 salidas basadas en MOSFET, 6 A cada una, control Dir/PWM |
| Conexión USB | Micro USB (cable de programación incluido) |
| I/O para sensores | 9 en total; 8 protegidos contra sobretensión hasta 15 V |
| Entrada USB | Sí, Micro USB |

---

## Elementos de la placa

- **Microcontrolador** — ATMEGA 32U4 con bootloader de Arduino Leonardo.
- **Regulador de voltaje** — modo conmutado para entrada de 7–24 V; salida de 5 V para todas las conexiones de sensores.
- **Drivers de motor** — dos salidas de 6 A basadas en MOSFET, a prueba de cortocircuito, controlables por Dir/PWM (hay ejemplos en la librería XMotion).
- **LEDs de usuario y de dirección** — 2 LEDs de usuario programables + 4 LEDs de dirección de motor (2 canales × 2 LEDs) para ver el sentido sin necesidad del motor.
- **Botón y pin de arranque (Start)** — compatible con módulos de arranque de robots; en paralelo con un pulsador.
- **Dipswitch** — 3 posiciones, 8 combinaciones digitales (útil para modos/tácticas del robot).
- **Trimpot** — entrada analógica, salida 0–5 V (p. ej., control de velocidad o variables de tiempo).
- **Capacitores** — cerámicos multicapa de bajo ESR distribuidos en la placa.
- **Resistencias serie** — todos los pines del MCU tienen resistencias limitadoras de corriente en serie.
- **Terminales de potencia** — vienen sin soldar; aceptan bornes de tornillo de 3.5 mm, headers macho de 2.54 mm o soldadura directa.
- **Conexión Micro USB** — programación directa desde la placa; cable incluido.

---

## Conexiones y puertos (mapa físico)

**Cara superior**
- Puerto I2C
- LEDs de usuario
- Trimpot
- LEDs de dirección de motor izquierdo / derecho
- Salida de motor izquierdo (máx. 6 A) / salida de motor derecho (máx. 6 A)
- Puerto Micro USB
- Entrada de alimentación 7–24 V
- Interruptor de encendido (Power Switch)
- Dipswitch
- LED de encendido (Power Led)

**Cara inferior**
- Pads de soldadura del terminal I/O — paso estándar de 0.1" (2.54 mm)
- "Holy Pin Table" — serigrafía con el mapeo de pines
- Puerto de encoder de motor izquierdo / derecho (opcional)
- Puerto Bluetooth
- Entrada de voltaje (Voltage Input)

---

## Protecciones incorporadas

- Protección contra **inversión de batería** (caída de solo ~60 mV en el MOSFET a plena corriente).
- Protección contra **cortocircuito en la salida de 5 V**.
- Protección contra **cortocircuito en la salida de motor**.
- Protección contra **sobretensión** en las entradas (8 de 9 I/O protegidas hasta 15 V).
- Protección contra **sobrecorriente** y **sobretemperatura** para regulador y driver de motor.

---

## Sensores compatibles

Admite todos los sensores con salida digital o analógica. Con las 8 entradas protegidas también acepta directamente sensores PNP de 12 V (industriales tipo Omron, Keyence, etc.).

Modelos citados como compatibles:
- JS40F (sensor digital de oponente)
- MZ80 (sensor infrarrojo)
- Omron E3Z-D62-82 *(sin divisor de resistencias)*
- Pepperl Fuchs MLT1000 *(sin divisor de resistencias)*
- Banner Q20NDXL
- Parallax Ping
- HC-SR04 (ultrasónico)
- QTR1A y QTR1RC (sensores de línea)
- QTR8RC (sensores de línea) — **el QTR8A NO es adecuado**
- Xline
- ML1

---

## Recursos oficiales

- Página del producto / básicos: https://www.jsumo.com/xmotion
- XMotion Basics (XMotion 101): https://blog.jsumo.com/xmotion-basics-xmotion-101/
- Cómo hacer un robot mini-sumo con XMotion: https://blog.jsumo.com/how-to-make-mini-sumo-robot-with-xmotion/

---
