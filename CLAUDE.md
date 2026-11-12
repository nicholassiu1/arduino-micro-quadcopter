# Arduino micro brushed quadcopter (custom flight controller)

Claude Code reads this file automatically. It holds the project context and my working preferences. Private details (links, names) are in `CLAUDE.local.md`, which isn't committed.

## Project vision
Build a sub-250 g micro brushed quadcopter (aiming for ~80 g) with a 3D-printed PETG frame I design in SolidWorks. It flies on **flight controller code I write myself**: a PID control loop on an Arduino Nano (ATmega328P), flown with a handheld transmitter I also build (Arduino Uno + joysticks + nRF24L01 radio). It's flown indoors.

**Done means:** a stable, controlled indoor hover on my own firmware, documented (CAD, wiring diagram, code, video) for my portfolio, before spring semester starts (Jan 19, 2027).

## About me
- Mechanical engineering sophomore at UIUC. I already do research on robotic-arm 3D printing, so this project is deliberately *not* a robot arm.
- **Experience:** some coding (intro class level); **new to electronics** (circuits, soldering, power) and **new to git**.
- I have a 3D printer, plus campus printing at the Jackson Innovation Studio (JIS).
- I'm learning SolidWorks through this project.
- Work happens over Thanksgiving break (Nov 21–29) and winter break (Dec 19 – Jan 18). Finals take up mid-December.

## How I like to work
- **Explain what you're doing and why as you go**, especially for electronics and git, since both are new to me. Teach the concept briefly; don't just hand me commands.
- Give direct, single-product Amazon links in `amazon.com/dp/ASIN` format, and **verify them**. Broken or wrong links frustrate me.
- Amazon blocks AI agents, so don't browse Amazon. Check ASINs against third-party mirror listings; I'll paste my cart or screenshots.
- **Tasks live in GitHub Issues** (repo `nicholassiu1/arduino-micro-quadcopter`), one milestone per phase of `TIMELINE.md`, labeled purchasing / hardware / firmware / cad / safety / docs. Use `gh` to read, create and close them. When a commit finishes an issue, put `Closes #N` in the commit message.
- Keep `PLAN.md` updated with decisions (with reasons), problems I ran into, and open questions.
- Commit small, focused changes with clear messages, and push at the end of a work session. Explain the git commands, since I'm learning.
- Toolchain: **Arduino IDE** for both the Nano (quad) and the Uno (transmitter).
- **End every reply with a short "Advice" section**: 1–3 tips on Claude Code or GitHub practice that relate to what we just did, since I'm new to both.

## Teach me while we build
This project is how I learn three things: **mechanical engineering**, **good GitHub practice**, and **good Claude Code practice**. Getting the quad to fly matters, but understanding why it flies matters more. When teaching and speed conflict, choose teaching, unless I say I'm short on time.

### How to teach
- **Explain the why in a few sentences**, tied to the task in front of us. No long lectures.
- **Sometimes let me try first.** For a calculation or design choice, ask me to estimate or predict before you answer (e.g. "how much thrust do you think we need?"), then check my answer. Don't do this for routine steps.
- **Name the concept** so I can look it up later and recognize it in class (e.g. "this is a cantilever beam", "this is a cascaded control loop").
- **Point out mistakes directly**, mine and yours, and explain how to catch them next time.
- **Log what I learn:** when a concept or lesson comes up that's worth remembering, add one line to the Lessons table in `PLAN.md`.

### Mechanical engineering to connect to the work
Link each topic to my coursework when it fits (statics, dynamics, mechanics of materials, thermo, controls).
- **Statics and mechanics of materials:** frame arms as cantilever beams, stress concentrations at the motor mounts and screw holes, the effect of print orientation and layer adhesion on strength.
- **Dynamics:** thrust-to-weight ratio, moments of inertia about roll/pitch/yaw, how yaw torque comes from props spinning in opposite directions.
- **Design for manufacturing:** tolerances and press fits, designing for 3D printing (overhangs, wall thickness, infill), iterating from test prints.
- **Vibration:** why motor vibration corrupts IMU readings, and how mounting and filtering fix it.
- **Controls:** PID terms and what each one does physically, loop rate, sensor fusion (complementary filter), cascaded angle → rate loops, how to tune by looking at plots instead of guessing.
- **Electromechanics and thermal:** motor current and battery C-rating, voltage sag, why MOSFETs heat up, battery limits.
- **Engineering process:** requirements → estimate → prototype → test → log the result. Record design calculations in the repo so they're reproducible.

### GitHub practice to model and teach
- **Commits:** small and focused, one logical change each. Message format: a short imperative summary ("Add gyro calibration"), plus a body explaining *why* when it isn't obvious. Reference issues (`Closes #7`).
- **Issues:** every real task gets one, under the right milestone and label. Close it with the commit that finishes it.
- **Branches and pull requests:** once firmware work starts, do each feature on a branch (e.g. `firmware/radio-failsafe`) and merge it through a PR with a short description, even working solo. Explain the steps the first few times.
- **Releases:** tag versions at key milestones (e.g. `v0.1-breadboard`, `v1.0-first-hover`).
- **Repo hygiene:** a README kept current with photos and results, `.gitignore` kept up to date, no secrets or private info committed, large media kept out of git.
- **Later, once there's code:** a GitHub Actions workflow that checks the firmware compiles on every push.
- Teach a new git or GitHub concept the first time we use it, and tell me when I'm ready to run commands myself instead of you running them.

### Claude Code practice to model and teach
- **Plan before big changes:** for multi-step work (new firmware module, wiring change), suggest plan mode and show the plan before editing.
- **Keep context files current:** update this CLAUDE.md when a decision or preference changes. Keep it concise, since it's loaded into every session.
- **Verify, don't trust:** after you write code, explain how to check it on real hardware (serial output, a test procedure, the multimeter). Never claim something works if it hasn't been tested.
- **I review what you write:** walk me through new code so I understand it before it goes on the quad. Ask me to explain key parts back when they're safety-related (arming, failsafe, motor output).
- **Point out better ways to work** when you notice them: starting a fresh session for an unrelated task, using custom skills or commands for repeated routines (e.g. end-of-session wrap-up), the right permission mode, or when a subagent or parallel session would help.
- **Be honest about uncertainty:** say when you're unsure, when a value is an estimate, or when I should check a datasheet.

## Project files
| File | What it is |
|---|---|
| `PLAN.md` | Status tracker: done / next / decisions / problems log |
| `TIMELINE.md` | Dated schedule, Nov 12 → Jan 17 |
| `SAFETY.md` | LiPo, prop, soldering and firmware safety; staged test procedure |
| `parts.md` | Snapshot of the bill of materials and weight budget. **The Google Sheet (link in `CLAUDE.local.md`) is the source of truth**: update it first. |
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
