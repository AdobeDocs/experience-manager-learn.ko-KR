---
title: MyAccountForm 만들기
description: 애플리케이션 ID와 전화 번호를 성공적으로 확인하면 부분적으로 완료된 양식을 검색할 myaccount 양식을 만듭니다.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# MyAccountForm 만들기

양식 **MyAccountForm** 사용자가 애플리케이션 id 및 애플리케이션 id와 연결된 모바일 번호를 확인한 후 부분적으로 완료된 적응형 양식을 검색하는 데 사용됩니다.

![내 계정 양식](assets/6599.JPG)

사용자가 애플리케이션 ID를 입력하고 을 클릭하면 **FetchApplication** 단추. 양식 데이터 모델의 가져오기 작업을 사용하여 애플리케이션 id와 연결된 모바일 번호를 데이터베이스에서 가져옵니다.

이 양식은 양식 데이터 모델의 POST 호출을 사용하여 OTP를 사용하여 모바일 번호를 확인합니다. 양식의 제출 작업은 다음 코드를 사용하여 모바일 번호를 성공적으로 확인할 때 트리거됩니다. 이름이 인 제출 단추의 클릭 이벤트를 트리거합니다. **submitForm**.

>[!NOTE]
> 에만 해당하는 API 키 및 API 암호 값을 제공해야 합니다 [넥스모](https://dashboard.nexmo.com/) MyAccountForm의 해당 필드에 계정이 있습니다.

![trigger submit](assets/trigger-submit.JPG)



이 양식은 양식 제출을 위에 마운트된 서블릿에 전달하는 사용자 지정 제출 작업과 관련되어 있습니다 **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

에 마운트된 서블릿의 코드 **/bin/renderaf** 저장된 데이터로 미리 채워진 storeafwithattachments 적응형 양식을 렌더링하기 위한 요청을 전달합니다.


* MyAccountForm은 [여기에서 다운로드](assets/my-account-form.zip)

* 샘플 양식은 [사용자 지정 적응형 양식 템플릿](assets/custom-template-with-page-component.zip) 샘플 양식을 올바르게 렌더링하려면 AEM으로 가져와야 합니다.

* [사용자 지정 제출 처리기](assets/custom-submit-my-account-form.zip) myAccountForm 제출과 연결된 을 AEM으로 가져와야 합니다.

## 다음 단계

[샘플 자산을 배포하여 솔루션을 테스트합니다](./deploy-this-sample.md)
