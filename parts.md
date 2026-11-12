# Parts list

> **The Google Sheet (linked in `CLAUDE.local.md`) is the source of truth.** This file is a snapshot for the public repo, last synced on Nov 12, 2026, and may lag behind it.

Prices are estimates from planning (Nov 2026); update them from the real cart and receipts.
**Totals:** Amazon ~$343 · UIUC ~$44 · **~$387** (optional items not counted).

Status key: ☐ not bought · 🛒 ordered · ✅ in hand · ✔ tested

## Amazon: quadcopter
| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | ELEGOO Nano V3.0 (ATmega328P), 3-pack | 1 | $19 | [amazon.com/dp/B0713XK923](https://www.amazon.com/dp/B0713XK923) | Flight controller + 2 spares. No USB cable (needs a Mini-B *data* cable). |
| ☐ | HiLetgo GY-521 MPU-6050 IMU, 3-pack | 1 | $11 | [amazon.com/dp/B00LP25V1A](https://www.amazon.com/dp/B00LP25V1A) | Gyro + accelerometer over I2C. |
| ☐ | MakerFocus nRF24L01+ radio, 10-pack | 1 | $13 | [amazon.com/dp/B00O9O868G](https://www.amazon.com/dp/B00O9O868G) | One on the quad, one on the transmitter. 3.3 V only; 10 µF + 0.1 µF caps across VCC/GND. |
| ☐ | HiLetgo AMS1117-3.3 regulator module, 10-pack (800 mA) | 1 | $7 | [amazon.com/dp/B01HXU1NQY](https://www.amazon.com/dp/B01HXU1NQY) | **Powers the nRF24L01** from the 5 V rail. The Nano's 3V3 pin can't supply the radio's transmit spikes. ~1 g. |
| ☐ | Hobbypower 8520 coreless motors (8) + 75 mm props | 1 | $16 | [amazon.com/dp/B076M7G24G](https://www.amazon.com/dp/B076M7G24G) | 4 to fly + 4 spares. Confirm the listing says (8) motors: the same ASIN is a 4-motor set on some non-US sites. Thrust is unmeasured (the ~30 g each is a seller claim). |
| ☐ | GRASKY 75 mm props, 1 mm hole, CW/CCW | 1 | $8 | [amazon.com/dp/B0CNP12BFM](https://www.amazon.com/dp/B0CNP12BFM) | **Pick the "5 sets / 20 pcs" option.** Spares for crashes. |
| ☐ | URGENEX 1S 3.7 V 600 mAh LiPo, 5-pack + USB charger | 1 | $27 | [amazon.com/dp/B08XZM3NG3](https://www.amazon.com/dp/B08XZM3NG3) | ~18 g each, 57×18×7.6 mm, 25C per a third-party listing. Confirm the plug is the red 2-pin JST-RCY. |
| ☐ | 2-pin JST (RCY) pigtails, 20 pairs, 22 AWG | 1 | $8 | [amazon.com/dp/B013WTV270](https://www.amazon.com/dp/B013WTV270) | Battery lead. Check plug gender and polarity. Spare pigtails supply the 22 AWG wire for the power bus. |
| ☐ | MT3608 DC-DC boost converter (multi-pack) | 1 | $9 | [amazon.com/dp/B089JYBF25](https://www.amazon.com/dp/B089JYBF25) | 1S → 5.0 V. **Set the voltage with a multimeter BEFORE connecting.** |
| ☐ | IRLZ44N logic-level MOSFET, 10-pack | 1 | $10 | [amazon.com/dp/B0CBKH4XGL](https://www.amazon.com/dp/B0CBKH4XGL) | One per motor; ~220 Ω gate resistor + 10 kΩ pull-down. ~2 g each (TO-220). Test on arrival: cheap packs are often relabeled. |
| ☐ | CHANZON 1N5819 Schottky diode (DO-41), 100 pcs | 1 | $6 | [amazon.com/dp/B079KG1TN2](https://www.amazon.com/dp/B079KG1TN2) | Flyback diode across each motor. ~0.35 g each. ASIN found on only one third-party page: check the title. |

## Amazon: transmitter and breadboarding
| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | ELEGOO UNO R3 Super Starter Kit | 1 | $44 | [amazon.com/dp/B01D8KOZF4](https://www.amazon.com/dp/B01D8KOZF4) | Uno for the transmitter; breadboard, jumpers, resistors, LEDs, buttons, buzzer, 9 V battery. **No capacitors, and only 10 female-to-male jumpers.** |
| ☐ | Teyleten KY-023 joystick, 5-pack | 1 | $10 | [amazon.com/dp/B0CPFDKWZF](https://www.amazon.com/dp/B0CPFDKWZF) | 2 sticks: throttle/yaw + pitch/roll. |
| ☐ | ELEGOO 120-piece jumper wire kit (40 M-F, 40 M-M, 40 F-F) | 1 | $7 | [amazon.com/dp/B01EV70C78](https://www.amazon.com/dp/B01EV70C78) | The nRF24L01's 2×4 header can't plug into a breadboard, so each radio needs 7 female-to-male jumpers. |

## Amazon: safety and tools
| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | Zeee LiPo safe bag | 1 | $12 | [amazon.com/dp/B07KZQZBN2](https://www.amazon.com/dp/B07KZQZBN2) | Charge and store the batteries in it. |
| ☐ | AstroAI AM33D digital multimeter | 1 | $18 | [amazon.com/dp/B01ISAMUA6](https://www.amazon.com/dp/B01ISAMUA6) | Skip if you own one. Sets the MT3608, checks polarity and shorts, measures motor current (10 A range). |
| ☐ | YIHUA 926 III 60 W soldering station kit (926LED IIIBK) | 1 | $44 | [amazon.com/dp/B082F1WKP9](https://www.amazon.com/dp/B082F1WKP9) | Temperature-controlled. The kit includes 2 helping hands, a solder sucker, tweezers, 6 tips, a sponge and lead-free solder. |
| ☐ | MG Chemicals 836-P no-clean flux pen | 1 | $11 | [amazon.com/dp/B01ELUI19Q](https://www.amazon.com/dp/B01ELUI19Q) | Makes solder flow onto pads and wire. |
| ☐ | MG Chemicals 425-NS desoldering wick, #3 | 1 | $6 | [amazon.com/dp/B00424S2C8](https://www.amazon.com/dp/B00424S2C8) | Removes solder bridges. ASIN found on only one third-party page: check the title. |
| ☐ | Klein Tools 11047 wire stripper, 22–30 AWG | 1 | $20 | [amazon.com/dp/B000I1L7GW](https://www.amazon.com/dp/B000I1L7GW) | ASIN found on only one third-party page: check the title. |
| ☐ | Hakko CHP-170 micro flush cutter | 1 | $7 | [amazon.com/dp/B00FZPDG1K](https://www.amazon.com/dp/B00FZPDG1K) | Trims component leads. |
| ☐ | Neiko 01407A digital caliper, 150 mm | 1 | $18 | [amazon.com/dp/B000GSLKIW](https://www.amazon.com/dp/B000GSLKIW) | Press-fit holes, motor diameter, shaft and prop bore. |
| ☐ | AMIR KA25 pocket scale, 200 g × 0.01 g | 1 | $12 | [amazon.com/dp/B07HGWFBTV](https://www.amazon.com/dp/B07HGWFBTV) | Weight budget and thrust test. ASIN found on only one third-party page: check the title. |

## UIUC campus
| Status | Item | Qty | Est. | Where | Notes |
|---|---|---|---|---|---|
| ☐ | PETG printing (frame, guards, test rig, transmitter case) | ~150 g | $0.04/g | JIS, MEB 0100 | Print before Dec 10 (JIS closes for breaks). |
| ☐ | M2/M3 screws, locknuts, washers | 1 lot | ~$2 | JIS, MEB 0100 | Board mounting, rig pivots. |
| ☐ | 470 µF 25 V electrolytic cap | 2 | $0.49 | ECE Supply, ECEB 1031 | Across the motor power bus, close to the MOSFETs. |
| ☐ | 10 µF 25 V electrolytic cap | 4 | $0.34 | ECE Supply | One per radio, one for bootloader programming, one spare. Stock no. 180301501. |
| ☐ | 0.1 µF 50 V ceramic cap | 6 | $0.51 | ECE Supply | One per radio, one on the battery-sense pin, spares. Stock no. 181201250. |
| ☐ | 26 AWG stranded hook-up wire, 25 ft spools, black + red | 2 spools | $11.33 total | ECE Supply | Stranded, not solid: solid wire cracks from vibration. |
| ☐ | Perfboard, 5 × 7 cm | 2 | $1.98 | ECE Supply | One for the quad (cut to size after layout), one for practice. |
| ☐ | Mini USB to USB-A cable | 1 | $3.46 | ECE Supply | To program the Nanos. |
| ☐ | Safety glasses, ANSI Z87.1 | 1 | $3.55 | ECE Supply | Get a second pair if anyone will watch. |
| ☐ | Heat-shrink tubing, 3/32 in and 1/8 in | 4 pieces | $1.54 total | ECE Supply | Battery leads and motor joints. |
| ☐ | 60/40 rosin-core solder, 1 oz | 1 | $6.82 | ECE Supply | Easier to learn with than the lead-free solder in the station kit. |

ECE Supply is open Mon–Fri, 9–noon and 1–3:30. Stock numbers and prices were read from its online catalog on Nov 12.

## Optional (not in the totals)
| Item | Est. | Why it's optional |
|---|---|---|
| QuadHands Classic helping hands | $30 | The soldering station kit already includes 2 helping hands. |
| Hakko 599B brass-wool tip cleaner | $10 | The station kit includes a cleaning sponge. |
| iFlight battery straps, 10-pack | $8 | A rubber band or the printed tray holds the battery and is lighter. |
| MBR360G Schottky diodes (ECE) | $3 | Was a backup for the 1N5819, which now comes from Amazon. |

## From home (check you have these before break)
None of these are on the shopping list, but the build needs them:
- A small flat screwdriver for the MT3608's adjustment screw, and a screwdriver or hex keys for the M2/M3 screws
- A lighter or hair dryer to shrink heat-shrink tubing
- A utility knife or small saw to cut the perfboard
- Double-sided foam tape (soft-mounts the IMU), rubber bands, electrical tape
- A USB wall adapter for the battery charger, and a tile or metal tray to charge on
- String or fishing line for the tether stage; a small fan for solder fumes

## Weight budget (target ≤ 80 g all-up)
Estimates are typical values for these part types, not measurements. Fill in "Measured" with the 0.01 g scale as parts arrive.

| Part | Estimate | Measured |
|---|---|---|
| 4× 8520 motors | ~20 g | |
| 1S 600 mAh LiPo | ~17 g | |
| PETG frame | ~15 g (10–25) | |
| Prop guards | ~11 g (8–16) | |
| 4× IRLZ44N (TO-220) | ~7.5 g | |
| Arduino Nano, with headers | ~7 g | |
| Wire, solder, heat shrink | ~4.5 g | |
| Perfboard, cut to size | ~4 g | |
| 4× 75 mm props | ~2.5 g | |
| MPU-6050 (GY-521) | ~2 g | |
| nRF24L01+ | ~2 g | |
| MT3608 | ~2 g | |
| Screws + rubber band | ~1.8 g | |
| 470 µF cap | ~1.6 g | |
| Resistors + small caps | ~1.5 g | |
| 4× 1N5819 | ~1.4 g | |
| AMS1117-3.3 module | ~1 g | |
| JST pigtail | ~0.8 g | |
| **Total** | **~92 g without guards, ~103 g with** | |

The estimate is over the target, so the thrust test (#14) decides the real weight limit. Where grams can come off, cheapest first: design the frame and guards to a gram limit → SOT-23 MOSFETs on adapter boards in place of the TO-220s (about 6–7 g) → a smaller battery → a 3.3 V/8 MHz Pro Mini in place of the Nano, boost converter and regulator (about 8 g, but a bigger change that also needs the SOT-23 MOSFETs).
