
# METADATA.md

## Metadata Description

Each mission folder in the dataset contains a `metadata.json` file describing the experimental configuration, active test conditions, environment, and associated data files. Only tests 5 and 9 are not provided with metadata and are left unclassified. The metadata file provides the contextual information necessary to correctly interpret the measurements and to reproduce the experimental conditions.

The metadata structure is consistent across all missions.

---

## Metadata Structure Overview

The `metadata.json` file contains the following main sections:

- mission_id
- test_name
- test_case
- active_conditions
- traffic_profile
- trajectory_profile
- radio_profile
- mobility_profile
- propagation_profile
- experiment_timing
- environment
- files
- notes
- source

Each section is described below.

---

## Mission Identification

| Field | Description |
|------|-------------|
| mission_id | Unique mission identifier |
| test_name | Name of the test |
| test_case.id | Test case family ID |
| test_case.title | Test case category |
| test_case.option | Specific option within the test case |
| test_case.goal | Experimental objective |

The test case describes the experimental scenario category, such as application traffic tests, trajectory variation tests, or transmission power tests.

---

## Active Conditions

This section provides a human-readable description of the experimental conditions active during the mission.

| Field | Description |
|------|-------------|
| active_conditions.traffic | Active traffic conditions |
| active_conditions.trajectory | Active trajectory conditions |
| active_conditions.tx_power | Transmission power configuration |
| active_conditions.mobility | Mobility pattern |
| active_conditions.los | Propagation conditions |

This section is intended to provide a quick description of the scenario without inspecting all detailed fields.

---

## Traffic Profile

This section describes the application traffic generated during the mission.

### Synthetic Traffic

| Field | Description |
|------|-------------|
| synthetic.enabled | Indicates if synthetic traffic is present |
| synthetic.tool | Traffic generation tool (e.g., iperf3) |
| synthetic.protocol | TCP or UDP |
| synthetic.direction | UL, DL, or bidirectional |
| synthetic.target_rate | Target rate (e.g., 5M, 20M, 50M, unlimited) |

### Video Traffic

| Field | Description |
|------|-------------|
| video.enabled | Indicates if video traffic is present |
| video.direction | UL or DL |
| video.quality | Video quality level (e.g., low, high) |

| Field | Description |
|------|-------------|
| mixed | True if both synthetic and video traffic are active |

---

## Trajectory Profile

This section describes the UAV trajectory configuration.

| Field | Description |
|------|-------------|
| nominal_path | Nominal mission trajectory defined by waypoints |
| right_offset_cm | Right lateral offset from nominal trajectory |
| left_offset_cm | Left lateral offset from nominal trajectory |
| left_offset_multiplier | Multiplier applied to left offset |

These parameters describe trajectory variation experiments.

---

## Radio Profile

This section describes the radio configuration used during the mission.

| Field | Description |
|------|-------------|
| tx_power_label | Human-readable label for transmission power configuration |
| p0_nominal_with_grant | Uplink power control parameter (dBm) |

The p0_nominal_with_grant parameter controls the uplink transmission power and is used to emulate different uplink coverage conditions.

---

## Mobility Profile

This section describes UAV mobility behavior.

| Field | Description |
|------|-------------|
| stop_pattern | Description of stop-and-go pattern |
| hover_events_present | Indicates if hover phases are present |

---

## Propagation Profile

This section describes propagation conditions.

| Field | Description |
|------|-------------|
| condition | LOS or NLOS |
| obstacle | Obstacle identifier if present |

---

## Experiment Timing

| Field | Description |
|------|-------------|
| duration_s | Mission duration |
| duration_range_s | Typical mission duration range |
| time_reference | Timestamp reference (Unix time) |
| time_sync | Time synchronization method |

All dataset files use the same time reference to enable cross-layer synchronization.

---

## Environment

This section describes the experimental environment and measurement systems.

| Field | Description |
|------|-------------|
| setup | Indoor or outdoor |
| drone_count | Number of drones |
| positioning_system | Position tracking system (e.g., VICON) |
| vicon_rate_hz | VICON sampling rate |
| ran_rate_hz | RAN sampling rate |
| app_rate_hz | Application sampling rate |
| base_station_position | Base station coordinates |

---

## Files

This section lists all files associated with the mission.

| Field | Description |
|------|-------------|
| mission_log | Mission log with waypoint and mission events |
| drone_setpoint | Commanded position and yaw references |
| drone_action | Action and mode events |
| drone_pose | Estimated UAV position, velocity, and acceleration |
| drone_attitude | UAV orientation (quaternion) |
| drone_battery | Battery telemetry |
| ran | RAN measurements |
| iperf | Synthetic traffic measurements (if present) |
| video_timeseries | Video streaming time series (if present) |
| video_summary | Video session summary (if present) |

This section defines the mapping between metadata and dataset files.

---

## Notes

| Field | Description |
|------|-------------|
| notes | Additional mission-specific notes |

---

## Source

This section describes how the metadata was generated.

| Field | Description |
|------|-------------|
| generated_from | Source file used to generate metadata |
| sheet | Excel sheet name |
| row_index | Row index in the Excel file |

This section ensures traceability between the dataset and the experimental planning file.

---

## Summary

The metadata file provides:
- Experiment configuration
- Traffic configuration
- Radio configuration
- UAV trajectory configuration
- Mobility and propagation conditions
- Environment description
- File mapping
- Metadata generation traceability

The metadata enables reproducibility of experiments and supports cross-layer analysis across UAV mobility, radio access network performance, and application-level performance.