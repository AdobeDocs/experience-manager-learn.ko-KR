---
title: AEM Forms에서 Adobe Sign API 사용
description: Adobe Sign 도우미 메서드를 사용하여 서명용 문서 보내기
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 81026b569ae0dc9976f714715682448a41d9f8bc
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Adobe Sign 도우미 메서드 사용

특정 사용 사례에서는 AEM 워크플로를 사용하지 않고 서명을 위해 문서를 보내야 할 수도 있습니다. 이러한 경우, 이 문서에 제공된 샘플 번들에 의해 노출된 래퍼 방법들을 사용하는 것이 매우 편리할 것이다.

## 샘플 OSGi 번들 배포

[OSGi 번들 배포](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar) AEM OSGi 웹 콘솔을 통해 AEM OSGi 웹 콘솔의 구성 관리자를 통해 아래와 같이 OSGi 구성을 사용하여 API 통합 키 및 API 사용자를 지정합니다.

 다음 사항에 유의하십시오. `AdobeSignHelperMethods` OSGi 번들은 AEM(Adobe Experience Manager) 제품 코드로 인식되지 않으며 따라서 Adobe 지원에서 지원되지 않습니다.
![서명 구성](assets/sign-configuration.png)


## API 설명서

다음을 통해 사용할 수 있습니다 `AcrobatSignHelperMethods` OSGi 번들에서 제공되는 OSGi 서비스.

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


계약 또는 웹 양식을 만드는 데 사용되는 문서입니다. 보낸 사람이 먼저 Acrobat Sign에 문서를 업로드합니다. 이를 다음이라고 합니다. _임시_ 업로드 후 7일 동안만 사용할 수 있기 때문입니다. 이 메서드는 다음을 허용합니다. `com.adobe.aemfd.docmanager.Document` 임시 문서 ID를 반환합니다.

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

서명용 임시 문서 ID를 사용하여 전자 메일 매개 변수로 식별된 사용자에게 서명용 문서를 보냅니다.

### getWidgetID

`String getWidgetID(String transientDocumentID)`

위젯은 사용자에게 여러 번 표시되고 여러 번 서명될 수 있는 재사용 가능한 템플릿과 같습니다. 이 메서드를 사용하면 임시 문서 ID를 사용하여 위젯 ID를 가져올 수 있습니다.

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

특정 위젯 ID에 대한 위젯 URL을 가져옵니다. 그런 다음 문서 서명을 위해 사용자에게 이 위젯 URL을 제공할 수 있습니다.

## API 사용

다음 `AcrobatSignHelperMethods` 는 OSGi 서비스이므로 java 코드의 @Reference 주석을 사용하여 주석을 달아야 합니다.

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```

