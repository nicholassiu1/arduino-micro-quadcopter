# Micro Quadcopter with a Custom Arduino Flight Controller

A sub-100 g brushed quadcopter built from scratch: a 3D-printed frame designed in SolidWorks, flight control code written from the ground up for an Arduino Nano, and a handheld radio transmitter built from an Arduino Uno.

**Status:** 🛠 In progress. Parts ordering and CAD (Nov 2026); target first hover Jan 2027.

## What it is
| Subsystem | Approach |
|---|---|
| **Flight controller** | Arduino Nano (ATmega328P) running my own firmware: an IMU complementary filter, cascaded PID (angle → rate), motor mixing, arming and failsafes |
| **Sensors** | MPU-6050 6-axis gyro + accelerometer over I2C |
| **Propulsion** | 4× 8520 coreless brushed motors, 75 mm props, each driven by a logic-level MOSFET |
| **Power** | 1S 600 mAh LiPo; MT3608 boost converter to 5 V for the logic |
| **Radio** | nRF24L01+ 2.4 GHz link with a 250 ms failsafe |
| **Transmitter** | Arduino Uno + two analog joysticks in a 3D-printed case |
| **Frame** | ~120 mm diagonal PETG frame + prop guards, designed in SolidWorks |

Target all-up weight is about 80 g, for a thrust-to-weight ratio of about 1.5–2:1.

## Repository layout
| Path | Contents |
|---|---|
| `PLAN.md` | Progress log, decisions and reasoning, problems I ran into |
| `TIMELINE.md` | Build schedule |
| `SAFETY.md` | Battery, prop and soldering safety; staged test procedure |
| `parts.md` | Bill of materials + weight budget |
| `firmware/` | *(coming)* Flight controller code (Arduino IDE) |
| `transmitter/` | *(coming)* Transmitter code |
| `cad/` | *(coming)* SolidWorks parts and STL exports |

## Safety
It's flown indoors only, with prop guards and a staged test procedure: bench, then props off, then a single-axis test rig, then tethered, then free hover. The aircraft is under 250 g and flown for recreation by a TRUST-certified pilot (FAA's free recreational safety test). See [SAFETY.md](SAFETY.md).

## About
A personal project by a mechanical engineering student at the University of Illinois Urbana-Champaign, built to learn embedded control, electronics and CAD.
