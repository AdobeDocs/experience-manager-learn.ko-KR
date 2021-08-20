---
title: AEM Workflow[Part2]의 변수
description: AEM 워크플로우에서 XML, JSON, ArrayList, Document 유형의 변수 사용
version: 6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# AEM Workflow에서 JSON 유형의 변수

이제 AEM Forms 6.5부터 AEM Workflow에서 JSON 유형의 변수를 만들 수 있습니다. 일반적으로 JSON 스키마를 기반으로 하는 적응형 Forms을 AEM Workflow에 제출하거나 양식 데이터 모델 호출 작업의 결과를 저장하려는 경우, JSON 유형의 변수를 만듭니다. 다음 비디오에서는 AEM 워크플로우에서 JSON 유형 변수를 만들고 사용하는 데 필요한 단계를 안내합니다

**AEM Forms 6.5.0을 사용하는 경우**

워크플로우 모델에서 제출된 데이터를 캡처하기 위해 JSON 유형 변수를 만드는 경우 JSON 스키마를 변수와 연결하지 마십시오. JSON 스키마 기반의 적응형 양식을 제출할 때 제출된 데이터가 JSON 스키마와 호환되지 않기 때문입니다. JSON 스키마 수신 거부 데이터는 afData.afBoundData.data 요소에 들어 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**AEM Forms 6.5.1 이상을 사용하는 경우**

워크플로우 모델에서 스키마를 JSON 유형의 변수에 매핑할 수 있습니다. 그런 다음 스키마 브라우저를 사용하여 스키마 요소를 워크플로우 모델의 문자열/숫자 변수와 매핑할 수 있습니다

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

시스템에서 자산을 작동시키려면 다음 단계를 수행하십시오.

* [패키지 관리자를 사용하여 자산을 다운로드하여 AEM에 가져옵니다](assets/jsonandstringvariable.zip)
* [워크플로우 모델](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 을 탐색하여 워크플로우에 사용되는 변수를 이해합니다
* [이메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 세부 사항을 입력하고 양식을 제출하십시오
