# Arduino micro brushed quadcopter (custom flight controller)

Project context and conventions. Personal preferences and private links are in the git-ignored `CLAUDE.local.md`.

## Goal
A ~80 g (under 250 g) brushed quad: PETG frame I design in SolidWorks, flight code I write myself (PID on an Arduino Nano), and a transmitter I build (Uno + joysticks + nRF24L01). Indoor only. Done means a stable hover on my own firmware, documented (CAD, wiring diagram, code, video), before Jan 19, 2027.

## Conventions
- Tasks are GitHub Issues in `nicholassiu1/arduino-micro-quadcopter`: one milestone per `TIMELINE.md` phase, labels purchasing / hardware / firmware / cad / safety / docs. Use `gh`. Put `Closes #N` in the commit that finishes an issue.
- `PLAN.md` logs decisions (with reasons), problems, lessons and open questions.
- Small focused commits with an imperative summary and a "why" body when needed. Push at the end of a session. Firmware features go on a branch (e.g. `firmware/radio-failsafe`) and merge through a PR. Tag releases at milestones (`v0.1-breadboard`, `v1.0-first-hover`). Later: a GitHub Actions compile check.
- Public text (docs, README, issues, commits) is in my voice: "I"/"my", never "you". No em or en dashes, bold only for real warnings or named concepts, no decorative arrows or emoji. Run the humanizer skill on it before committing.
- Design math goes in `docs/design-calcs.md`, test data in `docs/data/`. Replace estimates with measurements as they come in.
- No secrets or private info in git; large media stays out of git.
- Arduino IDE for the Nano and the Uno.
- Verify on real hardware (serial output, test procedure, multimeter) and never claim untested code works. Walk me through safety code (arming, failsafe, motor output) before it goes on the quad. Say when a value is an estimate or needs a datasheet check.

## Files
`PLAN.md` status and logs · `TIMELINE.md` dated schedule · `SAFETY.md` safety rules and staged tests · `parts.md` BOM snapshot (the Google Sheet is the source of truth: update it first) · `docs/design-calcs.md` · `README.md`

## Hardware
Nano · MPU-6050 · nRF24L01+ · 4× 8520 motors + 75 mm props · 1S 600 mAh LiPo · MT3608 boost to 5 V · AMS1117-3.3 for the radio · IRLZ44N + 1N5819 per motor · Uno + 2 KY-023 joysticks. Details in `parts.md`.

## Technical constraints
- **Weight is the biggest risk.** ~120-130 g thrust (seller claim; issue #14 measures it) means all-up weight ≤ ~80 g for T/W 1.5-2.
- The 16 MHz ATmega328P isn't rated for 1S, so the Nano runs from the MT3608 at **5.0 V**. Set it with a multimeter before connecting.
- Each motor: logic-level MOSFET, ~220 Ω gate resistor, 10 kΩ pull-down, flyback diode. A pin can't drive a >1 A motor.
- The radio runs from the AMS1117-3.3 on the 5 V rail (the CH340 Nano's 3V3 pin can't supply transmit spikes), with 10 µF + 0.1 µF across VCC/GND.
- **Set every motor pin LOW before `radio.begin()`**: `SPI.begin()` drives D10 (SS) HIGH unless it's already an output.
- **USB and LiPo together only with props off.** With props on, data comes back in the radio's ack payload.
- Required before props go on: motors off at boot, arming gesture, throttle-low check, 250 ms radio failsafe, kill switch, tilt cutoff, low-battery warning, watchdog. See `SAFETY.md`.
- Frame: ~120 mm motor-to-motor diagonal; motor holes ~0.2 mm oversize for 8.5 mm motors (test print first); battery tray for 57×18×7.6 mm.
- Fallback flight code: MultiWii (same chip, supports brushed motors).

## Rules
- FAA: pass the free TRUST test. Under 250 g recreational needs no registration or Remote ID; indoor flight isn't FAA airspace. Most of Champaign-Urbana is controlled airspace.
- UIUC FO-05: flying on campus property needs Division of Public Safety permission, so I fly at home over break.

## Campus resources
- JIS (Sidney Lu MEB 0100): PLA $0.03/g, PETG $0.04/g, metric hardware, card only, no personal filament, closed weekends and breaks.
- ECE Supply Center (ECEB 1031): in person, Mon-Fri 9-noon and 1-3:30. Catalog with prices and stock, no login: my.ece.illinois.edu/storeroom/catalog.asp.
- SolidWorks: free at webstore.illinois.edu (current-year student license, Windows only).
