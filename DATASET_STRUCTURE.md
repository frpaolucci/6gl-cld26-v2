# DATASET_STRUCTURE.md

## Dataset Structure

This document describes the folder and file organization of the Cross-Layer UAV 5G/6G Dataset.

The dataset is organized by mission (test). Each mission folder contains all data collected during a single UAV flight mission, including drone telemetry, RAN measurements, application metrics, and metadata.

---

## Root Folder Structure

```
dataset/
│
├── README.md
├── DATASET_STRUCTURE.md
├── DATA_FORMAT.md
├── METADATA.md
│
└── missions/
    ├── test01/
    ├── test02/
    ├── test03/
    ├── ...
    └── test23/
```

---

## Mission Folder Structure

Each mission folder contains all files related to a specific test.

```
missions/testXX/
│
├── metadata.json
│
├── drone/
│   ├── mission_log.log
│   ├── drone_setpont.csv
│   ├── drone_action.csv
│   ├── drone_pose.csv
│   ├── drone_attitude.csv
│   └── drone_battery.csv
│
├── ran/
│   └── testXX_ran_ue_ds.csv
│
├── app/
│   ├── testXX_iperf.csv (optional)
│   ├── testXX_video_timeseries.csv (optional)
│   └── testXX_video_summary.json (optional)
│
└── notes.txt (optional)
```

Not all application files are present in every mission. The presence of application files depends on the experimental scenario and is specified in `metadata.json`.

---

## File Description

### Metadata
| File | Description |
|------|-------------|
| metadata.json | Mission configuration, environment, traffic, radio parameters, and file mapping |

### Drone Telemetry (/drone subfolder)
| File | Description |
|------|-------------|
| 6GLeader_mission_X.log.log | Mission events and waypoint timeline |
| log_X_setpont.csv | Commanded position and yaw references |
| log_X_action.csv | UAV actions and flight modes |
| log_X_pose.csv | Estimated position, velocity, acceleration |
| log_X_attitude.csv | UAV orientation (quaternion) |
| log_X_battery.csv | Battery and power telemetry |

### RAN Measurements
| File | Description |
|------|-------------|
| testXX_ran_ue_ds.csv | RAN metrics collected from dRAX Kafka (UE, DU, CU metrics) |

### Application Measurements
| File | Description |
|------|-------------|
| testXX_tcp_XXX.csv | Synthetic traffic measurements (iperf3) |
| testXX_receiver_XX.csv | Video streaming QoS/QoE time series |
| testXX_receiver_XX.json | Video streaming session summary |

---

## Time Synchronization

All files in the dataset share a common time reference:
- Unix time in microseconds (`unix_time_us` or `Timestamp`)

This allows synchronization across:
- Drone telemetry
- RAN measurements
- Application measurements

---

## Notes on Data Availability

Not all missions contain all file types. Depending on the experimental scenario, a mission may include:

| Scenario | Files Present |
|----------|--------------|
| Synthetic traffic only | RAN + iperf |
| Video only | RAN + video |
| Mixed traffic | RAN + iperf + video |
| No application traffic | RAN only |
| All missions | Drone telemetry + metadata |

The `metadata.json` file specifies which files are present for each mission.

---

## Summary

The dataset structure is designed to:
- Keep all data from a mission together
- Enable cross-layer analysis across drone, RAN, and application domains
- Ensure reproducibility through metadata files
- Allow easy parsing and automated processing

Each mission represents a complete cross-layer experiment including UAV mobility, radio measurements, and application performance.
