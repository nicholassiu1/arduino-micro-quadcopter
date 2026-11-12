# Micro quadcopter with a custom Arduino flight controller

I'm building a sub-100 g brushed quadcopter. I'm designing the frame in SolidWorks and 3D printing it, writing the flight control code myself for an Arduino Nano, and building the handheld radio transmitter from an Arduino Uno.

Status, Nov 2026: I'm ordering parts and starting the CAD. No hardware has been built or tested yet. The target for a first hover is January 2027.

## Design
| Subsystem | Approach |
|---|---|
| Flight controller | Arduino Nano (ATmega328P) running my own firmware: a complementary filter on the IMU data, cascaded PID (an angle loop feeding a rate loop), motor mixing, arming and failsafes |
| Sensors | MPU-6050 6-axis gyro and accelerometer over I2C |
| Propulsion | 4× 8520 coreless brushed motors with 75 mm props, each driven by a logic-level MOSFET |
| Power | 1S 600 mAh LiPo, with an MT3608 boost converter to 5 V for the logic |
| Radio | nRF24L01+ 2.4 GHz link with a 250 ms failsafe |
| Transmitter | Arduino Uno and two analog joysticks in a 3D-printed case |
| Frame | PETG frame, about 120 mm motor to motor on the diagonal, with prop guards, designed in SolidWorks |

The target all-up weight is about 80 g, for a thrust-to-weight ratio of about 1.5-2:1.

## Repository layout
| Path | Contents |
|---|---|
| `PLAN.md` | Progress log, decisions and the reasons for them, problems I ran into |
| `TIMELINE.md` | Build schedule |
| `SAFETY.md` | Battery, prop and soldering safety, and the staged test procedure |
| `parts.md` | Bill of materials and weight budget |
| `docs/design-review-2026-11-12.md` | Design review done before buying parts |
| `firmware/` | Flight controller code (Arduino IDE), not added yet |
| `transmitter/` | Transmitter code, not added yet |
| `cad/` | SolidWorks parts and STL exports, not added yet |

## Safety
The quad will fly indoors only, with prop guards on. Testing goes in stages: on the bench, then with props off, then on a single-axis test rig, then tethered, then a free hover. It's under 250 g and flown for recreation, and I'll take the FAA's free TRUST safety test before it flies. Details are in [SAFETY.md](SAFETY.md).

## Progress
I track the work in [Issues](../../issues) and [Milestones](../../milestones), with one milestone per build phase.

## License
[MIT](LICENSE)

## About
This is a personal project. I'm a mechanical engineering student at the University of Illinois Urbana-Champaign, and I'm building it to learn embedded control, electronics and CAD.
