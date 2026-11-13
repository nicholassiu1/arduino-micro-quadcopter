# Parts list

> The Google Sheet (linked in `CLAUDE.local.md`) is the source of truth. This file is a snapshot for the public repo, last synced on Nov 13, 2026, and may lag behind it. The Sheet also tracks each item's status and delivery date.

Prices are estimates from planning (Nov 2026), to be replaced with the real cart and receipt prices.
Every online order ships home, due by Sat Nov 21. Campus items I carry home on Nov 20.
Totals (match the Sheet): Amazon ~$193, AliExpress ~$13, UIUC ~$51, Bambu Lab ~$19, Target ~$199, ~$475 together (skipped items and JIS PETG not counted). The printer and PETG are $218 of that; the quad itself is ~$257.
Status key: ☐ not bought · 🛒 ordered · ✅ in hand · ✔ tested

## Amazon: quadcopter
| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | HiLetgo GY-521 MPU-6050 IMU, 3-pack | 1 | $11 | [amazon.com/dp/B00LP25V1A](https://www.amazon.com/dp/B00LP25V1A) | Gyro + accelerometer over I2C. |
| ☐ | HiLetgo nRF24L01+ radio, 4-pack (PCB antenna) | 1 | $7.89 | [amazon.com/dp/B00LX47OCY](https://www.amazon.com/dp/B00LX47OCY) | One on the quad, one on the transmitter, two spares. Replaces the MakerFocus 10-pack, unavailable on Nov 13. 3.3 V only; 22 µF + 0.1 µF caps across VCC/GND. Amazon listing checked Nov 13: 4 pcs, nRF24L01+, 1.9-3.6 V, printed zigzag antenna. Use two from the same pack. |
| ☐ | HiLetgo AMS1117-3.3 regulator module, 10-pack (800 mA) | 1 | $7 | [amazon.com/dp/B01HXU1NQY](https://www.amazon.com/dp/B01HXU1NQY) | Powers the nRF24L01 from the 5 V rail. The Nano's 3V3 pin can't supply the radio's transmit spikes. ~1 g. |
| ☐ | Hobbypower 8520 coreless motors (8) + 75 mm props | 1 | $16 | [amazon.com/dp/B076M7G24G](https://www.amazon.com/dp/B076M7G24G) | 4 to fly + 4 spares, with 8 props (4 to fly + 4 spares). Confirm the listing says (8) motors: the same ASIN is a 4-motor set on some non-US sites. Thrust is unmeasured (the ~30 g each is a seller claim). |
| ☐ | URGENEX 1S 3.7 V 600 mAh LiPo, 5-pack + USB charger | 1 | $27 | [amazon.com/dp/B08XZM3NG3](https://www.amazon.com/dp/B08XZM3NG3) | ~18 g each, 57×18×7.6 mm, 25C per a third-party listing. Confirm the plug is the red 2-pin JST-RCY. |
| ☐ | 2-pin JST (RCY) pigtails, 20 pairs, 22 AWG | 1 | $8 | [amazon.com/dp/B013WTV270](https://www.amazon.com/dp/B013WTV270) | Battery lead. Check plug gender and polarity. Spare pigtails supply the 22 AWG wire for the power bus. |
| ☐ | MT3608 DC-DC boost converter (multi-pack) | 1 | $9 | [amazon.com/dp/B089JYBF25](https://www.amazon.com/dp/B089JYBF25) | Boosts 1S to 5.0 V. Set the voltage with a multimeter before connecting it. |
| ☐ | IRLZ44N logic-level MOSFET, 10-pack | 1 | $10 | [amazon.com/dp/B0CBKH4XGL](https://www.amazon.com/dp/B0CBKH4XGL) | One per motor; ~220 Ω gate resistor + 10 kΩ pull-down. ~2 g each (TO-220). Test on arrival: cheap packs are often relabeled. |
| ☐ | CHANZON 1N5819 Schottky diode (DO-41), 100 pcs | 1 | $6 | [amazon.com/dp/B079KG1TN2](https://www.amazon.com/dp/B079KG1TN2) | Flyback diode across each motor. ~0.35 g each. ASIN found on only one third-party page: check the title. |

## Amazon: transmitter and breadboarding
| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | Teyleten KY-023 joystick, 5-pack | 1 | $10 | [amazon.com/dp/B0CPFDKWZF](https://www.amazon.com/dp/B0CPFDKWZF) | 2 sticks: throttle/yaw + pitch/roll. |
| ☐ | ELEGOO 120-piece jumper wire kit (40 M-F, 40 M-M, 40 F-F) | 1 | $7 | [amazon.com/dp/B01EV70C78](https://www.amazon.com/dp/B01EV70C78) | The nRF24L01's 2×4 header can't plug into a breadboard, so each radio needs 7 female-to-male jumpers. The M-M ones wire the transmitter breadboard. |

The transmitter is the spare Nano on a breadboard, not an Uno (see `PLAN.md`). Its breadboard, buzzer and buttons come from ECE Supply; its LEDs and LED resistors come from the IOLab pack.

## Amazon: safety and tools
| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | YIHUA 926 III 60 W soldering station kit (926LED IIIBK) | 1 | $44 | [amazon.com/dp/B082F1WKP9](https://www.amazon.com/dp/B082F1WKP9) | Temperature-controlled. The kit includes 2 helping hands, a solder sucker, tweezers, 6 tips, a sponge and lead-free solder. Campus soldering benches don't lend tools to take home, and the build happens at home over break. |
| ☐ | Neiko 01407A digital caliper, 150 mm | 1 | $18 | [amazon.com/dp/B000GSLKIW](https://www.amazon.com/dp/B000GSLKIW) | Press-fit holes, motor diameter, shaft and prop bore. Harbor Freight's $11.99 composite caliper reads only to 0.1 mm, too coarse for 0.2 mm clearances. |
| ☐ | AMIR KA25 pocket scale, 200 g × 0.01 g | 1 | $12 | [amazon.com/dp/B07HGWFBTV](https://www.amazon.com/dp/B07HGWFBTV) | Weight budget and thrust test. ASIN found on only one third-party page: check the title. |

## AliExpress
Ships from China. I check the delivery date at checkout: it has to arrive by Nov 21.

| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | Nano V3.0 clone (ATmega328P, CH340), Mini USB version | 5 | $1.34 each | [aliexpress.us/item/3256805941736729.html](https://www.aliexpress.us/item/3256805941736729.html) | One flies, one runs the transmitter, three spares. A spare may become the radio-to-PPM bridge for the MultiWii fallback (#19). Pick the Mini USB variant (the ECE Mini-B cable fits it). Variant prices may differ from the $1.34 shown on Nov 12. If it won't arrive by Nov 21, I buy two Amazon ELEGOO 3-packs instead. |
| ☐ | AO3400 / AO3400A N-channel MOSFET, SOT23-3, 50 pcs | 1 | $1.63 | [aliexpress.us/item/2251832324745822.html](https://www.aliexpress.us/item/2251832324745822.html) | Weight backup for the IRLZ44N: 30 V, 5 A, rated for 2.5 V gate drive. ~215 orders, against 22 for the first listing I picked. Replaces an Amazon pack that was unavailable on Nov 13. Fakes are common, so the MOSFET check on Nov 26-27 decides. |
| ☐ | SMD to DIP adapter boards, 10 pcs, option "SOP10(SOT23)" | 1 | $0.64 | [aliexpress.us/item/3256803315588878.html](https://www.aliexpress.us/item/3256803315588878.html) | Double-sided: SOT-23 on one side, SOP10 on the other. Bigger than a SOT-23-only board (maybe 0.5-1 g each, estimate), so weigh on arrival and trim. Check which pads gate, source and drain land on. |
| ☐ | No-clean rosin flux pen, 10 mL | 1 | $3.87 | [aliexpress.us/item/3256809270180694.html](https://www.aliexpress.us/item/3256809270180694.html) | Only for the SOT-23 MOSFETs (0.95 mm pin pitch). Replaces the $11 MG Chemicals pen. Never use plumbing flux: it is acid flux and corrodes electronics. |

The three SOT-23 parts are prices without the new-user deal (Nov 13). Every item's delivery date must be on or before Nov 21.

## Target
My own printer, shipped home by Nov 21.

| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | Bambu Lab A1 mini 3D printer (not the AMS lite Combo) | 1 | ~$199 | [target.com/p/bambu-lab-a1-mini-3d-printer/-/A-1010019390](https://www.target.com/p/bambu-lab-a1-mini-3d-printer/-/A-1010019390) | 180 × 180 × 180 mm build volume, PETG and TPU listed as ideal, 0.4 mm nozzle and textured PEI plate included. Guards molded into the frame would be about 170 mm square (estimate), so I'd print the guards separately. 5.5 kg; it stays with me once it arrives, breaks included. $199 is the Bambu store sale price on Nov 12 (list $219); the Target price isn't checked yet. The box has only a 20 g filament sample. |

## Bambu Lab store
Target doesn't sell PETG or spools, so the filament comes from here.

| Status | Item | Qty | Est. | Link | Notes |
|---|---|---|---|---|---|
| ☐ | Bambu PETG Basic, 1.75 mm, 1 kg, "Filament with spool" | 1 | ~$19 | [us.store.bambulab.com/products/petg-basic](https://us.store.bambulab.com/products/petg-basic) | The final parts are ~150 g; with test prints and reprints, ~200-400 g (estimate). The Refill option ($13.99) has no spool, and a reusable spool costs $11.99. With-spool price not confirmed. Not PETG HF, which is discontinued. Free shipping only on filament orders over $59, so expect a shipping charge. |

## UIUC campus
| Status | Item | Qty | Est. | Where | Notes |
|---|---|---|---|---|---|
| ☐ | PETG printing (frame, guards, test rig, transmitter case) | ~150 g | not priced | JIS, MEB 0100 | Not in the total. Backup for my A1 mini; the thrust-test fixture prints at school before Nov 20. |
| ☐ | M2 screws, length from the frame CAD (about 8-12 mm) | 12 | ~$0.10 each | JIS, MEB 0100 | Dec run, after the frame CAD. A 3 mm plate + 1.6 mm board + ~2.8 mm locknut is already 7.4 mm, so 8 mm is likely too short (estimate). 4 for the flight board (one per corner), 4 for the prop guards (one per arm), 4 spares. |
| ☐ | M2 nylon-insert locknuts | 12 | ~$0.10 each | JIS, MEB 0100 | Dec run. One per M2 screw; the nylon insert stops vibration from loosening them. |
| ☐ | M2 flat washers | 8 | ~$0.05 each | JIS, MEB 0100 | Dec run. Under the nuts on the PETG side so tightening doesn't crush the print. Skip if weight is tight. |
| ☐ | M3 × 25 mm screws | 2 | ~$0.15 each | JIS, MEB 0100 | Dec 3-9 run, after the rig CAD sets the length. Test rig pivot axles. |
| ☐ | M3 nylon-insert locknuts | 2 | ~$0.10 each | JIS, MEB 0100 | Dec run. Snug only, so the quad still swings on the pivot. |
| ☐ | M3 flat washers | 4 | ~$0.05 each | JIS, MEB 0100 | Dec run. Two per axle, between the rig upright and the quad. |
| ☐ | 0.1 µF 50 V ceramic cap | 8 | $0.51 | ECE Supply, ECEB 1031 | One per radio, one on the battery-sense pin, 4 for optional noise caps across the motors (only if motor noise resets the Nano), 1 spare. The IOLab pack has no ceramic caps. Stock no. 181201250. |
| ☐ | 26 AWG stranded hook-up wire, 25 ft spools, black + red | 2 spools | $11.33 total | ECE Supply | Stranded, not solid: solid wire cracks from vibration. |
| ☐ | Perfboard, 5 × 7 cm | 2 | $1.98 | ECE Supply | One for the quad (cut to size after layout), one for practice. |
| ☐ | Mini USB to USB-A cable | 2 | $3.46 each | ECE Supply | Two, so the transmitter Nano and the flight Nano can both be on USB during the radio link and range tests. Stock no. 900511500. |
| ☐ | Heat-shrink tubing, 3/32 in and 1/8 in | 4 pieces | $1.54 total | ECE Supply | Battery leads and motor joints. |
| ☐ | 60/40 rosin-core solder, 1 oz | 1 | $6.82 | ECE Supply | Easier to learn with than the lead-free solder in the station kit. |
| ☐ | Solderless breadboard, 2.2 × 6.5 in (103-1100) | 1 | $9.06 | ECE Supply | Transmitter Nano and its AMS1117. The IOLab pack's small breadboard is kept for flight-Nano bench tests. Stock no. 590116850. |
| ☐ | 1/4 W resistors: 6× 220 Ω, 6× 10 kΩ | 12 | $0.07 | ECE Supply | MOSFET gate resistors and pull-downs, 4 each plus 2 spares; one spare 220 Ω is the quad's status-LED resistor. Stock no. 150103000; confirm both values at the counter. |
| ☐ | Piezo buzzer, SBZ-204 | 1 | $1.62 | ECE Supply | Transmitter low-battery and failsafe warning. Stock no. 210142110. |
| ☐ | 5 mm red LED | 1 | $0.34 | ECE Supply | Status LED on the quad. Stock no. 30150250. |
| ☐ | 6 mm tactile push button | 2 | $0.43 | ECE Supply | Kill switch and arm button on the transmitter. Stock no. 620110710. |

ECE Supply is open Mon-Fri, 9-noon and 1-3:30. Stock numbers and prices come from its online catalog.

## Already own
My own safety glasses, my dad's LiPo safe bag and multimeter, plus the IOLab E&M small accessory pack. The pack's contents come from the Macmillan accessory page; check the bag before the ECE run.

| Item | Qty used | Job |
|---|---|---|
| Safety glasses | 1 | Every test with props on, and soldering. Check they're marked Z87 (ANSI Z87.1). Get a second pair if anyone will watch. |
| LiPo safe bag (my dad's) | 1 | Charge and store the batteries in it, never unattended, on a non-flammable surface. Check for holes or torn seams before the first charge. |
| Digital multimeter (my dad's) | 1 | MT3608 setup, shorts and continuity, MOSFET check, motor current in the thrust test (#14). Checked Nov 13: it has the ranges I need, including 10 A DC. Keep 10 A readings short. |
| 220 µF 25 V electrolytic cap | 1 (+1 spare) | Motor power bus, close to the MOSFETs. Replaces the 470 µF: a typical size for a micro brushed quad (estimate) and ~0.6 g lighter (estimate). If the Nano resets under motor load in the Nov 25 test, add the second in parallel. |
| 22 µF 25 V electrolytic cap | 2 | One per radio, beside a 0.1 µF ceramic. Anything from 10 µF up works. |
| 56 µF 25 V electrolytic cap | 1 | Reset-pin cap on the programmer Nano, only if a Nano needs Optiboot (#18). |
| 1 kΩ resistor | 2 | Transmitter LED resistors. |
| 10 kΩ + 4.7 kΩ resistors | 2× 10 kΩ, 1× 4.7 kΩ | Battery sense: both 10 kΩ in series on top, the 4.7 kΩ to GND, read on the 1.1 V internal reference (`docs/design-calcs.md` section 5). One 4.7 kΩ is spare. |
| 5 mm LEDs, red + green | 2 | Transmitter armed and link status. |
| Small prototype breadboard | 1 | Flight-Nano bench tests (radio link, one motor driver). Size not listed: check a Nano fits with room for the MOSFET stage. If it's a 170-point mini board, buy the ECE 2.1 × 3.6 in board (590116750, $8.44). |
| Alligator-clip leads | 10 | Thrust test (#14) and MT3608 setup without soldering. |
| Wire stripper | 1 | Must strip 26 AWG stranded wire without cutting strands. Try it on a scrap of the ECE wire. |
| Flush cutter | 1 | Trims component leads. Safety glasses on: clipped ends fly. |

The pack has no jumper wires, ceramic caps, 220 Ω resistors, buzzer or buttons, so those stay on the list. Its 4 silicon diodes and magnet wire aren't used: the 1N5819 Schottky diodes from Amazon are the flyback diodes.

## Skipped (not in the totals)
| Item | Est. | Why it's skipped |
|---|---|---|
| GRASKY 75 mm props, 20 pcs ([B0CNP12BFM](https://www.amazon.com/dp/B0CNP12BFM)) | $8 | Unavailable on Nov 13. The motor set includes 8 props (4 to fly, 4 spares). I reorder any 75 mm, 1 mm hole, CW + CCW pack once I'm down to 2 spares. |
| ELEGOO Nano V3.0, 3-pack ([B0713XK923](https://www.amazon.com/dp/B0713XK923)) | $19 | Swapped for 5 AliExpress clones (~$7). Backup if the AliExpress order won't arrive by Nov 21 (two packs for 5 Nanos). |
| ELEGOO UNO R3 Super Starter Kit | $44 | The spare Nano runs the transmitter. The parts I needed from the kit come from ECE Supply (~$12) and the IOLab pack. |
| AstroAI AM33D multimeter | $18 | My dad's multimeter covers it. |
| Harbor Freight Cen-Tech multimeter (item 59434) | ~$5 | My dad's multimeter covers it. |
| MG Chemicals 425-NS desoldering wick | $6 | The station kit's solder sucker clears through-hole joints. ECE sells wick for $8.85 if I need it. |
| QuadHands Classic helping hands | $30 | The soldering station kit already includes 2 helping hands. |
| Hakko 599B brass-wool tip cleaner | $10 | The station kit includes a cleaning sponge. |
| iFlight battery straps, 10-pack | $8 | A rubber band or the printed tray holds the battery and is lighter. |
| MBR360G Schottky diodes (ECE) | $3 | Was a backup for the 1N5819, which now comes from Amazon. |

Not worth switching: Tayda nRF24L01 modules (~$1 each, but express shipping from Thailand likely costs ~$10 and may not arrive by Nov 20), ECE's own Uno R3 ($46.49), and a 0.01 g scale from Walmart.com (~$8, unknown brand).

## Check at home (not confirmed yet)
The build needs these, but I haven't checked whether I have them. They're not in the totals. They're at home, so I check before Nov 21 and buy the fallback for anything missing. The charging surface and USB adapter are needed Nov 21, the screwdriver Nov 22. Fallback prices haven't been checked.

| Status | Item | For | Fallback if missing |
|---|---|---|---|
| ? | Precision screwdrivers (small flat ~2 mm, Phillips #0) | MT3608 trim pot; M2 pan-head screws (socket head takes a 1.5 mm hex key) | Harbor Freight precision set, ~$5 |
| ? | Heat source for heat-shrink | A hair dryer usually isn't hot enough. A lighter works if kept moving, away from the battery. | The soldering iron: rub the side of the hot barrel on the tubing, $0 |
| ? | Utility knife | Score the perfboard along a row of holes on both sides, then snap | Harbor Freight snap-off knife, ~$2 |
| ? | Double-sided foam tape, rubber bands, electrical tape | IMU soft-mount, battery hold-down, insulation | Walmart or Target, ~$6 together |
| ? | USB wall adapter, 5 V, 1 A+ | Battery charger (a phone charger works) | Walmart or Target, ~$5 |
| ? | Ceramic tile, baking sheet or metal tray | Non-flammable charging surface | A spare floor tile, ~$1 |
| ? | USB power bank | Transmitter Nano. Check it doesn't switch off at the Nano's small load. | Walmart or Target, ~$12 |
| ? | String or fishing line, small desk fan | Tethered hover; solder fumes | Walmart or Target, ~$3 string |
| ? | Gallon zip-top bag + silica desiccant packs | Keeps the opened PETG dry (wet PETG prints stringy and weak) | Walmart or Target, ~$5 |

## Weight budget (target ≤ 80 g all-up)
The estimates are typical values for these part types. Nothing has been weighed yet; the Measured column gets filled in from the 0.01 g scale as parts arrive.

| Part | Estimate | Measured |
|---|---|---|
| 4× 8520 motors | ~20 g | |
| 1S 600 mAh LiPo | ~17 g | |
| PETG frame | ~15 g (10-25) | |
| Prop guards | ~11 g (8-16) | |
| 4× IRLZ44N (TO-220) | ~7.5 g | |
| Arduino Nano, with headers | ~7 g | |
| Wire, solder, heat shrink | ~4.5 g | |
| Perfboard, cut to size | ~4 g | |
| 4× 75 mm props | ~2.5 g | |
| MPU-6050 (GY-521) | ~2 g | |
| nRF24L01+ | ~2 g | |
| MT3608 | ~2 g | |
| Screws + rubber band | ~1.8 g | |
| 220 µF cap | ~1 g | |
| Resistors + small caps | ~1.5 g | |
| 4× 1N5819 | ~1.4 g | |
| AMS1117-3.3 module | ~1 g | |
| JST pigtail | ~0.8 g | |
| **Total** | **~92 g without guards, ~103 g with** | |

The estimate is over the target, so the thrust test (#14) decides the real weight limit. Where grams can come off, cheapest first:
1. Design the frame and guards to a gram limit.
2. SOT-23 MOSFETs on adapter boards in place of the TO-220s (about 6-7 g). The AO3400s, adapters and flux pen are in the AliExpress order, so this needs no reorder. The adapters are bigger than SOT-23-only boards, so the real saving gets measured.
3. A smaller battery.
4. A 3.3 V/8 MHz Pro Mini in place of the Nano, boost converter and regulator (about 8 g, but a bigger change that also needs the SOT-23 MOSFETs).
