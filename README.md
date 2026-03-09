# 🩸 Edge AI 기반 피부톤 맞춤형 스마트 산소포화도 측정 시스템
> **(Smart SpO2 Monitoring System with Edge AI Skin Tone Calibration)**
>
> 💡 기존 광학식 산소포화도계의 **멜라닌 색소(피부톤) 왜곡 한계를 극복**하기 위해, AI가 피부톤을 즉각 판별하고 오차를 실시간 자동 보정하는 지능형 웰니스 디바이스 및 모바일 앱입니다.

<br>

## 📢 1. 프로젝트 개요 (Project Overview)
- **프로젝트명:** Edge AI 기반 피부톤 맞춤형 스마트 산소포화도 측정 시스템
- **개발 기간:** 2026.03 ~ 2026.06 (약 14주)
- **기획 배경:** 스마트 헬스케어 기기(스마트워치 등)의 산소포화도 측정 시, 피부톤이 어두울수록 빛 흡수율의 차이로 인해 심각한 오차가 발생합니다. 이를 소프트웨어적으로 보정하여 인종과 피부색에 구애받지 않는 고정밀 측정 시스템을 구축하고자 합니다.

<br>

## 🛠 2. 기술 스택 (Tech Stack)

### 💻 Hardware & Edge AI
<img src="https://img.shields.io/badge/Arduino UNO R4 WiFi-00979D?style=for-the-badge&logo=Arduino&logoColor=white"> <img src="https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white">
- **Sensors:** `MAX30102` (산소포화도/심박수), `TCS34725` (RGB 컬러 센서)

### 🧠 AI & Data Modeling
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=Python&logoColor=white"> <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white"> <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white">
- **Dataset:** `11K-Hands Dataset` (11,000명의 손 이미지 피부톤 라벨링 데이터)

### 📱 Frontend (Android App)
<img src="https://img.shields.io/badge/Android Studio-3DDC84?style=for-the-badge&logo=Android Studio&logoColor=white"> <img src="https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=Java&logoColor=white"> <img src="https://img.shields.io/badge/BLE 통신-0082FC?style=for-the-badge&logo=Bluetooth&logoColor=white">

### ⚙️ Backend & Database
<img src="https://img.shields.io/badge/Spring Boot-6DB33F?style=for-the-badge&logo=Spring Boot&logoColor=white"> <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=MySQL&logoColor=white"> <img src="https://img.shields.io/badge/AWS EC2-FF9900?style=for-the-badge&logo=Amazon EC2&logoColor=white">

<br>

## 🏛 3. 시스템 아키텍처 (System Architecture)

전체 시스템은 **데이터 수집(하드웨어) ➔ 실시간 표출(안드로이드) ➔ 데이터 적재(스프링 서버)**의 3계층 파이프라인으로 구성되어 있습니다.

```mermaid
graph TD
    subgraph Edge AI (Arduino UNO R4)
        TCS34725[TCS34725 RGB 센서] -->|피부톤 RGB 측정| EdgeAI{C++ 하드코딩된<br/>가중치 수식 연산}
        EdgeAI -->|피부톤 보정 계수| Calc[SpO2/BPM 최종 연산]
        MAX30102[MAX30102 센서] -->|적외선/적색광| Calc
        Calc --> BLE[BLE 모듈 (GATT)]
    end

    subgraph Android App
        BLE -->|실시간 데이터 수신| App[측정 화면 UI]
        App -->|측정 완료| HTTP[Retrofit2 API 통신]
    end

    subgraph Spring Boot Server
        HTTP -->|POST /api/measurements| API[REST API 접수처]
        API --> DB[(MySQL 기록 저장)]
    end
