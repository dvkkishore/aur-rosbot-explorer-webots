# Development Status

## Current publication source

The current Version A publication candidate is the Maze 1 working controller:

| Field | Value |
|---|---|
| Path | `Maze1/controllers/my_controller/my_controller.py` |
| Identifier | `Task22N_Fix12_global_frontier_execution_handoff` |
| Lines at audit | 31,139 |
| SHA-256 at audit | `79bcf4f34876383a0145a4d4975eb582f38a296c4f425af764c9da24b56e8aa7` |
| Evaluated world | Private academic Maze 1 world; not included in the public snapshot |
| Git relationship | Uncommitted successor to HEAD `e5958a1` (`Task22M`, 27,216 lines) |
| Status | Under Refinement |

This selection is based on the active development-world linkage and controller lineage, not filename recency alone. The private Maze 1 world declares `controller "my_controller"`, and the Fix12 file is the latest active Version A continuation with a recorded run. It is selected for transparent publication of current work, not because it is the most successful revision.

The public copy must retain the same bytes and hash at export time. Any later source change requires updating this table and treating the result as a new audited snapshot.

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

## Related Version A snapshots

| Source | Identifier/evidence | Relationship | Publication interpretation |
|---|---|---|---|
| `my_controller (copy).py` | Task18 Fix2; 12,332 lines; SHA-256 `1c27692a…` | Frozen unchanged since commit `6513f39`; not selected by the world | Backup/reference only, not current. |
| Commit `ffa4a11` | Task18 Fix7; 14,365 lines | Commit labelled `First Success`; corresponding Maze 3 run reached both targets | Historical successful revision, not Fix12 validation. |
| Commit `e5958a1` | Task22M; 27,216 lines | Current repository HEAD and predecessor of working Fix12 | Reproducible Git checkpoint, but not the newest working file. |
| Maze 1 Task22N/Fix5 run | `run_20260710_182926` | Later uncommitted development run; both targets recorded reached | Historical experiment only. |
| Maze 1 Fix12 run | `run_20260710_220612` | Run produced by the selected identifier | Current evidence; neither target recorded reached. |

Root-level `progressed.py` is Task16W (10,291 lines), and `old_code.py`/`my_controller_stable_backup.py` represent earlier mapping-era code. They are development history, not the current publication source.

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

## Version B boundary

The `teammate_controller` directories belong to the separate submitted team implementation. They are not dependencies of Fix12 and are excluded from the Version A public repository. The separate `clean_slam` controller family is also outside Version A publication scope.

## Redistribution status

The controller is the owner's independent Version A source. The evaluated maze entered the archive with the academic project, contains no license notice, and is discussed by course material that provides no redistribution grant. The referenced Cyberbotics R2025a PROTOs have their own upstream terms, but those terms do not establish ownership of the course maze layout. The present public snapshot is consequently code-only.

This restriction also applies to screenshots or maps that visibly reproduce the unapproved maze layout until permission is confirmed.
