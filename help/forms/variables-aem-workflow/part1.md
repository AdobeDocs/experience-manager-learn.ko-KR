---
title: AEM Workflow[1부]의 변수
seo-title: AEM Workflow[1부]의 변수
description: aem 워크플로우에서 xml,json,arraylist,문서 유형의 변수 사용
seo-description: aem 워크플로우에서 xml,json,arraylist,문서 유형의 변수 사용
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# AEM 워크플로우의 XML 변수

XML 유형의 변수는 일반적으로 XSD 기반 적응형 양식이 있고 워크플로의 적응형 양식 제출 시 값을 추출하려는 경우에 사용됩니다.

다음 비디오에서는 String 및 XML 유형의 변수를 만들고 이를 워크플로우에서 사용하는 데 필요한 단계를 안내합니다.

XML 변수를 사용하여 적응형 양식을 미리 채우거나 워크플로우에 적응형 양식의 제출 데이터를 저장할 수 있습니다.

Xpathing에서 XML 변수에 대한 문자열 변수를 채울 수 있습니다. 그러면 이 문자열 변수는 일반적으로 이메일 전송 구성 요소의 전자 메일 템플릿 자리 표시자를 채우는 데 사용됩니다

>[!NOTE]
>
>응용 양식이 XSD와 연결되지 않으면 XPath에서 요소의 값을 가져오면
>
>**/afData/afUnboundData/data/submitterName**

적응형 양식 데이터는 위에 표시된 대로 데이터 요소 아래에 저장됩니다. **_위의 XPath submitterName에서 는 적응형 양식의 텍스트 필드 이름입니다._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - 워크플로 모델에서 제출된 데이터를 캡처하기 위해 XML 유형의 변수를 만드는 경우 XSD를 변수에 연결하지 마십시오. XSD 기반 적응형 양식을 제출할 때 제출된 데이터가 XSD와 호환되지 않기 때문입니다. XSD 수신 거부 데이터는 /afData/afBoundData/ 요소로 포함되어 있습니다.
>
>**AEM Forms 6.5.1**  - XSD를 XML 변수와 연결하는 경우 스키마 요소를 찾아 변수 매핑을 수행할 수 있습니다. 스키마 요소에 바인딩되지 않은 양식 데이터에 액세스할 수 없습니다. 사용 사례가 스키마 요소에 바인딩되어 있는 데이터 및 언바운드 데이터에 액세스하는 경우에는 워크플로우에서 스키마를 XML 변수와 바인딩하지 마십시오. 필요한 데이터에 액세스하려면 적절한 XPath 표현식을 사용해야 합니다

## XML 변수 만들기

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### XML 변수와 함께 스키마 사용

**스키마로 XML 변수 매핑. AEM Forms 6.5.1 이상 버전에서 이 기능 사용**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### 이메일 보내기에 변수 사용

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

시스템에서 에셋을 작동하려면 다음 단계를 따르십시오.

* [패키지 관리자를 사용하여 에셋을 다운로드하고 AEM으로 가져오기](assets/xmlandstringvariable.zip)
* [워크플로우 모델을 ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) 탐색하여 워크플로우에 사용되는 변수를 파악합니다.
* [이메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 세부 사항을 작성하고 양식을 제출합니다.

