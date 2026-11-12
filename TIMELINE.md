# Quadcopter timeline (Nov 12, 2026 to mid-January 2027)

Goal: a stable indoor hover on my own flight controller code before spring classes start.

Dates I'm assuming, still to be checked against the UIUC academic calendar:
- Thanksgiving break: Sat Nov 21 to Sun Nov 29
- Last day of classes: around Wed Dec 9
- Finals: around Fri Dec 11 to Fri Dec 18
- Spring classes start: Tue Jan 19, 2027 (MLK Day is Mon Jan 18)

Hard deadlines: JIS and probably ECE Supply are closed during breaks, so anything I need to buy or print on campus has to be done by Fri Nov 20 (for Thanksgiving) and about Thu Dec 10 (for winter break). ECE Supply is open Mon-Fri, 9-noon and 1-3:30 only. Amazon still delivers to my home over break, so a forgotten part costs two days, but a forgotten print can't be fixed until January.

The critical path (what depends on what): thrust test, then the weight plan (which MOSFETs, how light the frame must be), then the board layout on paper, then the frame CAD, then the frame printed by Dec 10. Each step needs the one before it, so a late thrust test delays everything after it.

## Phase 0: Before Thanksgiving (Thu Nov 12 to Fri Nov 20)
Goal: have every part in hand, know that each part works, and know how much thrust the motors really make.

| When | Task | Done when |
|---|---|---|
| Thu Nov 12 | Order the Amazon cart from the Google Sheet (#1). In the cart, check every delivery date: anything due after Thu Nov 19 should ship to my home address instead. | Order confirmed |
| Nov 12 | Install SolidWorks (#3). Done. | It opens and I have a license |
| Nov 13-15 | Take the FAA TRUST test (#4; free, ~30 min). Save the certificate PDF. | Certificate saved |
| Nov 13-15 | SolidWorks lesson 1: motor-mount test coupon (#5). One arm stub with five press-fit holes, 8.5 to 8.9 mm in 0.1 mm steps. The coupon also holds the motor for the thrust test. | STL exported |
| Mon Nov 16 to Wed Nov 18 | ECE Supply (ECEB 1031) (#6): 470 µF caps, 10 µF and 0.1 µF caps, 26 AWG stranded wire, 2 perfboards, Mini-USB cable, safety glasses, heat shrink, 60/40 solder. JIS (MEB 0100): M2/M3 screws, nuts, washers. Submit the coupon print early and ask how long prints take. | Bag of parts; coupon printed |
| Once the soldering station and the ECE parts are in hand | Soldering practice (#16) on the spare perfboard. Then solder the GY-521 header pins and the MT3608 leads. | Joints pass a tug test and a continuity check |
| As the Amazon order arrives | Smoke test each part (#7): the Nano uploads Blink; the MT3608 is set to 5.0 V with a multimeter before anything is connected; the AMS1117 gives 3.3 V; the MPU-6050 shows readings (I2C scanner, then raw values); each motor spins on a battery; battery polarity is checked. | Every part checked off |
| As soon as the motors, scale, multimeter and a charged battery are in hand | Thrust and current test (#14). One motor, no Arduino. Record four numbers: battery voltage at rest, voltage across the motor while it runs, amps, grams. Follow the thrust-stand rules in `SAFETY.md`. If the parts arrive too late, this is the first job at home on Nov 21. | Numbers recorded in the repo |
| By Fri Nov 20 | Try the motors in the coupon and choose the hole size. Pack the whole kit to take home: soldering station, solder, multimeter, scale, calipers, LiPo bag, spare Nanos, safety glasses, every part. | Everything packed |

> Why five hole sizes: a printed hole comes out about 0.1-0.3 mm smaller than drawn, so the best fit is probably near 8.7 mm. Test sizes on both sides of the guess. The result only holds for the same printer, material and print orientation, so print the real frame the same way.

> If Amazon is late: Phase 1 doesn't need the frame, so a late delivery to my home address only costs a few days.

## Phase 1: Thanksgiving break (Sat Nov 21 to Sun Nov 29), about 20-25 hrs
Goal: every subsystem works on the breadboard, and the frame is designed around a board layout that exists on paper. No frame or flying yet.

| Days | Task | Done when |
|---|---|---|
| Sat Nov 21 | If it isn't done yet: thrust test (#14). Then make the weight plan: from the measured thrust, set the all-up weight limit, choose the MOSFET type (TO-220 or the lighter SOT-23), and set a gram limit for frame + guards. | Decision written in `PLAN.md` |
| Sat-Sun Nov 21-22 | IMU code (#9): read gyro + accel, calibrate gyro offsets at startup, write a complementary filter for roll/pitch angle. Plot it in the Serial Plotter. | Tilting the board gives a smooth, correct angle with no drift |
| Mon-Tue Nov 23-24 | Pin map first (#15), then the radio link (#10): an Uno with 2 joysticks sends through an nRF24L01 to the Nano. Send a 4-channel packet (throttle, yaw, pitch, roll) at about 50 Hz. Add the failsafe: motors off if no packet arrives for 250 ms. Decide how the spring-centered throttle stick maps to throttle. Send data back to the transmitter in the acknowledgement packet (telemetry), because tuning with props on can't use a USB cable. | Stick values print on the Nano; unplugging the transmitter trips the failsafe; the Uno prints data sent back by the Nano |
| Wed Nov 25 | One motor driver (#11): MOSFET + gate resistor + pull-down + flyback diode, driven by PWM. Ramp the throttle from the joystick. Props off (USB may stay connected only because the props are off). Check the MOSFET is genuine: under 0.1 V from drain to source with the motor at full PWM. | The motor speed follows the stick smoothly; the Nano doesn't reset under load; the MOSFET stays cool |
| Thu-Fri Nov 26-27 | Thanksgiving. Light work only: lay out the perfboard on paper (#17). Place every part, the power and ground bus, the battery-sense wire and the mounting holes, then fix the board's outline. If time allows, start the arming logic (#18). | A layout drawing with outer dimensions |
| Sat-Sun Nov 28-29 | SolidWorks lessons 2-3 (#12): main frame plate (≈120 mm motor-to-motor diagonal) designed around the board outline from the layout, battery tray for 57×18×7.6 mm, and attachment points for the single-axis rig on both the roll and pitch axes. Use the mass properties tool with PETG to estimate the frame weight. Weigh every real part (#13) on the 0.01 g scale and update the weight budget. | Frame STL ready; weight budget filled in with measured values |

Checkpoint, Nov 29: add the measured part weights to the CAD frame mass. If the total is above the limit from the weight plan, fix it now. Options, cheapest first: a thinner frame, SOT-23 MOSFETs, a smaller battery, or a 3.3 V Pro Mini instead of a Nano with a boost converter.

## Phase 2: Back on campus (Mon Nov 30 to Thu Dec 10), about 8-10 hrs
Goal: finish every printed part before JIS closes. Keep it light; classes and finals come first.

| When | Task |
|---|---|
| Nov 30 to Dec 3 | Fix any frame problems. SolidWorks lesson 4: prop guards. Lesson 5: assembly with GrabCAD models. Check that every part fits, that nothing tall reaches the prop plane (lay the MOSFETs and the 470 µF cap flat), and that neighboring guards don't collide. |
| Dec 3-9 (JIS) | Print in PETG: 2-3 frame variants that differ where I'm least sure (for example a light one and a stiffer one), 1 set of prop guards plus spares, and the single-axis test rig. The rig's pivot line should pass through the quad's center of mass. I can't measure that yet, so the rig gets pivot holes at a few heights. Submit early enough for a reprint. |
| By Dec 10 | Last campus run: spare screws, the rig's pivot hardware, extra wire, anything that broke. |
| Optional | Lesson 6: transmitter case. It isn't essential, so it's the first thing to cut if I run short of time. |

> Everything printed here is designed before the electronics are assembled, which is the riskiest part of this schedule. If my own 3D printer is at home, a wrong frame can be reprinted over break and this deadline matters much less.

## Finals (about Dec 11 to 18): project paused
Nothing scheduled. Optional: read a reference Nano quad build or the MultiWii source for PID structure and starting gains, and decide what the fallback would really be (#19): stock MultiWii has no nRF24 receiver and puts a motor on pin D11, which the radio needs.

## Phase 3: Winter break week 1 (Sat Dec 19 to Sun Dec 27): build
Goal: an assembled quad that runs all four motors correctly with the props off.

| Days | Task | Done when |
|---|---|---|
| Dec 19-21 | Solder the perfboard from the layout: Nano, 4 MOSFET channels, diodes, 470 µF cap, battery lead, MT3608, AMS1117, radio with its 10 µF + 0.1 µF caps, battery-sense wire, status LED. Check continuity and for shorts with the multimeter before the first power-up. | It powers on from the LiPo with no smoke or resets |
| Dec 22-23 | Mount everything on the frame. Weigh the finished quad. Note the IMU's orientation in code. Stage 0 (#18): all eight firmware safety features in `SAFETY.md` section 5 work on USB power, with no battery connected. | Under the weight limit; IMU axes match the frame; Stage 0 passes |
| Dec 24-26 | Christmas. | |
| Dec 27 | Stage 1: motor mixing, props off. Confirm each motor number, its spin direction, and that roll/pitch/yaw commands speed up the correct motors. Plug the battery in ten times: no motor may twitch at power-up. | All mixing checks pass on the bench |

## Phase 4: Week 2 (Mon Dec 28 to Sun Jan 3): control loop on the test rig
Goal: a tuned rate (gyro) loop on one axis at a time.

- Run the main loop at a fixed rate (aim for 250-500 Hz) and measure the loop time.
- Rate PID first (gyro only). Start with P only, then add D, then a little I.
- Put the quad on the single-axis rig, props on, with a guard or tether. Tune roll, then pitch.
- No USB cable with props on. Plot the setpoint against the measured value from the data the quad sends back over the radio.
- Before leaving the rig, repeat the test near the estimated hover throttle. A motor's thrust responds more strongly at high throttle, so gains tuned at low throttle can oscillate at hover.
- Then add the angle (self-level) loop on top: an outer P loop on angle feeds the inner rate loop.

Decision gate, Sun Jan 3: if the rate loop still can't hold steady on the rig, switch to the fallback chosen in #19 (a MultiWii variant on a spare Nano). Get it flying, then compare its loop with mine. A quad flying on MultiWii still counts as a finished project, and my own code becomes the next step.

## Phase 5: Week 3 (Mon Jan 4 to Sun Jan 10): first hovers
- Tethered hover first: the quad tied loosely to a heavy object, in an open room, prop guards on.
- Then low hovers (under 30 cm) without a tether, over carpet. Tune yaw.
- Set the low-battery thresholds from the voltage sag measured in the air. The check itself must already work before props go on (Stage 0).
- Record video of each test session. It's useful for tuning and for the portfolio.

## Phase 6: Week 4 (Mon Jan 11 to Sun Jan 17): buffer and wrap-up
- This week is a buffer for anything that ran late, most likely weight fixes or tuning.
- If I'm on schedule: transmitter case, trimming, a longer flight time.
- Write it up: photos, CAD renders, a wiring diagram, code on GitHub, a short video. Internship applications will use this.

## Milestone dates
| Date | Milestone |
|---|---|
| Nov 12 | Amazon order placed |
| Nov 20 | Campus purchases + test coupon done; kit packed |
| Nov 20-21 | Thrust measured; weight plan decided |
| Nov 27 | Board layout fixed (the frame CAD depends on it) |
| Nov 29 | All subsystems work on breadboard; real weight measured; frame CAD ready |
| Dec 10 | All PETG parts printed (JIS closes) |
| Dec 23 | Quad assembled; Stage 0 safety features pass |
| Dec 27 | Mixing checked with props off |
| Jan 3 | Rate loop tuned on the rig, or switch to the fallback |
| Jan 10 | Untethered hover |
| Jan 17 | Done + documented |
