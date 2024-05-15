---
title: 외부 서버에 적응형 양식 제출
description: 외부 서버에서 실행 중인 REST 끝점에 적응형 양식 제출
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 78
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 12%

---

# 외부 서버에 적응형 양식 제출 {#submitting-adaptive-form-to-external-server}

REST 끝점에 제출 액션을 사용하여 제출된 데이터를 REST URL에 게시합니다. URL은 내부 서버(양식이 렌더링되는 서버) 또는 외부 서버일 수 있습니다.

일반적으로 고객은 추가 처리를 위해 양식 데이터를 외부 서버에 제출하려고 합니다.

데이터를 내부 서버에 게시하려면 리소스의 경로를 제공합니다. 데이터는 리소스 경로에 게시됩니다. 예를 들어, &lt;/content restendpoint=&quot;&quot;> . 이러한 post 요청의 경우 제출 요청의 인증 정보가 사용됩니다.

데이터를 외부 서버에 게시하려면 URL을 제공합니다. URL 형식은 <http://host:port/path_to_rest_end_point>입니다. POST 요청을 익명으로 처리하도록 경로를 구성했는지 확인합니다.

이 문서에서는 tomcat 인스턴스에 배포할 수 있는 간단한 war 파일을 작성했습니다. tomcat이 포트 8080에서 실행 중인 경우 POST URL은 다음과 같습니다.

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

이 끝점에 제출하도록 적응형 양식을 구성할 때 양식 데이터 및 첨부 파일(있는 경우)은 다음 코드를 사용하여 서블릿에서 추출할 수 있습니다

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![formsubmission](assets/formsubmission.gif)
서버에서 테스트하려면 다음을 수행하십시오

1. 아직 Tomcat이 없는 경우 설치합니다. [tomcat 설치 지침은 여기에서 확인할 수 있습니다.](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. 다운로드 [zip 파일](assets/aemformsenablement.zip) 이 문서와 연결됩니다. war 파일을 가져오려면 파일의 압축을 풉니다.
1. tomcat 서버에 war 파일을 배포합니다.
1. 위의 스크린샷과 같이 파일 첨부 구성 요소를 사용하여 간단한 적응형 양식을 만들고 제출 액션을 구성합니다. POST URL은 <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. AEM 및 tomcat이 localhost에서 실행되지 않는 경우 그에 따라 URL을 변경하십시오.
1. tomcat에 다중 부분 양식 데이터 제출을 활성화하려면 다음 속성을 의 컨텍스트 요소에 추가하십시오. &lt;tomcatinstalldir>\conf\context.xml을 실행하고 Tomcat 서버를 다시 시작합니다.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. 적응형 양식을 미리 보고 첨부 파일을 추가하고 제출합니다. Tomcat 콘솔 창에서 메시지를 확인합니다.
