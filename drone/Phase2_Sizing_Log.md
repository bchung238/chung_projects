# Drone Build Project — Phase 2 Engineering Log: Conceptual Sizing

**Author:** Brian Chung
**Status:** Phase 2 (Conceptual Sizing) — in progress
**Last updated:** this session

---

## 1. Requirements (Phase 1 — locked)

| Parameter | Value | Notes |
|---|---|---|
| Configuration | Quadcopter | 4 motors, standard for first build |
| Propeller size target | 8 in | Midpoint of 7-10in range; starting assumption, not a calculated value |
| Frame size (est.) | ~350-450mm | Wheelbase (motor-to-motor diagonal) |
| Camera payload | ~150g | GoPro-style action camera, fixed mount, no gimbal |
| Flight time target | ~10 min | Hover-equivalent estimate |
| Target thrust-to-weight ratio | 2.5:1 | Good margin for a stable camera platform without over-building |
| Flight modes | Manual now; GPS/waypoints deferred | Mechanical design doesn't change for this — it's a flight-controller firmware decision made later (Phase 5) |

**Why 8in propellers:** This was simply the midpoint of the 7-10in range set as a Phase 1 requirement. It was a starting assumption to get the sizing math moving, not derived from a calculation. The final prop size may end up as 7, 9, or 10in depending on what motors/props are actually available and how the thrust numbers work out.

---

## 2. Weight Budget (Pass 1)

Built component-by-component using real reference parts found online, not arbitrary guesses.

| Component | Weight | Source / Reasoning |
|---|---|---|
| Frame (arms + center plate + mounts) | ~350g | Found a real 450mm carbon fiber frame at 280g. Scaled up for 3D-printed PLA, which is heavier than carbon fiber for the same strength. |
| Propellers (4x, 2-blade, 8x5) | ~40g (10g each) | HQProp 8x5 bi-blade, found at racedayquads.com |
| Camera (GoPro-style) | ~150g | Locked in from Phase 1 requirements |
| Motors (4x, A2212 920KV) | ~208g (52g each) | Found at hawks-work.com |
| ESCs (4x, 30A) | ~100g (25g each) | Found in an F450 kit (speedyfpv.com) — also confirms 30A is a sensible rating given hover current draw (~2.7A/motor, calculated below) |
| Flight controller (F4-class) | ~8g | Standalone F4 board, SoloGood (Amazon) |
| Wiring / connectors / misc | ~35g | Placeholder — minor line item, not worth detailed research at this stage |
| **Subtotal (everything except battery)** | **~891g** | |
| Battery (4S, ~2250mAh) | ~430g | Calculated via iteration — see Section 3 below |
| **Total Estimated AUW (All-Up Weight)** | **~1321g (1.32kg)** | |

---

## 3. Battery Sizing — Iterative Calculation

Battery sizing is circular: battery weight depends on power needed, which depends on total drone weight, which includes the battery. Solved by guessing, calculating, and converging.

**Step 1 — Starting guess:** 400g battery weight
Total estimated AUW = 891g (everything else) + 400g (guess) = **1291g**

**Step 2 — Required total thrust** (AUW × target T/W ratio)
1291g × 2.5 = **3228g total thrust** (all 4 motors) → **807g per motor**

**Step 3 — Hover thrust per motor** (AUW ÷ 4 motors — what it takes just to stay level, not max throttle)
1291g ÷ 4 = **323g per motor**

**Step 4 — Power draw per motor at hover**
Using ~8 grams of thrust per watt (a commonly-cited rule of thumb for a decent-quality motor/prop combo at ~50% hover throttle):
323g ÷ 8 g/W = **~40W per motor**

**Step 5 — Current draw per motor**
At 4S (14.8V nominal): 40W ÷ 14.8V = **~2.7A per motor**

**Step 6 — Total current draw (4 motors)**
2.7A × 4 = **~10.8A total**

**Step 7 — Required battery capacity** (for 10 min flight time, 80% usable capacity — LiPo batteries shouldn't be drained below ~20% regularly)
(10 min × 10.8A × 1000 ÷ 60) ÷ 0.8 = **~2250 mAh**

**Step 8 — Battery weight** (using ~190g per 1000mAh, a typical density for 4S LiPo packs)
2250 ÷ 1000 × 190 = **~428g**

**Step 9 — Convergence check**
Guessed 400g, calculated 428g — within ~30g, close enough to call converged.

**Result: Battery ≈ 430g, ~2250mAh, 4S**

---

## 4. Final Pass 1 Summary

| Result | Value | Used for |
|---|---|---|
| Total Estimated AUW | ~1.32 kg | Target weight to validate against real CAD mass later |
| Required thrust per motor | ~825g (at final AUW of 1321g × 2.5 ÷ 4) | Matching against motor+prop thrust tables in Phase 5 |
| Required battery capacity | ~2250 mAh | Selecting a real battery in Phase 5 |
| Battery cell count | 4S (~14.8V nominal) | Determines motor KV range and voltage compatibility |
| Target propeller size | 8 in | Starting point for motor/prop combo research |

*Note: this is a Pass 1 (hand-calculated) estimate. Pass 2 will replace the frame and electronics weights with real values pulled from SolidWorks mass properties once the frame is modeled (Phase 3), and re-validate thrust-to-weight with those real numbers before any purchasing happens (Phase 5).*

---

## 5. Concepts Reference

Plain-language explanations of every term used above, built up over the course of this project.

### AUW (All-Up Weight)
The total weight of the drone ready to fly: frame + motors + props + ESCs + flight controller + wiring + battery + camera. The single most important number in the whole design — almost everything else (thrust needed, motor size, flight time) is derived from it.

### Thrust-to-Weight Ratio (T/W)
Total thrust all 4 motors can produce, divided by AUW. A T/W of 2:1 means the drone could theoretically accelerate upward at 1g if motors were maxed out. You never fly at max throttle just to hover, so T/W is a safety/control margin, not a speed number.
- **Too low (under ~2:1):** sluggish, struggles in wind, motors run close to maxed out just hovering — no power in reserve.
- **Around 2:1 to 3:1:** good for a stable photography platform — enough margin without being twitchy. (Our target: 2.5:1)
- **High (4:1+):** very agile, fast, snappy — usually overkill for calm aerial photography, and costs weight/cost/efficiency you don't need.

### Thrust
The upward force a spinning propeller produces — it pushes air down, which (Newton's third law) pushes the drone up. Measured in grams or kg in the hobby world (easier to relate to than Newtons — "this motor produces 800g of thrust" means it can lift an 800g weight). For hover, total thrust from all 4 motors combined must at least equal the drone's total weight. Thrust above that at full throttle is your margin for climbing, accelerating, and handling wind.

### KV Rating (motors)
Motor KV = RPM produced per volt of input, with no load (no propeller) attached. Counterintuitively, **lower KV motors pair with bigger propellers, higher KV motors pair with smaller propellers**. Big propellers need more torque and less RPM to produce thrust efficiently; small propellers need to spin fast to produce the same thrust. For our 7-10in prop range, expect roughly 700-1000KV motors — refined once exact props are picked.

### Motor Size Codes (e.g. 2212, 2814)
The numbers describe the physical dimensions of the motor's stator (the stationary internal part with copper windings) — **first two digits = stator diameter (mm), last two digits = stator height (mm)**. So 2212 = 22mm diameter, 12mm height. 2814 = 28mm diameter, 14mm height. Bigger stator generally means more torque/power handling but more weight. 2212-class motors are common for lighter quads in the 8-10in prop range at lower KV; 2814-class motors are larger and used for heavier builds or bigger props.

### Propeller Pitch & Diameter
Props are labeled like "10x4.5" = 10 inch diameter, 4.5 inch pitch. Diameter affects how much air the prop moves (more diameter = more thrust, generally more efficient at hover). Pitch affects how "aggressive" the prop is per rotation. For a stable camera platform, larger diameter + moderate pitch favors efficiency (longer flight time) over speed.

### Propeller Blade Count (2-blade vs 3-blade)
2-blade props are more efficient (more thrust per watt), quieter, and give longer flight time, but slightly less total thrust at a given diameter. 3-blade props produce more thrust for the same diameter and feel snappier, but cost efficiency/flight time for the same battery — common in racing/freestyle where punch matters more than endurance. For a stable photography platform prioritizing flight time, 2-blade is the standard choice.

### Static Thrust
The thrust a motor+prop combo produces while hovering in place (not moving forward). This is what we size against, since hovering for photography is the main use case. Manufacturers publish static thrust tables for motor+prop+battery-voltage combinations — real published data will be used here once we get to Phase 5 (actual component selection).

### ESC (Electronic Speed Controller)
Sits electrically between the battery and each motor. Think of the battery as a power outlet and the motor as a lamp — the ESC is the dimmer switch. It takes a signal from the flight controller ("spin motor 1 at 70%") and converts it into the actual electrical power the motor needs to spin at that speed. Each motor needs its own ESC, or you can use a "4-in-1 ESC" (one board with 4 ESCs built in — saves weight/wiring, common on quadcopters). ESCs are rated by max current (must exceed what the motor actually draws, with margin) and voltage (must match the battery, e.g. "4S compatible"). The flight controller sends rapid signals to ESCs many times per second for fine stability adjustments.

### Flight Controller / F4 vs F7
F4 and F7 refer to the processor chip on the flight controller board (STM32F4 / STM32F7 series). The flight controller's job is to take in sensor data (gyroscope, accelerometer, sometimes barometer/compass) many times per second, run stabilization math, and send signals to the ESCs to keep the drone level and responsive. F4 is older but still very common and plenty powerful for a standard quad. F7 is newer/faster, useful for more demanding real-time processing (e.g. advanced autonomous features). For this build, F4 is sufficient.

### LiPo Cell Count (S rating)
LiPo batteries are made of cells in series, each ~3.7V nominal (4.2V full charge). "4S" = 4 cells in series = ~14.8V nominal. More cells = more voltage = generally more power available for the same current, allowing smaller/lighter wiring for a given power level. For a 7-10in prop quad, 4S or 6S is typical.

### Battery Capacity (mAh) & C-rating
Capacity (mAh) is how much energy the battery stores — bigger number = longer flight time but more weight. C-rating is how fast the battery can safely discharge that energy (higher C = can supply higher current bursts safely). mAh is sized directly from the power budget; C-rating just needs to be high enough that the battery doesn't bottleneck the motors at full throttle.

### Power Budget / Current Draw
Each motor at hover throttle draws a certain current (Amps). Total current × voltage = power (Watts). This determines how fast the battery drains, which determines flight time for a given battery capacity.

### Flight Time Estimate
Rough formula: Flight Time (minutes) = (Battery capacity in mAh × usable fraction, typically ~80%) ÷ (Total hover current draw in mA) × 60. This is an estimate — real flight time depends on wind, flying style, payload, and efficiency losses, but it's accurate enough to size a battery on paper.

### Vibration / Natural Frequency (preview for Phase 4)
Motors spinning at certain RPMs create vibration at that frequency. If that frequency matches the natural resonant frequency of the frame arms, vibration gets amplified (bad for camera footage, bad for structural fatigue over time). This will be checked in SolidWorks Frequency/Modal analysis once the frame is modeled.

### Why we size in this order
Weight estimate → thrust needed → motor/prop candidates → current draw of those motors → battery size for target flight time → battery weight feeds back into total weight. This is circular (battery weight affects the weight estimate that sized the battery!), so it's solved with two passes: a rough first pass (this document), then a refined second pass once real candidate parts and CAD mass properties are available.

---

## 6. Next Steps

1. Move into Phase 3 — model the frame in SolidWorks using the dimensions implied by this weight/thrust budget
2. Pull real mass properties from CAD and compare against the ~1.32kg AUW estimate above; iterate if they don't match
3. Re-run thrust-to-weight check with real CAD-derived weight before selecting final motor/prop combo
4. Move to Phase 4 (structural/vibration analysis) once frame geometry is locked
