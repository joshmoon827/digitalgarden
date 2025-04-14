---
{"dg-publish":true,"permalink":"/what-is-dynamo-db/"}
---


**DynamoDB**는 **AWS에서 제공하는 완전관리형 NoSQL 데이터베이스**입니다.

전통적인 SQL 기반 데이터베이스와 달리 **스키마가 유연하고**, **키-값 기반 또는 문서 기반**으로 데이터를 저장합니다.

**빠른 성능, 무제한 확장성, 서버리스 구조**를 특징으로 합니다.

---

## **🧱** 

## **DynamoDB의 구조**

|**구성 요소**|**설명**|
|---|---|
|테이블 (Table)|데이터를 담는 단위 (RDB의 테이블과 유사)|
|아이템 (Item)|테이블의 한 줄, 하나의 데이터 객체|
|속성 (Attribute)|키-값 쌍으로 이루어진 필드들|
|파티션 키 (Primary Key)|데이터를 고유하게 식별하는 키|
|정렬 키 (Sort Key, 옵션)|같은 파티션 키 내에서 정렬할 수 있는 키|

  

---

## **🚀** 

## **DynamoDB의 특징**

- **완전관리형**: 인프라, 백업, 복구 등 모두 AWS가 처리
    
- **서버리스**: 서버 설치나 유지보수가 필요 없음
    
- **낮은 지연 시간**: 밀리초 단위 응답 속도
    
- **자동 확장**: 사용량에 따라 읽기/쓰기 처리량 자동 확장
    
- **스키마 유연함**: 문서마다 필드가 달라도 저장 가능
    

---

## **🔄** 

## **MongoDB와 비교**

|**항목**|**DynamoDB**|**MongoDB**|
|---|---|---|
|**소속**|AWS 전용 서비스|오픈소스 / MongoDB Atlas (클라우드)|
|**데이터 모델**|Key-Value + Document|Document 기반 (BSON)|
|**스키마**|유연함 (Schema-less)|유연함 (Schema-less)|
|**확장성**|자동 수평 확장|Sharding으로 수평 확장 (수동 설정 필요)|
|**서버 관리**|필요 없음 (서버리스)|직접 관리 or Atlas 사용|
|**운영**|완전 관리형 (자동 백업, 확장, 보안)|자체 관리 or Atlas에서 일부 자동화|
|**성능**|빠르고 안정적 (초고속 읽기/쓰기)|쿼리 성능은 유연하나 튜닝 필요|
|**요금**|사용량 기반 과금 (On-demand 또는 고정 처리량)|사용한 인프라 기준 과금 (Atlas)|
|**제한사항**|복잡한 쿼리, JOIN 불가|쿼리 문법 풍부, 복잡한 연산 가능|
|**사용 사례**|실시간 게임, IoT, 서버리스 앱|CMS, 블로그, 분석 서비스 등|

  

---

## **📚** 

## **간단한 사용 예시**

  

### **🔸 DynamoDB**

```
// AWS SDK 사용 예시 (Node.js)
const params = {
  TableName: 'Users',
  Key: { userId: '123' }
};
const data = await dynamoDB.get(params).promise();
```

### **🔹 MongoDB**

```
// MongoDB Node.js 드라이버 사용 예시
const user = await db.collection('users').findOne({ userId: '123' });
```

  

---
