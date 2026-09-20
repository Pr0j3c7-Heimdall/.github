# <img width="40" height="40" alt="Heimdall_Logo (5)" src="https://github.com/user-attachments/assets/527a9ae5-d462-40c6-913c-4d4da802b2df" /> Heimdall  

## Introduction  
**Team Name:** Heimdall  
**Project Title:** 멀티미디어 기반의 AI 생성물 검증 기법 연구 및 탐지 도구 개발   
**Web Service Name:** Heimdall       
**Host Organization:** 성공회대학교 우로장학회   
**Note:** 재단법인 성공회대학교 우로장학회의 학생연구프로젝트 지원사업에 선정되어, 장학 지원을 받아 수행된 프로젝트   

## Team Composition   
**Advisor:** 이종현   
**Project Manager:** 최원혁   
**Project Member:** 김예은 / `Detection Tool Development`      
**Project Member:** 박종범 / `AI Product Research`  
**Project Member:** 송자운 / `AI Product Research`  
**Project Member:** 이재용 / `Detection Tool Development`  

## Web Service Skills 
### Image Detection Framework    
**C2PA**
- c2patool 사용
- Manifest에 포함된 서명/해시 기반 검증을 통해 "기록 신뢰 가능 여부" 검증
- Manifest에 남아 있는 생성 도구/플랫폼 정보 및 관련 내부 값을 해석 후 구조화해 저장
- C2PA가 없거나, 불완전/검증 실패 시 다음 단계인 이진분류를 수행 

**이진분류**
- DINOv3 + MLP
- F3Net
- U-Net
- Softmax 기반 앙상블 가중치(각 모듈의 출력 점수를 Softmax로 가중치화한 뒤 가중 결합하여 최종 AI/Real 판정)

**다중분류**
- DINOv3 + MLP
- F3Net
- U-Net
- 모듈 별 점수를 합산/통합하여 모델 후보 별 총점 계산 후, 최고 총점 모델 후보를 생성 모델로 추정

**메타데이터**
- exiftool 사용
- 촬영 기기/소프트웨어/저장 이력 관련 필드를 정리해 구조화 후 저장

<br>

### Audio Detection Framework  
**C2PA**
- c2patool 사용
- Manifest에 포함된 서명/해시 기반 검증을 통해 "기록 신뢰 가능 여부" 검증
- Manifest에 남아 있는 생성 도구/플랫폼 정보 및 관련 내부 값을 해석 후 구조화해 저장
- C2PA가 없거나, 불완전/검증 실패 시 다음 단계인 음성 파일 유형 분류를 수행 

**음성 파일 유형 분석**
- Yamnet(하기 3개의 조건으로 일반 음성, 가창, 예외 판별)
  - 최고 점수 ≥ 최소 신뢰도
  - 최고 점수 - 차순위 점수 ≥ 최소 Margin
  - 차순위 - 삼순위 ≥ 최소 Margin
- 예외 판정은 서비스 불가

**음성 이진분류**
- SSL-AASIST
- RawNet3
- CQCC+SSL+AASIST
- Softmax 기반 앙상블 가중치(각 모듈의 출력 점수를 Softmax로 가중치화한 뒤 가중 결합하여 최종 AI/Real 판정)

**가창 이분류**
- Demucs
  - 가창 파일의 배경음이 높을 경우, Demucs를 통해 가창음/배경음 분리
  - 가창 파일의 배경음이 낮을 경우, Demucs 미처리
    - Demucs 처리가 진행될 경우(원본 및 Demucs 처리 파일 모두에 대한 이진분류 진행)
    - Demucs 처리 안할 경우(원본 파일만 진행)
- AASIST
- RawNet3
- LCNN
- Softmax 기반 앙상블 가중치(각 모듈의 출력 점수를 Softmax로 가중치화한 뒤 가중 결합하여 최종 AI/Real 판정)

**메타데이터**
- exiftool 사용
- 녹음 기기/소프트웨어/저장 이력 관련 필드를 정리해 구조화 후 저장

<br>

### Infra
- **Server:** FastAPI + Uvicorn   
- **Database:** MySQL (SQLAlchemy Async ORM)   
- **CI/CD:** GitHub Actions (Self-hosted Runner)   
- **Deployment:** Linux (Ubuntu) 기반 배포 환경   
- **AI Inference:** PyTorch 기반 딥러닝 모델 파이프라인   

<br>

### Tech Stacks
**BE**
- Python 3.10+   
- SQL (MySQL)

**FE**   
- JavaScript (Next.js, React)
