---
title: 사용자 지정 자산 유형 등록
seo-title: 사용자 지정 자산 유형 등록
description: AEMForms Portal에서 목록에 대한 사용자 지정 자산 유형 활성화
seo-description: AEMForms Portal에서 목록에 대한 사용자 지정 자산 유형 활성화
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 2%

---


# 사용자 지정 자산 유형 등록 {#registering-custom-asset-types}

AEMForms Portal에서 목록에 대한 사용자 지정 자산 유형 활성화

>[!NOTE]
>
>SP1이 포함된 AEM 6.3과 해당 AEM Forms 추가 기능이 설치되어 있는지 확인합니다. 이 기능은 AEM Forms 6.3 SP1 이상에서만 작동합니다

## 기본 경로 {#specify-base-path} 지정

기본 경로는 사용자가 검색 및 목록 구성 요소에 나열할 수 있는 모든 자산을 포함하는 최상위 저장소 경로입니다. 원할 경우 기본 경로 내의 특정 위치를 구성 요소 편집 대화 상자에서 구성할 수도 있으므로 기본 경로 내의 모든 노드를 검색하는 대신 특정 위치에서 검색이 트리거됩니다. 기본적으로, 사용자가 이 위치 내에서 특정 경로 세트를 구성하지 않는 한, 기본 경로는 자산을 가져오기 위한 검색 경로 기준으로 사용됩니다. 성능 검색을 수행하려면 이 경로의 최적 값이 있어야 합니다. 모든 AEM Forms 자산이 **_/content/dam/formsanddocuments_**&#x200B;에 있으므로 기본 경로의 기본값은 **_/content/dam/formsanddocuments로 유지됩니다._**

기본 경로를 구성하는 단계

1. crx에 로그인
1. **/libs/fd/fp/extensions/querybuilder/basepath** 로 이동합니다.

1. 도구 모음에서 &quot;오버레이 노드&quot;를 클릭합니다
1. 오버레이 위치가 &quot;/apps/&quot;인지 확인합니다.
1. 확인을 클릭합니다
1. 저장을 클릭합니다
1. **/apps/fd/fp/extensions/querybuilder/basepath**&#x200B;에서 만든 새 구조로 이동합니다.

1. 경로 속성의 값을 **&quot;/content/dam&quot;**&#x200B;로 변경합니다.
1. 저장을 클릭합니다

경로 속성을 **&quot;/content/dam&quot;**&#x200B;로 지정하면 기본적으로 기본 경로를 /content/dam으로 설정합니다. 검색 및 목록 구성 요소를 열어 확인할 수 있습니다.

![바세패스](assets/basepath.png)

## 사용자 지정 자산 유형 등록 {#register-custom-asset-types}

검색 및 목록 구성 요소에 새 탭(자산 목록)을 추가했습니다. 이 탭에는 사용자가 구성하는 기본 자산 유형 및 추가 자산 유형이 나열됩니다. 기본적으로 다음 자산 유형이 나열됩니다

1. 적응형 양식
1. 양식 템플릿
1. PDF forms
1. 문서(정적 PDF)

**사용자 지정 자산 유형을 등록하는 단계**

1. **/libs/fd/fp/extensions/querybuilder/assettypes**&#x200B;의 오버레이 노드 만들기

1. 오버레이 위치를 &quot;/apps&quot;로 설정합니다.
1. **/apps/fd/fp/extensions/querybuilder/assets 유형에서 만든 새 구조로 **

1. 이 위치에서 등록할 유형에 대해 &#39;nt:un구조화되지 않음&#39; 노드를 만들고 노드 이름을 **mp4files로 지정합니다. 이 mp4files 노드**&#x200B;에 다음 두 속성을 추가합니다.

   1. jcr:title 속성을 추가하여 자산 유형의 표시 이름을 지정합니다. jcr:title 값을 &quot;Mp4 파일&quot;로 설정합니다.
   1. type 속성을 추가하고 값을 &quot;videos&quot;로 설정합니다. 다음은 비디오 유형 자산의 목록을 만드는 데 사용하는 값입니다. 변경 사항을 저장합니다.

1. mp4파일 아래에 &quot;nt:unrecrugend&quot; 유형의 노드를 만듭니다. 이 노드의 이름을 &quot;searchcriteria&quot;로 지정합니다.
1. 검색 기준에 하나 이상의 필터를 추가합니다. 사용자가 MIME 유형이 &quot;video/mp4&quot;인 mp4Files를 나열하는 검색 필터를 사용하려는 경우 여기에서 그렇게 할 수 있습니다
1. 노드 검색 기준 아래에 &quot;nt:un구조화되지 않음&quot; 유형의 노드를 만듭니다. 이 노드의 이름을 &quot;filetypes&quot;로 지정합니다.
1. 다음 두 속성을 이 &quot;filetypes&quot; 노드에 추가합니다

   1. 이름: ./jcr:content/metadata/dc:format
   1. 값:video/mp4

1. 즉, dc:format 속성이 비디오/mp4와 같은 속성을 갖는 자산은 &quot;Mp4 비디오&quot; 자산 유형으로 간주됩니다. 검색 기준에 대해 &quot;jcr:content/metadata&quot; 노드에 나열된 모든 속성을 사용할 수 있습니다

1. **작업 내용을 저장해야 합니다**

위의 단계를 수행한 후, 아래와 같이 검색 및 목록 구성 요소의 자산 유형 드롭다운 목록에 새 자산 유형(Mp4 파일)이 표시되기 시작합니다

![mp4파일](assets/mp4files.png)

[이 작업을 수행하는 데 문제가 있는 경우 다음 패키지를 가져올 수 있습니다.](assets/assettypeskt1.zip) 패키지에는 두 개의 사용자 지정 자산 유형이 정의되어 있습니다. Mp4 파일 및 Worddocuments. **/apps/fd/fp/extensions/querybuilder/assettypes**&#x200B;를 살펴보는 것이 좋습니다.

[사용자 지정 포털 패키지를 설치합니다](assets/customportalpage.zip). 이 패키지에는 샘플 포털 페이지가 들어 있습니다. 이 페이지는 이 자습서의 2부에서 사용됩니다

