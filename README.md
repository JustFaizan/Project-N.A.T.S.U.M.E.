# Project N.A.T.S.U.M.E.
### Networked Autonomous Triage & Supply Management Engine

![Status](https://img.shields.io/badge/Status-Active%20Development-yellow)
![Domain](https://img.shields.io/badge/Domain-Healthcare%20Robotics-blue)
![Platform](https://img.shields.io/badge/Platform-ESP32%20%7C%20Edge%20AI-green)
![Role](https://img.shields.io/badge/Role-Tech%20Lead-orange)

> *An AI-integrated welfare robot for resource-constrained clinical environments — designed for the hospitals that most robotics research ignores.*

---

## The Problem

Most clinical robot deployments assume infrastructure that the majority of the world does not have: pre-built facility maps, LiDAR arrays, cloud connectivity, and dedicated robotics maintenance staff. In a well-funded metropolitan hospital, these assumptions hold. In a district hospital in rural India — or any of the thousands of resource-constrained clinical facilities across the developing world — they do not.

If autonomous robotics is to be meaningful for these environments, it must be designed around their constraints from the beginning. Not retrofitted to work within them later.

N.A.T.S.U.M.E. is an attempt to build that robot.

---

## Project Overview

N.A.T.S.U.M.E. is a frugal, edge-native welfare robot for indoor clinical environments. It targets three core functions:

- **Active Patient Triage** — real-time fall and emergency detection using on-device pose estimation
- **Autonomous Navigation** — mapless indoor navigation using visual geometry and OCR, with no LiDAR SLAM dependency
- **Supply Management** — autonomous ward-to-ward delivery

All inference runs locally on embedded hardware. No cloud dependency. No pre-built maps. No expensive sensor infrastructure.

---

## Architecture

```
┌──────────────────────────────────────────────────┐
│            Edge-AI Command Dashboard             │
│        (Centralised monitoring & control)        │
└───────────────────────┬──────────────────────────┘
                        │ MQTT
               ┌────────┴────────┐
               ▼                 ▼
      ┌──────────────┐   ┌──────────────┐
      │  Perception  │   │  Navigation  │
      │    Module    │   │    Module    │
      │              │   │              │
      │ YOLOv8 Pose  │   │ OCR-based    │
      │ Estimation   │   │ Spatial      │
      │ (Fall/Alert  │   │ Inference    │
      │  Detection)  │   │ Visual Geom. │
      │              │   │ ToF LiDAR    │
      │ Dual         │   │ (close-range │
      │ ESP32-CAM    │   │  avoidance)  │
      └──────────────┘   └──────────────┘
```

### Design Decisions

**Why no LiDAR SLAM for navigation?**
LiDAR SLAM requires expensive hardware and upfront facility mapping — both infeasible in low-resource hospitals. Instead, N.A.T.S.U.M.E. reads environmental signage via OCR and infers spatial layout through visual geometry dynamically. The robot learns its environment the same way a new human employee would.

**Why YOLOv8 on embedded hardware?**
Cloud-dependent inference introduces unacceptable latency for clinical safety applications. A patient fall requires immediate response — not a round-trip to a remote server. YOLOv8 nano variants are optimised for edge deployment while maintaining acceptable accuracy for pose-based fall detection.

---

## Hardware Stack

| Component | Role | Status |
|-----------|------|--------|
| ESP32-CAM × 2 | Primary vision and compute | Sourced |
| ToF LiDAR | Close-range obstacle avoidance | Sourced |
| Ultrasonic sensors | Secondary proximity sensing | Sourced |
| Motor driver + chassis | Locomotion platform | Sourced |

> **Current Status:** All components sourced. Mechanical assembly and hardware integration in progress. Software development begins post-assembly.

---

## Open Research Questions

Building this system has surfaced two research questions that go beyond engineering implementation:

**1. Robustness of Pose Estimation Under Clinical Distributional Shift**
YOLOv8 pose estimation performs reliably in controlled conditions. Clinical environments introduce partial occlusion from bedding, non-standard patient postures, and highly variable illumination. How does detection confidence degrade under these distributional shifts — and where is the formal failure boundary?

**2. Failure Bounds of Mapless Visual Navigation**
The OCR + visual geometry navigation approach works empirically in tested environments. But under what levels of visual ambiguity does spatial inference break down? How does this compare against LiDAR SLAM baselines in constrained indoor geometries with poor signage?

These questions are currently being investigated as part of the project's research agenda.

---

## Implementation Roadmap

- [x] System architecture design
- [x] Component selection and sourcing
- [x] Research question formulation
- [ ] Mechanical assembly and chassis integration
- [ ] Sensor stack integration and calibration
- [ ] YOLOv8 pose estimation deployment on ESP32-CAM
- [ ] OCR-based navigation pipeline implementation
- [ ] Edge-AI command dashboard development
- [ ] Full system integration testing
- [ ] Clinical environment trials
- [ ] Exhibition and documentation

---

## Research Context

N.A.T.S.U.M.E. is part of a broader research programme in **frugal edge AI** — building intelligent systems designed from the ground up for resource-constrained real-world environments. The parallel project, [Eco-Twin](https://github.com/JustFaizan/eco-twin), investigates the same design philosophy applied to industrial predictive maintenance on severely memory-constrained hardware.

The motivating framework is **frugal innovation**: rather than asking "how do we make expensive systems cheaper," asking "what is the minimum viable intelligence for a given task in a given constraint environment?"

---

## Related Work

- Frugal AI in healthcare robotics for low-resource settings
- Mapless navigation using semantic visual cues (OCR, geometry)
- Edge deployment of pose estimation models (YOLOv8, MoveNet)
- Automation trust in high-stakes clinical environments

---

## Author

**Faizan Ahmed Khan**
B.Tech CSE (Data Science) | IPS Academy IES, Indore
Tech Lead — Project N.A.T.S.U.M.E.

📧 faizanahmedk09@gmail.com
🔗 [LinkedIn](https://linkedin.com/in/khanfaizanahmed)
🐙 [GitHub](https://github.com/JustFaizan)

---

## Project Status Note

This repository documents an active research and development project. The system is currently in hardware assembly phase. Code, schematics, and experimental results will be added progressively as development milestones are reached. The research questions and architectural documentation above reflect the current state of the investigation.

---

*"The goal is not to build a robot for hospitals that can afford robots. The goal is to build a robot for hospitals that cannot."*
