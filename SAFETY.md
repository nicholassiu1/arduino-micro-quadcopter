# Safety guide

This build has four kinds of hazard: **LiPo batteries** (fire), **spinning props** (cuts, eye injuries), **soldering** (burns, fumes), and **your own firmware** (a bug can start the motors when you don't expect it). The test procedure at the end works up through them in stages, so a mistake happens with as little energy present as possible.

**Three rules that prevent most accidents:**
1. **Props stay off** until the test stage says otherwise.
2. **The battery stays unplugged** whenever your hands are on the quad, and whenever you upload code.
3. **Never leave a battery charging unattended.**

---

## 1. LiPo batteries

Your batteries are 1S (one cell), 3.7 V, 600 mAh, about 2.2 Wh each. That's small, but a shorted or punctured cell can still catch fire violently and give off toxic smoke.

### Voltage limits (per cell)
| Voltage | Meaning |
|---|---|
| 4.20 V | Fully charged. Never charge above this. |
| ~3.80 V | **Storage charge.** Leave cells here if they won't be used for more than a few days. |
| 3.50 V (under load) | **Land.** Your firmware should warn or land at this voltage. |
| 3.0 V (resting) | Damaged. Below this the cell is unsafe to recharge: retire it. |

### Charging
- Charge **only with the included 1S USB charger**, **inside the LiPo bag**, on a non-flammable surface (tile, concrete, a metal tray). Don't charge on a bed, carpet or desk clutter.
- **Stay in the room.** No charging overnight or while you're out.
- Let a cell cool to room temperature after flying before you charge it.
- A smoke alarm should be within range of where you charge.

### Inspect before every charge and every flight
Retire the cell if it's **puffy or swollen**, dented, punctured, has split wrap or damaged leads, or gets **hot** (warm is normal after flying; hot to the touch is not).

### Wiring
- **Check polarity before you plug in.** JST pigtails and cheap batteries don't always use the same red/black convention. Measure with the multimeter: + on red. A reversed battery will destroy the boost converter and possibly the Nano.
- Cut and solder one battery lead at a time so the two wires can never touch. Cover every exposed joint with heat-shrink.
- Never let bare leads touch each other or metal (keys, tweezers, the multimeter probes across both pins).

### Storage and transport
- Store the cells in the LiPo bag at storage charge, away from heat and direct sun.
- **Driving home:** keep them in the bag, not in a hot car for long periods.
- **Flying home:** spare lithium batteries go in your **carry-on only, never in checked bags**. Tape over or cap the plugs. These cells are far below the 100 Wh limit.

### If a battery smokes or catches fire
1. Don't touch it with your hands. If it's on the charger, unplug the charger from the wall.
2. If you can do it safely, move the bag with tongs or a metal pan onto concrete, outdoors or into a sink.
3. LiPos contain no metallic lithium, so **water is fine** for cooling a small cell and putting out burning material around it. A standard ABC extinguisher works on the surroundings.
4. Get away from the smoke and ventilate the room. If the fire spreads, leave and call 911.

### Disposal
Discharge a dead or damaged cell completely (for example, through a resistor or a small bulb in the LiPo bag), tape over the leads, and drop it at a battery recycling point. Many hardware stores and Best Buy take them. **Don't put lithium batteries in the trash.**

---

## 2. Props and motors

8520 motors spin 75 mm props at roughly 40,000+ rpm. A prop can cut skin, and your **eyes** are the main risk: a broken blade or a flipping quad can hit them.

- **Wear safety glasses** for every test with props on. Anyone in the room wears them too.
- **Props off** for all bench work, wiring checks and code tests.
- Keep **prop guards** on for every flight until you're confident in the tuning.
- Replace chipped or bent props right away. A damaged prop vibrates, which also ruins your IMU readings.
- After a crash, **disarm first**, then unplug the battery, then pick it up.
- Hold the quad by the frame, never near the props, and only when it's **disarmed with the battery unplugged**.
- The motor cans get hot after a flight. Give them a minute.

---

## 3. Soldering

- Use a **stand** for the iron. Never set it down on the bench.
- Solder in a **ventilated** room and stay out of the fume plume. A small fan pulling the smoke away from your face is enough at this scale.
- If your solder contains lead, **wash your hands** afterward and don't eat while soldering.
- **Wear safety glasses**, especially when clipping leads. Clipped ends fly.
- Unplug the iron when you're done or leave the room. Let it cool before you pack it up.
- Never solder anything with the battery plugged in.
- For burns, run cool water for 10+ minutes. Get medical help for anything larger than a coin or blistering badly.

---

## 4. Electronics and power

- **Set the MT3608 to 5.0 V with the multimeter BEFORE connecting it to the Nano.** These boards can output far more than 5 V and will kill a Nano instantly.
- After soldering and **before the first power-up**: check with the multimeter for shorts between battery + and −, and between 5 V and GND. Look over every joint for solder bridges.
- The first time you power up, use the multimeter and watch for heat, smoke or smell. Unplug immediately if you see any.
- **Unplug the LiPo before connecting USB.** That way the motors can't spin during an upload, and the USB and boost converter supplies never fight each other.
- Each MOSFET gate needs its **10 kΩ pull-down**. Without it, motors can twitch on during reset or upload, while the Nano's pins are floating.
- Every motor needs its **flyback diode**. Without it, voltage spikes can reset the Nano mid-flight.
- Put the 10 µF cap across the radio's power pins and the 470 µF across the battery input. Brownouts make the quad behave unpredictably.

---

## 5. Firmware safety features (build these in before props go on)

These belong in your code before Stage 2 (section 6). Test each one with the props off.

| Feature | Behavior |
|---|---|
| **Motors off at boot** | Every motor output is set to 0 on the first line of `setup()`. |
| **Arming** | The motors start only after a deliberate gesture: throttle at minimum + yaw held fully right for 1 s, for example. The quad boots **disarmed**. |
| **Throttle-low check** | Refuse to arm unless the throttle is at minimum. |
| **Radio failsafe** | No valid packet for 250 ms → motors off and disarm. Test by turning off the transmitter. |
| **Kill switch** | A button on the transmitter disarms immediately, no matter what. |
| **Tilt cutoff** | Roll or pitch past ~60° → disarm. This stops a crashed quad from flailing on the floor. |
| **Low battery** | Under ~3.5 V while flying → warn (blink an LED or send a flag back) and limit throttle. |
| **Watchdog** | Enable the ATmega's watchdog timer so a hang resets the board, which then comes up disarmed. |

---

## 6. Test procedure

Move to the next stage only when the current one passes all its checks. If something surprising happens, **go back a stage.**

### Stage 0: Bench, no motors
- Parts are powered from USB only. Check the IMU readings, the radio packets, the failsafe and the arming logic over serial.
- **Pass:** every feature in section 5 works with the motors unplugged.

### Stage 1: Motors, props off
- LiPo connected, props off, quad held down or clamped.
- Confirm motor order, spin direction, and that roll/pitch/yaw speed up the correct motors. Test the failsafe, kill switch and tilt cutoff with the motors running.
- **Pass:** every motor behaves correctly; the failsafe and kill switch stop all motors immediately.

### Stage 2: Single-axis test rig, props on
- Safety glasses on. The quad is mounted on the rig so it can tilt in only one axis.
- Tune the rate PID loop with low throttle limits in code (cap it at ~50%).
- **Pass:** the quad holds level on the rig and recovers from a gentle push without oscillating.

### Stage 3: Tethered hover
- Prop guards on, safety glasses on. Tie the quad loosely to something heavy with ~30 cm of string, or use a fishing-line tether on each corner, so it can lift a little but can't fly into you.
- **Pass:** a steady low hover; the quad doesn't flip or race away; the kill switch works in the air.

### Stage 4: Free hover
- See the flight area rules below. Start under 30 cm over carpet; fly short flights and land at the voltage cutoff.
- **Pass:** controlled takeoff, hover and landing.

### Flight area (Stages 2–4)
- Indoors, a cleared room at least about 3 × 3 m. Use a carpet or a mat on the floor.
- **No people or pets in the flight area.** Anyone watching stays behind you with safety glasses on.
- Keep clear of ceiling fans, curtains, cords, anything fragile, and your face. Stand at least 2 m from the quad, never under it.
- Have the LiPo bag open nearby to put a damaged battery in.

### Checklist before each session
- [ ] Safety glasses on (and on anyone watching)
- [ ] Battery inspected (not puffy, not damaged) and charged
- [ ] Props and guards undamaged and tight
- [ ] Frame and wiring intact; nothing loose near the props
- [ ] Transmitter powered **first**, then plug in the quad
- [ ] Kill switch tested on the ground before arming
- [ ] Flight area clear

### After each session
- Disarm → unplug the quad's battery → turn off the transmitter.
- Check the battery temperature and condition. Put it in the bag; put it at storage charge if you won't fly again soon.
- Check the props and frame for cracks. Note what changed in your tuning log.

---

## 7. Rules and permissions
- Keep a copy of your **FAA TRUST certificate** on your phone.
- Under 250 g and flown for recreation: no FAA registration or Remote ID needed. **Indoor flight isn't in FAA airspace at all.**
- If you ever fly **outdoors**, most of Champaign-Urbana is controlled airspace. Check before you fly, and follow the recreational rules (under 400 ft, within sight, not over people).
- **On campus**, under UIUC policy FO-05 you need permission from the Division of Public Safety before flying on campus property. Assume that includes indoors (dorm rooms, buildings) until DPS tells you otherwise.
