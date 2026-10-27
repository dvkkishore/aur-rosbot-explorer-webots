# Development Status

## Current controller revision

The published controller originated from the Maze 1 development implementation:

| Field | Value |
|---|---|
| Path | `Maze1/controllers/my_controller/my_controller.py` |
| Identifier | `Task22N_Fix12_global_frontier_execution_handoff` |
| Lines at audit | 31,139 |
| SHA-256 at audit | `79bcf4f34876383a0145a4d4975eb582f38a296c4f425af764c9da24b56e8aa7` |
| Evaluated world | Private academic Maze 1 world; not included in the public snapshot |
| Development history | Originally an uncommitted successor to private archive commit `e5958a1` (`Task22M`, 27,216 lines); the public repository has a separate initial commit |
| Status | Under Refinement |

The controller was selected from the independently developed Maze 1 lineage because the private development world uses `controller "my_controller"` and the Fix12 revision has recorded run evidence. This is the current public source, not a claim that Fix12 was the most successful historical revision.

The recorded hash identifies the source at initial publication. If the controller changes, update its revision and evidence records rather than applying historical Fix12 observations to new code.

## Audited development environment

| Component | Observed version/configuration |
|---|---|
| Webots format | R2025a |
| Python | 3.10.12 |
| NumPy | 1.21.5 |
| OpenCV | 4.5.4 |
| Primary heading in recorded run | IMU zeroed yaw, with compass/odometry fallbacks |
| GPS in recorded run | Unavailable |

These are observed development versions, not a formally tested compatibility range.

## Historical development snapshots

| Source | Identifier/evidence | Relationship | Publication interpretation |
|---|---|---|---|
| `my_controller (copy).py` | Task18 Fix2; 12,332 lines; SHA-256 `1c27692a…` | Frozen unchanged since commit `6513f39`; not selected by the world | Backup/reference only, not current. |
| Commit `ffa4a11` | Task18 Fix7; 14,365 lines | Commit labelled `First Success`; corresponding Maze 3 run reached both targets | Historical successful revision, not Fix12 validation. |
| Private archive commit `e5958a1` | Task22M; 27,216 lines | Predecessor of Fix12 in the private development archive | Historical Git checkpoint, not the public repository HEAD. |
| Maze 1 Task22N/Fix5 run | `run_20260710_182926` | Later uncommitted development run; both targets recorded reached | Historical experiment only. |
| Maze 1 Fix12 run | `run_20260710_220612` | Run produced by the selected identifier | Current evidence; neither target recorded reached. |

The private development archive also contains `progressed.py` (Task16W), `old_code.py`, and `my_controller_stable_backup.py`. These are historical materials, not files supplied in this public repository.

## Current evidence

The Fix12 run summary reports 565.216 simulated seconds, 85.1004 m of estimated travel, eight replans, 23 rail deadlocks, and terminal state `MISSION_EXPLORE_FOR_BLUE`. It records no reached target. It also contains `physical_contact_suspected` events even though the final aggregate collision counters are zero, illustrating why those counters must be interpreted as heuristic instrumentation rather than ground truth.

The run demonstrates that the selected source initializes, maps, plans, moves, replans, logs, and continues exploration in Maze 1. It does not demonstrate mission completion or cross-maze reliability.

The documentation environment verified Python syntax but could not launch Webots' GUI because no display server was available. A clean-machine graphical replay with an authorized world remains required before any reproducibility claim.

## Known engineering issues

### Critical for a reproducible release

- Commit or otherwise freeze the exact selected Fix12 source and record its hash.
- Obtain written permission to redistribute the evaluated `Maze1.wbt`, or create and validate an independently authored replacement world.
- Replay the controller/world pair in Webots R2025a and verify controller/device resolution.
- Package a small, immutable Fix12 evidence set; do not publish multi-gigabyte raw logs by default.

### Important for engineering reliability

- Diagnose Fix12's failure to progress from `MISSION_EXPLORE_FOR_BLUE` and its repeated rail deadlocks.
- Establish deterministic scenario reset, stopping rules, and repeat counts.
- Add characterization tests for coordinate transforms, occupancy updates, frontier grouping, A*, clearance audits, and mission transitions.
- Reconcile or explicitly retire the divergent Maze 2–5 controller copies; do not imply one source was validated in all worlds.
- Make instrumentation internally consistent, especially contact events versus aggregate collision counters and scan-match naming.

### Maintainability improvements

- Reduce hidden coupling through typed state snapshots and narrow subsystem interfaces.
- Move logging/rendering and pure planning utilities out of `MappingController` after characterization tests exist.
- Document tuning provenance and group constants by the subsystem that owns them.
- Add a dependency manifest after verifying a clean environment.

### Optional research extensions

- Quantitative localization drift evaluation against simulator ground truth.
- Ablation studies for depth-derived obstacle layers, path shaping, and recovery policies.
- Coverage and map-quality metrics with fixed reference worlds.
- Hardware transfer only after sensor models, timing, safety, and calibration assumptions are redesigned and validated.

These extensions are not requirements for publishing the current work-in-progress repository.

## Separate academic implementation

The academic teammates' submitted implementation and the separate `clean_slam` working copy are not included in this repository and are not dependencies of the published controller. Their source and provenance remain outside this project's public scope.

## Redistribution status

The controller in this repository was independently engineered by its owner. The evaluated maze originated in the academic project and has no established redistribution permission; the referenced Cyberbotics R2025a PROTOs have separate upstream terms that do not authorize distribution of the course maze. This repository consequently distributes code and documentation only.

This restriction also applies to screenshots or maps that visibly reproduce the unapproved maze layout until permission is confirmed.
