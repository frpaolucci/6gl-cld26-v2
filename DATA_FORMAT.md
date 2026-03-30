
# DATA_FORMAT.md
## Data Format Specification

This document describes the format of all files included in the Cross-Layer 6G Dataset for Drone-Assisted Edge Applications.

### Time Reference
All timestamps use Unix time.
- `unix_time_us`: Unix timestamp in microseconds
- This field is used to synchronize all data sources.

---

## Mission Log
File: `6GLeader_mission_[date].log`

Contains mission events:
- `MISSION_START`
- `WAYPOINT_SET`
- `WAYPOINT_REACHED`
- `MISSION_END`

Fields:
- `unix_time_us`: timestamp (µs)
- `takeoff_x`, `takeoff_y`: takeoff reference position (m)
- `left_offset`: mission-specific lateral offset parameter
- `idx`: waypoint index
- `x`, `y`, `z`: waypoint target position (m)
- `yaw`: target yaw angle (deg)
- `success`: mission success flag

The mission log provides the high-level mission timeline and the sequence of commanded waypoints.

---
## Drone Setpoint Data
File: `[]setpont.csv`

This file contains commanded references sent to the UAV controller.

Main columns:
- `timestamp`: timestamp
- `x`, `y`, `z`: commanded position references (m)
- `vx`, `vy`, `vz`: commanded velocity references (m/s)
- `acceleration_1`, `acceleration_2`, `acceleration_3`: commanded acceleration references (m/s²)
- `thrust_1`, `thrust_2`, `thrust_3`: thrust-related control signals
- `yaw`: commanded yaw angle (deg)
- `yawspeed`: commanded yaw rate (deg/s)

This file can be used to compare reference commands with actual estimated motion and network conditions.

---
## Drone Action Data
File: `drone_action.csv`

This file contains discrete control actions and mode transitions.

Main columns:
- `timestamp`: timestamp
- `action`: action code
- `source`: action source
- `mode`: control or flight mode
- `action_decoded`: human-readable action description
- `source_decoded`: human-readable source description

This file is useful for aligning behavioral events and control-state changes with network and application measurements.

---
## Drone Pose Data
File: `drone_pose.csv`

This file contains estimated UAV state variables and uncertainty indicators.

Main columns:
- `timestamp`: timestamp
- `timestamp_sample`: sample timestamp
- `ref_timestamp`: reference timestamp
- `ref_lat`, `ref_lon`, `ref_alt`: reference geographic coordinates / altitude
- `x`, `y`, `z`: estimated position (m)
- `vx`, `vy`, `vz`: estimated velocity (m/s)
- `z_deriv`: vertical speed (m/s)
- `ax`, `ay`, `az`: estimated acceleration (m/s²)
- `heading`: estimated heading (deg)
- `heading_var`: heading variance
- `unaided_heading`: heading without aiding
- `delta_heading`: heading correction/error
- `tilt_var`: tilt variance
- `dist_bottom`: estimated distance from ground (m)
- `eph`, `epv`: horizontal and vertical position uncertainty
- `evh`, `evv`: horizontal and vertical velocity uncertainty
- `vxy_max`, `vz_max`: maximum allowed horizontal and vertical speed
- `hagl_min`, `hagl_max`: minimum and maximum height above ground
- `xy_valid`, `z_valid`, `v_xy_valid`, `v_z_valid`: validity flags
- `xy_reset_counter`, `z_reset_counter`, `vxy_reset_counter`, `vz_reset_counter`, `heading_reset_counter`: estimator reset counters
- `heading_good_for_control`: heading suitable for control
- `xy_global`, `z_global`: global reference availability flags
- `dist_bottom_valid`: ground-distance validity flag
- `dist_bottom_sensor_bitfield`: sensor status bitfield
- `dead_reckoning`: dead reckoning mode flag

Derived metrics often used in analysis:
- speed = sqrt(vx² + vy² + vz²)
- acceleration magnitude = sqrt(ax² + ay² + az²)
- distance from base station = sqrt((x-x_bs)² + (y-y_bs)² + (z-z_bs)²)

---

## Drone Attitude Data
File: `drone_attitude.csv`

This file contains estimated UAV attitude in quaternion form.

Main columns:
- `timestamp`: timestamp
- `timestamp_sample`: sample timestamp
- `q_1`, `q_2`, `q_3`, `q_4`: quaternion components
- `delta_q_reset_1`, `delta_q_reset_2`, `delta_q_reset_3`, `delta_q_reset_4`: quaternion reset values
- `quat_reset_counter`: quaternion reset counter

This file can be used to reconstruct orientation and derive roll, pitch, and yaw if needed.

---

## Drone Battery Data
File: `drone_battery.csv`

This file contains battery and power telemetry.

Main columns:
- `timestamp`: timestamp
- `voltage_v`, `voltage_filtered_v`: battery voltage (V)
- `current_a`, `current_filtered_a`, `current_average_a`: battery current (A)
- `discharged_mah`: discharged capacity (mAh)
- `remaining`: remaining battery level
- `scale`: battery scaling factor
- `time_remaining_s`: estimated remaining time (s)
- `temperature`: battery temperature
- `voltage_cell_v_1..14`: individual cell voltages (V)
- `max_cell_voltage_delta`: maximum cell voltage imbalance
- `average_power`: average power
- `available_energy`, `full_charge_capacity_wh`, `remaining_capacity_wh`: energy and capacity indicators
- `design_capacity`, `nominal_voltage`, `capacity`, `cycle_count`: battery characteristics
- `state_of_health`, `max_error`, `faults`, `custom_faults`, `interface_error`: health and fault indicators
- `connected`, `cell_count`, `source`, `priority`, `id`, `is_powering_off`, `is_required`, `warning`, `mode`: battery status fields

This file supports analysis of energy consumption and the relationship between mission dynamics, traffic load, and power usage.

---

## VICON and Drone Data
File: `drone/*`
Sampling rate: 60 Hz
The drone telemetry dataset includes position, velocity, acceleration, orientation, battery status, and control actions. The posDATA sheet contains the estimated drone state and is the main source for mobility analysis. The setDATA sheet contains control setpoints, attitudeDATA contains drone orientation in quaternion form, batteryData contains battery and power consumption information, and actionReq logs drone control actions and mode changes.

## RAN Data
File: `test[x]_ran_ue_ds.csv`  
Source: Accelleran dRAX Kafka telemetry bus  
Sampling rate: 1 Hz  
Timestamp format: Unix time in microseconds (`Timestamp`)

The RAN dataset aggregates measurements collected from multiple dRAX Kafka topics associated with UE, DU, and CU metrics. Each row represents a RAN measurement sample for a specific UE at a given time.

Kafka topics used:
- accelleran.drax.5g.du_metrics.ue_mac
- accelleran.drax.5g.ric.processed.pm_counters
- accelleran.drax.5g.ric.raw.ue_measurements

Reference documentation:
https://accelleran.github.io/drax-xapp-dev-guide/v6.0/

## Columns Description (UE RAN Metrics)

| Column | Unit | Description |
|--------|------|-------------|
| Timestamp | µs | Unix timestamp |
| Pci | - | Physical Cell ID |
| Rnti | - | Radio Network Temporary Identifier |
| Cqi | index | Channel Quality Indicator |
| RankIndicator | - | MIMO rank indicator |
| DlMcs | index | Downlink Modulation and Coding Scheme |
| DlPrbsUsed | PRB | Downlink Physical Resource Blocks used |
| DlDataRate | bps | Downlink MAC layer data rate |
| DlDataHARQAcks | count | Downlink HARQ ACKs |
| DlDataHARQNAcks | count | Downlink HARQ NACKs |
| DlDataHARQRetransmissions | count | Downlink retransmissions |
| DlBufferStatus | bytes | Downlink buffer size |
| AvgPuschSnr | dB | Average uplink PUSCH SNR |
| AvgPucchSnr | dB | Average uplink PUCCH SNR |
| UlMcs | index | Uplink Modulation and Coding Scheme |
| UlPrbsUsed | PRB | Uplink PRBs used |
| UlDataRate | bps | Uplink MAC layer data rate |
| UlDataHARQAcks | count | Uplink HARQ ACKs |
| UlDataHARQNAcks | count | Uplink HARQ NACKs |
| UlDataHARQRetransmissions | count | Uplink retransmissions |
| UlBufferStatusReport | bytes | Uplink buffer size |
| UlPuschRsrp | dBm | Uplink RSRP |
| TimingAdvance | - | Timing advance |
| PowerHeadroomReport | dB | UE power headroom |
| UL_GTP_THP | bps | Uplink GTP throughput |
| DL_GTP_THP | bps | Downlink GTP throughput |
| SsbRsrqResult | dB | SSB RSRQ |
| SsbRsrpResult | dBm | SSB RSRP |
| SsbSinrResult | dB | SSB SINR |

## Radio Link Quality Metrics

These metrics describe the radio link quality between the UE and the base station.

| Metric | Description |
|-------|-------------|
| Cqi | Channel Quality Indicator |
| SsbSinrResult | Signal-to-Interference-plus-Noise Ratio (SINR) |
| SsbRsrpResult | Reference Signal Received Power (RSRP) |
| SsbRsrqResult | Reference Signal Received Quality (RSRQ) |
| AvgPuschSnr | Uplink data channel SNR |
| AvgPucchSnr | Uplink control channel SNR |

These metrics are indicators of radio channel conditions and directly affect throughput, reliability, and modulation selection.

## Resource Allocation Metrics

These metrics indicate how many radio resources are allocated to the UE and the selected transmission configuration.

| Metric | Description |
|-------|-------------|
| DlPrbsUsed | Downlink allocated Physical Resource Blocks |
| UlPrbsUsed | Uplink allocated Physical Resource Blocks |
| DlMcs | Downlink Modulation and Coding Scheme |
| UlMcs | Uplink Modulation and Coding Scheme |
| RankIndicator | Number of MIMO spatial streams |

## Throughput Metrics

| Metric | Description |
|-------|-------------|
| DlDataRate | Downlink MAC layer throughput |
| UlDataRate | Uplink MAC layer throughput |
| DL_GTP_THP | Downlink GTP throughput |
| UL_GTP_THP | Uplink GTP throughput |

Note:
MAC layer throughput and GTP throughput are different.
MAC throughput represents the physical/MAC layer transmission rate, while GTP throughput represents the user-plane throughput and can be used as a proxy for application-level throughput.

## Reliability Metrics

| Metric | Description |
|-------|-------------|
| DlDataHARQAcks | Successful downlink transmissions |
| DlDataHARQNAcks | Failed downlink transmissions |
| DlDataHARQRetransmissions | Downlink retransmissions |
| UlDataHARQAcks | Successful uplink transmissions |
| UlDataHARQNAcks | Failed uplink transmissions |
| UlDataHARQRetransmissions | Uplink retransmissions |

Derived metric:

Block Error Rate (BLER):

BLER = HARQ_NACKs / (HARQ_ACKs + HARQ_NACKs)

## Buffer and Congestion Metrics

| Metric | Description |
|-------|-------------|
| DlBufferStatus | Downlink buffer occupancy |
| UlBufferStatusReport | Uplink buffer occupancy |

These metrics indicate buffer backlog and potential congestion conditions in the network.


## Power and Timing Metrics

| Metric | Description |
|-------|-------------|
| TimingAdvance | Timing correction related to UE distance from the base station |
| PowerHeadroomReport | Remaining UE transmission power margin |

Timing Advance is proportional to the distance between the UE and the base station and can be used as an indirect indicator of UE position.

## Summary

The RAN dataset enables analysis of:
- Radio link quality
- Resource allocation
- Throughput performance
- Transmission reliability
- Network congestion
- UE transmission power conditions
- Distance-related timing effects

These metrics can be correlated with UAV position, trajectory, and application performance to enable cross-layer analysis and network-based inference in 5G and 6G systems.


## Synthetic Traffic (iperf3)
File: `iperf.csv`

Columns:
- unix_time_us
- protocol (TCP/UDP)
- bitrate (bps)
- jitter (ms)
- packet_loss (%)
- retransmissions
- rtt (ms)
- cwnd
- interval (s)

---

## Video Streaming Time Series
File: `video_stream.csv`

Columns:
- unix_time_us
- bitrate (bps)
- throughput (bps)
- packet_loss (%)
- jitter (ms)
- latency (ms)
- fps
- dropped_frames
- buffer_level (ms)

---

## Video Streaming Summary
File: `video_summary.json`

Fields:
- start_time
- end_time
- duration
- avg_bitrate_bps
- avg_bitrate_mbps
- avg_fps
- packet_loss_rate
- frames_received
- bytes_received
- packets_lost

## Cross-Layer Mapping
The dataset enables cross-layer analysis among:
- mission intent and waypoints (`mission_log`)
- reference control signals (`drone_setpoint`)
- UAV state estimation (`drone_pose`, `drone_attitude`)
- energy state (`drone_battery`)
- RAN performance (`ran.csv`)
- application QoS/QoE (`iperf.csv`, `video_stream.csv`, `video_summary.json`)