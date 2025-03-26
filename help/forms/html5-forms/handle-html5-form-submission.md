---
title: HTML5 양식 제출 처리
description: HTML5 양식 제출 처리기를 만듭니다.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# HTML5 양식 제출 처리

HTML5 forms는 AEM에서 호스팅하는 서블릿에 제출할 수 있습니다. 제출된 데이터는 서블릿에서 입력 스트림으로 액세스할 수 있습니다. HTML5 양식을 제출하려면 AEM Forms Designer을 사용하여 양식 템플릿에 &quot;HTTP 제출 단추&quot;를 추가합니다.

## 제출 핸들러 만들기

간단한 서블릿은 HTML5 양식 제출을 처리할 수 있습니다. 다음 코드 조각을 사용하여 제출된 데이터를 추출합니다. 이 자습서에서 제공한 [서블릿](assets/html5-submit-handler.zip)을(를) 다운로드합니다. [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 [서블릿](assets/html5-submit-handler.zip)을(를) 설치합니다.

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

코드를 사용하여 J2EE 프로세스를 호출하려는 경우 [Adobe LiveCycle Client SDK 구성](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html)을 구성했는지 확인하십시오.

## HTML5 양식의 제출 URL 구성

![URL 제출](assets/submit-url.PNG)

- xdp를 열고 _속성_->_고급_&#x200B;으로 이동합니다.
- http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html 을 복사하여 제출 URL 텍스트 필드에 붙여넣습니다.
- _SaveAndClose_ 단추를 클릭합니다.

### 제외 경로에 항목 추가

- [configMgr](http://localhost:4502/system/console/configMgr)&#x200B;(으)로 이동합니다.
- _Adobe Granite CSRF 필터_&#x200B;를 검색합니다.
- 제외된 경로 섹션에 _/content/AemFormsSamples/handlehml5formsubmission_ 항목을 추가합니다.
- 변경 사항을 저장합니다.

### 양식 테스트

- xdp 템플릿을 엽니다.
- _미리 보기_->HTML으로 미리 보기를 클릭합니다.
- 양식에 데이터를 입력하고 제출을 클릭합니다.
- 서버의 stdout.log 파일에서 제출된 데이터를 확인합니다.

### 추가 읽기

HTML5 양식 제출을 통해 PDF를 생성하는 방법에 대한 자세한 내용은 이 [문서](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html)를 참조하십시오.

