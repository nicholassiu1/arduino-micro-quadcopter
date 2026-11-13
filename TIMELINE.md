# Quadcopter timeline (Nov 12, 2026 to mid-January 2027)

Goal: a stable indoor hover on my own flight controller code before spring classes start.

Dates I'm assuming, still to be checked against the UIUC academic calendar:
- Thanksgiving break: Sat Nov 21 to Sun Nov 29
- Last day of classes: around Wed Dec 9
- Finals: around Fri Dec 11 to Fri Dec 18
- Spring classes start: Tue Jan 19, 2027 (MLK Day is Mon Jan 18)

Hard deadlines: JIS and probably ECE Supply are closed during breaks, so anything I need to buy on campus has to be done by Fri Nov 20 (for Thanksgiving) and about Thu Dec 10 (for winter break). ECE Supply is open Mon-Fri, 9-noon and 1-3:30 only.

Shipping: every online order (Amazon, AliExpress, Target, Bambu) ships home, where I am from Fri Nov 20 to Sun Nov 29. Everything has to be delivered by Sat Nov 21. So no part is in my hands before Thanksgiving break, and all the testing starts on Nov 21. Amazon still delivers home over break, so a forgotten part costs two days. My own Bambu A1 mini arrives with the rest and then goes everywhere I go, so a failed print costs a day instead of a month.

The critical path (what depends on what): parts delivered, thrust test, then the weight plan (which MOSFETs, how light the frame must be), then the board layout, then the frame CAD, then the frame printed by Dec 10. Each step needs the one before it, so a late delivery or a late thrust test delays everything after it.

## Phase 0: Before Thanksgiving, no parts yet (Thu Nov 12 to Fri Nov 20)
Goal: every order placed and due at home by Nov 21, campus parts bought, and all the design work that doesn't need parts done, so break starts with testing.

| When | Task | Done when |
|---|---|---|
| Fri Nov 13 | Order everything shipped home (#1): the Amazon cart (pasted for a title check against the Sheet first), 5 AliExpress Nanos plus the SOT-23 backup parts (AO3400s, adapters, flux pen), the A1 mini from Target, the PETG from the Bambu store. Every delivery date must be on or before Sat Nov 21. If the AliExpress date is later, buy two Amazon ELEGOO 3-packs instead. | Every order confirmed with a date by Nov 21 |
| Nov 12 | Install SolidWorks (#3). Done. | It opens and I have a license |
| Nov 13-15 | Take the FAA TRUST test (#4; free, ~30 min). Save the certificate PDF. | Certificate saved |
| Nov 13-15 | SolidWorks lesson 1: motor-mount test coupon (#5). One arm stub with five press-fit holes, 8.5 to 8.9 mm in 0.1 mm steps. Also the thrust-test fixture that holds the coupon on the scale, and a battery-tray test piece. The coupon prints on the A1 mini at home, because the hole size only holds for the printer that made it. | STLs exported |
| Nov 13-15 | KiCad: the pin map on paper first (#15), then the schematics for the quad and the transmitter. Download the GrabCAD models (Nano, MPU-6050, nRF24L01, 8520 motor). | Both schematics drawn |
| Mon Nov 16 to Wed Nov 18 | Check the IOLab bag against `parts.md`. ECE Supply (ECEB 1031) (#6): 0.1 µF caps, transmitter parts (breadboard, resistors, buzzer, buttons, LED), 26 AWG stranded wire, 2 perfboards, 2 Mini-USB cables, heat shrink, 60/40 solder. JIS (MEB 0100): print the thrust-test fixture (the M2 and M3 hardware waits for the Dec run, after the CAD sets the lengths) and ask how long JIS prints take. | Bag of parts; fixture printed |
| Nov 17-19 | Arduino IDE, the CH340 driver and the RF24 library. A firmware skeleton that compiles: motor pins set LOW before `radio.begin()`, the radio packet struct, the failsafe timer. Compiling proves nothing about whether it works. Watch the soldering videos. | Skeleton compiles |
| Fri Nov 20 | Pack for home: the ECE bag, the IOLab pack, safety glasses, the laptop, the printed fixture. | Everything packed |

> Why five hole sizes: a printed hole comes out about 0.1-0.3 mm smaller than drawn, so the best fit is probably near 8.7 mm. Test sizes on both sides of the guess. The result only holds for the same printer, material and print orientation, so print the real frame the same way.

> Design every part to print without supports: flat face on the bed, overhangs under about 45°. Bambu Studio shows overhangs in red after slicing, so I check there before printing. This is why I don't need support filament.

> If a package is late: everything on Nov 21-23 waits on it. Reorder from Amazon for two-day delivery the same day, and do the breadboard work that doesn't need the missing part first.

## Phase 1: Thanksgiving break at home (Sat Nov 21 to Sun Nov 29), about 25-30 hrs
Goal: every part tested, the thrust measured, every subsystem working on the breadboard, and the board layout fixed. No frame or flying yet.

| Days | Task | Done when |
|---|---|---|
| Sat Nov 21 | Unbox everything and check it against the Sheet; reorder anything wrong or missing the same day. A1 mini setup: firmware update, calibration, Wi-Fi, one PETG test print, then the motor-mount coupon. Charge one LiPo in Dad's LiPo bag, watching it. | Every package checked; coupon printed on the A1 mini |
| Sun Nov 22 | Soldering practice (#16) on the spare perfboard: through-hole joints, tinned wires, a wire-to-wire splice with heat shrink, a solder bus. Then the GY-521 header pins and the MT3608 leads. Smoke test each part (#7): the Nano uploads Blink; the MT3608 is set to 5.0 V with a multimeter before anything is connected; the AMS1117 gives 3.3 V; the MPU-6050 shows readings (I2C scanner, then raw values); each motor spins on a battery; battery polarity is checked. | Joints pass a tug test and a continuity check; every part checked off |
| Mon Nov 23 | Try the motors in the coupon and choose the hole size. Thrust and current test (#14): one motor in the coupon on the fixture, no Arduino. Record four numbers: battery voltage at rest, voltage across the motor while it runs, amps, grams. Follow the thrust-stand rules in `SAFETY.md`. Then the weight plan: from the measured thrust, set the all-up weight limit, choose the MOSFET type (TO-220 or the lighter SOT-23), and set a gram limit for frame + guards. If SOT-23: solder five AO3400As onto adapter boards with the flux pen before Nov 26, and use them in the motor-driver test. | Numbers in `docs/data/thrust-test.csv`; decision written in `PLAN.md` |
| Mon-Tue Nov 23-24 | IMU code (#9): read gyro + accel, calibrate gyro offsets at startup, write a complementary filter for roll/pitch angle. Plot it in the Serial Plotter. Build the transmitter: second Nano on the ECE breadboard, both joysticks, arm and kill buttons, buzzer, two LEDs, its own AMS1117 and radio, powered from the USB power bank. | Tilting the board gives a smooth, correct angle with no drift; every transmitter input prints correctly over serial |
| Wed Nov 25 | Radio link (#10): the transmitter sends a 4-channel packet (throttle, yaw, pitch, roll) at about 50 Hz through the nRF24L01 to the flight Nano. Add the failsafe: motors off if no packet arrives for 250 ms. Decide how the spring-centered throttle stick maps to throttle. Send data back to the transmitter in the acknowledgement packet (telemetry), because tuning with props on can't use a USB cable. | Stick values print on the Nano; unplugging the transmitter trips the failsafe; the transmitter prints data sent back by the flight Nano; the link holds across the room (range test, props off) |
| Thu-Fri Nov 26-27 | Thanksgiving, lighter days. One motor driver (#11): MOSFET + gate resistor + pull-down + flyback diode, driven by PWM. Ramp the throttle from the joystick. Props off (USB may stay connected only because the props are off). Check the MOSFET is genuine: under 0.1 V from drain to source with the motor at full PWM. Battery sense: compare the Nano's reading with the multimeter at two charge levels and set the scale factor. | The motor speed follows the stick smoothly; the Nano doesn't reset under load; the MOSFET stays cool; the battery reading is within 0.05 V of the multimeter |
| Sat-Sun Nov 28-29 | Lay out the perfboard (#17) from the KiCad schematic: every part, the power and ground bus, the battery-sense wire and the mounting holes, then fix the board's outline. Weigh every real part (#13) on the 0.01 g scale and update the weight budget. Start the frame CAD (#12) if time allows. | A layout drawing with outer dimensions; weight budget filled in with measured values |

Checkpoint, Nov 29: add the measured part weights to the frame's estimated mass. If the total is above the limit from the weight plan, fix it now. Options, cheapest first: a thinner frame, SOT-23 MOSFETs, a smaller battery, or a 3.3 V Pro Mini instead of a Nano with a boost converter.

## Phase 2: Back on campus (Mon Nov 30 to Thu Dec 10), about 15-20 hrs
Goal: the frame designed and every printed part done by Dec 10, so the build over break starts with a known-good frame. I bring the A1 mini and all the parts back to campus. This phase is heavier than it was before the shipping change, right before finals, so the first things to cut are the transmitter case and then the extra frame variants.

| When | Task |
|---|---|
| Nov 30 to Dec 3 | SolidWorks lessons 2-3 (#12): main frame plate (≈120 mm motor-to-motor diagonal) designed around the board outline from the layout, battery tray for 57×18×7.6 mm, and attachment points for the single-axis rig on both the roll and pitch axes. Use the mass properties tool with PETG to estimate the frame weight. Print a first frame on the A1 mini and check the fit with the real parts. |
| Dec 3-6 | Fix any frame problems. Lesson 4: prop guards. Lesson 5: assembly with GrabCAD models. Check that every part fits, that nothing tall reaches the prop plane (lay the MOSFETs and the 220 µF cap flat), and that neighboring guards don't collide. |
| Dec 5-9 (my A1 mini, JIS as backup) | Print in PETG: 2-3 frame variants that differ where I'm least sure (for example a light one and a stiffer one), 1 set of prop guards plus spares, and the single-axis test rig. The rig's pivot line should pass through the quad's center of mass. I can't measure that yet, so the rig gets pivot holes at a few heights. Start early enough for a reprint. |
| By Dec 10 | Last campus run: the M2 frame hardware (12 screws at the length from the CAD, 12 locknuts, 8 washers), the rig pivot hardware (2 M3 screws, 2 M3 locknuts, 4 M3 washers), extra wire, anything that broke. |
| Optional | Lesson 6: transmitter case. It isn't essential, so it's the first thing to cut if I run short of time. |

> The frame is designed before the electronics are soldered, which is the riskiest part of this schedule. The A1 mini is always with me, so a wrong frame can still be reprinted over winter break, at the cost of build days.
## Finals (about Dec 11 to 18): project paused
Nothing scheduled. Optional: read a reference Nano quad build or the MultiWii source for PID structure and starting gains, and decide what the fallback would really be (#19): stock MultiWii has no nRF24 receiver and puts a motor on pin D11, which the radio needs.

## Phase 3: Winter break week 1 (Sat Dec 19 to Sun Dec 27): build
Goal: an assembled quad that runs all four motors correctly with the props off.

| Days | Task | Done when |
|---|---|---|
| Dec 19-21 | Solder the perfboard from the layout: Nano, 4 MOSFET channels, diodes, 220 µF cap, battery lead, MT3608, AMS1117, radio with its 22 µF + 0.1 µF caps, battery-sense wire, status LED. Check continuity and for shorts with the multimeter before the first power-up. | It powers on from the LiPo with no smoke or resets |
| Dec 22-23 | Mount everything on the frame. Weigh the finished quad. Balance it at the frame center with the battery in, and move the battery until it sits level. Note the IMU's orientation in code. Stage 0 (#18): all eight firmware safety features in `SAFETY.md` section 5 work on USB power, with no battery connected. | Under the weight limit; IMU axes match the frame; Stage 0 passes |
| Dec 24-26 | Christmas. | |
| Dec 27 | Stage 1: motor mixing, props off. Confirm each motor number, its spin direction, and that roll/pitch/yaw commands speed up the correct motors. Plug the battery in ten times: no motor may twitch at power-up. | All mixing checks pass on the bench |

## Phase 4: Week 2 (Mon Dec 28 to Sun Jan 3): control loop on the test rig
Goal: a tuned rate (gyro) loop on one axis at a time.

- Before the first spin with props on, check each prop matches its motor's spin direction and pushes air down.
- Vibration check on the rig: spin the motors at several throttles and plot the IMU angle. If it shakes, soft-mount the IMU on foam tape or lower the filter cutoff before tuning.
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
| Nov 13 | Every order placed, due at home by Nov 21 |
| Nov 20 | Campus purchases done; schematics drawn; packed for home |
| Nov 21 | Every package checked; coupon printed on the A1 mini |
| Nov 23 | Thrust measured; weight plan decided |
| Nov 29 | All subsystems work on breadboard; board layout fixed; real weight measured |
| Dec 6 | Frame CAD and prop guards done |
| Dec 10 | All PETG parts printed |
| Dec 23 | Quad assembled; Stage 0 safety features pass |
| Dec 27 | Mixing checked with props off |
| Jan 3 | Rate loop tuned on the rig, or switch to the fallback |
| Jan 10 | Untethered hover |
| Jan 17 | Done + documented |
