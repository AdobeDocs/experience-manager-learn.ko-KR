---
title: MyAccountForm 만들기
description: 응용 프로그램 ID 및 전화 번호를 성공적으로 확인하면 부분적으로 완료된 양식을 검색하도록 myaccount 양식을 만듭니다.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 53
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# MyAccountForm 만들기

**MyAccountForm** 양식은 사용자가 응용 프로그램 ID 및 응용 프로그램 ID와 연결된 모바일 번호를 확인한 후 부분적으로 완료된 적응형 양식을 검색하는 데 사용됩니다.

![내 계정 양식](assets/6599.JPG)

사용자가 응용 프로그램 ID를 입력하고 **FetchApplication** 단추를 클릭하면 양식 데이터 모델의 Get 작업을 사용하여 데이터베이스에서 응용 프로그램 ID와 연결된 모바일 번호를 가져옵니다.

이 양식은 양식 데이터 모델의 POST 호출을 사용하여 OTP를 사용하여 모바일 번호를 확인합니다. 다음 코드를 사용하여 휴대폰 번호를 성공적으로 확인하면 양식의 제출 작업이 트리거됩니다. 이름이 **submitForm**&#x200B;인 제출 단추의 클릭 이벤트를 트리거하고 있습니다.

>[!NOTE]
> MyAccountForm의 해당 필드에 [Nexmo](https://dashboard.nexmo.com/) 계정과 관련된 API 키 및 API 암호 값을 제공해야 합니다.

![trigger-submit](assets/trigger-submit.JPG)



이 양식은 양식 제출을 **/bin/renderaf**&#x200B;에 탑재된 서블릿으로 전달하는 사용자 지정 제출 액션과 연결되어 있습니다.

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

**/bin/renderaf**&#x200B;에 탑재된 서블릿의 코드는 저장된 데이터로 미리 채워진 storeafwithattachments 적응형 양식을 렌더링하도록 요청을 전달합니다.


* MyAccountForm은 [여기에서 다운로드](assets/my-account-form.zip)할 수 있습니다.

* 샘플 양식은 샘플 양식을 올바르게 렌더링하기 위해 AEM으로 가져와야 하는 [사용자 지정 적응형 양식 템플릿](assets/custom-template-with-page-component.zip)을 기반으로 합니다.

* MyAccountForm 제출과 연결된 [사용자 지정 제출 처리기](assets/custom-submit-my-account-form.zip)를 AEM으로 가져와야 합니다.

## 다음 단계

[샘플 자산을 배포하여 솔루션 테스트](./deploy-this-sample.md)
