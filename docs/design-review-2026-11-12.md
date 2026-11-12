# Design review: Nov 12, 2026 (before buying parts)

An independent, skeptical review of the design as recorded in `CLAUDE.md`, `PLAN.md`, `TIMELINE.md`, `SAFETY.md`, `parts.md`, the planning handoff, and the **Parts List** tab of the Google Sheet (read Nov 12). Its job is to find what will make the project fail. Nothing here has been tested on hardware.

**How to read the numbers:** every weight, thrust and current below is an **estimate** from typical values for these part types, not a measurement and not from a datasheet unless it says so. The point is the *range*. Replace each estimate with a measured value as parts arrive.

**Ranking:** **Blocker** = the project fails or stalls unless fixed, with a "fix by" date · **Should fix** = likely to cost days or a part · **Nice to have** = makes it better or easier.

---

## 1. Will it fly? Thrust-to-weight

### All-up weight estimate (grams)
| Part | Low | Likely | High | Basis |
|---|---|---|---|---|
| 4× 8520 motors, with leads | 18 | 20 | 22 | typical 4.5–5.5 g each |
| 4× 75 mm props | 2 | 2.5 | 3 | **missing from the current budget** |
| 1S 600 mAh LiPo | 15 | 17 | 18 | listing says ~18 g |
| PETG frame, 120 mm diagonal | 10 | 15 | 25 | depends entirely on the CAD |
| Nano, headers soldered | 5 | 7 | 7.5 | |
| GY-521 (MPU-6050) | 1.5 | 2 | 2.5 | |
| nRF24L01+ (PCB antenna) | 1.5 | 2 | 2.5 | |
| AMS1117-3.3 module | 0.8 | 1 | 1.5 | |
| MT3608 module | 1.5 | 2 | 2.5 | |
| 4× IRLZ44N, TO-220 | 7 | 7.5 | 8 | ~1.9 g each |
| 4× MBR360G, DO-201AD | 4 | 4.4 | 4.8 | ~1.1 g each; a 1N5819 is ~0.35 g |
| 470 µF 25 V radial cap | 1.2 | 1.6 | 2.2 | the 25 V can is bigger than a 10 V one |
| 8 resistors + 10 µF cap | 1 | 1.5 | 2 | |
| Perfboard 1.5×1.75 in | 3 | 4 | 5 | FR-4 is ~1.85 g/cm³ |
| Wire, solder, heat shrink | 3 | 4.5 | 6 | |
| JST pigtail | 0.6 | 0.8 | 1 | |
| Screws + battery strap | 1.5 | 2.5 | 3.5 | |
| **Total, no prop guards** | **77** | **95** | **117** | |
| Prop guards (4 rings) | 8 | 11 | 16 | **missing from the current budget**; SAFETY.md requires them |
| **Total, with guards** | **85** | **106** | **133** | |

The `parts.md` budget (82–95 g) leaves out props and guards, and assumes the low end for nearly everything.

### Thrust
The only thrust figure in the project is "~30 g per motor" from the motor listing. It hasn't been measured. Thrust from a fixed-pitch prop goes roughly with rpm², and a brushed motor's rpm goes roughly with voltage, so **thrust ∝ V²** (an approximation). A 1S cell sags from 3.7 V to ~3.5 V under load, which costs about 11% of thrust.

| Thrust per motor (at 3.7 V) | Total at 3.7 V | Total at 3.5 V (sagged) | T/W at 95 g | T/W at 106 g (guards) |
|---|---|---|---|---|
| 25 g | 100 g | 89 g | 1.05 → 0.94 | 0.94 → 0.84 |
| **30 g (listing)** | **120 g** | **107 g** | **1.26 → 1.13** | **1.13 → 1.01** |
| 35 g | 140 g | 125 g | 1.47 → 1.31 | 1.32 → 1.18 |
| 40 g | 160 g | 143 g | 1.68 → 1.50 | 1.51 → 1.35 |

**Verdict:** with the listed thrust and the likely weight, T/W is about **1.0–1.3**, below the 1.5 minimum in `CLAUDE.md`. At that ratio the quad hovers near 80–100% throttle, which leaves the PID loop almost no headroom to correct a tilt. It might lift off, but it won't be controllable. It only works if the motors make ≥ 35 g each *and* the build is kept light.

### What the lighter parts would save (estimates)
| Change | Saves |
|---|---|
| 1N5819 (DO-41) instead of MBR360G | ~3 g |
| SOT-23 MOSFETs (e.g. AO3400) on small adapter boards instead of TO-220 | ~5–6 g |
| Frame designed to 12 g instead of 15–25 g | 3–13 g |
| A 10 V (smaller) 470 µF cap | ~0.8 g |
| 450–500 mAh battery instead of 600 mAh | ~4–5 g (and ~1 min of flight) |

Even with the first three, the likely build is ~82 g without guards / ~90 g with light guards. That's why measuring the thrust is the first thing to do.

### Flight time (estimate)
If hover takes 6–8 A total and you use 80% of 600 mAh: 0.48 Ah ÷ 6–8 A ≈ **3.5–5 min**. Shorter if the quad is heavy, since hover throttle rises.

---

## 2. Will the electronics work?

| Check | Result | Reasoning |
|---|---|---|
| IRLZ44N gate drive from a 5 V pin | **OK** (if genuine) | Datasheet: R<sub>DS(on)</sub> 25 mΩ at V<sub>GS</sub> = 5 V, V<sub>GS(th)</sub> 1–2 V. At 3 A: P = I²R = 9 × 0.025 ≈ 0.23 W per FET, so it stays cool. With 220 Ω and Q<sub>g</sub> ≈ 48 nC, switching takes a few µs, negligible at ~1 kHz PWM. |
| IRLZ44N gate drive from 3.3 V (the Pro Mini fallback) | **Not OK** | The datasheet only rates R<sub>DS(on)</sub> at 4 V and up. The "3.3 V Pro Mini" fallback in `PLAN.md` would need different MOSFETs (e.g. AO3400, rated at 2.5 V). |
| MT3608 current for Nano + IMU + radio | **OK** | 5 V load ≈ 25 mA Nano + 6 mA IMU + 22 mA radio/AMS1117 + ~40 mA LED/buzzer ≈ 0.09 A. Battery side ≈ 5 × 0.09 / (3.3 × 0.85) ≈ 0.16 A. The MT3608's switch limit is several amps. |
| Logic brownout from motor current | **OK** with the boost | The MT3608 keeps regulating with its input down to ~2 V, so battery sag from motor surges doesn't reach the Nano. |
| MT3608 → Nano connection | **Trap** | It must go to the Nano's **5V** pin. Into **VIN** it would pass through the Nano's own 5 V regulator and come out at ~4 V. |
| AMS1117-3.3 dropout from 5 V | **OK** | Headroom 5.0 − 3.3 = 1.7 V; the datasheet's max dropout is 1.3 V at full load and less at the radio's ~15 mA. |
| nRF24 decoupling | **OK**, add a ceramic | 10 µF electrolytic **plus** a 0.1 µF ceramic (in the starter kit), both soldered right at the module's VCC/GND pins. |
| 470 µF cap "stops brownouts" | **Wrong reason** | Hold-up time: ΔV = I·t / C = 10 A × 1 ms / 470 µF ≈ 21 V, so the cap can't hold the voltage up for even 1 ms of sag. It absorbs fast switching spikes from the PWM edges. The boost converter is what prevents brownouts. Mount it close to the MOSFETs and motor + bus. |
| Flyback diode placement | Note | Across each **motor's** terminals: cathode (stripe) to battery +, anode to the MOSFET drain. Put it close to the MOSFET, not out at the motor. |
| Grounds | **Needs a plan** | ~10 A of motor current returns through the MOSFET sources to battery −. If that current flows through the same thin path as the IMU's ground, the IMU reads noise. Run a thick bare-wire ground bus from battery − to the four sources, and tap logic ground once at battery − (a **star ground**). |
| Wire gauge | **Needs a plan** | 26 AWG is fine for one motor (≤ 3 A, 10 cm round trip ≈ 13 mΩ, 40 mV drop). The battery lead and the + / − buses carry all four motors (~8–12 A, estimate): keep the 22 AWG pigtail short and use bare 22 AWG bus wire on the board. |
| GY-521 I²C pull-ups | Marginal, usually works | The board pulls SDA/SCL up to 3.3 V; a 5 V ATmega needs ≥ 3.0 V for a logic high. Most builds work. If the IMU drops out, add pull-ups. |
| PWM pins vs radio pins | **Conflict** | See finding B3. |

---

## 3. Findings

### Blockers

**B1. Thrust-to-weight is ~1.0–1.3, not 1.5–2.** *Fix by: before choosing the driver parts (now) and before the frame CAD.*
Reason: section 1. The budget leaves out props and guards and takes the optimistic end for everything else. The 30 g thrust figure is unverified.
Fix:
1. **Measure the thrust first** (Phase 0, as soon as the motors arrive): print a stand that holds one motor with the prop blowing *up* (so it presses down on a scale), put it on a scale, and connect the motor straight to a charged battery through the multimeter's 10 A range (if the AM33D has one; check the manual). Record grams and amps. Props on, safety glasses on, motor clamped. No Arduino is involved, so no firmware risk.
2. Add props and guards to the weight budget, and give each subsystem a gram limit (e.g. frame + guards ≤ 20 g).
3. Choose the lighter parts *now*, while you're buying: 1N5819 instead of MBR360G (finding S1). Consider SOT-23 MOSFETs on SOT-23-to-DIP adapter boards; the soldering is harder (see section 5), so order a few and decide after practicing.
4. Decision rule: if measured thrust × 4 × 0.89 (sag) is under 1.5 × your weight, cut weight (frame, battery) before building further, or look for a higher-thrust brushed motor.

**B2. No soldering tools or measuring tools on the required list.** *Fix by: the Amazon order, today.*
Reason: the iron is "optional" with no link, but soldering starts on **Nov 21**: the GY-521 headers usually come loose, and the MT3608 needs leads before you can set its voltage. Nothing on the list includes flux, solder wick, wire strippers or flush cutters. The ME side needs tools too: the 0.2 mm press-fit tolerance can't be checked without **calipers**, and "weigh every part on a kitchen scale" fails at 1 g resolution when parts weigh 1–2 g.
Fix: add to the order a temperature-controlled iron (a fixed 25 W iron struggles with the battery and ground-bus joints), a flux pen, solder wick, a brass tip cleaner, wire strippers rated for 22–30 AWG, flush cutters, a helping-hands clamp, **digital calipers** and a **0.01 g pocket scale**. (I haven't verified links for these. Ask for verified ASINs before buying.)

**B3. The radio's SPI pins collide with the motor PWM pins.** *Fix by: before soldering the perfboard (Dec 19), ideally before Phase 1 radio work.*
Reason: the nRF24 must use the ATmega's hardware SPI on **D11 (MOSI), D12 (MISO), D13 (SCK)**. The Nano's hardware PWM pins are D3, D5, D6, D9, D10, D11, so with SPI in use only five are left. D5 and D6 run on Timer0, which also runs `millis()`/`micros()`, so you can't change their PWM frequency without breaking your loop timing.
Fix: write a **pin map** in the repo now. One that works: motors on **D3, D9, D10, D5** · radio CE = D7, CSN = D8 · SPI D11–D13 · IMU on A4 (SDA) / A5 (SCL) · battery sense on A0 · status LED/buzzer on D4 (the onboard LED on D13 blinks with SPI traffic, so it can't be your warning light). Then set Timer1 and Timer2 to ~980 Hz fast PWM so all four motors match D5's frequency (or accept 490 Hz/980 Hz mismatch at first). Concept: **timer/counter peripherals**; each timer drives two specific pins.

### Should fix

**S1. MBR360G diodes: heavy, and the leads may not fit the perfboard.** *Fix by: the ECE trip (Nov 16–18).*
Reason: DO-201AD leads are ~1.2–1.3 mm thick (onsemi package drawing); standard perfboard holes are ~1.0 mm. Four of them weigh ~4.4 g against ~1.4 g for 1N5819s.
Fix: order a pack of 1N5819 (or SS14 if you go surface-mount) with the Amazon cart; ask for a verified link. For the Nov 25 breadboard test, the starter kit's 1N4007 is fine at low PWM frequency.

**S2. The 1.5×1.75 in perfboard is too small for the planned parts.** *Fix by: the ECE trip.*
Reason: the Nano alone is 45 mm long; the board is 44.5 mm. Four TO-220s side by side take ~45 mm more, plus the MT3608 (~37×17 mm), AMS1117 and radio.
Fix: lay the board out first, on graph paper or as a SolidWorks sketch at 2.54 mm pitch. Buy the 5×7 cm board **and** a second cheap board for practice. Cut the real one to size after layout. Alternatively, mount the MT3608 and radio on the frame and wire them in.

**S3. Battery current, C-rating and connector are unchecked.** *Fix by: when the motors arrive (thrust test).*
Reason: four 8520s at full throttle probably draw **8–12 A** total (estimate; measure it). A 600 mAh cell needs a **≥ 20C** continuous rating to supply 12 A. The Sheet doesn't list the rating. The JST-RCY connector is commonly rated around 3 A; at 3× that it heats up and drops voltage, which costs thrust.
Fix: look up the battery's C-rating in the listing. Measure one motor's full-throttle current in the B1 test and multiply by 4. After the first hovers, feel the connector; if it's hot, that's lost thrust.

**S4. Check the motor/prop/plug details before ordering the spares.** *Fix by: today.*
Reason: these are cheap mistakes that become expensive ones.
- **Prop bore vs motor shaft:** 8520s are sold with both 0.8 mm and 1.0 mm shafts. A loose prop flies off. Check the motor listing; measure with calipers on arrival.
- **CW/CCW:** brushed motors spin one way set by polarity; 8520 packs are usually 4 CW (red/blue wires) + 4 CCW (black/white). Check the listing says so.
- **Motor connector:** bare leads or a small plug? If it's a plug, plan to cut it off and solder.
- **Battery plug type:** confirm the battery really has a JST-RCY (vs Molex 51005, PH2.0, etc.) from the listing photos before buying pigtails. The pigtail pairs give you both genders, so gender is fine if the type matches.

**S5. Test the IRLZ44Ns on arrival: cheap multi-packs are often relabeled.** *Fix by: Phase 0 smoke test.*
Reason: fake "IRLZ44N"s are frequently IRFZ44Ns (not logic-level, the same problem as the IRF520). A half-on MOSFET overheats and costs thrust.
Fix: with one motor (props off) on the 5 V-driven MOSFET at full PWM, measure the drain–source voltage. Genuine: under ~0.1 V at 2–3 A. Hundreds of mV or a hot tab = fake.

**S6. The watchdog may lock up the Nano.** *Fix by: before the Stage 0 safety tests.*
Reason: many Nano clones ship with the **old bootloader**, which doesn't clear the watchdog after a watchdog reset. The board then resets forever. The 10 kΩ pull-downs keep the motors off, so it's safe, but the quad is dead until unplugged. (I'm fairly but not fully sure about this; it's a known issue with the old ATmegaBOOT.)
Fix: run a 2-minute test sketch that enables the watchdog and then hangs on purpose. If the board doesn't come back, burn **Optiboot** onto it using the Uno as an ISP programmer (Arduino IDE: *Arduino as ISP*, then *Burn Bootloader* with the board set to the "new bootloader" option).

**S7. The MultiWii fallback isn't drop-in.** *Fix by: finals week (when the timeline already suggests reading MultiWii).*
Reason: mainline MultiWii reads a standard PWM/PPM receiver, not an nRF24, and on a 328P it puts a motor on **D11**, which the radio needs for SPI (B3).
Fix: either find and bench-test a MultiWii fork that supports nRF24 on your pinout, or plan the fallback as a spare Nano acting as an nRF24 → PPM receiver (+~7 g, which makes B1 worse). Write the chosen fallback into `PLAN.md`.

**S8. Not all safety features are scheduled, and low-battery sensing has no hardware.** *Fix by: before Stage 1 (Dec 27).*
Reason: `SAFETY.md` requires all eight firmware safety features before motors run with the battery. `TIMELINE.md` schedules the failsafe (Nov 23–24) and arming (Dec 27), but not the kill switch, tilt cutoff or watchdog, and puts the low-battery check in Phase 5, after props are on. The low-battery feature needs a battery-sense wire to an analog pin (A0, with a ~10 kΩ series resistor) and an LED or buzzer, and neither is in the wiring plan.
Fix: add a task "implement and bench-test all 8 safety features" to Phase 1 or Dec 19–21, and add the battery-sense resistor and LED/buzzer (both in the starter kit) to the pin map and board layout.

**S9. Prop guards collide at a 120 mm diagonal, and tall parts may reach the props.** *Fix by: frame CAD (Nov 28–29).*
Reason: adjacent motors are 120 / √2 = **84.9 mm** apart. Prop tips then clear each other by 84.9 − 75 = 9.9 mm, but a guard ring with a 3 mm gap and 1.2 mm wall is 75 + 2(3 + 1.2) = **83.4 mm** across, so neighboring guards nearly touch. Each prop disc also reaches within ~22 mm of the frame center, over the board corners. A standing TO-220 (~20 mm tall with tab) or the 470 µF can (10 × 16–20 mm) could reach the prop plane.
Fix: model the guards as one connected ring frame, or widen to ~130 mm. Lay the TO-220s and the cap flat. Check prop clearance in the SolidWorks assembly (lesson 5).

**S10. The first real soldering is the hardest board, with no practice scheduled.** *Fix by: Thanksgiving break.* See section 5.

### Nice to have

- **N1. Higher PWM frequency.** At ~490–980 Hz the coreless motors whine and draw more RMS current for the same thrust. Once the basics work, try ~4–8 kHz on Timer1/Timer2. Keep the 220 Ω gate resistor (or drop it to ~100 Ω) and check the MOSFETs stay cool.
- **N2. Fix the 470 µF note in the Sheet**: it absorbs switching spikes; the boost converter stops brownouts (section 2). A 10–16 V low-ESR part is smaller and lighter.
- **N3. 0.1 µF ceramic caps** across each motor's terminals cut brush noise (starter kit has them; ~0.05 g each).
- **N4. Soft-mount the IMU** (foam tape or rubber grommets) and set the MPU-6050's digital low-pass filter (DLPF ~42 Hz) in firmware. Motor vibration otherwise swamps the accelerometer.
- **N5. Transmitter power.** The kit's 9 V battery lasts only a few hours through the Uno's regulator. A USB power bank is better. Add a 10 µF cap on the transmitter's nRF24 too.
- **N6. Where is your own 3D printer?** If it's at home, you can print over break and the JIS deadline matters much less.
- **N7. Check motor temperature after flights.** PETG softens near 80 °C, and hot motor cans can loosen a press fit over time (creep).
- **N8. Define a "minimum portfolio result"** now: e.g. "rate loop holds level on the single-axis rig, with plots and video." Then a late hover still leaves a finished project.
- **N9. Upload gotcha:** ELEGOO/CH340 Nanos often need *Processor: ATmega328P (Old Bootloader)* in the Arduino IDE until you burn Optiboot (S6).

### Checked and fine
MT3608 current capacity · AMS1117 dropout from 5 V · IRLZ44N gate drive at 5 V (if genuine) · boost converter as the brownout fix · 250 ms radio failsafe · the bench → props-off → rig → tether → free-hover order in `SAFETY.md` · 26 AWG stranded for individual motor leads · the USB cable for the Uno (included in the starter kit) · ECE's Mini-B cable for the Nanos · the URGENEX charger matching its own battery plug.

---

## 4. Build/test sequence and schedule

**The order is sound:** bench with USB only → motors props-off → single-axis rig → tether → free hover, with "go back a stage if surprised." Keep it.

**Gaps:**
1. **Thrust/current test is missing** and should come first (B1), before frame CAD.
2. **Safety features aren't all scheduled** before Stage 1 (S8).
3. **Soldering practice is missing** (S10, section 5).
4. **The single-motor breadboard test (Nov 25)** is fine props-off (~0.3–0.5 A), but don't run a motor with a prop through breadboard contacts, which are rated about 1 A.
5. **The Nov 29 weight checkpoint is too late** to change what you buy. Move the weight decision to as soon as the motors and battery arrive (measure them, plus the thrust).

**Is the schedule realistic?** Phases 0–3 (parts, subsystems, build) are achievable but tight for someone new to soldering; the radio link is the most common place to lose two days. Phases 4–5 (tuning to a hover in ~2 weeks) are the real risk, and the fallback (S7) needs work before it's a real fallback. I'd estimate a free hover by Jan 10 at roughly even odds; a stable rig result by Jan 17 is likely. That's a judgment, not a calculation.

---

## 5. Soldering: every joint, easiest to hardest

About **110–130 joints** in total (estimate; depends on layout).

| # | Joint | Count | Why it's this hard |
|---|---|---|---|
| 1 | Header pins on GY-521 (and AMS1117 if loose) | ~8–11 | Big pads, board held by the breadboard. Best first job. |
| 2 | Resistors on perfboard (4× 220 Ω, 4× 10 kΩ, battery sense) | ~18 | Through-hole, easy to inspect. |
| 3 | MOSFET legs (TO-220) | 12 | Easy pads, but the tab sinks heat; legs are close together, so watch for bridges. |
| 4 | Nano header pins into the perfboard | 12–30 | Many joints in a row; bridges between neighbors are the risk. |
| 5 | Caps: 470 µF, 10 µF + 0.1 µF at radio | ~6 | Easy, but **polarity** matters on electrolytics. |
| 6 | Flyback diodes | 8 | Easy, but **direction** matters (stripe to battery +). |
| 7 | Wires to MT3608 pads (IN+, IN−, OUT+, OUT−) | 4 | Small pads near the inductor; strip and tin first. |
| 8 | Signal wires: radio (7) and IMU (4), both ends | ~22 | 26 AWG strands fray; tin first. On the radio's 2×4 header, adjacent pins bridge easily. |
| 9 | Power/ground bus (bare 22 AWG along perfboard pads) | ~10–20 | Needs more heat than signal joints; this is where cold joints hide. |
| 10 | Motor leads to board (8) + any inline splices with heat shrink | 8–16 | Thin wire next to big joints; splices need heat shrink slid on *before* soldering. |
| 11 | Battery pigtail to board | 2 | Carries all the current; a short here is a LiPo fire risk. Highest consequence. |
| 12 | *(If chosen)* SOT-23 MOSFETs onto adapter boards | 12 | Surface-mount, 0.95 mm pin pitch; needs flux, tweezers and a fine tip. Hardest on the list. |

### Practice plan (~4 short sessions over Thanksgiving, before Dec 19)
1. **Basics, 1 hr:** read the free *Adafruit Guide to Excellent Soldering* first. On the spare perfboard, solder 20 header pins and 10 resistors. A good joint is shiny and cone-shaped, wetting both pad and lead. Remove three of them with solder wick.
2. **Wire, 1 hr:** strip and tin 26 AWG and 22 AWG; do 10 wire-to-pad joints and 5 wire-to-wire splices with heat shrink. Tug-test each.
3. **Real parts, spares first:** GY-521 headers (you have 3 boards), MT3608 leads (multi-pack), AMS1117. Then set the MT3608 to 5.0 V.
4. **One motor channel on scrap perfboard:** MOSFET + 220 Ω + 10 kΩ + diode + motor leads. Test it with the motor. It becomes the reference copy for the real board and lets you run the S5 MOSFET test.

After each session, inspect with your phone camera zoomed in, and run a continuity check between neighboring pins with the multimeter. Write what went wrong in the Problems table.
