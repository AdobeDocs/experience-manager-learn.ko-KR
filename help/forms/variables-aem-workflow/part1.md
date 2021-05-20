---
title: AEM Workflow[Part1]의 변수
seo-title: AEM Workflow[Part1]의 변수
description: aem 워크플로우에서 xml,json,arraylist,document 유형의 변수 사용
seo-description: aem 워크플로우에서 xml,json,arraylist,document 유형의 변수 사용
feature: 워크플로우
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# AEM 워크플로우의 XML 변수

XML 유형의 변수는 일반적으로 XSD 기반 적응형 양식이 있고 워크플로우의 적응형 양식 제출에서 값을 추출하려고 할 때 사용됩니다.

다음 비디오에서는 String 및 XML 유형의 변수를 만들고 워크플로우에서 사용하는 데 필요한 단계를 안내합니다.

XML 변수를 사용하여 적응형 양식을 미리 채우거나 워크플로우에 적응형 양식의 제출 데이터를 저장할 수 있습니다.

XML 변수로의 Xpathing으로 문자열 변수를 채울 수 있습니다. 그런 다음 이 문자열 변수는 일반적으로 이메일 전송 구성 요소에서 이메일 템플릿 자리 표시자를 채우는 데 사용됩니다

>[!NOTE]
>
>적응형 양식이 XSD와 연결되어 있지 않으면 XPath에서 요소의 값을 가져오는 모습은 다음과 같습니다
>
>**/afData/afUnboundData/data/submitterName**

적응형 양식 데이터는 위에 표시된 대로 데이터 요소 아래에 저장됩니다. **_위의 XPath submitterName은 적응형 양식에 있는 텍스트 필드의 이름입니다._**

>[!NOTE]
>
>**AEM Forms 6.5.0**  - 워크플로우 모델에서 제출된 데이터를 캡처하기 위해 XML 유형의 변수를 만드는 경우 XSD를 변수와 연결하지 마십시오. XSD 기반 적응형 양식을 제출할 때 제출된 데이터가 XSD와 호환되지 않기 때문입니다. XSD 수신 거부 데이터는 /afData/afBoundData/ 요소에 들어 있습니다.
>
>**AEM Forms 6.5.1**  - XSD를 XML 변수와 연결하는 경우 변수 매핑을 수행할 스키마 요소를 찾을 수 있습니다. 스키마 요소에 바인딩되지 않은 양식 데이터에 액세스할 수 없습니다. 사용 사례에서 스키마 요소 및 바인딩되지 않은 데이터에 바인딩된 데이터에 액세스하려는 경우 워크플로에서 XML 변수에 스키마를 바인딩하지 마십시오

## XML 변수 만들기

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### XML 변수와 함께 스키마 사용

**스키마에 XML 변수 매핑. AEM Forms 6.5.1 이상에서 이 기능 사용**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### 이메일 전송에서 변수 사용

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

시스템에서 자산을 작동시키려면 다음 단계를 수행하십시오.

* [패키지 관리자를 사용하여 자산을 다운로드하여 AEM에 가져옵니다](assets/xmlandstringvariable.zip)
* [워크플로우 모델](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) 을 탐색하여 워크플로우에 사용되는 변수를 이해합니다
* [이메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 세부 사항을 입력하고 양식을 제출하세요.

