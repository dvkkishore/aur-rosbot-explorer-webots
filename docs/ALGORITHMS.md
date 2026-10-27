# Algorithms and Design Decisions

This document describes code present in the selected Version A Fix12 controller. “Implemented” means source code exists; “enabled” means the audited constants select that path; neither term alone implies successful validation.

## Localization

The pose update combines differential-drive wheel odometry with an external heading source. In the audited Maze 1 run, GPS was unavailable, IMU zeroed yaw was the primary heading, compass XY was a fallback, and odometry was the final fallback.

`CorrelativeScanMatcher` samples LiDAR endpoints and searches a local pose window against an occupancy score table. The selected configuration sets `SCAN_MATCH_APPLY_TO_POSE = False`, so scan matching participates in diagnostics and map-update gating rather than serving as the authoritative pose correction. Run summaries include legacy “applied” counters; those counters must not be interpreted as nonzero physical pose corrections without inspecting the recorded correction values and configuration.

Assumptions include a differential-drive model, calibrated wheel radius/separation, sensor offsets defined in constants, and a primarily planar environment.

## Occupancy and semantic mapping

`OccupancyGrid` stores a sparse log-odds map at 0.05 m resolution. LiDAR rays mark traversed cells free and endpoints occupied using Bresenham traversal, bounded log odds, and occupancy/free thresholds.

The controller also maintains semantic and source-specific layers:

- LiDAR structural occupancy
- depth-derived chassis-height obstacles and structural slab/wall cells
- green hazard cells
- blue and yellow pillar observations and locked landmark positions
- transient confidence, hit-count, age, and source metadata used by cleanup and planning gates

RGB masks provide color candidates. Depth projects image observations into the ground/world frame, and LiDAR/map geometry is used to reject, snap, or promote candidates. Map-update gates reduce commits during rotation, suspected contact, pose uncertainty, or inconsistent sensor observations. These heuristics are environment-specific and have not been quantitatively calibrated for real hardware.

## Exploration

The controller derives known-free and unknown cells, detects frontier cells at their boundary, groups them into clusters, and scores candidate goals using path feasibility, distance, information gain, target-search context, and penalties. Commitment rules reduce rapid goal switching.

Unreachable handling includes temporary cluster/goal blacklists, relaxed gateway attempts, scans, backtracking, loop detection, remembered useful poses, and rescue paths. These mechanisms are implemented because dynamic mapping and conservative clearance tests can temporarily make frontiers unreachable. Their coexistence also contributes significant state-machine complexity.

Exploration ends through mission-state outcomes rather than a separately validated coverage threshold. Therefore the repository should not claim complete maze coverage.

## Global planning

The principal planner is grid A* (`_astar_cells`) with eight-connected exploration neighbors, geometric move cost, obstacle constraints, and configurable soft clearance risk. A cached distance field supplies clearance estimates. Candidate paths are audited for hard occupancy, green hazards, depth/chassis evidence, footprint clearance, and corridor geometry.

The planner can relax selected constraints during replanning and rescue, but preserves hard-block checks. Raw grid paths may receive light, safety-checked simplification before tracking.

ESDF optimization code exists but `ESDF_OPT_ENABLED = False` in Fix12. It must be described as experimental/disabled, not as the active global planner.

## Mission and semantic goals

With `MISSION_ENABLED = True`, the mission state machine seeks blue and then yellow pillars. It distinguishes searching, access exploration, route planning, navigation, final approach, visual verification, reached states, completion, and safe hold.

Target locks combine repeated semantic observations with geometric checks. The controller plans standoff or terminal viewpoints rather than treating the visual centroid as a drive-through point. Later Fix12 code adds access-frontier handoff, route-readiness caching, final-approach gates, side-orbit behavior, and target verification.

These states are implemented, but the available Fix12 run ended while exploring for blue. Historical mission completion by Fix5/Fix7 is revision-specific evidence only.

## Path preparation and tracking

The selected tracker mode is `rail`. It projects the robot relative to path segments, manages align/lock/rejoin modes, and produces linear/angular body commands subject to cross-track and heading conditions. Command ramping limits step changes.

Path-MPC code generates and evaluates local reference variants with swept-footprint checks. `PATH_MPC_ENABLED = True`, while `PATH_MPC_ALWAYS_KEEP_GLOBAL_FALLBACK = True`; the run logs show that candidates may be rejected and the raw/global reference retained. This is path shaping, not a claim of a fully authoritative model-predictive controller.

Pure-pursuit, trajectory, and simple waypoint helpers remain in the source for alternate/fallback paths. Their presence does not mean they are all simultaneously active.

## Local safety and recovery

The active live safety filter classifies front, diagonal, and side LiDAR clearance bands, slows or stops risky commands, adds steering bias, and watches for low-progress loops. Dynamic map hazards can invalidate a path and trigger replanning. Contact is inferred from clearances, commands, progress, and watchdog state; the robot has no source-verified ground-truth collision sensor in this controller.

Recovery sequences use bounded rotate/reverse/scan/replan actions with rear-clearance checks and failure accounting. Rail alignment and final-motion watchdogs detect deadlocks.

CBF and MPC-lite implementations are present but disabled (`CBF_FILTER_ENABLED = False`, `MPC_LITE_ENABLED = False`) in the selected configuration. They should not appear in feature claims as active safety controllers.

## Observability

Each run can produce:

- `run_meta.json`: controller version, selected constants, sensors, and geometry
- `run_samples.csv`: periodic pose, state, command, clearance, and path samples
- `run_events.jsonl`: transitions, failures, safety interventions, and diagnostics
- `run_paths.jsonl`: planned-path records
- `run_summary.json`: aggregate counters and terminal mission state

The logs are valuable for debugging, but many metrics are internal heuristics. A public result should include the exact world, controller hash, configuration, stopping rule, and raw-to-summary derivation.
