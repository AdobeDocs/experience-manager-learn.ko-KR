---
title: 사용자 지정 자산 유형 등록
description: AEMForms 포털에 나열할 사용자 지정 자산 유형 활성화
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
duration: 129
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 0%

---

# 사용자 지정 자산 유형 등록 {#registering-custom-asset-types}

AEMForms 포털에 나열할 사용자 지정 자산 유형 활성화

>[!NOTE]
>
>AEM 6.3 SP1과 해당 AEM Forms 추가 기능이 설치되어 있는지 확인합니다. 이 기능은 AEM Forms 6.3 SP1 이상에서만 작동합니다

## 기본 경로 지정 {#specify-base-path}

기본 경로는 사용자가 검색 및 목록 구성 요소에 나열할 수 있는 모든 에셋을 구성하는 최상위 저장소 경로입니다. 원할 경우 구성 요소 편집 대화 상자에서 기본 경로 내의 특정 위치를 구성할 수도 있으므로 기본 경로 내의 모든 노드를 검색하는 대신 특정 위치에서 검색이 트리거됩니다. 기본적으로 사용자가 이 위치 내에서 특정 경로 집합을 구성하지 않으면 기본 경로가 자산을 가져오는 검색 경로 기준으로 사용됩니다. 수행적 검색을 하기 위해서는 이 경로의 최적 값을 갖는 것이 중요하다. 모든 AEM Forms 자산이 **_/content/dam/formsanddocuments._**&#x200B;에 있으므로 기본 경로의 기본값은 **_/content/dam/formsanddocuments_**(으)로 유지됩니다.

기본 경로 구성 단계

1. crx에 로그인
1. **/libs/fd/fp/extensions/querybuilder/basepath**(으)로 이동

1. 도구 모음에서 &quot;오버레이 노드&quot;를 클릭합니다.
1. 오버레이 위치가 &quot;/apps/&quot;인지 확인합니다.
1. 확인을 클릭합니다.
1. 저장을 클릭합니다.
1. **/apps/fd/fp/extensions/querybuilder/basepath**&#x200B;에서 만든 새 구조로 이동합니다.

1. 경로 속성의 값을 **&quot;/content/dam&quot;**(으)로 변경합니다.
1. 저장을 클릭합니다.

**&quot;/content/dam&quot;**(으)로 경로 속성을 지정하면 기본적으로 기본 경로를 /content/dam으로 설정합니다. 검색 및 목록 구성 요소를 열어 이를 확인할 수 있습니다.

![basepath](assets/basepath.png)

## 사용자 지정 자산 유형 등록 {#register-custom-asset-types}

검색 및 목록 구성 요소에 새 탭(자산 목록)을 추가했습니다. 이 탭에는 사용자가 구성하는 기본 에셋 유형 및 추가 에셋 유형이 나열됩니다. 기본적으로 다음 자산 유형이 나열됩니다

1. 적응형 양식
1. 양식 템플릿
1. PDF forms
1. Document(정적 PDF)

**사용자 지정 자산 유형을 등록하는 단계**

1. **/libs/fd/fp/extensions/querybuilder/assettypes**&#x200B;의 오버레이 노드 만들기

1. 오버레이 위치를 &quot;/apps&quot;(으)로 설정
1. `/apps/fd/fp/extensions/querybuilder/assettypes`에서 만든 새 구조로 이동

1. 이 위치에서 등록할 형식에 대한 &#39;nt:unstructured&#39; 노드를 만들고 노드 이름을 **mp4files로 지정합니다. 이 mp4files 노드**&#x200B;에 다음 두 속성을 추가하십시오.

   1. jcr:title 속성을 추가하여 에셋 유형의 표시 이름을 지정합니다. jcr:title의 값을 &quot;Mp4 Files&quot;로 설정합니다.
   1. &quot;type&quot; 속성을 추가하고 값을 &quot;videos&quot;로 설정합니다. 유형 비디오의 자산을 나열하기 위해 템플릿에서 사용하는 값입니다. 변경 사항을 저장합니다.

1. mp4files 아래에 &quot;nt:unstructured&quot; 유형의 노드를 만듭니다. 이 노드의 이름을 &quot;searchcriteria&quot;로 지정합니다.
1. 검색 기준에 필터를 하나 이상 추가합니다. 사용자가 MIME 유형이 &quot;video/mp4&quot;인 mp4Files를 나열하는 검색 필터를 원한다고 가정해 보겠습니다.
1. 노드 검색 기준 아래에 &quot;nt:unstructured&quot; 유형의 노드를 만듭니다. 이 노드의 이름을 &quot;filetypes&quot; 로 지정합니다.
1. 이 &quot;filetypes&quot; 노드에 다음 2개의 속성을 추가합니다.

   1. 이름: ./jcr:content/metadata/dc:format
   1. 값: video/mp4

1. 즉, dc:format 속성이 video/mp4와 동일한 자산은 자산 유형 &quot;Mp4 Videos&quot;로 간주됩니다. 검색 조건에 &quot;jcr:content/metadata&quot; 노드에 나열된 모든 속성을 사용할 수 있습니다

1. **작업을 저장하십시오**

위의 단계를 수행하면 새 자산 유형(Mp4 파일)이 아래와 같이 검색 및 목록 구성 요소의 자산 유형 드롭다운 목록에 표시되기 시작합니다

![mp4파일](assets/mp4files.png)

[이 작업을 수행하는 데 문제가 있으면 다음 패키지를 가져올 수 있습니다.](assets/assettypeskt1.zip) 패키지에 두 개의 사용자 지정 자산 유형이 정의되어 있습니다. Mp4 파일 및 Worddocuments. **/apps/fd/fp/extensions/querybuilder/assettypes**&#x200B;을(를) 살펴보십시오.

[customeportal 패키지를 설치합니다](assets/customportalpage.zip). 이 패키지에는 샘플 포털 페이지가 포함되어 있습니다. 이 페이지는 이 자습서의 2부에서 사용됩니다.
