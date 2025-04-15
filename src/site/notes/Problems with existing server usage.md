---
{"dg-publish":true,"permalink":"/problems-with-existing-server-usage/"}
---

# ✅ Server Architecture Evolution (서버 아키텍처 진화)

![Pasted image 20250413014500.png](/img/user/Pasted%20image%2020250413014500.png)

___


### ✅ EC2 + Docker 기반 클라우드 서버 배포 순서

	1.	📦 GitHub
→ 코드 저장소로 사용. 개발한 코드를 올려둠.

	2.	💻 EC2 (Elastic Compute Cloud)
→ AWS에서 가상 머신을 생성. 여기서 서버가 돌아감.

	3.	🐳 Docker 설치 (EC2 내부)
→ EC2 인스턴스에 Docker 설치해서 컨테이너 환경 구성.

	4.	🛠️ Docker 이미지 빌드
→ Dockerfile로 이미지를 생성 (docker build)

	5.	☁️ DockerHub에 Push
→ 빌드된 이미지를 DockerHub에 업로드

	6.	📥 EC2에서 Docker 이미지 Pull
→ DockerHub에서 이미지 가져오기 (docker pull)
	7.	🚀 Docker 컨테이너 실행
→ 이미지를 기반으로 컨테이너를 실행 (docker run)

___

![Pasted image 20250415231740.png](/img/user/Pasted%20image%2020250415231740.png)

### ✅ Serverless Architecture 구성 요소

- **AWS Lambda**: 서버 없이 함수 단위로 실행
- **API Gateway**: HTTP 요청을 Lambda로 라우팅
- **DynamoDB**: 고성능 NoSQL DB
- **S3 (Simple Storage Service)**: 정적 파일 저장소 (이미지, HTML, PDF 등)



___


✅ EC2 vs Serverless 비교

| 항목    | EC2 기반                          | Serverless           |
| ----- | ------------------------------- | -------------------- |
| 서버 관리 | 직접 관리 필요                        | 필요 없음                |
| 비용    | 인스턴스 단위 과금 (24시간 켜져있으면 돈 계속 나감) | 요청량 기반 과금 (비용 절감 가능) |
| 확장성   | 수동 설정 필요                        | 자동 확장 지원             |
| 배포    | 복잡 (EC2 + Docker + GitHub 등)    | 간단 (코드만 Lambda에 업로드) |
| 상태 유지 | 가능 (Stateful)                   | Stateless            |
| 실행 속도 | 빠름                              | cold start 문제 가능     |


____


### ✅ Serverless의 이벤트 스트림 처리 장점
{ #event-stream}


서버리스 아키텍처는 **이벤트 기반 처리(event-driven)**에 아주 강함.

예시 상황:
	•	S3에 이미지 업로드됨 → Lambda가 자동 실행되어 썸네일 생성
	•	DynamoDB에 데이터 추가됨 → Lambda가 자동 실행되어 알림 발송

장점:
	•	⚡ 즉시 반응: 어떤 이벤트가 발생하면 바로 함수 실행 가능
	•	🤖 자동화 쉬움: 복잡한 로직 없이 자동 처리 가능
	•	💸 비용 절감: 이벤트가 발생할 때만 실행되므로 불필요한 리소스 낭비 없음
	•	🧩 구성 유연성: 마치 레고처럼 여러 이벤트와 함수 조합 가능 (이벤트 흐름으로 구성)
