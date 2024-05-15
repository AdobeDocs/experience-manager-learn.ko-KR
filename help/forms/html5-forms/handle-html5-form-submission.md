---
title: HTML5 양식 제출 처리
description: HTML5 양식 제출 처리기 만들기
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# HTML5 양식 제출 처리

HTML5 양식은 AEM에서 호스팅하는 서블릿에 제출할 수 있습니다. 제출된 데이터는 서블릿에서 입력 스트림으로 액세스할 수 있습니다. HTML5 양식을 제출하려면 AEM Forms Designer를 사용하여 양식 템플릿에 &quot;HTTP 제출 단추&quot;를 추가해야 합니다

## 제출 핸들러 만들기

HTML5 양식 제출을 처리하는 간단한 서블릿을 만들 수 있습니다. 그런 다음 다음 다음 코드를 사용하여 제출된 데이터를 추출할 수 있습니다. 이 [서블릿](assets/html5-submit-handler.zip) 은 이 자습서의 일부로 사용할 수 있습니다. 다음을 설치하십시오. [서블릿](assets/html5-submit-handler.zip) 사용 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)

라인 9의 코드는 J2EE 프로세스를 호출하는 데 사용할 수 있습니다. 다음을 구성했는지 확인하십시오. [Adobe LiveCycle 클라이언트 SDK 구성](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) 코드를 사용하여 J2EE 프로세스를 호출하려는 경우

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

* xdp를 탭하고 _속성_->_고급_
* http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html 을 복사하여 제출 URL 텍스트 필드에 붙여넣습니다.
* 클릭 _저장 및 닫기_ 단추를 클릭합니다.

### 제외 경로에 항목 추가

* 다음으로 이동 [configMgr](http://localhost:4502/system/console/configMgr).
* 검색 대상 _Adobe Granite CSRF 필터_
* 제외된 경로 섹션에 다음 항목을 추가합니다
* _/content/AemFormsSamples/handlehml5formsubmission_
* 변경 사항 저장

### 양식 테스트

* xdp 템플릿을 탭합니다.
* 클릭 _미리 보기_->HTML으로 미리 보기
* 양식에 데이터를 입력하고 제출을 클릭합니다
* 제출된 데이터가 서버의 stdout.log 파일에 기록되는 것을 볼 수 있습니다

### 추가 읽기

이 [기사](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) HTML5 양식 제출로 PDF을 생성할 때도 권장됩니다.
