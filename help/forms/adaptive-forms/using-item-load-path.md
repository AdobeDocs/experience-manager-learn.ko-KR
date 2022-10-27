---
title: 항목 로드 경로를 사용하여 드롭다운 목록 채우기
description: crx 노드에서 값을 읽도록 드롭다운 목록을 구성하고 채웁니다
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# AEM Forms의 항목 로드 속성

항목 로드 경로 속성을 사용하여 드롭다운 목록을 구성하고 채웁니다.
항목 로드 경로 필드를 사용하면 작성자가 드롭다운 목록에서 사용할 수 있는 옵션을 로드하는 URL을 제공할 수 있습니다.
crx에서 이러한 노드를 만들려면 아래 설명된 단계를 수행하십시오.
* crx에 로그인
* 컨텐츠 아래에 assets라는 노드를 만듭니다(요구 사항에 따라 이 노드의 이름을 지정할 수 있음). sling:folder 를 입력합니다.
* 저장
* 새로 만든 자산 노드를 클릭하고 아래에 표시된 대로 속성을 설정합니다
* assettypes(요구 사항에 따라 이름을 지정할 수 있음)라는 String 유형의 속성을 만들어야 합니다.속성이 multivalue인지 확인하십시오. 원하는 값을 제공하고 저장합니다.
   ![item-load-path](assets/item-load-path-crx.png)

드롭다운 목록에서 이러한 값을 로드하려면 항목 로드 경로 속성에 다음 경로를 제공합니다  **/content/assets/assets/assetstypes**

샘플 패키지는 [여기에서 다운로드](assets/item-load-path-package.zip)
