# 1️⃣ 전체 프로세스 흐름

## 📡 시스템 구조

<img width="2008" height="130" alt="mermaid-diagram (1)" src="https://github.com/user-attachments/assets/ee40c4fc-55db-4a6e-9c28-848b63834290" />


---

## 🔄 동작 흐름

1. 카메라 (USB / RTSP)
2. 영상 프레임 수집
3. AI 객체 탐지
4. 위험 조건 판단 (룰 기반)
5. GPIO 신호 출력
6. 릴레이 작동
7. 경광등 ON/OFF


---

## ⚙️ 핵심 로직

```pseudo
objects = detect(frame)

IF (person AND no helmet) → 위험
IF (person ∈ 위험구역) → 위험

→ 위험이면 경광등 ON
````

---

## 🔥 필수 설계 요소

* AI 단독 판단 ❌ → 룰 기반 결합 필수
* Fail-safe 설계 필수
  → AI 장애 시 경광등 ON
* 클라우드 의존 ❌ → 로컬(엣지) 처리

---

# 2️⃣ AI 모델 + 학습 전략

## 🧠 기본 모델

* YOLOv8

---

## 학습 프로세스
<img width="2290" height="130" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/a6fff79d-a1eb-423d-a7aa-a233926e06cb" />

---

## ⚠️ 현실 문제

* 기본 모델 그대로 사용 시 정확도 부족
* 현장 환경에 따라 성능 크게 변동

---

## 🎯 실무 추천 전략

### ✔️ 최소 커스터마이징 (필수 수준)

* 현장 이미지 200 ~ 1000장 수집
* 객체 라벨링 (person / helmet 등)
* YOLOv8 파인튜닝

---

## 📌 학습이 필요한 이유

현장 변수:

* 조명 (야간, 역광, 실내/외)
* 카메라 각도
* 작업복 색상
* 먼지 / 연기

👉 환경 차이로 인해
기본 모델만으로는 정확도 확보 어려움

---

## 🔥 핵심 요약

> 모델보다 중요한 것은 **현장 데이터 기반 학습**

---

# 3️⃣ 엣지 디바이스 + 비용

## 🥇 추천 디바이스

* NVIDIA Jetson Orin Nano Module

---

## 💰 비용 구조 (1세트 기준)

```
Jetson 모듈 + 보드     40 ~ 80만원
카메라               5 ~ 20만원
릴레이 + 경광등      5 ~ 15만원
--------------------------------
총 비용             약 50 ~ 100만원
```

---

## 📊 대안 디바이스

### 저가형

* Raspberry Pi 5
  → 테스트/PoC 용도

---

### 초소형

* Google Coral TPU Module
  → 저전력 / 소형 / 모델 제약 존재

---

## 🎯 최종 추천 구성

```
카메라
→ Jetson Orin Nano Module
→ YOLOv8 (현장 파인튜닝)
→ 릴레이
→ 경광등
```

---

# 🔥 최종 한 줄 정리

* 흐름: **카메라 → AI → 룰 → 물리제어**
* 모델: **YOLOv8 + 현장 데이터 학습**
* 비용: **약 50 ~ 100만원 / 1세트**

```
