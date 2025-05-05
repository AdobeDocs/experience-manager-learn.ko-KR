---
title: 맞춤형 제출 서비스를 만들어 Headless 적응형 양식 제출 처리
description: 제출된 데이터를 기반으로 사용자 지정 응답 반환
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: c23275d7-daf7-4a42-83b6-4d04b297c470
duration: 115
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 사용자 정의 제출 만들기

AEM Forms에서는 대부분의 사용 사례를 충족하는 다양한 제출 옵션을 제공합니다.이러한 사전 정의된 제출 작업 외에도 AEM Forms에서는 사용자 정의 제출 핸들러를 작성하여 필요에 따라 양식 제출을 처리할 수 있습니다.

사용자 정의 제출 서비스를 작성하려면 다음 단계를 따르십시오

## AEM 프로젝트 만들기

기존 AEM Forms as a Cloud Service 프로젝트가 있는 경우 [사용자 지정 제출 서비스로 이동](#Write-the-custom-submit-service)할 수 있습니다.

* c 드라이브에 cloudmanager라는 폴더를 만듭니다.
* 새로 만든 이 폴더로 이동
* 명령 프롬프트 창에서 [이 텍스트 파일](./assets/creating-maven-project.txt)의 내용을 복사하여 붙여 넣으십시오. [최신 버전](https://github.com/adobe/aem-project-archetype/releases)에 따라 DarchetypeVersion=41을 변경해야 할 수 있습니다. 이 글을 쓸 당시 최신판은 41개였다.
* Enter 키를 눌러 명령을 실행합니다.모든 내용이 올바르게 실행되면 빌드 성공 메시지가 표시됩니다.

## 사용자 정의 제출 서비스 작성{#Write-the-custom-submit-service}

IntelliJ를 시작하고 AEM 프로젝트를 엽니다. 아래 스크린샷과 같이 **HandleRegistrationFormSubmission**&#x200B;이라는 새 Java 클래스를 만듭니다.
![사용자 지정 제출 서비스](./assets/custom-submit-service.png)

다음 코드는 서비스를 구현하기 위해 작성되었습니다

```java
package com.aem.bankingapplication.core;
import java.util.HashMap;
import java.util.Map;
import com.google.gson.Gson;
import org.osgi.service.component.annotations.Component;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.google.gson.JsonObject;
import org.slf4j.*;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class HandleRegistrationFormSubmission implements FormSubmitActionService {
    private static final String serviceName = "Core Custom AF Submit";
    private static Logger logger = LoggerFactory.getLogger(HandleRegistrationFormSubmission.class);



    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        logger.error("in my custom submit service");
        Map<String, Object> result = new HashMap<>();
        logger.error("in my custom submit service");
        String data = formSubmitInfo.getData();
        JsonObject formData = new Gson().fromJson(data,JsonObject.class);
        logger.error("The form data is "+formData);
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("firstName",formData.get("firstName").getAsString());
        jsonObject.addProperty("lastName",formData.get("lastName").getAsString());
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
        result.put("json",jsonObject.toString());
        return result;
    }

}
```

## 앱 아래에 crx 노드 만들기

ui.apps 노드를 확장하면 아래 스크린샷과 같이 앱 노드 아래에 **HandleRegistrationFormSubmission**&#x200B;이라는 새 패키지가 만들어집니다
![crx-node](./assets/crx-node.png)
**HandleRegistrationFormSubmission**&#x200B;에 .content.xml이라는 파일을 만듭니다. 다음 코드를 복사하여 .content.xml에 붙여넣습니다

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="Handle Registration Form Submission"
    jcr:primaryType="sling:Folder"
    guideComponentType="fd/af/components/guidesubmittype"
    guideDataModel="xfa,xsd,basic"
    submitService="Core Custom AF Submit"/>
```

**submitService** 요소의 값은 FormSubmitActionService 구현에서 **serviceName = &quot;Core Custom AF Submit&quot;**&#x200B;과(와) 일치해야 합니다.

## 로컬 AEM Forms 인스턴스에 코드 배포

변경 사항을 Cloud Manager 저장소에 푸시하기 전에 로컬 클라우드 지원 작성자 인스턴스에 코드를 배포하여 코드를 테스트하는 것이 좋습니다. 작성자 인스턴스가 실행 중인지 확인합니다.
클라우드 준비 작성자 인스턴스에 코드를 배포하려면 AEM 프로젝트의 루트 폴더로 이동하고 다음 명령을 실행합니다

```
mvn clean install -PautoInstallSinglePackage
```

이렇게 하면 코드가 작성자 인스턴스에 단일 패키지로 배포됩니다

## 코드를 cloud manager에 푸시하고 코드를 배포합니다

로컬 인스턴스에서 코드를 확인한 후 클라우드 인스턴스에 코드를 푸시합니다.
변경 사항을 로컬 git 저장소로 푸시한 다음 cloud manager 저장소로 푸시합니다. [Git 설정](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/setup-git.html?lang=ko), [AEM 프로젝트를 Cloud Manager 저장소로 푸시](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git.html?lang=ko) 및 [개발 환경에 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/deploy-to-dev-environment.html?lang=ko) 문서를 참조할 수 있습니다.

파이프라인이 성공적으로 실행되면 아래 스크린샷과 같이 양식의 제출 액션을 사용자 지정 제출 핸들러에 연결할 수 있어야 합니다
![제출 액션](./assets/configure-submit-action.png)

## 다음 단계

[React 앱에 사용자 지정 응답 표시](./handle-response-react-app.md)
