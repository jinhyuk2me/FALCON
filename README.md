![Banner](https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/banner.png?raw=true)

<p align="center">
  <a href="https://www.apache.org/licenses/LICENSE-2.0">
    <img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=for-the-badge" alt="License">
  </a>
  <a href="https://docs.google.com/presentation/d/1z73na_gwi2OX0oAGJ8FHGI7qYufhDPk5QCgtm7bIQoM/edit?usp=sharing">
    <img src="https://img.shields.io/badge/PRESENTATION-GoogleSlides-yellow?style=for-the-badge&logo=google-slides&logoColor=white" alt="Presentation Deck">
  </a>
  <a href="https://youtube.com/playlist?list=PLCGG9KRfKwMmQqXvp43pChNMyyLSyjHp9&si=Qhge_jErHH7Qlb4e">
    <img src="https://img.shields.io/badge/DEMO-YouTube-red?style=for-the-badge&logo=youtube&logoColor=white" alt="Demo Playlist">
  </a>
</p>

# 📚 Table of Contents

- [1. Project Overview](#1-project-overview)
- [2. Key Features](#2-key-features)
- [3. Core Technologies](#3-core-technologies)
- [4. Technical Challenges and Solutions](#4-technical-challenges-and-solutions)
- [5. System Design](#5-system-design)
- [6. Project Structure](#6-project-structure)
- [7. Tech Stack](#7-tech-stack)
- [8. Project Schedule Management](#8-project-schedule-management)
- [9. Team](#9-team)
- [10. License](#10-license)

---

# 1. Project Overview

Major airports worldwide continue to report severe incidents such as **bird strikes**, **Foreign Object Debris (FOD) accidents**, and **runway incursions**. These incidents are usually the result of a combination of factors: high **cognitive load** for controllers and pilots, **sensor limitations**, and delayed information handoffs.

| Case | Year | Root Cause |
|------|------|------------|
| Muan Airport bird strike | 2024 | Lack of detection system |
| Concorde FOD accident | 2000 | Debris left on runway |
| Austin runway incursion | 2023 | Control error + situational awareness failure |

FALCON was created to raise the **safety and efficiency** of flight operations and pursues three core values.

## 💡 FALCON's Core Values

- **Real-time risk discovery**  
  Automatically detects threats humans may miss to **eliminate blind spots**

- **Decision-support assistance**  
  Hand signal interpretation, risk assessment, and voice responses **reduce cognitive load**

- **Immediate information delivery**  
  Provides risk information with **GUI/TTS** so crews can respond faster

---

# 2. Key Features

## 🛫 Air Traffic Controller AI Service: `Hawkeye`

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/hawkeye_mainpage.gif?raw=true" width="60%">
</p>

- **Ground hazard detection**
  - CCTV-based video analytics
  - When birds, FOD, people, or vehicles are detected, show GUI popups and map markers
  - Continuously update risk levels and write logs
  - [Watch video](https://youtu.be/lctXpBYrVsU)

- **Ground fall detection**
  - Recognizes civilians/workers who have fallen
  - Visualizes risk gauge (location, time, severity)
  - Provides visual summary to help decide if rescue is needed
  - [Watch video](https://youtu.be/jvWLBKryymM)

- **Ground access control**
  - Configure zone-based clearance levels (Level 1–3)
  - Detect and alert on access violations automatically
  - Reflect clearance updates on the GUI in real time
  - [Watch video](https://youtu.be/5NFzvtAFr_I)

---

## ✈️ Pilot AI Service: `RedWing`

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/dynamic_pose.gif?raw=true" width="60%">
</p>

- **Flight risk alerts**
  - Real-time TTS warnings for bird strikes and runway hazards
  - Connects video analytics with the risk assessment model
  - [Watch video](https://youtu.be/-si0u8I1h2A)

- **Risk inquiry auto-response**
  - Voice query (STT) → LLM classification → Voice response (TTS)
  - Example: “Runway Alpha status?” → “Runway Alpha is CLEAR.”
  - [Watch video](https://youtu.be/VvQjRLMTrvU)

- **Ground guidance assistance**
  - Recognizes marshaller hand signals (stop, proceed, turn left/right) in CCTV footage
  - Converts the signal into spoken instructions for pilots
  - [Watch video](https://youtu.be/sB_zEFfP7kI)

---

# 3. Core Technologies

## 1) Simulation-based Training and Prediction

- **Unity-based airport environment simulator (`RunwaySim`)**
  - Models runways and surrounding infrastructure
  - Generates automated pipelines for training ground-object detection models

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/runwaysim.gif?raw=true" width="60%">
</p>

- **Unity-based real-time bird strike risk simulator (`BirdRiskSim`)**
  - Predicts bird positions from fixed CCTV footage
  - Calculates **collision probability** using relative distances and velocities between birds, aircraft, and air routes

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/bird_sim.gif?raw=true" width="60%">
</p>

## 2) Object Detection

Airport risks are divided into **ground** and **aerial** zones, and each zone uses a dedicated detection pipeline.

### 🧱 Ground Object Detection

- **Detection classes**: birds, FOD, people, wildlife, aircraft, and vehicles (6 total)

- **Dataset composition**:
  - Hybrid dataset of Unity-simulated imagery and miniature airport photos
  - Automated labeling pipeline using Polycam, Blender, and Unity-based 3D scans
  - Simulated diverse lighting, angles, and environments to increase variation
  - Automatically generate ~3,000 labeled images per hour

<table align="center">
  <tr>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/poly_bird.gif?raw=true" width="200px"><br>
      <sub>Bird</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/poly_fod.gif?raw=true" width="200px"><br>
      <sub>FOD</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/poly_animal.gif?raw=true" width="200px"><br>
      <sub>Wild animal</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/poly_truck.gif?raw=true" width="200px"><br>
      <sub>Vehicle</sub>
    </td>
  </tr>
</table>

<div align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/blender.gif?raw=true" width="60%"><br>
  <sub>Automated labeling pipeline using Blender</sub>
</div>

- **Model architecture and training setup**:
  - YOLOv8n-box (960×960 input, 150 epochs, batch size 8)
  - Dataset split: Train 69.4% / Validation 20.9% / Test 9.8%

- **Post-processing classification**:
  - Detect hi-vis (HV) vests with OpenCV to determine if a person is authorized
  - Use vehicle color features to distinguish work vehicles vs. general vehicles

<table align="center">
  <tr>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/work_person.gif?raw=true" width="200px"><br>
      <sub>Authorized / unauthorized personnel</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/work_vehicle.gif?raw=true" width="200px"><br>
      <sub>Authorized / unauthorized vehicles</sub>
    </td>
  </tr>
</table>

- **Model performance (v0.3)**:
  - mAP@0.5: **0.9902**
  - mAP@0.5:0.95: **0.9005**
  - Precision: **0.9928**
  - Recall: **0.9672**

- **Key improvements**:
  - ~50% lighter and faster than the initial YOLOv11-seg model
  - Added negative samples to eliminate ArUco marker false positives

### 🛩️ Aerial Object Detection

Specialized YOLOv8-based model for detecting **aerial threats** such as birds. Powers the **BDS (Bird Detection System)** and provides the **flight risk alert** feature.

- **Training data**
  - Total epochs: `72`, final learning rate: `0.000495`
  - Framework: YOLOv8

- **Performance summary**

  | Metric | Epoch 69 (best) | Epoch 72 (final) |
  |--------|-----------------|------------------|
  | `mAP@0.5` | 0.9455 | 0.9438 |
  | `mAP@0.5:0.95` | 0.8278 | **0.8342** |
  | `Precision` | **0.9850** | 0.9787 |
  | `Recall` | 0.8949 | **0.9031** |

---

## 3) Object Tracking

### (1) Ground object tracking
- Uses the `ByteTrack` algorithm (Ultralytics built-in)
- Combines low-score detection with a `Kalman Filter`
- Meets both real-time and accuracy requirements

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/byte_track.gif?raw=true" width="50%">
</p>

### (2) Aerial object tracking

To predict and respond to aerial threats such as bird strikes, FALCON integrates **triangulation-based localization**, **ByteTrack-based object tracking**, and **Unity-driven risk computation**.

- **📌 Feasibility validation**
  - Reconstructed the bird flock trajectory from the 2024 Muan Airport incident using nearby CCTV after the fact.
  - Confirmed that triangulation and tracking can deliver real-time bird-strike risk scores.

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/bds_muan.png?raw=true" width="60%">
</p>

- **🌐 Simulation environment**
  - Modeled actual airport terrain in Unity
  - Generated multiple weather and flight scenarios
  - Aircraft follow Bézier-curve paths and support multi-flight simulations

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/bird_sim.gif?raw=true" width="60%">
</p>

- **🛰️ CCTV-based bird localization (triangulation) and path tracking**
  - Two synchronized CCTV feeds inside the Unity simulator
  - Detect 2D bird and aircraft positions in each view
  - Estimate 3D coordinates using triangulation
  - Track frame-to-frame positions with ByteTrack using the 3D data

<table align="center">
  <tr>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/triangulation.gif?raw=true" width="400px"><br>
      <sub>Triangulation-based localization</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/skytrack.gif?raw=true" width="400px"><br>
      <sub>ByteTrack trajectory tracking</sub>
    </td>
  </tr>
</table>

- **🧠 Real-time bird strike risk scoring**
  - Analyze relative distance, velocity, and direction of birds vs. aircraft  
    Output qualitative risk levels (e.g., `BR_MEDIUM`)
  - Deliver warnings to controllers and pilots via GUI and voice interface

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/bds_result.gif?raw=true" width="80%">
</p>

---

## 4) Pose Estimation

Combined static and dynamic pose estimation to precisely interpret marshaller gestures.

### (1) Static pose estimation
- `YOLOv8n-pose` extracts 17 keypoints
- Trained on 683 Blender-generated synthetic images + real captures
- Detects falls by analyzing keypoint tilt

### (2) Dynamic pose estimation

- **Model**
  - Temporal Convolutional Network (TCN)
  - Input: 17 joints × (x, y) coordinates → 34 features over 30 frames
  - Output classes: `stop`, `forward`, `left`, `right`

- **Dataset**
  - 3,984 sequences (train 80%, test 20%)
  - MediaPipe-based 17-joint coordinates

- **Performance**
  - Accuracy: **98.99%**
  - Precision: **99.00%**, Recall: **98.99%**, F1-Score: **98.99%**
  - Avg. confidence: **98.62%**, Std: 6.64%

- **Per-class metrics (test set)**

  | Gesture | Precision | Recall | F1-Score |
  |---------|-----------|--------|----------|
  | Stop    | 98.55%    | 99.51% | 99.03%   |
  | Forward | 99.46%    | 97.87% | 98.66%   |
  | Left    | 98.57%    | 99.52% | 99.04%   |
  | Right   | 99.49%    | 98.98% | 99.23%   |

---

## 5) Coordinate Mapping

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/output_trim.gif?raw=true" width="50%">
</p>

- **ArUco-based world coordinate mapping**
  - Uses OpenCV `perspectiveTransform()`
  - Maps ArUco marker pixel centers to real-world coordinates
  - Achieves ±5 mm/pixel accuracy

- **Object center correction**
  - Converts detected bounding-box centers into real-world positions
  - Used to check zone intrusions and access violations

---

# 4. Technical Challenges and Solutions

### 📉 YOLO accuracy degradation

**Problem**  
- Low detection accuracy during real-world tests

<div align="center">

| Original YOLO PR curve | Original real-world test |
|:--:|:--:|
| <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/pr_curve_Image_seg_model.png?raw=true" width="400"> | <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/old_real_test.gif?raw=true" width="400"> |

</div>

**Solution**  
- Built a **hybrid dataset** combining real and synthetic data  
- Retrained using YOLOv8n-box

<div align="center">

| Hybrid model PR curve | Hybrid model real-world test |
|:--:|:--:|
| <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/pr_curve_Hybrid_box_model.png?raw=true" height="300" width="400"> | <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/real_test.gif?raw=true" height="300" width="400"> |

</div>

---

### 🧍‍♂️ Pose keypoint detection errors

**Problem**  
- Keypoints were inaccurate when subjects lay down or were upside down

**Solution**  
- Generated **683** Blender-based synthetic poses  
- Retrained YOLOv8n-pose → higher fall-detection accuracy

<div align="center">

| Previous pose model | Improved pose model |
|:--:|:--:|
| <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/old_poseㅐ.png?raw=true" height="300" width="400"> | <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/pose.png?raw=true" height="300" width="400"> |

</div>

---

# 5. System Design

## System Architecture
![system_architecture](https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/software_architecture.png?raw=true)

## ER Diagram
![er_diagram](https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/er_diagram.png?raw=true)

---

# 6. Project Structure

```
FALCON/
├── src/
│   ├── systems/              # Core systems
│   │   ├── bds/              # Bird Detection System
│   │   └── ids/              # Intrusion Detection System
│   │
│   ├── simulation/          # Simulation
│   │   ├── bird_sim/        # Bird strike simulation
│   │   └── runway_sim/      # Runway simulation
│   │
│   ├── interfaces/          # User interfaces
│   │   ├── hawkeye/         # ATC GUI
│   │   └── redwing/         # Pilot GUI
│   │
│   ├── infrastructure/      # System infrastructure
│   │   └── server/          # Server code
│   │
│   ├── shared/              # Shared modules
│   │   └── utils/           # Utilities
│   │
│   └── tests/               # Tests
│       └── technical_test/  # Technical validation
│
├── docs/                    # Documentation
├── assets/                  # Assets
├── tools/                   # Tools
└── README.md                # Project overview
```

---

# 7. Tech Stack

| Category | Technologies |
|----------|--------------|
| **ML / DL** | ![YOLOv8](https://img.shields.io/badge/YOLOv8-FFB400?style=for-the-badge&logo=yolov5&logoColor=black) ![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white) ![ByteTrack](https://img.shields.io/badge/ByteTrack-222222?style=for-the-badge&logo=github&logoColor=white) ![TCN](https://img.shields.io/badge/TCN-800080?style=for-the-badge&logo=neural&logoColor=white) ![MediaPipe](https://img.shields.io/badge/MediaPipe-FF6F00?style=for-the-badge&logo=google&logoColor=white)<br>![Whisper](https://img.shields.io/badge/Whisper-9467BD?style=for-the-badge&logo=openai&logoColor=white) ![Ollama](https://img.shields.io/badge/Ollama-333333?style=for-the-badge&logo=vercel&logoColor=white) ![Coqui](https://img.shields.io/badge/Coqui-FFD166?style=for-the-badge&logo=soundcloud&logoColor=black) ![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white) ![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white) |
| **GUI** | ![PyQt6](https://img.shields.io/badge/PyQt6-41CD52?style=for-the-badge&logo=qt&logoColor=white) |
| **Database** | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white) |
| **Networking / Communication** | ![Socket](https://img.shields.io/badge/Socket-000000?style=for-the-badge&logo=socketdotio&logoColor=white) ![JSON](https://img.shields.io/badge/JSON-292929?style=for-the-badge&logo=json&logoColor=white) ![UDP](https://img.shields.io/badge/UDP-D8B4FE?style=for-the-badge&logo=wifi&logoColor=white) ![TCP](https://img.shields.io/badge/TCP-004E64?style=for-the-badge&logo=networkx&logoColor=white) |
| **Analytics / Visualization** | ![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white) ![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=chartdotjs&logoColor=white) |
| **Simulation / Synthetic Data** | ![Unity](https://img.shields.io/badge/Unity-000000?style=for-the-badge&logo=unity&logoColor=white) ![Blender](https://img.shields.io/badge/Blender-F5792A?style=for-the-badge&logo=blender&logoColor=white) ![Polycam](https://img.shields.io/badge/Polycam-272727?style=for-the-badge&logo=camera&logoColor=white) |

---

# 8. Project Schedule Management

Managed the program with Confluence and Jira. Visualized task assignment, development progress, and issue tracking to keep collaboration on track.

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/Jjra_manage.gif?raw=true" width="100%">
</p>

---

# 9. Team

### 🧑‍💼 Jongmyung Kim [`@jongbob1918`](https://github.com/jongbob1918)
- Project lead (documentation and schedule management)
- Built the ground-object detection AI system (IDS)
- Researched and implemented ground-object detection models
- Investigated and tested ArUco-based map coordinate mapping

### 🧑‍💼 Jiyeon Kim [`@heyjay1002`](https://github.com/heyjay1002)
- Generated pose keypoints and synthetic datasets with Blender
- Built a custom YOLO pose model for fall detection
- Designed and implemented the Hawkeye ATC GUI
- Researched LLM/STT/TTS for the RedWing pilot AI service

### 🧑‍💼 Hyojin Park [`@Park-hyojin`](https://github.com/Park-hyojin)
- Led system design and backend
- Built and maintained the main server
- Designed and managed the database
- Defined system interfaces and communication architecture
- Designed the ArUco-based coordinate mapping logic

### 🧑‍💼 Jinhyuk Jang [`@jinhyuk2me`](https://github.com/jinhyuk2me)
- Implemented the Unity/Blender synthetic data pipeline  
- Designed and built the real-time bird strike risk analysis AI system (BDS)
- Developed deep learning models for BDS
- Developed deep learning models for the IDS ground monitoring system
- Implemented RedWing ground-guidance assistance for pilots
- Built RedWing TTS warning and auto-response features

---

# 10. License

This project is open-sourced under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).  
See the [`LICENSE`](./LICENSE) file for details.
