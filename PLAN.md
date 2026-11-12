# Project plan & log

Living tracker, updated as work happens. Dated schedule: `TIMELINE.md`. Parts status: `parts.md`.

**Current phase:** Phase 0, before Thanksgiving (Nov 12–20). Get every part in hand and smoke-test it.

## Tasks
Tasks are tracked as **GitHub Issues**, grouped into one milestone per phase:
- [Open issues](https://github.com/nicholassiu1/arduino-micro-quadcopter/issues)
- [Milestones and progress](https://github.com/nicholassiu1/arduino-micro-quadcopter/milestones)

This file keeps what Issues don't: the decisions and their reasons, problems and lessons, and open questions.

## Done
- [x] Chose the project and parts list (Nov 11–12, planning session)
- [x] Wrote TIMELINE.md and SAFETY.md (Nov 12)
- [x] Set up the project repo: CLAUDE.md, PLAN.md, README.md, parts.md, git + GitHub (Nov 12)
- [x] GitHub setup: MIT license, topics, labels, 7 phase milestones, issues #1–#13 (Nov 12)

## Decisions
| Date | Decision | Why |
|---|---|---|
| Nov 11 | **Quadcopter, not an actively stabilized model rocket** | A rocket gets very few test flights (each uses up a motor, needs a launch site and calm weather; Champaign winters are bad for that). The quad can be tested indoors whenever I want. |
| Nov 11 | **Micro brushed quad under 250 g, not a 450 mm brushless quad** | Under 250 g recreational: no FAA registration or Remote ID, and indoor flight avoids airspace rules entirely. Small props are much safer. The 450 mm option needed registration, a Remote ID module ($50–90), airspace authorization, and DPS permission. |
| Nov 11 | **Write my own flight controller (PID on a Nano)** | That's the learning goal. MultiWii is the fallback (same chip, supports brushed motors). |
| Nov 11 | **nRF24L01 radios instead of HC-05 Bluetooth** | Changed from the TikTok build I started from. Longer range, low latency, two-way packets for the failsafe and telemetry. |
| Nov 11 | **Boost converter to 5 V for the Nano** | An ATmega328P at 16 MHz isn't rated for 1S voltage and can brown out mid-flight. Alternative kept in reserve: a 3.3 V/8 MHz Pro Mini. |
| Nov 11 | **MOSFET + flyback diode per motor** | An Arduino pin can't supply >1 A, and motor spikes reset the board. |
| Nov 12 | **AMS1117-3.3 regulator for the radio** instead of the Nano's 3V3 pin | The CH340 Nano's 3V3 pin can supply very little current; the nRF24's transmit spikes cause dropped packets, the most common failure in Nano + nRF24 builds. |
| Nov 12 | **Added a multimeter, safety glasses, heat shrink, spare props and battery straps** | Gaps found in the parts review. The multimeter and glasses are required by SAFETY.md. |
| Nov 12 | **Arduino IDE** for the firmware | Simplest start; most Nano quad references use it. Could move to PlatformIO later if the code grows. |
| Nov 12 | **Public GitHub repo** | Portfolio. Private links and names go in the git-ignored `CLAUDE.local.md`. |

## Problems & lessons
*Log problems here as they come up: what happened, the cause, the fix.*

| Date | Problem | Cause | Fix / lesson |
|---|---|---|---|
| | | | |

## Open questions
- Does UIUC policy FO-05 (DPS permission) cover indoor flight on campus? Assume yes until DPS says otherwise.
- Is there a documented Arduino Nano brushed-quad build to use as a wiring/PID reference? Compare it against `parts.md`.

## SolidWorks learning order
1. Motor mount (sketch, extrude, cut, dimensions)
2. Main frame plate with arms + board mounting holes (patterns, fillets, design for printing)
3. Battery tray + strap slot
4. Prop guards (revolve / sweep)
5. Full assembly with GrabCAD models (Nano, MPU-6050, nRF24L01); set material to PETG and check the mass
6. Transmitter case (shell)
