---
title: HTML5 양식 제출 처리
description: HTML5 양식 제출 처리기 만들기
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 1%

---


# HTML5 양식 제출 처리

AEM에서 호스팅되는 서블릿에 HTML5 양식을 제출할 수 있습니다. 제출된 데이터는 서블릿에서 입력 스트림으로 액세스할 수 있습니다. HTML5 양식을 제출하려면 AEM Forms 디자이너를 사용하여 양식 템플릿에 &quot;HTTP 제출 단추&quot;를 추가해야 합니다

## 제출 처리기 만들기

HTML5 양식 제출을 처리하기 위해 간단한 서블릿을 만들 수 있습니다. 그런 다음 다음 다음 코드를 사용하여 제출된 데이터를 추출할 수 있습니다. 이 [서블릿은](assets/html5-submit-handler.zip) 이 자습서의 일부로 사용할 수 있습니다. 패키지 관리자를 [사용하여](assets/html5-submit-handler.zip) 서블릿을 [설치하십시오.](http://localhost:4502/crx/packmgr/index.jsp)

라인 9의 코드는 J2EE 프로세스를 호출하는 데 사용할 수 있습니다. 코드를 사용하여 J2EE 프로세스를 호출하려는 경우 [Adobe LiveCycle 클라이언트](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) SDK 구성을 구성했는지 확인하십시오.

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## HTML5 양식의 전송 URL 구성

![submit-url](assets/submit-url.PNG)

* xdp를 누르고 _속성_->_고급을 클릭합니다_
* http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html을 복사하고 URL 제출 텍스트 필드에 붙여 넣습니다.
* 저장 _및 닫기_ 단추를 클릭합니다.

### 제외 경로에 항목 추가

* configMgr로 [이동합니다](http://localhost:4502/system/console/configMgr).
* [MOCK] Search for _Adobe Granite CSRF Filter_
* 제외된 경로 섹션에 다음 항목 추가
* _/content/AemFormsSamples/handlehtml5formsubmission_
* 변경 내용 저장

### 양식 테스트

* xdp 템플릿을 누릅니다.
* 미리 _보기_->HTML로 미리 보기를 클릭합니다.
* 양식에 데이터를 입력하고 제출을 클릭합니다.
* 서버의 stdout.log 파일에 기록된 전송된 데이터를 확인해야 합니다.

### 추가 읽기

HTML5 [양식 제출에서 PDF 생성에 대한 이](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) 문서도권장합니다.




