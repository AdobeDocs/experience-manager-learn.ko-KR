---
title: HTML5 양식 제출 처리
description: HTML5 양식 제출 핸들러 만들기
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 4%

---


# HTML5 양식 제출 처리

HTML5 양식을 AEM에서 호스팅되는 서블릿에 제출할 수 있습니다. 제출된 데이터는 서블릿에서 입력 스트림으로 액세스할 수 있습니다. HTML5 양식을 제출하려면 AEM Forms 디자이너를 사용하여 양식 템플릿에 &quot;HTTP 전송 단추&quot;를 추가해야 합니다

## 제출 처리기 만들기

HTML5 양식 제출을 처리하기 위해 간단한 서블릿을 만들 수 있습니다. 그런 다음 다음 다음 코드를 사용하여 제출된 데이터를 추출할 수 있습니다. 이 [서블릿](assets/html5-submit-handler.zip)을 이 자습서의 일부로 사용할 수 있습니다. [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 [서블릿](assets/html5-submit-handler.zip)을 설치하십시오

라인 9의 코드는 J2EE 프로세스를 호출하는 데 사용할 수 있습니다. 코드를 사용하여 J2EE 프로세스를 호출하려면 [Adobe LiveCycle 클라이언트 SDK 구성](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html)을 구성했는지 확인하십시오.

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


## HTML5 양식의 제출 URL 구성

![submit-url](assets/submit-url.PNG)

* xdp를 탭하고 _속성_->_고급_&#x200B;을 클릭합니다
* http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html 을 복사하여 URL 제출 텍스트 필드에 붙여넣습니다.
* _SaveAndClose_ 단추를 클릭합니다.

### 제외 경로에 항목 추가

* [configMgr](http://localhost:4502/system/console/configMgr)로 이동합니다.
* _Granite CSRF 필터 Adobe_
* 제외된 경로 섹션에 다음 항목을 추가합니다
* _/content/AemFormsSamples/handlehml5formsubmission_
* 변경 내용을 저장합니다

### 양식 테스트

* xdp 템플릿을 누릅니다.
* _미리 보기_->HTML로 미리 보기를 클릭합니다
* 양식에 데이터를 입력하고 제출을 클릭합니다
* 서버의 stdout.log 파일에 기록된 제출된 데이터가 표시됩니다

### 추가 읽기

HTML5 양식 제출에서 PDF 생성 시 이 [article](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html)도 권장됩니다.




