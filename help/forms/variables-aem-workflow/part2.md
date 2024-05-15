---
title: AEM 워크플로우의 변수[Part2]
description: AEM 워크플로우에서 XML, JSON, ArrayList, Document 유형의 변수 사용
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 354
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# AEM Workflow에 있는 JSON 유형의 변수

이제 AEM Forms 6.5부터 AEM Workflow에서 JSON 유형의 변수를 만들 수 있습니다. AEM Workflow에 JSON 스키마를 기반으로 하는 적응형 Forms을 제출하거나 양식 데이터 모델 호출 작업의 결과를 저장하려는 경우, 일반적으로 JSON 유형의 변수를 만듭니다. 다음 비디오에서는 AEM 워크플로에서 JSON 유형의 변수를 만들고 사용하는 데 필요한 단계를 안내합니다

**AEM Forms 6.5.0을 사용하는 경우**

워크플로우 모델에서 제출된 데이터를 캡처하기 위해 JSON 유형의 변수를 만들 때 JSON 스키마를 변수와 연결하지 마십시오. 이는 JSON 스키마 기반 적응형 양식을 제출할 때 제출된 데이터가 JSON 스키마와 호환되지 않기 때문입니다. JSON 스키마 컴플레인 데이터는 afData.afBoundData.data 요소에 포함됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**AEM Forms 6.5.1 이상을 사용하는 경우**

워크플로우 모델에서 스키마를 JSON 유형의 변수와 매핑할 수 있습니다. 그런 다음 스키마 브라우저를 사용하여 스키마 요소를 워크플로우 모델의 문자열/숫자 변수와 매핑할 수 있습니다

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

시스템에서 에셋을 작업하려면 다음 단계를 따르십시오.

* [패키지 관리자를 사용하여 자산을 AEM에 다운로드하고 가져옵니다.](assets/jsonandstringvariable.zip)
* [워크플로우 모델 살펴보기](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 워크플로우에 사용되는 변수를 이해하려면
* [이메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 세부 사항을 입력하고 양식을 제출하십시오.
