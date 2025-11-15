![Banner](https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/banner.png?raw=true)

<p align="center">
  <a href="https://www.apache.org/licenses/LICENSE-2.0">
    <img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=for-the-badge" alt="License">
  </a>
  <a href="https://docs.google.com/presentation/d/1z73na_gwi2OX0oAGJ8FHGI7qYufhDPk5QCgtm7bIQoM/edit?usp=sharing">
    <img src="https://img.shields.io/badge/PRESENTATION-GoogleSlides-yellow?style=for-the-badge&logo=google-slides&logoColor=white" alt="발표자료">
  </a>
  <a href="https://youtube.com/playlist?list=PLCGG9KRfKwMmQqXvp43pChNMyyLSyjHp9&si=Qhge_jErHH7Qlb4e">
    <img src="https://img.shields.io/badge/DEMO-YouTube-red?style=for-the-badge&logo=youtube&logoColor=white" alt="ATC AI Service">
  </a>
</p>

# 📚 목차

- [1. 프로젝트 개요](#1-프로젝트-개요)
- [2. 주요 기능](#2-주요-기능)
- [3. 핵심 기술](#3-핵심-기술)  
- [4. 기술적 문제 및 해결](#4-기술적-문제-및-해결)
- [5. 시스템 설계](#5-시스템-설계)
- [6. 프로젝트 구조](#6-프로젝트-구조)
- [7. 기술 스택](#7-기술-스택)
- [8. 일정 관리](#8-프로젝트-일정-관리)
- [9. 팀 구성](#9-팀-구성)
- [10. 라이선스](#10-라이선스)

---

# 1. 프로젝트 개요

국내외 공항에서는 **조류 충돌**, **FOD 사고**, **활주로 오진입** 등 중대 사고가 **반복적으로 발생**하고 있습니다.  
이는 관제사·조종사의 **인지 부담**, 감지 장비의 **한계**, 정보 전달 지연 등 복합적인 요인에 기인합니다.

| 사례 | 발생 연도 | 원인 요약 |
|------|------------|----------------|
| 무안공항 조류충돌 | 2024 | 감지 시스템 부재 |
| 콩코드 FOD 사고 | 2000 | 이물질 미제거 |
| 오스틴 활주로 오진입 | 2023 | 관제 실수 + 인지 오류 |

FALCON은 이러한 문제의식을 바탕으로, 항공 운항의 **안전성과 효율성**을 높이기 위한 **세 가지 핵심 가치를 제안합니다.**

## 💡 FALCON의 핵심 가치

- **위험요소 실시간 탐지**  
  사람이 놓칠 수 있는 위험요소를 자동 감지해 **사각지대 제거**

- **판단 지원 기능**  
  수신호 분석, 위험 판단, 음성 응답 등으로 **인지 부담 감소**

- **즉각적인 정보 전달**  
  위험 정보를 **GUI/TTS**로 자동 제공해 **반응 속도 향상**
---

# 2. 주요 기능

## 🛫 관제사 AI 서비스: `Hawkeye`

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/hawkeye_mainpage.gif?raw=true" width="60%">
</p>

- **지상 위험요소 탐지**
  - CCTV 기반 영상 분석
  - 조류, FOD, 사람, 차량 등 탐지 시 GUI 팝업 및 지도 마커 표시
  - 위험도 상태 갱신 및 로그 생성
  - [영상 보기](https://youtu.be/lctXpBYrVsU)

- **지상 쓰러짐 감지**
  - 일반인 / 작업자의 쓰러짐 상태 인식
  - 위험도 게이지 시각화 (예: 쓰러진 위치, 시간, 위험 수치)
  - 구조 필요성 판단을 위한 시각적 정보 제공
  - [영상 보기](https://youtu.be/jvWLBKryymM)

- **지상 출입 통제**
  - 구역별 출입등급 설정 (1~3단계)
  - 출입 위반 시 자동 감지 및 알림
  - 출입 조건 변경 시 실시간 GUI 반영
  - [영상 보기](https://youtu.be/5NFzvtAFr_I)

---

## ✈️ 조종사 AI 서비스: `RedWing`

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/dynamic_pose.gif?raw=true" width="60%">
</p>


- **운항 위험 경보**
  - 조류 충돌, 활주로 위험요소 등을 실시간 TTS로 경고
  - 영상 분석 + 위험 판단 모델 연동
  - [영상 보기](https://youtu.be/-si0u8I1h2A)

- **위험도 질의 자동응답**
  - 음성 질의(STT) → LLM 분류 → 음성 응답(TTS)
  - 예: “Runway Alpha status?” → “Runway Alpha is CLEAR.”
  - [영상 보기](https://youtu.be/VvQjRLMTrvU)

- **지상 유도 보조**
  - CCTV 영상 내 유도사 수신호(정지, 전진, 좌우회전 등) 인식
  - 수신호 분석 결과를 조종사에게 음성 안내로 전달
  - [영상 보기](https://youtu.be/sB_zEFfP7kI)
 
---

# 3. 핵심 기술

## 1) 시뮬레이션 기반 학습 및 예측

- **Unity 기반 공항 환경 시뮬레이터(`RunwaySim`) 구성**:
  - 모형 활주로 및 주변 환경 모델링
  - 지상 객체 탐지 딥러닝 모델 학습을 위한 자동 파이프라인을 위해 제작

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/runwaysim.gif?raw=true" width="60%">
</p>

- **Unity 기반 실시간 조류 충돌 위험도 시뮬레이터(`BirdRiskSim`) 구성**:
  - 고정 CCTV 영상 기반 조류 위치 예측
  - 조류-항공기 상대 거리, 조류-항로 상대 거리, 조류-항공기 상대 속도를 분석하여 **충돌 확률** 산출

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/bird_sim.gif?raw=true" width="60%">
</p>

## 2) 객체 탐지 (Object Detection)
공항 환경에서 발생할 수 있는 다양한 위험요소를 **지상(Ground)** 과 **상공(Aerial)** 영역으로 구분하여 탐지하는 이중 구조로 설계되었습니다.

### 🧱 지상 객체 감지 (Ground Object Detection)

- **탐지 클래스**: 조류, FOD, 사람, 야생동물, 항공기, 차량 (총 6종)

- **데이터셋 구성**:
  - Unity 기반 시뮬레이션 이미지 + 실제 공항 모형 촬영 이미지로 구성된 Hybrid Dataset
  - Polycam, Blender, Unity를 활용한 3D 스캔 기반 자동 라벨링 파이프라인 구축
  - 다양한 조명/각도/환경 조건을 시뮬레이션하여 데이터 다양성 확보
  - 1시간당 약 3,000장의 이미지와 라벨 자동 생성 가능
 
<table align="center">
  <tr>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/poly_bird.gif?raw=true" width="200px"><br>
      <sub>조류</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/poly_fod.gif?raw=true" width="200px"><br>
      <sub>FOD(이물질)</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/poly_animal.gif?raw=true" width="200px"><br>
      <sub>야생동물</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/poly_truck.gif?raw=true" width="200px"><br>
      <sub>차량</sub>
    </td>
  </tr>
</table>

<div align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/blender.gif?raw=true" width="60%"><br>
  <sub>Blender를 이용한 자동 라벨링 파이프라인 구축</sub>
</div>

- **모델 아키텍처 및 학습 설정**:
  - YOLOv8n-box 사용 (960×960 해상도, 150 epoch, batch size 8)
  - 데이터 분할: Train (69.4%) / Validation (20.9%) / Test (9.8%)

- **후처리 기반 식별 기능**:
  - OpenCV로 형광 조끼(HV 색상) 인식 → 작업자 여부 판단
  - 차량 색상 기반 분류 → 일반 차량과 작업 차량 구분
 
<table align="center">
  <tr>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/work_person.gif?raw=true" width="200px"><br>
      <sub>인가자/비인가자</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/work_vehicle.gif?raw=true" width="200px"><br>
      <sub>인가차량/비인가차량</sub>
    </td>
  </tr>
</table>



- **모델 성능 (v0.3 기준)**:
  - mAP@0.5: **0.9902**
  - mAP@0.5:0.95: **0.9005**
  - Precision: **0.9928**
  - Recall: **0.9672**

- **주요 개선사항**:
  - YOLOv11-seg 기반 초기 모델 대비 약 50% 경량화 및 속도 개선
  - Negative Sample 학습을 통해 ArUco 마커 오인식 문제 해결

### 🛩️ 상공 객체 감지 (Aerial Object Detection)

조류 등 **공중 위험요소**를 실시간으로 탐지하기 위해 YOLOv8 기반으로 개발된 특화 모델.  
FALCON의 **BDS (Bird Detection System)** 에 탑재되어 **운항 위험 경보** 기능을 수행한다.

- **학습 정보**
  - 총 Epoch: `72`, 최종 학습률: `0.000495`
  - 프레임워크: YOLOv8

- **성능 요약**

  | 지표 | Epoch 69 (최고 성능) | Epoch 72 (최종 성능) |
  |------|------------------------|------------------------|
  | `mAP@0.5` | 0.9455 | 0.9438 |
  | `mAP@0.5:0.95` | 0.8278 | **0.8342** |
  | `Precision` | **0.9850** | 0.9787 |
  | `Recall` | 0.8949 | **0.9031** |

---

## 3) 객체 추적 (Object Tracking)

### (1) 지상 객체 추적:
  - `ByteTrack` 알고리즘 사용 (Ultralytics 내장)
  - `Low Score Detection` + `Kalman Filter` 기반 예측
  - 실시간성과 정확성 우수

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/byte_track.gif?raw=true" width="50%">
</p>

### (2) 공중 객체 추적 (Aerial Object Tracking)

조류 충돌과 같은 공중 위험을 예측하고 대응하기 위해, FALCON은 **삼각측량 기반 위치 추정**, **ByteTrack 기반 객체 추적**, 그리고 **Unity 시뮬레이터 기반 위험도 계산** 기술을 통합하여 다음과 같은 시스템을 구현하였다.

- **📌 기술적 가능성 검토**
  - 2024년 무안공항 조류 충돌 사고를 기반으로 주변 CCTV 영상으로 충돌 직전 새떼의 이동 경로를 사고 이후에 복원.
  - 이를 활용해 삼각측량 및 트래킹 기술을 활용한 조류 충돌 실시간 위험도 산출 기능 구현이 실제로 가능할 것으로 판단하였음.

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/bds_muan.png?raw=true" width="60%">
</p>

- **🌐 시뮬레이션 환경 구성**
  - 실제 공항 지형을 Unity로 모델링
  - 다양한 기상 조건 및 비행 경로 시나리오 생성
  - 항공기는 베지어 곡선 기반 경로로 이동하며 다중 비행 지원

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/bird_sim.gif?raw=true" width="60%">
</p>

- **🛰️ CCTV 기반 조류 위치 추정 (Triangulation) 및 실시간 경로 추적**
  - Unity 시뮬레이터 내 2대의 고정 CCTV를 통해 **동기화된 영상 프레임 확보**
  - 각 CCTV에서 조류와 항공기의 2D 위치를 감지
  - 삼각측량 알고리즘을 통해 3D 실제 위치 계산
  - 추정된 3D 위치 데이터를 기반으로 ByteTrack으로 **프레임 간 추적**

<table align="center">
  <tr>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/triangulation.gif?raw=true" width="400px"><br>
      <sub>삼각측량을 통한 위치 추정</sub>
    </td>
    <td align="center">
      <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/skytrack.gif?raw=true" width="400px"><br>
      <sub>ByteTrack을 통한 경로 추적</sub>
    </td>
  </tr>
</table>


- **🧠 실시간 조류 충돌 위험도 계산**
  - 조류와 항공기의 **상대 거리, 속도, 방향**을 분석하여  
    **충돌 위험도 수치화 (예: BR_MEDIUM 등급)**  
  - GUI 및 음성 인터페이스를 통해 조종사/관제사에게 실시간 경고 전달

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/bds_result.gif?raw=true" width="80%">
</p>

---

## 4) 자세 감지 (Pose Estimation)

지상 유도사의 제스처를 정확하게 인식하기 위해 정적 및 동적 자세 감지 기술을 결합하여 적용하였다.

### (1) 정적 자세 감지
- `YOLOv8n-pose` 기반으로 17개 Keypoint 추출
- Blender 기반 합성 데이터(683장) + 실제 촬영 데이터로 학습
- Keypoint 기울기 분석을 통해 쓰러짐 판단 가능

### (2) 동적 자세 감지

- **모델 구조**:
  - Temporal Convolutional Network (TCN)
  - 입력: 17개 관절의 x, y 좌표 (총 34개 feature), 30프레임 시퀀스
  - 출력 클래스: `stop`, `forward`, `left`, `right` (총 4종)

- **데이터셋 구성**:
  - 총 3,984개의 시퀀스 (train: 80%, test: 20%)
  - MediaPipe 기반 17개 관절 좌표 사용

- **성능 요약**:
  - Accuracy: **98.99%**
  - Precision: **99.00%**, Recall: **98.99%**, F1-Score: **98.99%**
  - 평균 신뢰도: **98.62%**, 표준편차: 6.64%

- **클래스별 성능 (테스트셋 기준)**:

  | 제스처   | Precision | Recall | F1-Score |
  |----------|-----------|--------|----------|
  | Stop     | 98.55%    | 99.51% | 99.03%   |
  | Forward  | 99.46%    | 97.87% | 98.66%   |
  | Left     | 98.57%    | 99.52% | 99.04%   |
  | Right    | 99.49%    | 98.98% | 99.23%   |

---

## 5) 좌표계 변환 (Coordinate Mapping)

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/output_trim.gif?raw=true" width="50%">
</p>

- **ArUco 기반 실제 맵 좌표 변환**:
  - OpenCV의 `perspectiveTransform()` 사용
  - ArUco 마커 중심점의 픽셀 좌표 ↔ 실제 좌표로 매핑
  - 오차 범위 ±5mm/픽셀 수준의 정밀도

- **객체 중심 좌표 보정**:
  - 감지된 객체의 Bounding Box 중심을 실시간 위치로 변환
  - 구역 침범 여부, 출입 위반 판단 등에 활용

---

## 4. 기술적 문제 및 해결

### 📉 YOLO 정확도 저하

**문제**  
- 실사 기반 테스트 시 객체 탐지 정확도 낮음  

<div align="center">

| 기존 YOLO 모델 성능 (PR Curve) | 기존 모델 실사 테스트 |
|:--:|:--:|
| <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/pr_curve_Image_seg_model.png?raw=true" width="400"> | <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/old_real_test.gif?raw=true" width="400"> |

</div>

**해결**  
- 실제 + 합성 데이터 결합한 **Hybrid Dataset** 구성  
- YOLOv8n-box 모델 재학습

<div align="center">

| Hybrid 모델 PR Curve | Hybrid 모델 실사 테스트 |
|:--:|:--:|
| <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/pr_curve_Hybrid_box_model.png?raw=true" height="300" width="400"> | <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/real_test.gif?raw=true" height="300" width="400"> |

</div>

---

### 🧍‍♂️ Pose Keypoint 인식 오류

**문제**  
- 사람이 **눕거나 뒤집힌 자세**일 때 keypoint 인식률 저하

**해결**  
- Blender로 포즈 합성 이미지 **683장** 생성  
- YOLOv8n-pose 모델 학습 → 쓰러짐 감지 성능 향상

<div align="center">

| 기존 모델 Pose 인식 결과 | 개선된 모델 인식 결과 |
|:--:|:--:|
| <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/old_poseㅐ.png?raw=true" height="300" width="400"> | <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/pose.png?raw=true" height="300" width="400"> |

</div>



---

# 5. 시스템 설계

## 시스템 아키텍처
![system_architecture](https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/software_architecture.png?raw=true)

## ER 다이어그램
![er_diagram](https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/er_diagram.png?raw=true)

---

# 6. 프로젝트 구조

```
FALCON/
├── src/
│   ├── systems/              # 핵심 시스템
│   │   ├── bds/             # Bird Detection System
│   │   └── ids/             # Intrusion Detection System
│   │
│   ├── simulation/          # 시뮬레이션
│   │   ├── bird_sim/        # 새 충돌 시뮬레이션
│   │   └── runway_sim/      # 활주로 시뮬레이션
│   │
│   ├── interfaces/          # 사용자 인터페이스
│   │   ├── hawkeye/         # 관제사용 GUI
│   │   └── redwing/         # 조종사용 GUI
│   │
│   ├── infrastructure/      # 시스템 인프라
│   │   └── server/          # 서버 코드
│   │
│   ├── shared/              # 공통 모듈
│   │   └── utils/           # 유틸리티
│   │
│   └── tests/               # 테스트 코드
│       └── technical_test/  # 기술 검증
│
├── docs/                    # 문서
├── assets/                  # 리소스
├── tools/                   # 도구
└── README.md               # 프로젝트 설명서
```

---

# 7. 기술 스택

| 분류 | 사용 기술 |
|------|-----------|
| **ML / DL** | ![YOLOv8](https://img.shields.io/badge/YOLOv8-FFB400?style=for-the-badge&logo=yolov5&logoColor=black) ![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white) ![ByteTrack](https://img.shields.io/badge/ByteTrack-222222?style=for-the-badge&logo=github&logoColor=white) ![TCN](https://img.shields.io/badge/TCN-800080?style=for-the-badge&logo=neural&logoColor=white) ![MediaPipe](https://img.shields.io/badge/MediaPipe-FF6F00?style=for-the-badge&logo=google&logoColor=white)<br>![Whisper](https://img.shields.io/badge/Whisper-9467BD?style=for-the-badge&logo=openai&logoColor=white) ![Ollama](https://img.shields.io/badge/Ollama-333333?style=for-the-badge&logo=vercel&logoColor=white) ![Coqui](https://img.shields.io/badge/Coqui-FFD166?style=for-the-badge&logo=soundcloud&logoColor=black) ![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white) ![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white) |
| **GUI** | ![PyQt6](https://img.shields.io/badge/PyQt6-41CD52?style=for-the-badge&logo=qt&logoColor=white) |
| **데이터베이스** | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white) |
| **네트워크 / 통신** | ![Socket](https://img.shields.io/badge/Socket-000000?style=for-the-badge&logo=socketdotio&logoColor=white) ![JSON](https://img.shields.io/badge/JSON-292929?style=for-the-badge&logo=json&logoColor=white) ![UDP](https://img.shields.io/badge/UDP-D8B4FE?style=for-the-badge&logo=wifi&logoColor=white) ![TCP](https://img.shields.io/badge/TCP-004E64?style=for-the-badge&logo=networkx&logoColor=white) |
| **분석 / 시각화** | ![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white) ![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=chartdotjs&logoColor=white) |
| **시뮬레이션 / 합성 데이터** | ![Unity](https://img.shields.io/badge/Unity-000000?style=for-the-badge&logo=unity&logoColor=white) ![Blender](https://img.shields.io/badge/Blender-F5792A?style=for-the-badge&logo=blender&logoColor=white) ![Polycam](https://img.shields.io/badge/Polycam-272727?style=for-the-badge&logo=camera&logoColor=white) |



---

# 8. 프로젝트 일정 관리

Confluence와 Jira를 이용해 전체 일정을 관리하였습니다.
세부 업무 분배, 기능 개발 진척도, 이슈 트래킹 등을 시각적으로 체계화하여 효율적인 협업 환경을 조성하였습니다.

<p align="center">
  <img src="https://github.com/addinedu-ros-9th/deeplearning-repo-2/blob/main/assets/images/Jjra_manage.gif?raw=true" width="100%">
</p>

---

# 9. 팀 구성

### 🧑‍💼 김종명 [`@jongbob1918`](https://github.com/jongbob1918)
- 프로젝트 총괄 (문서 및 일정 관리)
- 지상 객체 탐지 AI 시스템(IDS) 구축
- 지상 객체 탐지를 위한 딥러닝 모델 기술조사 및 제작
- 아루코 마커 기반 맵 좌표 변환 기술조사 및 테스트 

### 🧑‍💼 김지연 [`@heyjay1002`](https://github.com/heyjay1002)
- Blender 이용 Pose Keypoint 추출 및 합성 데이터셋 생성
- 쓰러짐 기반 YOLO Pose Custom Model 제작
- 관제사 AI 서비스(Hawkeye) GUI 설계 및 기능 구현
- 파일럿 AI 서비스(RedWing) LLM 및 음성 처리 기능(STT/TTS) 기술조사

### 🧑‍💼 박효진 [`@Park-hyojin`](https://github.com/Park-hyojin)
- 시스템 설계 및 백엔드 총괄
- 메인 서버 구축 및 관리
- 데이터베이스 구축 및 관리
- 시스템 인터페이스 및 통신 구조 설계
- 아루코 마커 기반 맵 좌표 변환 로직 설계

### 🧑‍💼 장진혁 [`@jinhyuk2me`](https://github.com/jinhyuk2me)
- Unity / Blender 기반 합성 데이터 파이프라인 구현  
- 실시간 조류 충돌 위험도 분석 AI 시스템(BDS) 설계 및 구현
- 실시간 조류 충돌 위험도 분석 AI 시스템(BDS) 딥러닝 모델 제작
- 실시간 지상 객체 감시 시스템(IDS) 딥러닝 모델 제작
- 파일럿 AI 서비스(RedWing) 지상 유도 보조 기능 구현
- 파일럿 AI 서비스(RedWing) 운항 위험도 음성 경보 및 자동응답 기능 구현

---

# 10. 라이선스

이 프로젝트는 [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)에 따라 오픈소스로 제공됩니다.
자세한 사항은 [`LICENSE`](./LICENSE) 파일을 참고해주세요.
