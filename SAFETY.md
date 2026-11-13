# Safety guide

This build has four kinds of hazard: LiPo batteries (fire), spinning props (cuts, eye injuries), soldering (burns, fumes), and my own firmware (a bug can start the motors when I don't expect it). The test procedure in section 6 works up through them in stages, so that a mistake happens with as little energy present as possible.

Three rules prevent most accidents:
1. **Props stay off** until the test stage says otherwise. The one early exception is the thrust-stand test, which has its own rules in section 6.
2. **The battery stays unplugged** whenever my hands are on the quad, and whenever I upload code.
3. **Never leave a battery charging unattended.**

## 1. LiPo batteries

The batteries are 1S (one cell), 3.7 V, 600 mAh, about 2.2 Wh each. That's small, but a shorted or punctured cell can still catch fire violently and give off toxic smoke.

### Voltage limits (per cell)
| Voltage | Meaning |
|---|---|
| 4.20 V | Fully charged. Never charge above this. |
| ~3.80 V | Storage charge. Leave cells here if they won't be used for more than a few days. |
| ~3.5 V (under load) | Warn. The firmware blinks an LED or sends a flag back. |
| ~3.3 V (under load, for 2 s) | Land. A starting value; check it against a real measurement (see below). |
| 3.0 V (resting) | Damaged. Below this the cell is unsafe to recharge: retire it. |

The warn and land limits are under-load voltages. While the motors run, the battery reads lower than it does at rest. This is **voltage sag**, caused by the battery's internal resistance. With four motors on a cell this small, expect a drop of roughly 0.3-0.6 V (an estimate; measure it in the thrust test). A single 3.5 V "land" limit would probably trip within the first minute of a flight. To check the two numbers above: after landing at the "land" voltage, the battery should rest at about 3.7 V or higher (roughly 20% left). If it rests lower, raise the limit.

### Charging
- Charge only with the included 1S USB charger, inside the LiPo bag, on a non-flammable surface (tile, concrete, a metal tray). Don't charge on a bed, carpet or desk clutter.
- Stay in the room. No charging overnight or while out.
- Let a cell cool to room temperature after flying before charging it.
- A smoke alarm should be within range of the charging spot.

### Inspect before every charge and every flight
Retire the cell if it's puffy or swollen, dented, punctured, has split wrap or damaged leads, or gets hot (warm is normal after flying; hot to the touch is not).

### Wiring
- Check polarity before plugging in. JST pigtails and cheap batteries don't always use the same red/black convention. Measure with the multimeter: + on red. A reversed battery will destroy the boost converter and possibly the Nano.
- Cut and solder one battery lead at a time so the two wires can never touch. Cover every exposed joint with heat-shrink.
- Never let bare leads touch each other or metal (keys, tweezers, the multimeter probes across both pins).
- Multimeter leads: the red lead goes in the V socket to measure voltage. In the 10 A socket the meter is almost a plain wire, so touching the probes across a battery shorts it. Move the lead back to the V socket as soon as a current measurement is done.

### Storage and transport
- Store the cells in the LiPo bag at storage charge, away from heat and direct sun.
- Driving home: keep them in the bag, and don't leave them in a hot car for long periods.
- Flying home: spare lithium batteries go in carry-on only, never in checked bags. Tape over or cap the plugs. These cells are far below the 100 Wh limit.

### If a battery smokes or catches fire
1. Don't touch it by hand. If it's on the charger, unplug the charger from the wall.
2. If it's safe to do, move the bag with tongs or a metal pan onto concrete, outdoors or into a sink.
3. LiPos contain no metallic lithium, so water is fine for cooling a small cell and putting out burning material around it. A standard ABC extinguisher works on the surroundings.
4. Get away from the smoke and ventilate the room. If the fire spreads, leave and call 911.

### Disposal
Discharge a dead or damaged cell completely (for example, through a resistor or a small bulb in the LiPo bag), tape over the leads, and drop it at a battery recycling point. Many hardware stores and Best Buy take them. Don't put lithium batteries in the trash.

## 2. Props and motors

8520 motors spin 75 mm props at roughly 40,000 rpm or more. A prop can cut skin, and eyes are the main risk: a broken blade or a flipping quad can hit them.

- Wear safety glasses for every test with props on. Anyone in the room wears them too.
- Props off for all bench work, wiring checks and code tests.
- Keep prop guards on for every flight until I'm confident in the tuning.
- Replace chipped or bent props right away. A damaged prop vibrates, which also ruins the IMU readings.
- After a crash, disarm first, then unplug the battery, then pick the quad up.
- Hold the quad by the frame, never near the props, and only when it's disarmed with the battery unplugged.
- The motor cans get hot after a flight. Give them a minute.

## 3. Soldering

- Use a stand for the iron. Never set it down on the bench.
- Solder in a ventilated room and stay out of the fume plume. A small fan pulling the smoke away from my face is enough at this scale.
- If the solder contains lead, wash hands afterward and don't eat while soldering.
- Wear safety glasses, especially when clipping leads. Clipped ends fly.
- Unplug the iron when finished or when leaving the room. Let it cool before packing it up.
- Never solder anything with the battery plugged in.
- For burns, run cool water for 10+ minutes. Get medical help for anything larger than a coin or blistering badly.

## 4. Electronics and power

- Set the MT3608 to 5.0 V with the multimeter before connecting it to the Nano. These boards can output far more than 5 V and will kill a Nano instantly.
- After soldering and before the first power-up: check with the multimeter for shorts between battery + and −, and between 5 V and GND. Look over every joint for solder bridges.
- At the first power-up, use the multimeter and watch for heat, smoke or smell. Unplug immediately at any sign of them.
- USB and the LiPo together only with props off. An upload resets the Nano, so always unplug the LiPo before uploading. With props off (Stage 1 and the breadboard motor test) USB may stay connected for serial output. With props on, no USB cable at all: the quad sends its data back over the radio.
- Each MOSFET gate needs its 10 kΩ pull-down. Without it, motors can twitch on during reset or upload, while the Nano's pins are floating.
- Set every motor pin LOW before the radio starts. Pin D10 is the SPI "slave select" pin, and `SPI.begin()` (which `radio.begin()` calls) drives it HIGH unless it is already an output. If a motor is on D10 and the radio starts first, that motor runs at full power until the code writes the pin. The pull-down can't stop this: it only holds a floating pin low, and this pin is being driven.
- Every motor needs its flyback diode. Without it, voltage spikes can reset the Nano mid-flight.
- Put a 22 µF (10 µF or more) and a 0.1 µF cap across each radio's power pins, as close to the module as possible. Put the 220 µF cap across the motor power bus, close to the MOSFETs: it absorbs the fast switching spikes. It can't hold the voltage up through battery sag; the boost converter is what keeps the Nano from browning out.

## 5. Firmware safety features (build these in before props go on)

These go into the code before Stage 2 (section 6). Test each one with the props off.

| Feature | Behavior |
|---|---|
| Motors off at boot | Every motor pin is set to output and driven LOW at the very start of `setup()`, before `radio.begin()` (see section 4). |
| Arming | The motors start only after a deliberate gesture, for example throttle at minimum + yaw held fully right for 1 s. The quad boots disarmed. |
| Throttle-low check | Refuse to arm unless the throttle is at minimum. The joysticks are spring-centered, so the throttle stick rests in the middle: map the middle and everything below it to zero throttle. Otherwise letting go of the stick after arming commands 50% throttle. |
| Radio failsafe | If no valid packet arrives for 250 ms, the motors turn off and the quad disarms. Test by turning off the transmitter. |
| Kill switch | A button on the transmitter disarms immediately, no matter what. |
| Tilt cutoff | If roll or pitch passes ~60°, the quad disarms. This stops a crashed quad from flailing on the floor. |
| Low battery | Under ~3.5 V while flying, warn (blink an LED or send a flag back). Under ~3.3 V for 2 s, lower the throttle limit gradually so the quad lands. See the voltage table in section 1. |
| Watchdog | Enable the ATmega's watchdog timer so a hang resets the board, which then comes up disarmed. |

## 6. Test procedure

Move to the next stage only when the current one passes all its checks. If something surprising happens, go back a stage.

### Thrust-stand test (Phase 0: one motor, no Arduino)
This is the only props-on test before Stage 2. It's allowed early because no firmware is involved and the motor is held down.
- Safety glasses on. Press one motor into the test coupon and mount it upside down at the end of an arm, so the prop blows air up and pushes the stand down onto the scale. Keep the prop at least 10 cm above the scale. Tape or clamp the stand so it can't tip or walk.
- Solder or twist the motor leads to a JST pigtail and insulate every joint. Start and stop the motor only by plugging and unplugging the battery connector, never by touching bare wires together.
- Keep hands, hair, sleeves and cables out of the plane of the prop. Stand to the side, not in line with the blades.
- Run for about 10 seconds at a time, then let the motor cool for a minute.
- For the current reading the multimeter goes in series on its 10 A socket. Move the red lead back to the V socket straight afterwards (see section 1, Wiring).
- Record: battery voltage at rest, voltage across the motor while it runs, amps, and grams.

### Stage 0: Bench, no motors
- Parts are powered from USB only. Check the IMU readings, the radio packets, the failsafe and the arming logic over serial.
- Pass: every feature in section 5 works with the motors unplugged.

### Stage 1: Motors, props off
- LiPo connected, props off, quad held down or clamped. USB may stay connected at this stage because the props are off; unplug the LiPo before every upload.
- Plug the battery in ten times and watch the motors. None may twitch at power-up.
- Confirm motor order, spin direction, and that roll/pitch/yaw speed up the correct motors. Test the failsafe, kill switch and tilt cutoff with the motors running.
- Pass: every motor behaves correctly; no motor moves at power-up; the failsafe and kill switch stop all motors immediately.

### Stage 2: Single-axis test rig, props on
- Safety glasses on. The quad is mounted on the rig so it can tilt in only one axis. Clamp or weigh the rig down.
- No USB cable from this stage on. Read the data the quad sends back over the radio.
- Tune the rate PID loop with low throttle limits in code (cap it at ~50%). Before moving on, raise the cap and repeat the test near the estimated hover throttle.
- Pass: the quad holds level on the rig and recovers from a gentle push without oscillating, at low throttle and near hover throttle.

### Stage 3: Tethered hover
- Prop guards on, safety glasses on. Tie the quad loosely to something heavy with ~30 cm of string, or use a fishing-line tether on each corner, so it can lift a little but can't fly into anyone.
- Pass: a steady low hover; the quad doesn't flip or race away; the kill switch works in the air.

### Stage 4: Free hover
- See the flight area rules below. Start under 30 cm over carpet; fly short flights and land at the voltage cutoff.
- Pass: controlled takeoff, hover and landing.

### Flight area (Stages 2-4)
- Indoors, a cleared room at least about 3 × 3 m. Use a carpet or a mat on the floor.
- No people or pets in the flight area. Anyone watching stays behind the pilot with safety glasses on.
- Keep clear of ceiling fans, curtains, cords, anything fragile, and faces. Stand at least 2 m from the quad, never under it.
- Have the LiPo bag open nearby to put a damaged battery in.

### Checklist before each session
- [ ] Safety glasses on (and on anyone watching)
- [ ] Battery inspected (not puffy, not damaged) and charged
- [ ] Props and guards undamaged and tight
- [ ] Frame and wiring intact; nothing loose near the props
- [ ] Transmitter powered first, then the quad plugged in
- [ ] Kill switch tested on the ground before arming
- [ ] Flight area clear

### After each session
- Disarm, then unplug the quad's battery, then turn off the transmitter.
- Check the battery temperature and condition. Put it in the bag, and bring it to storage charge if the next flight isn't soon.
- Check the props and frame for cracks. Note what changed in the tuning log.

## 7. Rules and permissions
- Keep a copy of the FAA TRUST certificate on my phone.
- Under 250 g and flown for recreation: no FAA registration or Remote ID needed. Indoor flight isn't in FAA airspace at all.
- For any outdoor flight: most of Champaign-Urbana is controlled airspace. Check before flying, and follow the recreational rules (under 400 ft, within sight, not over people).
- On campus, UIUC policy FO-05 requires permission from the Division of Public Safety before flying on campus property. Assume that includes indoors (dorm rooms, buildings) until DPS says otherwise.
