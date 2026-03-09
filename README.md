# 🩸 Edge AI 기반 피부톤 맞춤형 스마트 산소포화도 측정 시스템
> **(Smart SpO2 Monitoring System with Edge AI Skin Tone Calibration)**
>
> 💡 기존 광학식 산소포화도계의 **멜라닌 색소(피부톤) 왜곡 한계를 극복**하기 위해, AI가 피부톤을 즉각 판별하고 오차를 실시간 보정하는 지능형 웰니스 디바이스 및 모바일 앱입니다.

<br>

## 📢 1. 프로젝트 개요 (Project Overview)

- **프로젝트명:** Edge AI 기반 피부톤 맞춤형 스마트 산소포화도 측정 시스템
- **개발 기간:** 2026.03 ~ 2026.06 (약 14주)

**[기획 배경 및 문제 제기]**
현재 널리 사용되는 스마트워치나 의료용 광학식 산소포화도계(PPG 센서)는 LED 빛을 피부에 쏘아 반사되는 광량을 분석하여 수치를 산출합니다. 하지만 피부 표피의 **'멜라닌 색소'가 빛을 흡수하는 성질** 때문에, 피부톤이 어두울수록 광 신호가 왜곡되어 실제 수치와 큰 오차가 발생하는 구조적 한계가 의료계에서도 지적되어 왔습니다.

**[핵심 해결 방안 (Core Solution)]**
본 프로젝트는 이러한 하드웨어 센서의 물리적 한계를 **Edge AI와 소프트웨어 아키텍처로 극복**합니다. 
기존 MAX30102 센서에 정밀 RGB 컬러 센서(TCS34725)를 결합하여 측정자의 피부톤을 선제적으로 수치화합니다. 이후, 11,000개의 손 이미지 데이터(11K-Hands)로 사전 학습된 선형 회귀 AI 모델의 가중치(Weight) 공식을 아두이노 C++에 이식합니다. 이를 통해 기기 내부에서 짧은 시간안에 피부톤에 따른 오차 보정 계수를 산출하고, 산소포화도 연산 로직에 실시간으로 반영하여 정확도를 극대화합니다.

**[기대 효과 및 프로젝트 의의]**
모든 인종과 피부색에 차별 없이 신뢰성 높은 건강 데이터를 제공하는 **'포용적 헬스케어(Inclusive Healthcare)'** 기기의 기술적 기반을 마련합니다. 나아가 무거운 클라우드 AI 연산 대신 하드웨어 단에서 즉각적으로 처리되는 엣지 컴퓨팅(Edge Computing)을 구현하고, 모바일 앱과 클라우드 백엔드를 엮은 완전한 형태의 풀스택 IoT 웰니스 시스템을 완성하는 것을 목표로 합니다.

<br>

## 🛠 2. 기술 스택 (Tech Stack)

### 💻 Hardware & Edge AI
<img src="https://img.shields.io/badge/Arduino%20UNO%20R4%20WiFi-00979D?style=for-the-badge&logo=arduino&logoColor=white"> <img src="https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white">
- **Arduino UNO R4 WiFi & C++:** 파이썬에서 추출한 Edge AI 수식(가중치)을 하드코딩하여 딜레이 없이 실시간으로 계산하고, 내장된 블루투스 모듈로 스마트폰과 데이터를 주고받습니다.
- **MAX30102:** 빛의 반사를 이용해 혈류량과 산소포화도(SpO2) 원시 데이터를 수집하는 메인 광학 센서입니다.
- **TCS34725:** 피부에 빛을 쏘아 정밀한 RGB(빨강, 초록, 파랑) 비율을 읽어내어 AI의 피부톤 판별 재료로 사용되는 컬러 센서입니다.

### 🧠 AI & Data Modeling
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white"> <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white">
- **11K-Hands Dataset:** 피부톤(Skin Color) 라벨링이 완료된 11,000명의 손 이미지 오픈소스 데이터셋입니다.
- **Python & Scikit-learn:** 가벼운 '다중 선형 회귀' 알고리즘을 학습시켜, 아두이노 메모리에 들어갈 수 있는 최적의 오차 보정 공식(가중치)을 뽑아냅니다.

### 📱 Frontend (Android App)
<img src="https://img.shields.io/badge/Android%20Studio-3DDC84?style=for-the-badge&logo=androidstudio&logoColor=white"> <img src="https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=java&logoColor=white"> <img src="https://img.shields.io/badge/BLE%20%ED%86%B5%EC%8B%A0-0082FC?style=for-the-badge&logo=bluetooth&logoColor=white">
- **Android Native (Java):** 아두이노에서 보정한 산소포화도 수치를 블루투스(BLE)로 받아, 실시간 심장 애니메이션과 함께 직관적인 화면으로 띄워주는 모바일 앱입니다.
- **Retrofit2 통신:** 측정이 완전히 끝나면 스마트폰의 인터넷망을 이용해 최종 결과를 백엔드 서버로 안전하게 전송합니다.

### ⚙️ Backend & Database
<img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white"> <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white"> <img src="https://img.shields.io/badge/AWS%20EC2-FF9900?style=for-the-badge&logo=amazonec2&logoColor=white">
- **Spring Boot (REST API):** 안드로이드 앱에서 보내는 측정 데이터를 받아 무결성을 검사하는 전용 접수처(API) 서버입니다.
- **MySQL:** 수신된 건강 기록(심박수, 산소포화도)을 체계적이고 영구적으로 쌓아두는 데이터베이스입니다.
- **AWS EC2:** 시연 중에도 서버가 24시간 안정적으로 돌아갈 수 있도록 구축한 클라우드 가상 서버 환경입니다.

## 🏛 3. 시스템 아키텍처 (System Architecture)

전체 시스템은 **데이터 수집(하드웨어) ➔ 실시간 표출(안드로이드) ➔ 데이터 적재(스프링 서버)**의 3계층 파이프라인으로 구성되어 있습니다.

```mermaid
graph TD
    subgraph Edge_AI ["Edge AI (Arduino UNO R4)"]
        TCS34725[TCS34725 RGB 센서] -->|피부톤 RGB 측정| EdgeAI_Calc{C++ 하드코딩된<br/>가중치 수식 연산}
        EdgeAI_Calc -->|피부톤 보정 계수| Calc[SpO2/BPM 최종 연산]
        MAX30102[MAX30102 센서] -->|적외선/적색광| Calc
        Calc --> BLE[BLE 모듈 GATT]
    end

    subgraph Android_App ["Android App"]
        BLE -->|실시간 데이터 수신| App[측정 화면 UI]
        App -->|측정 완료| HTTP[Retrofit2 API 통신]
    end

    subgraph Spring_Boot ["Spring Boot Server"]
        HTTP -->|POST /api/measurements| API[REST API 접수처]
        API --> DB[(MySQL 기록 저장)]
    end
