---
{"dg-publish":true,"permalink":"/webhook/"}
---

<div style="display: flex; gap: 20px;">
  <div style="flex: 1;">
    ```mermaid
    graph TD
    A[Start] --> B[Process 1]
    B --> C[Process 2]
    C --> D[End]
    ```
  </div>
  <div style="flex: 1;">
    ## 여기에 추가 내용
    Mermaid 다이어그램을 왼쪽 컬럼에 배치하고, 오른쪽에는 다른 내용을 넣을 수 있습니다.
  </div>
</div>



```mermaid
graph TD
  A[webhookHandler DELETE] --> B[handleDeleteRequest]
  B --> C[parseJsonBody]
  B --> D[isMemberAuthorizedToDelete]
  D --> E[getMemberWebhookParams]
  E --> F[GetItemCommand to check ownership]
  F --> G{Record exists?}
  G -- No --> H[Return 403 FORBIDDEN]
  G -- Yes --> I[deleteWebhook]
  I --> J[getMemberWebhookParams]
  J --> K[GetItemCommand to get item]
  K --> L{webhook_id exists?}
  L -- No --> M[Throw Error]
  L -- Yes --> N[DeleteItemCommand]
  N --> O[saveWebhookLog]
  O --> P[PutItemCommand to log deletion]
```
```mermaid
flowchart TD
Start --> Stop
id1[(Database)]
id1(((Some text)))

```

