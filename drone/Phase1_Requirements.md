# Drone Build Project — Phase 1: Requirements & Architecture

**Author:** Brian Chung
**Status:** Complete
**Goal of this phase:** Define, on paper, what this drone needs to be before any sizing math, CAD, or purchasing begins. Every number here is a starting assumption to anchor the rest of the design process — not a final, immovable spec.

---

## Project Context

This is a from-scratch camera-equipped quadcopter build, taken on as a resume project to demonstrate engineering depth across CAD modeling, structural/dynamics analysis, component selection, manufacturing, and flight testing. The build is being driven independently, with a mostly coursework-based engineering background and limited hands-on build experience going in. Access to SolidWorks is available via a school license.

**Primary goal for this drone:** the engineering process matters more than flight performance. Flying isn't the main point — the design, analysis, and documentation are. This shaped several requirement decisions below: where there was a tradeoff between "build something that flies really well" and "build something with rich, demonstrable engineering content," the latter was favored.

---

## Requirements and the Reasoning Behind Them

### Configuration: Quadcopter

Considered: quad vs. hex vs. other configurations.

A quadcopter (4 motors) was chosen as the standard, simplest configuration for a first build. It has well-documented dynamics, abundant reference data and community knowledge, and is still complex enough to provide meaningful engineering content (structural analysis, dynamics, control allocation) without the added complexity of a hex/octocopter, which would be overkill for a first build.

### Propeller Size: 7-10 inch (8 inch used as starting estimate)

This range was set as an initial sizing requirement based on landing in a "medium camera/photography platform" size class — bigger than a small racing-style 5" build, smaller than a heavy-lift 10"+ platform. 8 inch was picked as the working number for Phase 2 sizing math simply because it's the midpoint of that range — a reasonable place to start, not a calculated optimum. The final propeller size may shift to 7, 9, or 10 inches once real motor/prop combinations are evaluated in later phases.

### Frame Size: ~350-450mm wheelbase

Derived directly from the propeller size choice — frame wheelbase needs to accommodate 7-10" propellers with adequate clearance between adjacent props and the frame body. 350-450mm is the standard wheelbase range for this propeller class.

### Camera Payload: ~150g (GoPro-style action camera)

An action-camera-class payload (GoPro or similar) was chosen over a larger camera/gimbal setup. Reasoning: a fixed-mount action camera keeps the mechanical design simpler (no gimbal mechanism to design, analyze, and control) while still being a real, demonstrable payload integration — relevant for the "camera-equipped" part of the project goal. A larger gimbal camera setup (250g+) was considered but would add a significant mechanical subsystem (2-3 axis stabilization) that wasn't necessary to demonstrate the core engineering skills this project targets.

### Flight Time Target: ~10 minutes

Three options were weighed:
- **~5 min:** achievable with a small, light battery, but leaves little margin for iterative test flights, tuning, and bench testing — batteries would need constant swapping during development.
- **~10 min (chosen):** long enough for meaningful test flights and iteration, and roughly in line with what's realistic for a mid-size camera drone in this class.
- **~15-20 min:** appealing on paper, but battery weight grows faster than the benefit gained — a classic aircraft sizing problem where a bigger battery adds weight, which demands more thrust, which demands a bigger battery, with diminishing returns. Not worth the added weight/cost for a first build.

10 minutes was selected as the balanced target.

### Target Thrust-to-Weight Ratio: 2.5:1

T/W represents the margin between what the motors can produce at full throttle versus what's needed just to hover (T/W = 1:1 is bare minimum hover capability, with zero reserve power).
- Below ~2:1 risks a sluggish drone with little margin for wind or control authority.
- Above ~4:1 is typically unnecessary for calm aerial photography and wastes weight/cost on motors/battery capacity that won't be used.
- **2.5:1** was chosen as a middle ground — enough margin for a stable, controllable camera platform without over-building the propulsion system.

### Flight Modes: Manual first; GPS/autonomy deferred

Initially, full autonomy (GPS hold, waypoint missions) was of interest alongside manual flight. On review, it became clear that GPS hold and autonomous waypoint missions are primarily **flight-controller firmware features** (e.g. configured via ArduPilot or PX4), not mechanical design requirements. This means the decision doesn't block or change anything in the mechanical design phases (CAD, structural analysis) — it can be deferred to Phase 5, when the flight controller and firmware are actually selected and configured. Mechanical design proceeds now; flight mode capability is added later as a configuration choice.

---

## Locked Requirements Summary

| Parameter | Value |
|---|---|
| Configuration | Quadcopter |
| Propeller size target | 8 in (working estimate within 7-10 in range) |
| Frame size (est.) | ~350-450mm wheelbase |
| Camera payload | ~150g, GoPro-style, fixed mount, no gimbal |
| Flight time target | ~10 min |
| Target thrust-to-weight ratio | 2.5:1 |
| Flight modes | Manual now; GPS/waypoints deferred to firmware config in Phase 5 |
| Design priority | Engineering depth over outright flight performance |

---

## Next Phase

These requirements feed directly into **Phase 2: Conceptual Sizing** — see [`Phase2_Sizing_Log.md`](./Phase2_Sizing_Log.md), where they're used to build a component-by-component weight budget, calculate required thrust, and size the battery.
