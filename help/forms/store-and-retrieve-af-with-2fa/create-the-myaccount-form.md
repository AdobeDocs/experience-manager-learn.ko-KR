---
title: MyAccountForm 만들기
description: 응용 프로그램 ID와 전화 번호를 성공적으로 확인하면 부분적으로 완료된 양식을 검색할 myaccount 양식을 만듭니다.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# MyAccountForm 만들기

MyAccountForm **** 형식은 사용자가 응용 프로그램 ID 및 응용 프로그램 ID와 연결된 모바일 번호를 확인한 후 부분적으로 완료된 응용 양식을 검색하는 데 사용됩니다.

![내 계정 양식](assets/6599.JPG)

사용자가 응용 프로그램 ID를 입력하고 응용 프로그램 **가져오기** 단추를 클릭하면 양식 데이터 모델의 가져오기 작업을 사용하여 응용 프로그램 ID와 연결된 모바일 번호가 데이터베이스에서 가져옵니다.

이 양식에서는 양식 데이터 모델의 POST 호출을 사용하여 OTP를 사용하여 모바일 번호를 확인합니다. 다음 코드를 사용하여 모바일 번호를 성공적으로 확인하면 양식의 제출 작업이 트리거됩니다. submitForm이라는 제출 단추의 클릭 이벤트를 **트리거합니다**.

>[!NOTE]
> MyAccountForm의 해당 필드에 Nexmo [계정에](https://dashboard.nexmo.com/) 해당하는 API 키와 API 암호 값을 제공해야 합니다

![트리거 제출](assets/trigger-submit.JPG)



이 양식은 양식 제출을 **/bin/renderaf에 마운트된 서블릿으로 전달하는 사용자 지정 제출 작업과 연관되어 있습니다**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

/bin/renderaf에 마운트된 서블릿의 코드는 저장된 데이터로 미리 채워진 저장된 저장 **첨부 파일** 적응형 양식을 렌더링하도록 요청을 전달합니다.


* MyAccountForm은 여기에서 [다운로드할 수 있습니다.](assets/my-account-form.zip)

* 샘플 양식은 샘플 양식이 올바로 렌더링되도록 AEM으로 가져와야 하는 [사용자 정의 적응형 양식 템플릿을](assets/custom-template-with-page-component.zip) 기반으로 합니다.

* [MyAccountForm 제출과 연결된 사용자 지정 제출 핸들러를](assets/custom-submit-my-account-form.zip) AEM으로 가져와야 합니다.
