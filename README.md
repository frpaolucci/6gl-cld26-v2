
# 6G-LEADER Cross-Layer Dataset 2026 version 2(6GL-CLD26_v2)

## Overview

This dataset contains cross-layer measurements collected during UAV flight missions connected to a 5G O-RAN network. The dataset includes synchronized data from UAV telemetry, Radio Access Network (RAN) measurements, and application-level traffic, enabling cross-layer analysis across mobility, radio, and application domains.

The dataset was collected in a controlled indoor environment using a single UAV connected to a disaggregated O-RAN 5G network operating in band n78 with 50 MHz bandwidth. The UAV executed multiple flight missions under different experimental conditions, including varying traffic loads, uplink power control settings, trajectories, and mobility patterns.

The dataset is designed to support research on cross-layer performance analysis, Quality of Service (QoS), Quality of Experience (QoE), mobility-aware networking, uplink power control, network-based localization, and integrated communication and sensing for 5G and 6G systems.

---

## Dataset Content

Each mission includes the following data sources:

- UAV telemetry (setpoint, action, pose, attitude, battery)
- Mission log (waypoints and mission events)
- RAN measurements collected from dRAX Kafka (DU, CU, UE metrics)
- Synthetic traffic measurements (iperf3), when enabled
- Video streaming QoS/QoE measurements, when enabled
- Metadata describing the experimental conditions

All data sources are synchronized using Unix timestamps.

---

## Dataset Structure

The dataset is organized by mission:

```
dataset/
│
├── README.md
├── DATASET_STRUCTURE.md
├── DATA_FORMAT.md
├── METADATA.md
├── MISSIONS.md
├── tests_metadata_values.csv
│
└── missions/
    ├── test01/
    ├── test02/
    ├── ...
    └── test23/
```

Each mission folder contains:

```
missions/testXX/
│
├── metadata.json
│
├── drone/
│   ├── 6GLeader_mission_X.log
│   ├── log_X_setpont.csv
│   ├── log_X_action.csv
│   ├── log_X_pose.csv
│   ├── log_X_attitude.csv
│   └── log_X_battery.csv
│
├── ran/
│   └── testXX_ran_ue_ds.csv
│
├── app/
│   ├── testXX_tcp_XXX.csv (iperf3, if present)
│   ├── testXX_receiver_XX.csv (video performance, if present)
│   └── testXX_receiver_XX.json (video summary, if present)
```

---

## Experimental Scenarios

The dataset includes multiple experimental scenarios:

- Application traffic experiments (iperf, video, mixed traffic)
- Uplink power control experiments using `p0_nominal_with_grant`
- Trajectory variation experiments (nominal and offset trajectories)
- Mobility pattern experiments (continuous motion, stop-and-go, hover)
- Propagation condition experiments (LOS and obstructed conditions)

Each mission represents a different combination of these variables.

---

## Data Synchronization

All dataset files use a common time reference:
- Unix timestamp in microseconds (`unix_time_us` or `Timestamp`)

This enables time alignment across:
- UAV telemetry
- RAN measurements
- Application measurements

---

## Documentation Files

| File | Description |
|------|-------------|
| README.md | Dataset overview |
| DATASET_STRUCTURE.md | Folder structure |
| DATA_FORMAT.md | Description of all data files |
| METADATA.md | Description of metadata.json structure |
| MISSIONS.md | Description of experimental missions |
| tests_metadata_values.csv | Description of the mapping of the test metadata and parameters in all the tests |

---

## Potential Use Cases

The dataset enables research on:

- Cross-layer performance analysis
- QoS and QoE modeling
- Mobility-aware networking
- Uplink power control analysis
- Communication-aware trajectory optimization
- Network-based localization
- Integrated communication and sensing
- AI-driven network optimization
- 6G semantic and context-aware networking

---

## Notes

- Indoor environment
- Single UAV
- VICON motion capture system (60 Hz)
- RAN sampling rate: 1 Hz
- Application sampling rate: variable
- Mission duration: approximately 90–120 seconds

---

## License and Usage

This dataset is intended for research and educational purposes.  
Please cite the associated publication when using the dataset in academic work.
