# <img width="40" height="40" alt="Heimdall_Logo (5)" src="https://github.com/user-attachments/assets/527a9ae5-d462-40c6-913c-4d4da802b2df" /> Heimdall  

## Introduction  
**Team Name:** Team.Heimdall  
**Project Title:** 멀티미디어 기반의 AI 생성물 검증 기법 연구 및 탐지 도구 개발   
**Project Output Tool Name:** Heimdall `(Draft Name)`     
**Paper:** 멀티미디어 기반 AI 생성물 검증 기법 연구: 생성물의 부정 사용 방지 전략   
**Host Organization:** 성공회대학교 우로장학회   

## Team Composition   
**Advisor:** 이종현   
**Project Manager:** 최원혁   
**Project Member:** 김예은 / `Detection Tool Development`      
**Project Member:** 박종범 / `AI Product Research`  
**Project Member:** 송자운 / `AI Product Research`  
**Project Member:** 이재용 / `Detection Tool Development`  

## Web Serivce Skills 
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

### Infra
- **Server:** FastAPI + Uvicorn   
- **Database:** MySQL (SQLAlchemy Async ORM)   
- **CI/CD:** GitHub Actions (Self-hosted Runner)   
- **Deployment:** Linux (Ubuntu) 기반 배포 환경   
- **AI Inference:** PyTorch 기반 딥러닝 모델 파이프라인   

### Tech Stacks
**BE**
- Python 3.10+   
- SQL (MySQL)

**FE**   
- JavaScript (Next.js, React)
