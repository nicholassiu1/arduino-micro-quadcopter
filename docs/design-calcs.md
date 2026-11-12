# Design calculations

I keep the sizing math here so I can redo it when a number changes. Most inputs are still estimates or seller claims. Each one gets replaced with a measured value as the tests in `SAFETY.md` and the issues happen, and I note the date when it changes.

Last updated: Nov 12, 2026. Nothing has been measured yet.

## 1. Thrust-to-weight

A quad needs more thrust than its weight so it can climb and still have thrust left for the controller to correct with. For a small indoor quad I'm aiming for a thrust-to-weight ratio (T/W) of 1.5 to 2.

| Input | Value | Source |
|---|---|---|
| Thrust per 8520 motor with a 75 mm prop | ~30 g | Seller and reviewer claim, unmeasured |
| Total thrust, 4 motors | ~120 g (range I'm using: 120-130 g) | 4 × 30 g |

The heaviest the quad can be:

- At T/W = 1.5: 120 g / 1.5 = 80 g
- At T/W = 2.0: 120 g / 2.0 = 60 g

That's where the 80 g target comes from.

Where the weight budget in `parts.md` stands today:

| Build | Estimated weight | T/W at 120 g thrust | T/W at 130 g thrust |
|---|---|---|---|
| Without prop guards | ~92 g | 1.30 | 1.41 |
| With prop guards | ~103 g | 1.17 | 1.26 |

Both are under 1.5, so as planned the quad is too heavy. The thrust test (issue #14) decides how much weight has to come off. The cheapest cuts are listed at the bottom of `parts.md`.

Hover throttle is roughly 1 / (T/W). At T/W = 1.3 the motors would run at about 77% of full thrust just to hover, which leaves very little room for the PID loop to correct with.

## 2. Correcting the thrust test for battery sag

A single motor on a fresh battery sees about 3.9 V. In flight, four motors pull the same battery down to about 3.0-3.3 V (estimate), because of the battery's internal resistance. So a one-motor bench test overstates in-flight thrust.

For a fixed prop, thrust scales with prop speed squared, and speed rises with voltage. Thrust scales somewhere between V and V² depending on how much of the voltage is lost across the motor's own resistance. I'm using that range until I have measured points at two voltages.

| Flight voltage | Thrust factor if T ∝ V | Thrust factor if T ∝ V² |
|---|---|---|
| 3.3 V | 3.3 / 3.9 = 0.85 | 0.72 |
| 3.0 V | 3.0 / 3.9 = 0.77 | 0.59 |

So 30 g on the bench could be anything from about 18 to 25 g per motor in flight. During the test I record the voltage across the motor, not just the battery's resting voltage, so I can scale the result properly.

## 3. Motor current, battery and flight time

| Input | Value | Source |
|---|---|---|
| Current per 8520 motor at full throttle on 1S | ~1.5-2 A | Typical for this motor size, unmeasured |
| Battery | 1S 600 mAh, 25C | Third-party listing |

- Peak current is 4 × 2 A = 8 A.
- The battery is rated for 25C × 0.6 Ah = 15 A on paper. Cheap cells rarely meet their C rating, but 8 A is about half of it, so the battery should cope. Voltage sag matters more (section 2).
- Hover current: if hovering takes about 75% throttle, roughly 4 × 1.2 A ≈ 5 A (estimate).
- Flight time: using 80% of the capacity to protect the cell, 0.8 × 600 mAh = 480 mAh. At 5 A that's 480 / 5000 h ≈ 5.8 minutes. Real flights will be shorter, because the low-battery cutoff (3.3 V under load) comes before the cell is empty.

## 4. Motor driver: MOSFET, gate resistor, flyback diode

### Why a logic-level MOSFET
The Nano's pins put out 5 V. A standard MOSFET such as the IRF540 needs about 10 V on the gate to switch fully on. Half on, it acts like a resistor and gets hot. The IRLZ44N ("L" for logic level) is specified at 5 V gate drive.

| IRLZ44N spec (datasheet) | Value |
|---|---|
| R_DS(on) max at V_GS = 5 V | 0.025 Ω |
| R_DS(on) max at V_GS = 4 V | 0.035 Ω |
| Thermal resistance, junction to air, no heatsink (TO-220) | 62 °C/W |

Conduction loss at 2 A: P = I²R = 2² × 0.025 = 0.1 W. Temperature rise: 0.1 W × 62 °C/W ≈ 6 °C. No heatsink needed.

The catch is weight. A TO-220 package is about 2 g, so four of them are about 7.5 g, which is the biggest single saving in the electronics. A SOT-23 part on a small adapter board would save about 6-7 g. I'll decide after the thrust test (open question in `PLAN.md`).

### Gate resistor (220 Ω)
It limits the current the Nano pin supplies while the gate charges: 5 V / 220 Ω ≈ 23 mA peak. The ATmega328P's absolute maximum is 40 mA per pin, so that leaves margin.

### Gate pull-down (10 kΩ)
While the Nano boots, its pins float, and a floating gate can turn a motor on. The 10 kΩ resistor holds the gate at 0 V until the code drives the pin. This is the hardware half of "motors off at boot" in `SAFETY.md`.

### Flyback diode (1N5819)
A motor is an inductor. When the MOSFET turns off, the current keeps flowing and the voltage spikes until it finds a path. The diode across the motor gives it that path, so the spike doesn't reach the MOSFET or reset the Nano.

During PWM the diode carries the motor current for the off part of each cycle. Average diode current ≈ I_motor × (1 - duty). At 1.5 A and 50% duty that's 0.75 A, under the 1N5819's 1 A rating. At full throttle the duty is near 100% and the diode current is near zero. Its 25 A surge rating covers the brief peaks.

## 5. What gets measured, and where it goes

| Measurement | Replaces | Issue | Data file |
|---|---|---|---|
| Thrust and current per motor at two voltages | Sections 1, 2, 3 | #14 | `docs/data/thrust-test.csv` |
| Weight of every part | Weight budget in `parts.md`, section 1 | #13 | `parts.md` (Measured column) |
| Battery voltage under hover load | Section 2 voltages, low-battery limits | After first hover | to be added |
