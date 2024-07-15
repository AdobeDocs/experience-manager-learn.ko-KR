---
title: AEM 콘텐츠 조각에 대한 번역 지원
description: Adobe Experience Manager을 사용하여 콘텐츠 조각을 현지화하고 번역하는 방법에 대해 알아봅니다. 콘텐츠 조각과 연결된 혼합 미디어 에셋도 추출하고 번역할 수 있습니다.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
jira: KT-201
thumbnail: 18131.jpg
doc-type: Feature Video
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
duration: 223
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 2%

---

# AEM 콘텐츠 조각에 대한 번역 지원 {#translation-support-content-fragments}

Adobe Experience Manager을 사용하여 콘텐츠 조각을 현지화하고 번역하는 방법에 대해 알아봅니다. 콘텐츠 조각과 연결된 혼합 미디어 에셋도 추출하고 번역할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/18131?quality=12&learn=on)

## 콘텐츠 조각 번역 사용 사례 {#content-fragment-translation-use-cases}

콘텐츠 조각 은 AEM이 외부 번역 서비스로 전송하기 위해 추출하는 인식된 콘텐츠 유형입니다. 몇 가지 사용 사례가 즉시 지원됩니다.

1. 콘텐츠 조각은 [언어 복사 및 번역을 위해 Assets 콘솔에서 직접 선택할 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).
2. Sites 페이지에서 참조된 콘텐츠 조각은 언어 복사를 위해 Sites 페이지를 선택하면 해당 언어 폴더로 복사되고 번역을 위해 추출됩니다.
3. 콘텐츠 조각 내에 임베드된 인라인 미디어 에셋은 추출하고 번역할 수 있습니다.
4. 콘텐츠 조각과 연결된 에셋 컬렉션은 추출되고 번역될 수 있습니다.

## 번역 규칙 편집기 {#translation-rules-editor}

**번역 규칙 편집기**&#x200B;를 사용하여 Experience Manager 번역 동작을 업데이트할 수 있습니다. 번역을 업데이트하려면 [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)에서 **도구** > **일반** > **번역 구성**&#x200B;으로 이동합니다.

기본 구성은 리소스 유형이 `core/wcm/components/contentfragment/v1/contentfragment`인 `fragmentPath`의 콘텐츠 조각을 참조합니다. `v1/contentfragment`에서 상속하는 모든 구성 요소는 기본 구성으로 인식됩니다.

![번역 규칙 편집기](assets/translation-configuration.png)
