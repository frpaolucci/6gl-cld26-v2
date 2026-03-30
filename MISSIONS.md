# MISSIONS.md

## Mission Scenarios Description

This document describes the experimental missions included in the dataset. Each mission corresponds to a UAV flight performed under specific network, traffic, and radio conditions. The missions are designed to evaluate cross-layer interactions between UAV mobility, 5G RAN performance, and application-level performance.

The dataset includes 23 missions. Each mission lasts approximately 90–120 seconds and is conducted in a controlled indoor environment with a single UAV connected to a 5G O-RAN network.

All missions include:
- UAV telemetry
- RAN measurements
- Metadata file
- Mission log

Some missions also include:
- Synthetic traffic (iperf3)
- Video streaming traffic
- Mixed traffic (iperf3 + video)

---

## Mission Categories

The missions are divided into experimental categories based on the test objective.

### 1. Application Traffic Experiments

These missions evaluate network and application performance under different traffic conditions.

Traffic configurations include:
- No traffic
- Synthetic traffic (iperf3)
- Video streaming (uplink)
- Mixed traffic (synthetic + video)
- Different traffic rates (e.g., 5 Mbps, 20 Mbps, 50 Mbps, unlimited)

These experiments are used to analyze:
- Throughput performance
- Packet loss
- Latency and jitter
- Video Quality of Experience (QoE)
- Network congestion effects

---

### 2. Uplink Power Control Experiments

These missions evaluate the impact of uplink transmission power on network performance.

The uplink transmission power is controlled using the DU parameter:
- `p0_nominal_with_grant`

This parameter is varied across missions (e.g., from approximately -76 dBm to -130 dBm) to emulate different uplink coverage conditions.

These experiments are used to analyze:
- Uplink throughput
- Retransmissions and reliability
- HARQ performance
- Latency under uplink-limited conditions
- Application performance under poor radio conditions

---

### 3. Trajectory Experiments

These missions evaluate how UAV spatial position affects radio and application performance.

Trajectory variations include:
- Nominal trajectory
- Right offset trajectory
- Left offset trajectory
- Large left offset trajectory

These experiments are used to analyze:
- Throughput vs position
- SINR vs position
- Coverage variations
- Communication-aware trajectory planning

---

### 4. Mobility Pattern Experiments

These missions evaluate the impact of UAV motion on network performance.

Mobility patterns include:
- Continuous motion
- Stop-and-go motion
- Hover phases at specific waypoints

These experiments are used to analyze:
- Channel variability due to motion
- Throughput vs speed
- Link adaptation behavior
- Video performance during motion vs hover

---

## Mission Duration and Sampling Rates

| Component | Sampling Rate |
|-----------|---------------|
| Drone telemetry | Variable (typically high rate) |
| VICON positioning | 60 Hz |
| RAN measurements | 1 Hz |
| Application metrics | Variable |
| Mission duration | 90–120 s |

All data sources are synchronized using Unix timestamps.

---

## Summary of Mission Variables

| Variable | Description |
|---------|-------------|
| Traffic type | None, iperf, video, mixed |
| Traffic rate | Configured application rate |
| TX power | p0_nominal_with_grant parameter |
| Trajectory | Nominal or offset |
| Mobility | Continuous or stop-and-go |
| Propagation | LOS / obstructed |

Each mission represents a different combination of these variables.

---

## Purpose of the Mission Design

The mission set is designed to enable cross-layer analysis across:
- UAV mobility
- Radio Access Network performance
- Application performance
- Quality of Experience
- Energy consumption
- Network-based localization

The dataset supports research in:
- Mobility-aware networking
- Uplink power control
- QoS and QoE modeling
- Cross-layer optimization
- Network-based localization
- Integrated communication and sensing
- AI-driven network optimization
- 6G semantic and context-aware networking
