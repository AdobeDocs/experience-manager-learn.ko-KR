---
title: 외부 서버에 적응형 양식 제출
seo-title: 외부 서버에 적응형 양식 제출
description: 외부 서버에서 실행 중인 REST 끝점에 적응형 양식 제출
seo-description: 외부 서버에서 실행 중인 REST 끝점에 적응형 양식 제출
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: 적응형 양식
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---


# 외부 서버에 적응형 양식 제출 {#submitting-adaptive-form-to-external-server}

REST 엔드포인트에 제출 작업을 사용하여 제출된 데이터를 REST URL에 게시합니다. URL은 내부(양식이 렌더링되는 서버) 또는 외부 서버일 수 있습니다.

일반적으로 고객은 추가 처리를 위해 외부 서버에 양식 데이터를 제출하려고 합니다.

내부 서버에 데이터를 게시하려면 리소스의 경로를 제공합니다. 데이터는 리소스의 경로를 게시합니다. 예: &lt;/content/restEndPoint> . 이러한 post 요청의 경우 제출 요청의 인증 정보가 사용됩니다.

외부 서버에 데이터를 게시하려면 URL을 제공합니다. URL의 형식은 <http://host:port/path_to_rest_end_point>입니다. POST 요청을 익명으로 처리할 경로를 구성했는지 확인합니다.

이 문서를 위해, 나는 당신의 tomcat 인스턴스에 배포할 수 있는 간단한 전쟁 파일을 작성했습니다. tomcat이 포트 8080에서 실행 중이라고 가정할 경우 POST URL은

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

이 종단점에 제출하도록 적응형 양식을 구성할 때 다음 코드를 통해 서블릿에서 양식 데이터와 첨부 파일을 추출할 수 있는 경우

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

![](assets/formsubmission.gif)
양식 제출서버에서 테스트하려면 다음을 수행하십시오

1. Tomcat이 없는 경우 설치합니다. [tomcat 설치 지침은 여기에서 확인할 수 있습니다.](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. 이 문서와 연결된 [zip 파일](assets/aemformsenablement.zip)을 다운로드하십시오. 전쟁 파일을 가져올 파일의 압축을 해제합니다.
1. tomcat 서버에 war 파일을 배포합니다.
1. 첨부 파일 구성 요소로 간단한 적응형 양식을 만들고 위의 스크린샷에 표시된 대로 해당 제출 작업을 구성합니다. POST URL은 <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>입니다. AEM 및 tomcat이 localhost에서 실행되고 있지 않은 경우 그에 따라 URL을 변경하십시오.
1. tomcat에 다중 부분 양식 데이터 제출을 사용하려면 &lt;tomcatInstallDir>\conf\context.xml의 컨텍스트 요소에 다음 속성을 추가하고 Tomcat 서버를 다시 시작하십시오.
1. **&lt;context allowCasualMultipartParsing=&quot;true&quot;>**
1. 적응형 양식을 미리 보고, 첨부 파일을 추가하고, 제출합니다. tomcat 콘솔 창에서 메시지를 확인합니다.

