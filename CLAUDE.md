# Arduino micro brushed quadcopter (custom flight controller)

Claude Code reads this file automatically. It holds the project context and conventions. My personal working preferences and private details (links, names) are in `CLAUDE.local.md`, which isn't committed.

## Project vision
Build a sub-250 g micro brushed quadcopter (aiming for ~80 g) with a 3D-printed PETG frame I design in SolidWorks. It flies on **flight controller code I write myself**: a PID control loop on an Arduino Nano (ATmega328P), flown with a handheld transmitter I also build (Arduino Uno + joysticks + nRF24L01 radio). It's flown indoors.

**Done means:** a stable, controlled indoor hover on my own firmware, documented (CAD, wiring diagram, code, video) for my portfolio, before spring semester starts (Jan 19, 2027).

## Working conventions
- **Tasks live in GitHub Issues** (repo `nicholassiu1/arduino-micro-quadcopter`), one milestone per phase of `TIMELINE.md`, labeled purchasing / hardware / firmware / cad / safety / docs. Use `gh` to read, create and close them. When a commit finishes an issue, put `Closes #N` in the commit message.
- Keep `PLAN.md` updated with decisions (with reasons), problems I ran into, lessons, and open questions.
- **Commits:** small and focused, one logical change each. A short imperative summary ("Add gyro calibration"), plus a body explaining *why* when it isn't obvious. Push at the end of a work session.
- **Branches and pull requests:** once firmware work starts, do each feature on a branch (e.g. `firmware/radio-failsafe`) and merge it through a PR with a short description.
- **Releases:** tag versions at key milestones (e.g. `v0.1-breadboard`, `v1.0-first-hover`).
- **Repo hygiene:** README kept current with photos and results, no secrets or private info committed, large media kept out of git. Later, a GitHub Actions workflow that checks the firmware compiles on every push.
- **Public text is in my voice.** Docs, the README, issues and commit messages say "I" and "my" and never address me as "you". Keep them plain: no em or en dashes, bold only for a real warning or a named concept, no arrows or emoji as decoration. Run the humanizer skill on new public text before committing it.
- **Record design calculations in `docs/design-calcs.md`** so they're reproducible, and replace estimates with measured values as tests happen.
- Toolchain: **Arduino IDE** for both the Nano (quad) and the Uno (transmitter).
- **Verify, don't trust:** after writing code, give a way to check it on real hardware (serial output, a test procedure, the multimeter). Never claim something works if it hasn't been tested. Safety-related code (arming, failsafe, motor output) gets walked through before it goes on the quad.
- **Be honest about uncertainty:** say when a value is an estimate or should be checked against a datasheet.

## Project files
| File | What it is |
|---|---|
| `PLAN.md` | Status tracker: done / next / decisions / problems / lessons |
| `TIMELINE.md` | Dated schedule, Nov 12 → Jan 17 |
| `SAFETY.md` | LiPo, prop, soldering and firmware safety; staged test procedure |
| `parts.md` | Snapshot of the bill of materials and weight budget. **The Google Sheet (link in `CLAUDE.local.md`) is the source of truth**: update it first. |
| `docs/design-calcs.md` | Design calculations: thrust-to-weight, weight budget, motor current, MOSFET choice |
| `docs/data/` | Test data (CSV) |
| `README.md` | Public description of the project |

## Hardware summary
Arduino Nano (flight controller) · MPU-6050 IMU · nRF24L01+ radios · 4× 8520 coreless brushed motors + 75 mm props · 1S 600 mAh LiPo · MT3608 boost to 5 V · IRLZ44N MOSFET + 1N5819 flyback diode per motor · Uno + 2 KY-023 joysticks as the transmitter. Full list in `parts.md`.

## Technical constraints (remember these when writing code or advice)
- **Weight is the biggest risk.** ~120–130 g total thrust → all-up weight ≤ ~80 g (thrust-to-weight 1.5–2:1). The thrust figure is a seller's claim; the measured value from issue #14 replaces it.
- The ATmega328P at 16 MHz isn't rated for 1S voltage (3.3–4.2 V), so the Nano runs from the MT3608 set to **5.0 V** (set it with a multimeter *before* connecting).
- An Arduino pin can't drive an 8520 motor (>1 A). Each motor gets a logic-level MOSFET, a ~220 Ω gate resistor, a 10 kΩ gate pull-down and a flyback diode.
- The nRF24L01 runs from an **AMS1117-3.3 regulator** fed by the 5 V rail (not the Nano's 3V3 pin, which on CH340 Nanos can't supply the radio's transmit spikes), with 10 µF + 0.1 µF caps across VCC/GND. The firmware must have a **radio failsafe** (no packet for 250 ms → motors off).
- **Set every motor pin LOW before `radio.begin()`.** D10 is the SPI slave-select pin, and `SPI.begin()` drives it HIGH unless it's already an output.
- **USB and LiPo together only with props off.** With props on, data comes back over the radio (acknowledgement payload), not a USB cable.
- Safety features required in firmware before props go on: motors off at boot, arming gesture, throttle-low check, failsafe, kill switch, tilt cutoff, low-battery warning, watchdog. See `SAFETY.md`.
- Frame: ~120 mm diagonal motor-to-motor; press-fit motor holes ~0.2 mm oversize for 8.5 mm motors (print a test mount first); battery tray for 57×18×7.6 mm.
- Fallback if my own flight code stalls: **MultiWii** (runs on the same chip and supports brushed motors).

## Rules
- FAA: must pass the free TRUST test (on the to-do list in `PLAN.md`). Under 250 g recreational → no registration or Remote ID. Indoor flight isn't in FAA airspace. Most of Champaign-Urbana is controlled airspace.
- UIUC policy FO-05: need permission from the Division of Public Safety before flying on campus property. Plan to fly at home over break.

## Campus resources
- **JIS, Sidney Lu MEB 0100:** metric hardware; PLA $0.03/g, PETG $0.04/g; card only; no personal filament; **closed weekends and breaks**.
- **ECE Supply Center, ECEB 1031:** sells to students, in person only; open Mon–Fri 9–noon and 1–3:30. Its online catalog (my.ece.illinois.edu/storeroom/catalog.asp) shows prices and stock without a login. Has wire, capacitors, perfboard, USB cables, jumper wires, metric hardware; 1N5819 diodes were out of stock on Nov 12.
- **SolidWorks:** free via webstore.illinois.edu. Get the current-year student license (the 7/31/2026 one has expired). Windows only.
