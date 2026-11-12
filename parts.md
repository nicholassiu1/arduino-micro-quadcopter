# Parts list

Prices are estimates from planning (Nov 2026); update them from the real cart and receipts.
**Totals:** Amazon ~$179 · UIUC ~$21.70 · **~$200.70** (not counting the optional soldering iron).

Status key: ☐ not bought · 🛒 ordered · ✅ in hand · ✔ tested

## Amazon
| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | ELEGOO Nano V3.0 (ATmega328P), 3-pack | 1 | $19 | [amazon.com/dp/B0713XK923](https://www.amazon.com/dp/B0713XK923) | Flight controller + 2 spares. No USB cable (needs a Mini-B *data* cable). |
| ☐ | HiLetgo GY-521 MPU-6050 IMU, 3-pack | 1 | $11 | [amazon.com/dp/B00LP25V1A](https://www.amazon.com/dp/B00LP25V1A) | Gyro + accelerometer over I2C. |
| ☐ | MakerFocus nRF24L01+ radio, 10-pack | 1 | $13 | [amazon.com/dp/B00O9O868G](https://www.amazon.com/dp/B00O9O868G) | One on the quad, one on the transmitter. 3.3 V only; 10 µF cap across VCC/GND. |
| ☐ | Hobbypower 8520 coreless motors (8) + 75 mm props | 1 | $16 | [amazon.com/dp/B076M7G24G](https://www.amazon.com/dp/B076M7G24G) | 4 to fly + 4 spares. ~30 g thrust each. |
| ☐ | URGENEX 1S 3.7 V 600 mAh LiPo, 5-pack + USB charger | 1 | $27 | [amazon.com/dp/B08XZM3NG3](https://www.amazon.com/dp/B08XZM3NG3) | ~18 g each, 57×18×7.6 mm, JST (RCY) plug. |
| ☐ | 2-pin JST (RCY) pigtails, 20 pairs, 22 AWG | 1 | $8 | [amazon.com/dp/B013WTV270](https://www.amazon.com/dp/B013WTV270) | Battery lead. Check plug gender and polarity. |
| ☐ | MT3608 DC-DC boost converter (multi-pack) | 1 | $9 | [amazon.com/dp/B089JYBF25](https://www.amazon.com/dp/B089JYBF25) | 1S → 5.0 V. **Set the voltage with a multimeter BEFORE connecting.** |
| ☐ | IRLZ44N logic-level MOSFET, 10-pack | 1 | $10 | [amazon.com/dp/B0CBKH4XGL](https://www.amazon.com/dp/B0CBKH4XGL) | One per motor; ~220 Ω gate resistor + 10 kΩ pull-down. ~2 g each (TO-220). |
| ☐ | ELEGOO UNO R3 Super Starter Kit | 1 | $44 | [amazon.com/dp/B01D8KOZF4](https://www.amazon.com/dp/B01D8KOZF4) | Uno for the transmitter; breadboard, jumpers, resistors, caps. |
| ☐ | Teyleten KY-023 joystick, 5-pack | 1 | $10 | [amazon.com/dp/B0CPFDKWZF](https://www.amazon.com/dp/B0CPFDKWZF) | 2 sticks: throttle/yaw + pitch/roll. |
| ☐ | Zeee LiPo safe bag | 1 | $12 | [amazon.com/dp/B07KZQZBN2](https://www.amazon.com/dp/B07KZQZBN2) | Charge and store the batteries in it. |

## UIUC campus
| Status | Item | Qty | Est. | Where | Notes |
|---|---|---|---|---|---|
| ☐ | PETG printing (frame, guards, test rig, transmitter case) | ~150 g | $0.04/g | JIS, MEB 0100 | Print before Dec 10 (JIS closes for breaks). |
| ☐ | M2/M3 screws, locknuts, washers | 1 lot | ~$2 | JIS, MEB 0100 | Board mounting. |
| ☐ | 1N5819 Schottky diodes | 4+ | ~$0.30 | ECE Supply, ECEB 1031 | Flyback diode per motor. Check stock; buy spares. |
| ☐ | 470 µF 10 V electrolytic cap | 2 | ~$0.75 | ECE Supply | Across the battery input. Check stock. |
| ☐ | Hook-up wire 24–26 AWG | 1 | ~$3 | ECE Supply | |
| ☐ | Small perfboard | 1 | ~$3 | ECE Supply | Carrier for the Nano + MOSFETs. Check stock. |
| ☐ | USB Mini-B **data** cable | 1 | ~$5 | ECE Supply | To program the Nanos. Check stock. |

## Optional
| Status | Item | Est. | Notes |
|---|---|---|---|
| ☐ | Soldering iron kit | ~$25 | Most soldering happens at home over break while JIS is closed. No verified link yet. |

## Weight budget (target ≤ 80 g all-up)
Fill in the "Measured" column with a kitchen scale as parts arrive.

| Part | Estimate | Measured |
|---|---|---|
| 4× 8520 motors | ~20 g | |
| 1S 600 mAh LiPo | ~18 g | |
| PETG frame | 15–25 g | |
| 4× IRLZ44N (TO-220) | ~8 g | |
| Wiring + perfboard | 8–10 g | |
| Arduino Nano | ~7 g | |
| nRF24L01 | 2–3 g | |
| MPU-6050 | ~2 g | |
| MT3608 | ~2 g | |
| **Total** | **~82–95 g** | |

If the measured total runs high, the fixes in order are: a thinner frame → SOT-23 MOSFETs on a small board → a smaller battery → a 3.3 V/8 MHz Pro Mini in place of the Nano + boost converter.
