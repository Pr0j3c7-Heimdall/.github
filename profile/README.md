<div align="center">

<img width="88" height="88" alt="Heimdall Logo" src="https://github.com/user-attachments/assets/527a9ae5-d462-40c6-913c-4d4da802b2df" />

# Heimdall

### 자체 연구 프레임워크 기반 이미지·음성 AI 생성 콘텐츠 AIGC 판별 웹서비스 개발

이미지·음성의 **AI 생성 여부와 판정 근거**를 제공하는 오픈소스 검증 도구로, C2PA 출처 검증·다중 딥러닝 탐지·생성 모델 추정·메타데이터 분석을 결합한 자체 프레임워크를 사용합니다. 110만 장 규모의 이미지 데이터셋과 콘텐츠 유형별 분석 경로를 갖춘 음성 프레임워크로 재검토 가능한 분석 결과를 제공합니다.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-heimdall.ai.kr-4C8BF5?style=flat-square&logo=googlechrome&logoColor=white)](https://heimdall.ai.kr)
&nbsp;
[![GitHub Org](https://img.shields.io/badge/GitHub-Pr0j3c7--Heimdall-181717?style=flat-square&logo=github)](https://github.com/Pr0j3c7-Heimdall)
&nbsp;
![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-Uvicorn-009688?style=flat-square&logo=fastapi&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-15-000000?style=flat-square&logo=next.js&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)

</div>

<br>

## 📑 Table of Contents

- [Team](#-team)
- [Repositories](#-repositories)
- [Verification Framework](#-verification-framework)
- [Infra & Tech Stack](#️-infra--tech-stack)

<br>

## 👥 Team

| 역할 | 이름 | 담당 |
|:---:|:---:|:---|
| **Project Manager** | 최원혁 | 프로젝트 총괄 |
| Project Member | 김예은 | `Detection Tool Development` |
| Project Member | 이재용 | `Detection Tool Development` |
| Project Member | 박종범 | `AI Product Research` |
| Project Member | 송자운 | `AI Product Research` |

<br>

## 📦 Repositories

| Repository | 설명 |
|:---|:---|
| 🖥️ [**heimdall-frontend**](https://github.com/Pr0j3c7-Heimdall/heimdall-frontend) | Next.js 15 + React 19 기반 웹 프론트엔드. 이미지/음성 업로드, 검증 결과 조회, 마이페이지, 다크모드 지원 |
| ⚙️ [**heimdall-backend**](https://github.com/Pr0j3c7-Heimdall/heimdall-backend) | FastAPI 백엔드 서버. 이미지(C2PA·이진분류·다중분류)와 음성(YAMNet·이진분류·Fusion) 검증 파이프라인 및 REST API 제공 |
| 🖼️ [**heimdall-image**](https://github.com/Pr0j3c7-Heimdall/heimdall-image) | 이미지 AI 생성물 판별 모델 (DINOv3, F3Net, U-Net) 학습 및 앙상블 리포지토리 |
| 🔊 [**heimdall-sound**](https://github.com/Pr0j3c7-Heimdall/heimdall-sound) | 음성 AI 생성물 판별 모델 앙상블 (AASIST, LCNN, RawNet3, SSL-AASIST, CQCC-SSL-AASIST) 학습 리포지토리 |
| 🐳 [**heimdall-infra**](https://github.com/Pr0j3c7-Heimdall/heimdall-infra) | Docker Compose 기반 인프라 통합 관리 및 GitHub Actions CI/CD 배포 |

<br>

## 🔎 Verification Framework

### 🖼️ 이미지 판별

**C2PA**
- `c2patool`을 사용해 Manifest에 포함된 서명/해시 기반 검증으로 "기록 신뢰 가능 여부" 판별
- Manifest에 남아 있는 생성 도구/플랫폼 정보 및 관련 내부 값을 해석 후 구조화하여 저장
- 서명·바인딩·신뢰 검증을 모두 통과하고 AI 생성이 선언된 경우, 이진분류를 생략하고 AI로 확정 판정(확률 1.0)한 뒤 바로 다중분류로 진행
- 그 외(C2PA가 없거나 불완전/검증 실패)에는 이진분류부터 수행

**이진분류 (Real vs AI-Generated)**
- **DINOv3 + MLP** · **F3Net** · **U-Net**
- 검증 정확도 기반 고정 가중치(DINOv3 34.95% · F3-Net 46.28% · UNet 18.77%)로 Soft Voting 결합한 확률이 0.5 이상이면 AI로 판정

**다중분류 (생성 모델 추정)**
- **DINOv3 + MLP** · **F3Net** · **U-Net**
- 3개 모델 중 confidence score가 가장 높은 단일 모델의 예측을 최종 생성 모델로 채택

**메타데이터**
- `exiftool`을 사용해 촬영 기기/소프트웨어/저장 이력 관련 필드를 정리하여 구조화 후 저장

<br>

### 🔊 음성 판별

- **C2PA**: 이미지와 동일하게, 검증을 통과하고 AI 생성이 선언된 경우 모델 판별을 생략하고 AI로 확정 판정(확률 1.0)
- **YAMNet**: 그 외의 경우 YAMNet으로 음성(Speech) / 가창(Singing) / 예외를 분류 — 신뢰도·마진 조건을 만족해야 확정, 아니면 서비스 불가. 긴 파일은 15초 단위로 잘라 구간별 판정(비보컬 구간 희석 방지)
- **음성(Speech)**: SSL-AASIST · RawNet3 · CQCC-SSL-AASIST 3개 모델의 점수를 Platt Scaling으로 보정 후 Soft Voting으로 결합
- **가창(Singing)**: AASIST · RawNet3 · LCNN 3개 모델의 점수를 Platt Scaling으로 보정 후 Simple Mean으로 결합
- **메타데이터**: 실제 음성 파일의 메타데이터를 추출해 구조화 후 저장

<br>

## 🛠️ Infra & Tech Stack

| 구분 | 내용 |
|:---|:---|
| **Server** | FastAPI + Uvicorn, Python 3.12 (Docker: `python:3.12-slim-trixie`) |
| **Database** | MySQL 8.0 (SQLAlchemy Async ORM + aiomysql) |
| **Cache/Session** | Redis 7 (리프레시 토큰 저장 및 액세스 토큰 블랙리스트) |
| **AI Inference** | PyTorch · torchvision · transformers · timm · diffusers (이미지) · spafe · asteroid-filterbanks · ai-edge-litert(YAMNet) (음성) |
| **FE** | Next.js 15 + React 19, Node.js 20 (JavaScript/JSX) |

<br>

---

<div align="center">

모델 리포지토리(`heimdall-image`, `heimdall-sound`)는 <a href="https://github.com/Pr0j3c7-Heimdall/heimdall-image/blob/main/LICENSE">Apache License 2.0</a>을 따릅니다.

**Contact:** [Pr0j3c7-Heimdall](https://github.com/Pr0j3c7-Heimdall) · [Live Demo](https://heimdall.ai.kr)

</div>

