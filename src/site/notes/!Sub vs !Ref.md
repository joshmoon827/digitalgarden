---
{"dg-publish":true,"permalink":"/sub-vs-ref/"}
---

![Pasted image 20250428135232.png](/img/user/Pasted%20image%2020250428135232.png)

기존 보안정보들은 하드코딩되어 yaml에 배포 정보로 넘겨 졌었다.

이를 해결하기 위해 aws 파라미터 스토어에 환경변수를 등록해 가져오는 식의 로직으로
보안정보들을 마이그래이션 하는 일을 맡았다.

![Pasted image 20250428135618.png](/img/user/Pasted%20image%2020250428135618.png)
aws 콘솔을 이용해 ssm:PutParameter 권한을 얻고 파라미터 생성하고,

## !Sub vs !Ref

### !Sub
![Pasted image 20250428142640.png](/img/user/Pasted%20image%2020250428142640.png)
!Sub{{ssm을 포함한 환경변수 이름 (파라미터 경로)}} 를 통해 접근 후 값을 가져온다.

### !Ref
![Pasted image 20250428141044.png](/img/user/Pasted%20image%2020250428141044.png)
Resources 에서 각각의 환경변수의 타입을 지정하고, 환경변수 이름 (파라미터 경로)를 불러온다.
!Ref 로 Resources에서 지정한 환경변수들 값들을 불러와 파라미터를 수정했다.



### **왜 `!Sub`에서 문제가 발생했을까?**

	확실하진 않지만 , 2가지 가능성을 추론 할 수 있었다.

1. **`!Sub` 사용 시** SSM 파라미터를 **직접** 경로로 가져오게 되는데, 이 방식은 **Lambda 함수에서 SSM 파라미터를 읽는 권한이 제대로 설정되지 않으면** 문제가 발생할 수 있음.
    
2. **IAM 권한 차이**
    
    - **`!Ref` 방식**에서는 CloudFormation 내에서 SSM 파라미터 리소스를 **명시적으로 참조**하는 방식이라, **CloudFormation**이 자동으로 권한을 확인하고 적용할 수 있음.
        
    - 반면 **`!Sub`** 방식에서는 파라미터 경로를 **직접 호출**하므로, **IAM 역할**에서 **`ssm:GetParameter`** 권한이 제대로 설정되어 있어야만 값을 가져올 수 있음.

### **왜 `!Ref`에서는 잘 동작했을까?**

- **`!Ref`**를 사용할 때는 CloudFormation이 **리소스를 명시적으로 정의하고** 그것에 대한 권한을 할당하기 때문에, **IAM 역할**에 **`ssm:GetParameter`** 권한이 제대로 설정되면 문제가 없이 값이 읽혀.
