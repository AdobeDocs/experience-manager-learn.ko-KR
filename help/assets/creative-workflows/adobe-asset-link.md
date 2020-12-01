---
title: AEM Assets에서 Adobe 에셋 링크 익스텐션 사용
description: '이제 자주 사용하는 Adobe Creative Cloud 데스크탑 애플리케이션에서 디자이너와 크리에이티브 사용자가 Adobe Experience Manager 에셋을 사용할 수 있습니다. Adobe Creative Cloud Enterprise용 Adobe Asset Link 익스텐션은 Adobe Photoshop, InDesign 및 Illustrator과 같은 Creative Cloud 툴에서 AEM 에셋을 검색 및 검색하고 정렬, 미리 보기, 업로드, 체크 아웃, 수정, 체크 인 및 볼 수 있는 기능을 확장합니다. '
feature: adobe-asset-link
topics: authoring, collaboration, operations, sharing, metadata, images
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 1%

---


# AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}에서 Adobe 자산 링크 확장 사용

이제 자주 사용하는 Adobe Creative Cloud 데스크탑 애플리케이션에서 디자이너와 크리에이티브 사용자가 Adobe Experience Manager 에셋을 사용할 수 있습니다. Adobe Creative Cloud Enterprise용 Adobe Asset Link 익스텐션은 Adobe Photoshop, InDesign 및 Illustrator과 같은 Creative Cloud 툴에서 AEM 에셋을 검색 및 검색하고 정렬, 미리 보기, 업로드, 체크 아웃, 수정, 체크 인 및 볼 수 있는 기능을 확장합니다.


## Adobe 자산 링크 1.1

이제 Adobe 자산 링크 v1.1에서 Adobe 자산 링크와 AEM Assets 간 InDesign 직접 연결 지원을 제공합니다. 이제 InDesign 직접 연결 지원을 통해 Adobe 에셋 링크 패널을 통해 AEM Assets의 InDesign에 디지털 에셋을 배치(연결 가져오기 또는 복사 배치)하거나 드래그하여에 놓을 수 있습니다. 또한 *배치 전용*(FPO) 변환을 도입합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Adobe Creative Cloud Enterprise ID 또는 Federated ID만 사용하십시오. Adobe 자산 링크[에 대해 AEM을 구성해야 합니다.](https://helpx.adobe.com/enterprise/using/configure-aem-for-aal-prerelease.html)


### Adobe 자산 링크 기능

* Adobe Asset Link는 PS, AI, ID에서 작동하고 AEM Assets에 있는 디지털 자산에 직접 액세스할 수 있는 확장입니다
* Adobe IMS Enterprise ID 또는 Federated ID을 사용하여 크리에이티브 전문가가 AEM에 자동으로 로그인됩니다.
* 크리에이티브 전문가는 AEM Assets 및 Creative Cloud 에셋 검색 외에 AEM Assets에 있는 디지털 에셋을 검색할 수 있습니다
* 크리에이티브 전문가는 AEM Assets에 있는 자산의 파일 세부 정보에 액세스할 수 있습니다.축소판, 기본 메타데이터 및 패널 내의 버전
* 크리에이티브 전문가가 자산을 레이아웃에 배치, 다운로드 또는 드래그 앤 드롭할 수 있습니다
* 크리에이티브는 AEM Assets에서 자산을 체크아웃하고 Creative Cloud 자산 계정 내에서 (WIP)로 작업함으로써 자산을 수정할 수 있습니다
* 크리에이티브 전문가는 에셋을 수정한 후 다시 AEM Assets으로 체크 인하고 새 버전이 AEM Assets에 반영됩니다
* Creative Cloud 2020, 2019 및 2018 InDesign, Photoshop 및 Illustrator 데스크탑 앱 지원
* 사용자는 Adobe 에셋 링크 인앱 패널에서 에셋 검색을 수행하고 크기, 알파벳순 및 연관성에 따라 정렬할 수 있습니다
* 사용자는 에셋 링크 패널에서 직접 AEM Assets 컬렉션과 스마트 컬렉션에 액세스하여 찾아볼 수 있습니다
* 패널에서 직접 새로 만든 에셋을 AEM Assets에 추가
* 사용자는 에셋을 InDesign 프레임으로 바로 드래그하여 놓을 수 있습니다.

### AEM Assets InDesign에 배치

아래 옵션 중 하나를 사용하여 InDesign 레이아웃에 자산을 배치할 수 있습니다.

* **복사**  배치 - 자산을 포함(복사 배치 옵션 사용)하면 바이너리를 로컬 시스템에 다운로드한 후 원본 자산의 사본을 InDesign 레이아웃에 배치합니다. Adobe 자산 링크는 포함된 사본과 원본 자산 간의 링크를 유지하지 않습니다. 원본 자산이 AEM Assets에서 수정된 경우 InDesign 파일에서 포함된 자산을 삭제하고 AEM Assets에서 자산을 다시 포함해야 합니다.

* **연결**  배치 - 이제 InDesign 문서를 사용하여 작업할 때 컨텍스트 메뉴에서 복사 배치 옵션을 사용하여 자산을 직접 포함하여 AEM Assets의 자산을 참조할 수 있습니다. 자산을 참조하면 다른 사용자와 공동 작업을 수행하고 AEM Assets의 원래 자산에 대한 모든 업데이트를 통합할 수 있습니다. AEM Assets에서 자산을 참조하려면 컨텍스트 메뉴에서 [연결된 위치] 옵션을 사용합니다.

### FPO(배치 전용) 해상도의 경우

Adobe 에셋 링크를 사용하여 AEM Assets의 InDesign 문서에 대용량 에셋 파일을 배치할 때 크리에이티브 사용자는 가져오기 작업을 시작한 후 몇 초 동안 기다려야 합니다. 이는 전체 사용자 경험에 영향을 줍니다. 이제 Adobe 에셋 링크를 사용하여 AEM Assets에서 원본 에셋의 저해상도 이미지를 일시적으로 배치할 수 있으므로 이미지를 배치하는 데 소요되는 시간을 줄일 수 있습니다. 동시에 전반적인 사용자 경험과 생산성을 높일 수 있습니다. 낮은 해상도 이미지가 일시적으로 삽입되며 인쇄하거나 게시하는 데 최종 출력이 필요한 경우 FPO 변환을 원본으로 바꿔야 합니다. 여러 FPO 이미지를 각각의 원본 이미지로 바꾸려면 **_Windows > 링크_** 패널로 이동한 다음 원래 에셋을 다운로드합니다. 원본 이미지가 다운로드된 후 모든 FPO를 원본으로 바꾸기를 선택합니다.

>[!NOTE]
>
> *배치만(FPO)* 변환은 연결 배치 옵션에만 작동합니다. 또한 AEM Assets *Dam 자산 업데이트* 워크플로우 내에서 FPO 변환 지원을 활성화해야 합니다.

FPO 변환은 원본 자산의 작은 대체 항목입니다. 종횡비는 같지만 원본 이미지에 비해 크기가 작습니다. 현재 InDesign은 다음 이미지 유형에 대해서만 FPO 변환 가져오기를 지원합니다.

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

AEM Assets의 특정 자산에 대해 FPO 변환을 사용할 수 없는 경우 원본 고해상도 자산이 대신 참조됩니다. FPO 이미지의 경우 InDesign 링크 패널에 FPO 상태가 표시됩니다.



## AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}을(를) 사용한 Adobe 자산 링크 인증 이해

Adobe Identity Management 서비스(IMS) 및 Adobe Experience Manager 작성자 컨텍스트에서 Adobe 에셋 링크 인증이 작동하는 방식

![Adobe 에셋 링크 아키텍처](assets/adobe-asset-link-article-understand.png)

[Adobe 자산 링크 아키텍처 다운로드](assets/adobe-asset-link-article-understand-1.png)

1. Adobe 에셋 링크 익스텐션은 Adobe Creative Cloud 데스크탑 앱을 통해 IMS(Adobe ID 관리 서비스)에 대한 권한 부여를 요청하고 성공 시 베어러 토큰을 수신합니다.
2. Adobe 에셋 링크 확장은 **단계 1**&#x200B;에서 얻은 전달자 토큰을 포함하여 확장 프로그램의 설정 JSON에 제공된 스키마(HTTP/HTTPS), 호스트 및 포트를 사용하여 HTTP를 통해 AEM 작성자에 연결됩니다.
3. AEM Bearer Authentication Handler가 요청에서 Bearer 토큰을 추출하여 Adobe IMS에 대해 확인합니다.
4. Adobe IMS가 베어러 토큰을 검증하면 사용자는 AEM에서 만들어지고(아직 존재하지 않는 경우) Adobe IMS의 프로필 및 그룹/멤버십 데이터를 동기화합니다. AEM 사용자는 표준 AEM 로그인 토큰을 발행하여 HTTP(S) 응답의 쿠키로 Adobe 자산 링크 확장자로 다시 전송됩니다.
5. 이후 상호 작용(예: 검색, 검색, 자산 체크 인/체크 아웃 등) adobe 자산 링크 확장 기능을 사용하면 표준 AEM 토큰 인증 핸들러를 사용하여 AEM 로그인 토큰을 사용하여 유효성이 확인된 AEM 작성자에게 HTTP(S) 요청이 발생합니다.

>[!NOTE]
>
>로그인 토큰이 만료되면 **Steps 1-5**&#x200B;이 자동으로 호출되고, 베어러 토큰을 사용하여 Adobe 자산 링크 확장을 인증하고, 유효한 새 로그인 토큰을 다시 발행합니다.

## 추가 리소스{#additional-resources}

* [Adobe 자산 링크 웹 사이트](https://www.adobe.com/kr/creativecloud/business/enterprise/adobe-asset-link.html)