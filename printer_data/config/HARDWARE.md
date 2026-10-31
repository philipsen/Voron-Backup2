# Printer hardware

Recorded 2026-10-27. This file is not included by Klipper. Pinout and tuned values stay in `printer.cfg`.

## Machine

LDO Voron 2.4 350 mm, Leviathan V1.1. Build volume in config is X/Y 350 mm, Z 330 mm. Z homes on the Eddy probe (`probe:z_virtual_endstop`), then quad gantry level.

| Board | Role | Klipper section | Serial |
| --- | --- | --- | --- |
| Leviathan V1.1 (STM32F446) | Motion, bed, chamber fans | `[mcu]` | `usb-Klipper_stm32f446xx_39003F000251303532383235-if00` |
| BTT MMB (STM32G0B1) | ERCF | `[mcu unit0]` | `usb-Klipper_stm32g0b1xx_4A00470015504D5930393520-if00` |
| BTT Nitehawk-36 | Toolhead | `[mcu nhk]`, `[temperature_sensor NH36]` | `usb-Klipper_rp2040_4E363334320D393E-if00` |
| BTT Eddy USB | Probe | `[mcu eddy]` | `usb-Klipper_rp2040_5044506108AA3D1C-if00` |

## Toolhead

A4T, with a Crossbow filament cutter. The depressor is a fixed arm on the left gantry. Happy Hare cuts by increasing Y (`form_tip_macro: _MMU_CUT_TIP`): approach X0 Y324, touch X0 Y330, full press X0 Y345.

| Part | What is fitted | Where it is configured |
| --- | --- | --- |
| Toolhead | A4T | Part fan `nhk:gpio6`, hotend fan `nhk:gpio5` |
| Extruder | WWG2 (Orbiter 2 bolt spacing) | `[extruder]` on Nitehawk-36. Entry sensor `nhk:gpio3`, toolhead sensor `nhk:gpio13` |
| Hotend | Revo Voron, 0.4 mm nozzle | Semitec 104NT-4-R025H42G, 2200 ohm pullup, heater `nhk:gpio9` |
| Probe | BTT Eddy USB, coil behind the nozzle | `[probe_eddy_current btt_eddy]`, `x_offset` 0, `y_offset` 21.5 (A4T starting value, still to be measured) |
| Toolhead board | BTT Nitehawk-36 | Extruder TMC2209, ADXL345, status LED `nhk:gpio8`, neopixel `nhk:gpio7` (3 LEDs, GRBW) |

The `[extruder]` block is still the previous Galileo 2 calibration: `gear_ratio: 9:1` and `rotation_distance: 48.016`. A WWG2 uses Bondtech BMG gearing, normally `50:17` and a rotation distance near 22.7 mm. Those two values need to be recalibrated for the WWG2. The current numbers are what the running config will use until that is done.

## MMU

ERCF v2, 8 gates, Happy Hare. Display name `ERCF-0`. Gear drive is `80:20`, with a measured rotation distance of 23.13 mm before per-gate calibration in `mmu_vars.cfg`.
