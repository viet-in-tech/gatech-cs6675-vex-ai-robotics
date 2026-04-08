# VEX AI Robotics Competition Enhancement

![Demo](https://viet-in-tech.github.io/vex-ai-robotics-demo.gif)

**Portfolio write-up:** [VEX AI Robotics Competition: 3D Mapping, SpotFi & P2P Coordination](https://viet-in-tech.github.io/vex-ai-robotics.html)

**Georgia Tech CS6675 · Spring 2024**
**Author:** Viet Nguyen · pnguyen369@gatech.edu
**Submitted:** February 24, 2024

---

## Overview

This research paper proposes architectural improvements to the [VEX AI Robotics Competition](https://www.vexrobotics.com/v5/competition/vex-ai) — an autonomous robotics platform where AI-controlled robots compete in a 2-vs-2 game format without any human drivers.

The existing VEX AI system includes a GPS Sensor, AI Vision System, and Sensor Fusion Map. However, the Sensor Fusion Map's field of view is limited to what is directly in front of each robot, and the system has no Z-axis awareness — critical for the Elevation Bar scoring phase at the end of each match.

---

## Problem

- **Stale object tracking:** The Sensor Fusion Map timestamps object positions when observed but cannot update them if objects move outside the robot's field of view
- **No 3D depth perception:** The system only tracks X and Y coordinates; robots cannot detect elevation height — which directly determines Elevation Bar scoring
- **Individual robot isolation:** Each robot operates independently with no shared field awareness between teammates

---

## Proposed Solutions

### Baseline Design — SpotFi + RGB-D 3D Mapping
- **SpotFi** (Stanford, 2015): WiFi-based decimeter-level localization using Angle of Arrival from channel state information — improves positional accuracy without additional hardware beyond the existing V5 Radio
- **RGB-D Mapping**: Four Intel RealSense Depth Camera D457 units mounted at each field corner, providing real-time 3D depth imaging of the entire playing surface
- Both integrated with the **NVIDIA Jetson Nano** already embedded in the VEX AI system

### Design Refinement — P2P Decentralized Coordination
- **Peer-to-Peer system** via the V5 Radio: all four robots broadcast their GPS position, Vision System data, and Sensor Fusion Map observations to each other in real time
- Creates a distributed sensor network — every robot has access to every other robot's field observations
- Improves resiliency: no single point of failure; if one robot's sensors degrade, the other three continue sharing data
- Enables distributed load balancing across the four robots' NVIDIA Jetson Nano processors

---

## Evaluation Framework

Controlled experiments across ≥50 identical 2-minute matches:

| Metric | Measurement |
|--------|-------------|
| Gameplay scoring | Total points per match (integer) |
| CPU runtime | Milliseconds for V5 Brain + Jetson Nano task sets |
| 3D map quality | Qualitative comparison of Intel RealSense D457 renderings |
| Object positions | X, Y, Z coordinates stored as CSV |

---

## Technologies Referenced

- VEX AI System (GPS Sensor, AI Vision System, Sensor Fusion Map)
- NVIDIA Jetson Nano
- Intel RealSense Depth Camera D457
- SpotFi — Kotaru et al., Stanford University (2015)
- RGB-D SLAM — Eldemiry et al. (2022)
- VEXos (V5 Brain operating system)
- V5 Radio (P2P communication)

---

## Paper

See [`Assignment M3-Viet-Nguyen.pdf`](./Assignment%20M3-Viet-Nguyen.pdf) for the full research paper including figures, system architecture diagrams, and references.

---

## Course

**CS6675 — Advanced Internet Computing Systems and Applications**
Georgia Institute of Technology · Spring 2024
