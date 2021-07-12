---
title: AEM에서 번역 규칙 설정
description: 번역 구성 UI를 사용하여 AEM Sites에서 콘텐츠를 번역하기 위한 규칙을 관리할 수 있습니다. 이 비디오에서는 사용자 지정 구성 요소에 대한 새 번역 규칙 만들기에 대해 자세히 설명합니다.
feature: 언어 복사
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: 로컬라이제이션
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 3%

---


# 번역 규칙 설정 {#set-up-translation-rules-in-aem}

번역 구성 UI를 사용하여 AEM Sites에서 콘텐츠를 번역하기 위한 규칙을 관리할 수 있습니다. 이 비디오에서는 사용자 지정 구성 요소에 대한 새 번역 규칙 만들기에 대해 자세히 설명합니다.

>[!NOTE]
>
> 아래 비디오는 AEM 6.3에 녹화되었습니다. AEM 6.4+에서는 변환 규칙 XML 파일을 저장하는 새로운 저장소 구조를 도입했습니다. AEM 6.4 이상에서 번역 구성 UI를 사용할 때 규칙은 `/conf/global/settings/translation/rules/translation_rules.xml` 위치에 저장됩니다. 자세한 내용은 [번역할 콘텐츠 식별](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

번역 규칙은 번역을 위해 추출할 AEM의 컨텐츠를 식별합니다. 기본적으로 번역 규칙은 이미지 구성 요소에 대한 텍스트 구성 요소 및 대체 텍스트와 같은 일반적인 사용 사례를 다룹니다. 프로젝트 번역 요구 사항에 따라 추가 규칙이 필요할 수 있습니다. 일반적인 번역 규칙에서 사용자는 다음을 지정할 수 있습니다.

1. 경로 및/또는 리소스 유형을 기반으로 변환해야 하는 속성
2. 변환하지 말아야 하는 속성에 대한 필터
3. 번역해야 하는 참조된 컨텐츠(이미지 또는 컨텐츠 조각)를 참조하십시오

번역 xml 파일을 업데이트할 번역 규칙 편집기. 번역 구성 UI를 사용하면 XML을 직접 편집할 때 다양한 번역 규칙을 쉽게 관리하고 오타를 방지할 수 있습니다.

번역 구성 UI에 액세스:

* **[!UICONTROL AEM 시작 메뉴]  >  [!UICONTROL 도구]  >  [!UICONTROL 일반]  >  [[!UICONTROL 번역 구성]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3 이전 {#prior-to-aem}

이전 AEM 버전 번역 규칙은 번역 작업 과정 아래에 있는 XML 파일을 편집하여 수동으로 업데이트되었습니다. `/etc/workflow/models/translation/translation_rules.xml`

## 추가 리소스 {#additional-resources}

* [번역할 컨텐츠 식별](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [다국어 사이트의 컨텐츠 번역](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [번역 우수 사례](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
