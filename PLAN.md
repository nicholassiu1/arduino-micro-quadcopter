# Project plan and log

I update this file as the work happens. The dated schedule is in `TIMELINE.md` and the parts status is in `parts.md`.

Current phase: Phase 0, before Thanksgiving (Nov 12-20). Get every part in hand and smoke-test it.

## Tasks
Tasks are GitHub Issues, grouped into one milestone per phase:
- [Open issues](https://github.com/nicholassiu1/arduino-micro-quadcopter/issues)
- [Milestones and progress](https://github.com/nicholassiu1/arduino-micro-quadcopter/milestones)

This file holds what the issues don't: decisions and the reasons for them, problems and lessons, and open questions.

## Done
- [x] Chose the project and parts list (Nov 11-12, planning session)
- [x] Wrote TIMELINE.md and SAFETY.md (Nov 12)
- [x] Set up the project repo: CLAUDE.md, PLAN.md, README.md, parts.md, git + GitHub (Nov 12)
- [x] GitHub setup: MIT license, topics, labels, 7 phase milestones, issues #1-#13 (Nov 12)
- [x] GitHub profile README live at github.com/nicholassiu1 (Nov 12). Still to do by hand: name/bio in Settings, pin the repo, profile photo
- [x] Added teaching goals to CLAUDE.md and a Lessons log here (Nov 12)
- [x] Downloaded the recommended software tools, including SolidWorks (Nov 12)
- [x] Added the `/end-session` skill for the end-of-session routine (Nov 12)
- [x] Checked the parts list against the ECE Supply Center catalog and updated the Google Sheet (Nov 12)
- [x] Independent design review: `docs/design-review-2026-11-12.md`, 3 blockers; Sheet updated with tools and lighter diodes; issues #14-#16 created (Nov 12)
- [x] Second independent check of the materials and build sequence (section 6 of the design review): Sheet corrected (capacitors and jumper wires added, three redundant items made optional); `TIMELINE.md`, `SAFETY.md` and `parts.md` updated; issues #17-#19 created (Nov 12)
- [x] Edited the docs, issues and repo description with the humanizer skill: first person, plainer formatting (Nov 12)
- [x] Tidied the public repo: personal preferences moved from CLAUDE.md to CLAUDE.local.md, duplicate skill removed, Wiki and Projects turned off, README image and lessons added, `docs/design-calcs.md` and a thrust-test CSV started (Nov 12)
- [x] Cost check against local stores, campus and online sellers: Sheet regrouped by store, estimated total down from ~$387 to ~$319, counting parts from the IOLab pack and the safety glasses I already own (Nov 12)

## Next session
- Paste the Amazon cart for a title check against the Sheet, then order (#1). In the cart, confirm the motor listing says (8) motors, the battery plug is JST-RCY, and each delivery date is before Nov 19 (or ship it home).
- ECE Supply run Mon Nov 16 (#6): open Mon-Fri, 9-noon and 1-3:30. The list now includes 0.1 µF capacitors, plus the transmitter parts that neither the dropped starter kit nor my IOLab pack covers (breadboard, 220 Ω and 10 kΩ resistors, buzzer, buttons, one LED). Before going, check the IOLab bag against the list in `parts.md`. Check whether the $4.45 economy stripper handles 26 AWG, and whether the 1N5819 is back in stock.
- Check my dad's multimeter: a 10 A DC range, a 20 V DC range, a continuity beep, and a fresh battery. If it has no 10 A range, buy the Harbor Freight Cen-Tech (item 59434, ~$5, price unconfirmed).
- AliExpress Nanos: check the delivery date at checkout. If it's after Nov 20, buy the Amazon ELEGOO 3-pack instead.
- Measure motor thrust as soon as the motors arrive (#14), recording the voltage across the motor, before the frame CAD.

## Start to finish
The order I work in and the check that lets me move to the next stage. Dates are in `TIMELINE.md`; this list is the order and the gates. If a gate fails, I fix it before starting the next stage, because each stage builds on the one before.

These rules hold at every stage (details in `SAFETY.md`): safety glasses for soldering and any test with props on; USB and LiPo together only with props off; charge only in Dad's LiPo bag, on a non-flammable surface, never unattended; a new code feature is tested props off before props on.

### 1. Parts in hand and tested (Nov 12-20)
1. Check Dad's multimeter (10 A DC, 20 V DC, continuity). Buy the Cen-Tech only if there is no 10 A range.
2. Order the AliExpress Nanos. If the delivery date is after Nov 20, order the Amazon ELEGOO 3-pack instead.
3. Paste the Amazon cart for a title check, then order it (#1). Anything due after Nov 19 ships to my home address.
4. Take the FAA TRUST test and save the certificate (#4).
5. Draw the motor-mount coupon in SolidWorks (#5) and print it.
6. Check the IOLab bag, then do the ECE Supply and JIS runs on Mon Nov 16 (#6).
7. Go through the "Check at home" table in `parts.md` and buy anything missing.
8. Soldering practice on the spare perfboard (#16), then the GY-521 header pins and MT3608 leads.
9. Smoke test each part as it arrives (#7). The MT3608 gets set to 5.0 V before anything is connected to it.
10. Thrust and current test on one motor (#14).
11. Choose the motor hole size from the coupon and pack the kit.

Gate: every part works, and the thrust numbers are in `docs/data/thrust-test.csv`. A dead part gets reordered right away, shipped to my home.

### 2. Weight limit (Nov 21)
1. Scale the bench thrust to flight voltage with section 2 of `docs/design-calcs.md`.
2. Weight limit = 4 × in-flight thrust per motor / 1.5.
3. From that limit, choose TO-220 or SOT-23 MOSFETs and set a gram limit for the frame plus guards.
4. Log the decision in the Decisions table below.

Gate: the lightest build in the weight budget fits under the limit. If it doesn't, I stop and rethink the motors or battery before any CAD.

### 3. Every subsystem on the breadboard (Nov 21-27)
1. IMU: calibrated gyro, complementary filter, smooth angle in the Serial Plotter (#9).
2. Pin map (#15), then the radio link with the 250 ms failsafe and telemetry in the ack payload (#10).
3. One motor driver from the joystick, props off, with the MOSFET check (#11).
4. Board layout on paper with its outer dimensions (#17).

Gate: each "Done when" in `TIMELINE.md` passes, and the Nano doesn't reset under motor load. Tag `v0.1-breadboard`.

### 4. Frame designed and parts weighed (Nov 28-29)
1. Frame CAD around the board outline, battery tray, rig attachment points (#12).
2. Weigh every real part and fill in the Measured column (#13).

Gate: CAD mass plus measured parts is under the weight limit (the Nov 29 checkpoint in `TIMELINE.md`).

### 5. Everything printed (Nov 30 to Dec 10)
1. Prop guards and a full SolidWorks assembly; check fit and that nothing reaches the prop plane.
2. Print the frame variants, guards plus spares, and the test rig. Submit early enough for one reprint.
3. Last JIS run: rig pivot hardware, spare screws.

Gate: every printed part is in hand and fits the real motors, board and battery before I leave campus.

### 6. Build and safety firmware (Dec 19-27)
1. Solder the flight board from the layout. Check continuity and shorts with the multimeter before the first power-up.
2. Mount it on the frame and weigh the whole quad.
3. Stage 0: all eight firmware safety features in `SAFETY.md` section 5, on USB, no battery (#18).
4. Stage 1: motor order, spin direction and mixing, props off. Ten battery plug-ins with no twitch.

Gate: Stage 0 and Stage 1 both pass. I review the safety code line by line and can explain each feature before props go on.

### 7. Rate loop on the rig (Dec 28 to Jan 3)
1. Fixed loop rate, measured.
2. Rate PID on roll, then pitch, tuned from radio telemetry plots; recheck near hover throttle.
3. Angle loop on top.

Gate, Jan 3: the rig holds steady. If not, switch to the fallback chosen in #19 and get that flying first.

### 8. First hovers (Jan 4-10)
1. Tethered hover, guards on.
2. Low untethered hovers over carpet; tune yaw.
3. Set the low-battery limits from measured sag.

Gate: a stable untethered hover on my own firmware. Tag `v1.0-first-hover`.

### 9. Write-up (Jan 11-17)
README with photos, CAD renders, the wiring diagram, measured data and a short video. Close the remaining issues and milestones. This week is also the buffer for anything that ran late.

## Decisions
| Date | Decision | Why |
|---|---|---|
| Nov 11 | Quadcopter, not an actively stabilized model rocket | A rocket gets very few test flights (each uses up a motor, needs a launch site and calm weather; Champaign winters are bad for that). The quad can be tested indoors whenever I want. |
| Nov 11 | Micro brushed quad under 250 g, not a 450 mm brushless quad | A recreational quad under 250 g needs no FAA registration or Remote ID, and indoor flight avoids airspace rules entirely. Small props are much safer. The 450 mm option needed registration, a Remote ID module ($50-90), airspace authorization, and DPS permission. |
| Nov 11 | Write my own flight controller (PID on a Nano) | That's the learning goal. MultiWii is the fallback (same chip, supports brushed motors). |
| Nov 11 | nRF24L01 radios instead of HC-05 Bluetooth | Changed from the TikTok build I started from. The nRF24L01 has longer range, low latency, and two-way packets for the failsafe and telemetry. |
| Nov 11 | Boost converter to 5 V for the Nano | An ATmega328P at 16 MHz isn't rated for 1S voltage and can brown out mid-flight. Alternative kept in reserve: a 3.3 V/8 MHz Pro Mini. |
| Nov 11 | MOSFET + flyback diode per motor | An Arduino pin can't supply >1 A, and motor spikes reset the board. |
| Nov 12 | AMS1117-3.3 regulator for the radio instead of the Nano's 3V3 pin | The CH340 Nano's 3V3 pin can supply very little current; the nRF24's transmit spikes cause dropped packets, the most common failure in Nano + nRF24 builds. |
| Nov 12 | Added a multimeter, safety glasses, heat shrink, spare props and battery straps | Gaps found in the parts review. The multimeter and glasses are required by SAFETY.md. |
| Nov 12 | Arduino IDE for the firmware | Simplest start; most Nano quad references use it. Could move to PlatformIO later if the code grows. |
| Nov 12 | Public GitHub repo | Portfolio. Private links and names go in the git-ignored `CLAUDE.local.md`. |
| Nov 12 | CLAUDE.md holds only project context and conventions; my background and reply preferences live in `CLAUDE.local.md` | CLAUDE.md is public, and a portfolio reader doesn't need my working preferences. Both files still load in every local session. |
| Nov 12 | The Google Sheet is the source of truth for the parts list; `parts.md` is a snapshot | It's where I look when I'm shopping. Two editable copies drift apart, so I update the Sheet first, then refresh `parts.md` occasionally. |
| Nov 12 | Helping hands, brass tip cleaner and battery straps moved to Optional (Claude's recommendation in the second check; reverse it in the Sheet if I disagree) | The soldering station kit already includes helping hands and a sponge. A rubber band holds the battery and weighs less than a strap. Saves about $48. |
| Nov 12 | USB and LiPo together only with props off; with props on, data comes back over the radio | An upload resets the Nano, and a USB cable on a tilting rig can reach the props. Sending data back in the radio's acknowledgement packet keeps the rule simple. |
| Nov 12 | The transmitter is the spare Nano on a breadboard instead of an Uno; the ELEGOO starter kit is dropped | The Nano has the same ATmega328P and runs the same code. The kit cost $44; the parts I needed from it cost ~$12 at ECE Supply, with the rest from my IOLab pack. The 3-pack still leaves one spare Nano. |
| Nov 12 | Use my IOLab E&M small accessory pack first: 220 µF bus cap (instead of 470 µF), 22 µF radio caps (instead of 10 µF), LEDs, 1 kΩ LED resistors, battery-sense resistors, small breadboard, alligator leads | I already own it, so every part it covers comes off the ECE list (~$4). 220 µF is lighter than 470 µF; if the Nano resets under motor load, the pack's second 220 µF goes in parallel. Nothing in the Skipped list came back: the pack has no jumpers, buzzer, buttons or flux. |
| Nov 12 | Multimeter from Harbor Freight (Cen-Tech 59434, ~$5) instead of the AstroAI AM33D ($18); flux pen and solder wick skipped | Setting the MT3608 and checking for shorts and motor current doesn't need a better meter. Every joint is through-hole, so rosin-core solder's own flux and the kit's solder sucker are enough. Together with the Uno swap this saves about $60. |
| Nov 12 | Kept on Amazon: radios, Nanos, MOSFETs, soldering station, caliper, scale, LiPos | Tayda and AliExpress are cheaper per part, but shipping and delivery times don't fit an order that has to arrive by Nov 20. ECE has no logic-level TO-220 MOSFET. Campus soldering benches don't lend tools to take home. Harbor Freight's cheaper caliper reads only to 0.1 mm. |
| Nov 12 | My own 3D printer is at school, so I print there before Thanksgiving and between Nov 30 and finals | No printer at home means no reprints over either break unless I take the printer home. The frame v0 printed before Nov 21 is practice; the real frame is printed before I leave in December. |
| Nov 12 | Nanos from AliExpress (3 clones, ~$4) instead of the Amazon ELEGOO 3-pack ($19); this reverses the Nanos part of the "Kept on Amazon" row | About $15 cheaper. The Amazon pack stays in the Sheet's Skipped section as the backup if the AliExpress delivery date is after Nov 20. |
| Nov 12 | Optional printer: Bambu A1 mini from Target, PETG Basic (with spool) from the Bambu store | Target doesn't sell PETG or spools. 1 kg covers the whole project (~200-400 g, estimate). Every part is designed to print without supports, so no support filament. |
| Nov 12 | My dad's multimeter first; the Harbor Freight Cen-Tech only if it has no 10 A range | No purchase if his meter can read motor current. The 10 A range is what the thrust test needs. |
| Nov 12 | My dad's LiPo safe bag replaces the Zeee bag | One is already at home. Check it for holes or torn seams before the first charge. |
| Nov 12 | Low-battery limits: warn at ~3.5 V, land at ~3.3 V under load, to be checked against measured sag | A single 3.5 V "land" limit would probably trip in the first minute, because four motors pull the voltage down 0.3-0.6 V (estimate). |

## Problems
| Date | Problem | Cause | Fix / lesson |
|---|---|---|---|
| Nov 12 | 1N5819 out of stock at ECE; I swapped in the MBR360G, which the review flagged as too heavy | I checked availability but not weight | For every part swap, check weight and fit as well as function. 1N5819 now comes from Amazon; MBR360G is a backup |
| Nov 12 | Design review: estimated thrust-to-weight only ~1.0-1.3 | The weight budget left out props and guards; motor thrust is an unverified listing figure | Measure thrust first (#14); set per-subsystem weight limits |
| Nov 12 | The plan counted on capacitors and enough jumper wires from the starter kit, but the kit has no capacitors and only 10 female-to-male jumpers | I assumed the kit's contents without reading its parts list | Read the packing list of any kit before counting on it. Capacitors added to the ECE run; a jumper kit added to the Amazon order |
| Nov 12 | The thrust test as first written would overstate in-flight thrust by roughly 25-50% | One motor on a fresh battery sees about 3.9 V; four motors in flight pull the same battery down to about 3.0-3.3 V (estimate) | Record the voltage across the motor during the test and scale the result to the in-flight voltage (#14) |
| Nov 12 | `TIMELINE.md` still described the plan from before the first design review | The review's fixes went into issues and the Sheet, but not the timeline | When a review changes the plan, update every document that states the plan, in the same session |

## Lessons
| Date | Area | Lesson |
|---|---|---|
| Nov 12 | ME | Thrust-to-weight of 1.5-2:1 is needed for control margin; with ~120-130 g of thrust, the quad must weigh ≤ ~80 g. |
| Nov 12 | Electronics | A regulator's current limit matters as much as its voltage: the Nano's 3V3 pin can't handle the nRF24's transmit spikes. |
| Nov 12 | Git | Git saves snapshots locally (commit); GitHub stores a copy online (push). `.gitignore` keeps private files out. |
| Nov 12 | GitHub | Issues hold tasks, milestones group them with due dates, and `Closes #N` in a commit closes the issue automatically. |
| Nov 12 | Claude Code | CLAUDE.md is read at the start of every session, so it's where lasting context and preferences belong. `CLAUDE.local.md` holds private notes that stay out of git. |
| Nov 12 | GitHub | A public repo named exactly after my username shows its README on my profile page. |
| Nov 12 | GitHub | Use one account for life with several emails on it. Add the school email for the Student Pack; keep a personal email as primary. |
| Nov 12 | Claude Code | A routine I repeat every session belongs in a skill (`.claude/skills/<name>/SKILL.md`), run with `/<name>`. Committing it to the repo means every future session has it. |
| Nov 12 | Electronics | A MOSFET's part number matters: IRF520/IRF540 need ~10 V on the gate to fully turn on; a *logic-level* part (IRLZ44N, the "L") turns on fully from a 5 V Arduino pin. A half-on MOSFET gets hot. |
| Nov 12 | Process | Keep one source of truth for any list; a second editable copy will drift out of sync. |
| Nov 12 | Claude Code | Context space is limited, so keep CLAUDE.md short (specialized routines go in skills), turn off unused connectors, and send noisy work (serial dumps, compiler logs, long reviews) to a subagent or separate session that returns a summary. |
| Nov 12 | Electronics | A battery acts like an ideal voltage source with a small resistor in series (its **internal resistance**; the model is a **Thevenin equivalent**). More current means more voltage lost inside the battery, so four motors see less voltage than one motor does. |
| Nov 12 | Electronics | Some pins have a second job. D10 is the SPI "slave select" pin, and starting the radio drives it HIGH, so motor pins must be set LOW first. Check a pin's other functions before giving it a job. |
| Nov 12 | Process | Find the **critical path**: the chain of tasks where each one waits on the one before (thrust test, weight plan, board layout, frame CAD, print). A delay anywhere on it delays the end date. |
| Nov 12 | ME | Bracket a tolerance test: print hole sizes on both sides of the expected best fit, because printed holes come out 0.1-0.3 mm smaller than drawn. |
| Nov 12 | Electronics | Plumbing flux from a hardware store is acid flux and corrodes circuit boards. Electronics work uses rosin or no-clean flux only. |
| Nov 12 | Claude Code | A second review in a fresh session catches different things than the first. Ask the reviewer to check facts against sources (listings, datasheets, source code) instead of trusting memory, its own included. |

## Open questions
- Does UIUC policy FO-05 (DPS permission) cover indoor flight on campus? Assume yes until DPS says otherwise.
- Is there a documented Arduino Nano brushed-quad build to use as a wiring/PID reference? Compare it against `parts.md`. Leads found Nov 12, not yet vetted: the `execuc/multwii-brushed-nrf24l01` fork on GitHub (its author calls it not fully tested), iforce2d's nRF24 radio-control thread on the Arduino forum (a second Arduino turns nRF24 packets into a PPM signal for MultiWii), and an Instructables "Arduino Mini Quadcopter With NRF24L01" build on MultiWii 2.3. See #19.
- How long does a JIS print take from submitting to pickup? The Nov 16-18 coupon and the Dec 3-9 frame prints both depend on it.
- MOSFETs: stay with the TO-220 IRLZ44N or switch to SOT-23 parts (about 6-7 g lighter, harder to solder)? Decide at the weight plan, after the thrust test.
- Throttle stick: the joysticks are spring-centered. Planned mapping is middle and below = zero throttle, so letting go cuts power. Confirm when writing the radio code (#10).

## SolidWorks learning order
1. Motor mount (sketch, extrude, cut, dimensions)
2. Main frame plate with arms + board mounting holes (patterns, fillets, design for printing)
3. Battery tray + strap slot
4. Prop guards (revolve / sweep)
5. Full assembly with GrabCAD models (Nano, MPU-6050, nRF24L01); set material to PETG and check the mass
6. Transmitter case (shell)
