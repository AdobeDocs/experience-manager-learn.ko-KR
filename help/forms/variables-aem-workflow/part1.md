---
title: AEM 워크플로우의 변수[Part1]
description: AEM 워크플로우에서 XML, JSON, ArrayList, Document 유형의 변수 사용
feature: Adaptive Forms, Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 590
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# AEM Workflow의 XML 변수

XML 유형의 변수는 일반적으로 XSD 기반 적응형 양식이 있고, 워크플로우의 적응형 양식 제출에서 값을 추출하려는 경우 사용됩니다.

다음 비디오에서는 String 및 XML 유형의 변수를 만들고 이를 워크플로우에서 사용하는 데 필요한 단계를 안내합니다.

XML 변수를 사용하여 적응형 양식을 미리 채우거나 적응형 양식의 제출 데이터를 워크플로우에 저장할 수 있습니다.

문자열 변수는 XML 변수에 대한 Xpathing으로 채울 수 있습니다. 그런 다음 이 문자열 변수는 일반적으로 이메일 보내기 구성 요소에서 이메일 템플릿 자리 표시자를 채우는 데 사용됩니다

>[!NOTE]
>
>적응형 양식이 XSD와 연결되어 있지 않으면 요소 값을 가져오기 위한 XPath는 다음과 같이 표시됩니다
>
>**/afData/afUnboundData/data/submitterName**

적응형 양식 데이터는 위와 같이 데이터 요소 아래에 저장됩니다. **_위의 XPath submitterName은 적응형 양식의 텍스트 필드 이름입니다._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - 워크플로우 모델에서 제출된 데이터를 캡처하기 위해 유형 XML의 변수를 만들 때 XSD를 변수와 연결하지 마십시오. 이는 XSD 기반 적응형 양식을 제출할 때 제출된 데이터가 XSD를 준수하지 않기 때문입니다. XSD 컴플레인 데이터는 /afData/afBoundData/ 요소에 포함됩니다.
>
>**AEM Forms 6.5.1** - XSD를 XML 변수와 연결하면 스키마 요소를 검색하여 변수 매핑을 수행할 수 있습니다. 스키마 요소에 바인딩되지 않은 양식 데이터에는 액세스할 수 없습니다. 스키마 요소에 바인딩된 데이터와 바인딩되지 않은 데이터에 액세스하는 경우 워크플로우에서 XML 변수에 스키마를 바인딩하지 마십시오.필요한 데이터에 액세스하려면 적절한 XPath 표현식을 사용해야 합니다

## XML 변수 만들기

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### XML 변수에 스키마 사용

**XML 변수를 스키마와 매핑합니다. AEM Forms 6.5.1 이상에서 이 기능 사용**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### 이메일 전송에서 변수 사용

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

시스템에서 에셋을 작업하려면 다음 단계를 따르십시오.

* [패키지 관리자를 사용하여 자산을 AEM에 다운로드하고 가져옵니다.](assets/xmlandstringvariable.zip)
* [워크플로우 모델 살펴보기](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) 워크플로우에 사용되는 변수를 이해하려면
* [이메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* 세부사항을 입력하고 양식을 제출하세요.
