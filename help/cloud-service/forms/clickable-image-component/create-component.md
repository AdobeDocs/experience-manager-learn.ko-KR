---
title: 클릭 가능한 이미지 구성 요소 만들기
description: AEM Forms as a Cloud Service에서 클릭 가능한 이미지 구성 요소 만들기
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
exl-id: b635f171-775d-480e-bf7a-c92ab4af0aee
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 1%

---

# 구성 요소 만들기

이 문서에서는 사용자가 AEM Forms CS용 을 개발한 몇 가지 경험이 있다고 가정합니다. 또한 AEM Forms Archetype 프로젝트를 만들고 있다고 가정합니다.

IntelliJ 또는 선택한 다른 IDE에서 AEM Forms 프로젝트를 엽니다. 아래에 svg라는 새 노드를 만듭니다.

```
apps\corecomponent\components\adaptiveForm
```

>[!NOTE]
>
> ``corecomponent``은(는) Maven 프로젝트를 만들 때 제공된 appId입니다. 이 appId는 사용자 환경에서 다를 수 있습니다.


## .content.xml 파일 만들기

svg 노드 아래에 .content.xml 이라는 파일을 만듭니다. 새로 만든 파일에 다음 내용을 추가합니다. 요구 사항에 따라 jcr:description, jcr:title 및 componentGroup을 변경할 수 있습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="USA MAP"
    jcr:primaryType="cq:Component"
    jcr:title="USA MAP"
    sling:resourceSuperType="wcm/foundation/components/responsivegrid"
    componentGroup="CustomCoreComponent - Adaptive Form"/>
```

## svg.html 만들기

svg.html이라는 파일을 만듭니다. 이 파일은 USA 맵의 SVG을 렌더링합니다. [svg.html](assets/svg.html)의 내용을 새로 만든 파일에 복사합니다. 당신이 복사한 것은 미국 SVG 지도입니다. 파일을 저장합니다.

## 프로젝트 배포

로컬 클라우드 준비 인스턴스에 프로젝트를 배포하여 구성 요소를 테스트합니다.

프로젝트를 배포하려면 명령 프롬프트 창에서 프로젝트의 루트 폴더로 이동하여 다음 명령을 실행해야 합니다.

```
mvn clean install -PautoInstallSinglePackage
```

이렇게 하면 프로젝트가 로컬 AEM Forms 인스턴스에 배포되고 구성 요소는 적응형 양식에 포함될 수 있습니다

![usa-map](./assets/usa-map.png)
