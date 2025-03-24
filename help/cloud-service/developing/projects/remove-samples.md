---
title: AEM Maven 프로젝트에서 샘플 제거
description: AEM Project Archetype에서 생성한 AEM 프로젝트에서 샘플 코드를 정리하고 제거하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
duration: 341
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 3%

---

# AEM Maven 프로젝트에서 샘플 제거

AEM Project Archetype에서 생성한 AEM 프로젝트에서 생성된 샘플 코드를 정리하고 제거하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## 리소스

+ [AEM Maven Project Archetype](https://github.com/adobe/aem-project-archetype)

## 명령

다음 명령을 실행하여 AEM Maven 프로젝트에서 생성된 샘플 파일을 제거할 수 있습니다.

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## 편집

다음에서 `<div class="helloworld" ...></div>` 제거:

```
ui.frontend/src/main/webpack/static/index.html
```

다음 위치에서 `<helloworld>` 구성 요소 인스턴스 정의 제거:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
