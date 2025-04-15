---
{"dg-publish":true,"permalink":"/event-stream/"}
---

[[Problems with existing server usage#^event-stream\|Problems with existing server usage#^event-stream]]
에 파생된 내용 실제 어떻게 쓰이는지 코드 / 로직으로 알아보자

### **🔥 예시 1:** 

### **DynamoDB Streams** **+**  **Lambda**

  

이 예시는 DynamoDB에 데이터가 추가되면, **Lambda**가 트리거되고 그 데이터를 처리하는 구조입니다.

  

#### **1️⃣ DynamoDB 테이블에 Stream 활성화하기**

  

DynamoDB 테이블을 만들 때 **Stream**을 활성화해야합니다.

```
aws dynamodb update-table \
    --table-name YourTableName \
    --stream-specification StreamEnabled=true,StreamViewType=NEW_IMAGE
```

#### **2️⃣ Lambda 함수 코드 (Node.js)**

  

DynamoDB 스트림에서 발생한 이벤트를 처리하는 Lambda 함수 코드는 다음과 같습니다:

```
exports.handler = async (event) => {
    console.log('Received event:', JSON.stringify(event, null, 2));

    for (const record of event.Records) {
        // 각 레코드에 대해 이벤트 처리
        if (record.eventName == 'INSERT') {
            const newImage = record.dynamodb.NewImage;
            console.log('New item inserted:', newImage);
            // 추가적인 처리 (예: S3 업로드, 외부 API 호출 등)
        }
    }

    return `Successfully processed ${event.Records.length} records.`;
};
```

#### **3️⃣ Lambda와 DynamoDB Stream 연결**

  

Lambda 함수를 DynamoDB Streams와 연결하려면, **Event Source Mapping**을 사용해 연결해야합니다.

```
aws lambda create-event-source-mapping \
    --function-name YourLambdaFunctionName \
    --event-source-arn arn:aws:dynamodb:region:account-id:table/YourTableName/stream/stream-arn \
    --batch-size 5
```

이 설정이 완료되면, DynamoDB 테이블에 **새 데이터가 추가될 때마다 Lambda 함수가 자동으로 호출**됩니다.

---

### **🔥 예시 2:** 

### **Kinesis Stream**  **+** **Lambda**


#### **1️⃣ Kinesis Stream 생성**

```
aws kinesis create-stream --stream-name YourStreamName --shard-count 1
```

#### **2️⃣ Lambda 함수 코드 (Node.js)**

```
exports.handler = async (event) => {
    console.log('Received event:', JSON.stringify(event, null, 2));

    // 스트림에서 데이터를 읽어서 처리
    for (const record of event.Records) {
        const payload = Buffer.from(record.kinesis.data, 'base64').toString('utf-8');
        console.log('Decoded payload:', payload);
        // 데이터 처리 (예: DB 저장, 알림 전송 등)
    }

    return `Successfully processed ${event.Records.length} records.`;
};
```

#### **3️⃣ Lambda와 Kinesis Stream 연결**

  

Kinesis 스트림을 Lambda와 연결하려면 Event Source Mapping을 사용:

```
aws lambda create-event-source-mapping \
    --function-name YourLambdaFunctionName \
    --event-source-arn arn:aws:kinesis:region:account-id:stream/YourStreamName \
    --batch-size 5
```

  

---

### **🎯 요약: 이벤트 스트림 처리 장점**

- **실시간 데이터 처리**: 실시간으로 발생하는 이벤트를 Lambda 함수가 처리하게 해줌.

- **자동화**: 데이터 흐름에 따라 자동으로 동작함.

- **비용 효율성**: 트리거될 때만 실행되므로 비용이 절감됨.
