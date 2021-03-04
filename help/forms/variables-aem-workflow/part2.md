---
title: AEM Workflow[Part2]의 변수
seo-title: AEM Workflow[Part2]의 변수
description: aem 워크플로우에서 xml,json,arraylist,문서 유형의 변수 사용
seo-description: aem 워크플로우에서 xml,json,arraylist,문서 유형의 변수 사용
feature: 워크플로우
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: 개발
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 1%

---

# AEM Workflow에서 JSON 유형의 변수

AEM Forms 6.5부터 이제 AEM 워크플로우에서 JSON 유형의 변수를 만들 수 있습니다. JSON 스키마를 기반으로 하는 적응형 Forms을 AEM Workflow에 제출하거나 양식 데이터 모델 호출 작업의 결과를 저장하려는 경우 일반적으로 JSON 유형의 변수를 만듭니다. 다음 비디오에서는 AEM 워크플로우에서 JSON 유형의 변수를 만들고 사용하는 데 필요한 단계를 안내합니다

**AEM Forms 6.5.0 사용 시**

워크플로우 모델에서 제출된 데이터를 캡처하기 위해 JSON 유형의 변수를 만드는 경우 JSON 스키마를 변수와 연결하지 마십시오. 이는 JSON 스키마 기반 적응형 양식을 제출할 때 제출된 데이터가 JSON 스키마를 준수하지 않기 때문입니다. JSON 스키마 불만 데이터는 afData.afBoundData.data 요소로 포함되어 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**AEM Forms 6.5.1 이상을 사용하는 경우**

워크플로우 모델에서 JSON 유형의 변수와 함께 스키마를 매핑할 수 있습니다. 그런 다음 스키마 브라우저를 사용하여 워크플로우 모델의 문자열/숫자 변수와 함께 스키마 요소를 매핑할 수 있습니다

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

시스템에서 에셋을 작동하려면 다음 단계를 따르십시오.

* [패키지 관리자를 사용하여 에셋을 다운로드하고 AEM으로 가져오기](assets/jsonandstringvariable.zip)
* [워크플로우 모델을 ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 탐색하여 워크플로우에 사용되는 변수를 파악합니다.
* [이메일 서비스 구성](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 세부 사항을 작성하고 양식을 제출합니다.
