# Quadcopter timeline (Nov 12, 2026 → mid-January 2027)

**Goal:** a stable indoor hover on your own flight controller code before spring classes start.

**Dates assumed here (check them against the UIUC academic calendar):**
- Thanksgiving break: Sat Nov 21 – Sun Nov 29
- Last day of classes: around Wed Dec 9
- Finals: around Fri Dec 11 – Fri Dec 18
- Spring classes start: Tue Jan 19, 2027 (MLK Day is Mon Jan 18)

**Hard deadline:** JIS and probably ECE Supply are closed during breaks. Anything you need to buy or print on campus has to be done by **Fri Nov 20** (for Thanksgiving) and **about Thu Dec 10** (for winter break).

---

## Phase 0: Before Thanksgiving (Thu Nov 12 – Fri Nov 20)
*Goal: have every part in hand and know that each part works.*

| When | Task | Done when |
|---|---|---|
| Thu Nov 12 | **Order the Amazon cart today.** Decide on the soldering iron now (you'll be soldering at home over break). | Order confirmed |
| Nov 12–13 | Install SolidWorks (current 2026–27 student license from webstore.illinois.edu). | It opens and you have a license |
| Nov 13–15 | Take the FAA TRUST test (free, ~30 min). Save the certificate PDF. | Certificate saved |
| Nov 13–17 | **SolidWorks lesson 1: motor mount.** One arm stub with an 8.5 mm press-fit hole. Make 3 variants: 8.5 / 8.6 / 8.7 mm. | STL exported |
| Mon Nov 16 – Wed Nov 18 | **ECE Supply (ECEB 1031):** 1N5819 diodes, 470 µF caps, 24–26 AWG wire, perfboard, Mini-B data cable. Call or check stock first; buy extras. **JIS (MEB 0100):** M2/M3 screws, nuts, washers. Print the motor-mount test piece. | Bag of parts; test mounts printed |
| As the Amazon order arrives | **Smoke test each part:** Nano uploads Blink · MT3608 set to 5.0 V with a multimeter *before* anything is connected · MPU-6050 shows readings (I2C scanner, then raw values) · each motor spins when touched to a battery. | Every part checked off |
| By Fri Nov 20 | Try the motors in the test mounts and choose the hole size. **Pack the whole kit to take home**, including the multimeter, the LiPo bag and the spare Nanos. | Everything packed |

> If Amazon is late: Phase 1 doesn't need the frame, so a late delivery to your home address only costs a few days.

---

## Phase 1: Thanksgiving break (Sat Nov 21 – Sun Nov 29), about 20–25 hrs
*Goal: every subsystem works on the breadboard. No frame or flying yet.*

| Days | Task | Done when |
|---|---|---|
| Sat–Sun Nov 21–22 | **IMU code:** read gyro + accel, calibrate gyro offsets at startup, write a complementary filter for roll/pitch angle. Plot it in the Serial Plotter. | Tilting the board gives a smooth, correct angle with no drift |
| Mon–Tue Nov 23–24 | **Radio link:** Uno + 2 joysticks → nRF24L01 → Nano. Send a 4-channel packet (throttle, yaw, pitch, roll) at about 50 Hz. Add the **failsafe**: no packet for 250 ms → motors off. | Stick values print on the Nano; unplugging the transmitter trips the failsafe |
| Wed Nov 25 | **One motor driver:** MOSFET + gate resistor + pull-down + flyback diode, driven by PWM. Ramp the throttle from the joystick. **Props off.** | The motor speed follows the stick smoothly; the Nano doesn't reset under load |
| Thu–Fri Nov 26–27 | Thanksgiving. Take a break, or do light CAD. | |
| Sat–Sun Nov 28–29 | **SolidWorks lessons 2–3:** main frame plate (≈120 mm motor-to-motor diagonal), board holes, battery tray for 57×18×7.6 mm. Use the mass properties tool with PETG to estimate the frame weight. **Weigh every real part** on a kitchen scale and update the weight budget. | Frame STL ready; weight budget measured, not estimated |

**Checkpoint, Nov 29:** if the measured all-up weight is above ~85 g, fix it now. Options: a thinner frame, SOT-23 MOSFETs, a smaller battery, or a Pro Mini instead of a Nano with a boost converter.

---

## Phase 2: Back on campus (Mon Nov 30 – Thu Dec 10), about 8–10 hrs
*Goal: finish every printed part before JIS closes. Keep it light; classes and finals come first.*

| When | Task |
|---|---|
| Nov 30 – Dec 3 | Fix any frame problems. **SolidWorks lesson 4:** prop guards. **Lesson 5:** assembly with GrabCAD models to check that everything fits. |
| **Dec 3–9 (JIS)** | **Print in PETG:** 2–3 frame variants (in case one breaks or comes out wrong), 1 set of prop guards plus spares, battery strap parts. Also print a **single-axis test rig**: a stick or bearing hinge that lets the quad tilt in only one axis (used in Phase 4). |
| By Dec 10 | Last campus run: spare screws, extra diodes and wire, anything that broke. |
| Optional | **Lesson 6:** transmitter case. It's not essential; this is the first thing to cut if you run short of time. |

## Finals (about Dec 11 – 18): project paused
Nothing scheduled. Optional: read a reference Nano quad build or the MultiWii source for PID structure and starting gains.

---

## Phase 3: Winter break week 1 (Sat Dec 19 – Sun Dec 27): build
*Goal: an assembled quad that runs all four motors correctly with the props off.*

| Days | Task | Done when |
|---|---|---|
| Dec 19–21 | Solder the perfboard: Nano, 4 MOSFET channels, diodes, 470 µF cap, battery lead, MT3608, radio + 10 µF cap. **Check continuity and for shorts with the multimeter before the first power-up.** | It powers on from the LiPo with no smoke or resets |
| Dec 22–23 | Mount everything on the frame. Weigh the finished quad. Note the IMU's orientation in code. | Under 85 g; IMU axes match the frame |
| Dec 24–26 | Christmas. | |
| Dec 27 | **Motor mixing, props off:** confirm each motor number, its spin direction, and that roll/pitch/yaw commands speed up the correct motors. Add the arming logic (throttle low + stick gesture to arm; disarm on failsafe). | All mixing checks pass on the bench |

## Phase 4: Week 2 (Mon Dec 28 – Sun Jan 3): control loop on the test rig
*Goal: a tuned rate (gyro) loop on one axis at a time.*

- Run the main loop at a fixed rate (aim for 250–500 Hz) and measure the loop time.
- **Rate PID first (gyro only).** Start with P only, then add D, then a little I.
- Put the quad on the single-axis rig, props on, with a guard or tether. Tune roll, then pitch.
- Then add the **angle (self-level) loop** on top: an outer P loop on angle feeds the inner rate loop.
- Log or serial-plot the setpoint against the measured value while you tune.

**Decision gate, Sun Jan 3:** if the rate loop still can't hold steady on the rig, flash **MultiWii** onto a spare Nano. Get it flying, then compare its loop with yours. Flying with MultiWii still counts as a finished project; your own code becomes the next step.

## Phase 5: Week 3 (Mon Jan 4 – Sun Jan 10): first hovers
- **Tethered hover** first: the quad tied loosely to a heavy object, in an open room, prop guards on.
- Then low hovers (under 30 cm) without a tether, over carpet. Tune yaw. Add a battery-voltage check (land below ~3.5 V per cell).
- Record video of each test session; it's useful both for tuning and for your portfolio.

## Phase 6: Week 4 (Mon Jan 11 – Sun Jan 17): buffer and wrap-up
- This week is a buffer for anything that ran late. Most likely: weight fixes or tuning.
- If you're on schedule: transmitter case, trimming, a longer flight time.
- **Write it up:** photos, CAD renders, a wiring diagram, code on GitHub, a short video. Your future internship applications will use this.

---

## Summary of key dates
| Date | Milestone |
|---|---|
| **Nov 12** | Amazon order placed |
| **Nov 20** | Campus purchases + test mount done; kit packed |
| Nov 29 | All subsystems work on breadboard; real weight measured |
| **Dec 10** | All PETG parts printed (JIS closes) |
| Dec 27 | Quad assembled; mixing checked with props off |
| **Jan 3** | Rate loop tuned on the rig, or switch to MultiWii |
| Jan 10 | Untethered hover |
| Jan 17 | Done + documented |
