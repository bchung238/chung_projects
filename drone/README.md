# Camera Drone Build — Engineering Project

A from-scratch quadcopter design and build, covering CAD modeling, structural/dynamics analysis, component selection, manufacturing, and flight testing. Built as a personal engineering project to develop and demonstrate skills across the full product development cycle — not just flying a drone, but engineering one.

**Background:** Coursework-heavy engineering background, limited hands-on build experience going in. This project is being driven independently, with research, documentation, and design decisions made and owned at each step.

**Tools:** SolidWorks (CAD + FEA + Motion analysis), GitHub for documentation/version control.

---

## Project Requirements (locked from Phase 1)

| Parameter | Value |
|---|---|
| Configuration | Quadcopter |
| Propeller size | 7-10 in |
| Frame size | ~350-450mm wheelbase |
| Camera payload | ~150g (GoPro-style action camera, fixed mount) |
| Target flight time | ~10 min |
| Target thrust-to-weight ratio | 2.5:1 |
| Flight modes | Manual first; GPS/waypoints deferred to firmware config later |

---

## Roadmap / Milestones

**Short Term**

1. **Complete conceptual sizing (Phase 2)** — ✅ Done. See [`Phase2_Sizing_Log.md`](./Phase2_Sizing_Log.md). Weight budget, thrust requirement, and battery sizing all converged (~1.32kg AUW estimate).
2. **Select candidate components** — identify specific motors, ESCs, flight controller, and battery that meet the Phase 2 sizing numbers (research only, no purchasing yet).
3. **CAD the frame (Phase 3)** — model the full frame in SolidWorks: arms, center plate, motor mounts, camera mount, electronics bay.
4. **Validate mass properties** — pull real weight from SolidWorks and compare against the Phase 2 estimate; iterate if they don't match.
5. **Structural analysis (Phase 4)** — FEA on the arms under thrust load, stress concentrations at motor mounts, modal/frequency analysis to check for vibration resonance.

**Long Term**

6. **Finalize component selection and purchase** — only after CAD and analysis confirm the sizing numbers hold up.
7. **3D print and assemble the frame** — document the build process.
8. **Bench test** — motors spinning without props, then static thrust test with props, before any flying.
9. **First flight** — basic hover and manual control, no camera yet.
10. **Camera integration** — mount the GoPro, tune for stable footage, assess vibration impact on video quality.
11. **Add autonomous features** — GPS hold, return to home, waypoint missions via flight controller firmware.
12. **Engineering report** — document the full process from requirements through test flights.

---

## Current Status

**Working on:** Milestone 2 — candidate component selection, heading into Phase 3 (CAD).

---

## Repo Contents

| File | Description |
|---|---|
| `Phase2_Sizing_Log.md` | Full conceptual sizing work: weight budget, thrust-to-weight calculation, iterative battery sizing, and a concepts glossary covering every term used (AUW, KV, ESC, thrust, etc.) |
| `README.md` | This file — project overview and roadmap |

*(Will be updated as later phases are completed — CAD files, FEA results, component lists, and the final engineering report will be added here as they're produced.)*
