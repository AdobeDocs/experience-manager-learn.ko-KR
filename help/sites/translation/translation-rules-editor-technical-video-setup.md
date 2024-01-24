---
title: AEM에서 번역 규칙 설정
description: 사용자는 번역 구성 UI를 통해 AEM Sites에서 콘텐츠를 번역하기 위한 규칙을 관리할 수 있습니다. 이 비디오에서는 사용자 지정 구성 요소에 대한 새 번역 규칙 만들기에 대해 자세히 설명합니다.
feature: Language Copy
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
duration: 549
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# 번역 규칙 설정 {#set-up-translation-rules-in-aem}

사용자는 번역 구성 UI를 통해 AEM Sites에서 콘텐츠를 번역하기 위한 규칙을 관리할 수 있습니다. 이 비디오에서는 사용자 지정 구성 요소에 대한 새 번역 규칙 만들기에 대해 자세히 설명합니다.

>[!NOTE]
>
> 아래 비디오는 AEM 6.3에 녹화되었습니다. AEM 6.4+에는 번역 규칙 XML 파일을 저장하는 새로운 저장소 구조가 도입되었습니다. AEM 6.4+에서 번역 구성 UI를 사용하는 경우 규칙이 위치에 저장됩니다 `/conf/global/settings/translation/rules/translation_rules.xml`. 다음을 참조하십시오 [번역할 콘텐츠 식별](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) 을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

번역 규칙은 번역을 위해 추출될 AEM의 콘텐츠를 식별합니다. 즉시 사용 가능한 번역 규칙은 텍스트 구성 요소 및 이미지 구성 요소에 대한 대체 텍스트와 같은 일반적인 사용 사례를 다룹니다. 프로젝트 번역 요구 사항에 따라 추가 규칙이 필요할 수 있습니다. 일반적으로 번역 규칙을 사용하여 다음을 지정할 수 있습니다.

1. 경로 및/또는 리소스 유형에 따라 변환해야 하는 속성입니다
2. 변환해서는 안 되는 속성에 대한 필터
3. 번역해야 하는 참조된 콘텐츠(예: 이미지 또는 콘텐츠 조각)

번역 xml 파일을 업데이트할 번역 규칙 편집기. 번역 구성 UI를 통해 다양한 번역 규칙을 보다 쉽게 관리하고 XML을 직접 편집할 때 오타를 방지할 수 있습니다.

번역 구성 UI에 액세스:

* **[!UICONTROL AEM 시작 메뉴] > [!UICONTROL 도구] > [!UICONTROL 일반] > [[!UICONTROL 번역 구성]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3 이전 {#prior-to-aem}

이전 AEM 버전의 번역 규칙은 번역 워크플로 아래에 있는 XML 파일을 편집하여 수동으로 업데이트되었습니다. `/etc/workflow/models/translation/translation_rules.xml`.

## 추가 리소스 {#additional-resources}

* [번역할 콘텐츠 식별](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [다국어 사이트를 위한 콘텐츠 번역](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [번역 모범 사례](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
