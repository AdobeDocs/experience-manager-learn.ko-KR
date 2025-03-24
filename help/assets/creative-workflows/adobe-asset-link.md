---
title: Adobe Asset Link 및 AEM
description: Adobe Experience Manager 에셋은 디자이너와 크리에이티브 사용자가 즐겨 사용하는 Adobe Creative Cloud 데스크탑 애플리케이션 내에서 사용할 수 있습니다. Adobe Creative Cloud for enterprise용 Adobe Asset Link 확장은 Adobe XD, Photoshop, InDesign 및 Illustrator과 같은 Creative Cloud 도구 내에서 AEM 에셋의 메타데이터를 검색 및 찾아보기, 정렬, 미리보기, 업로드, 체크아웃, 수정, 체크인 및 보기 기능을 확장합니다.
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
jira: KT-8413, KT-3707
last-substantial-update: 2022-06-25T00:00:00Z
doc-type: Feature Video
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
duration: 1027
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 1%

---

# Adobe Asset Link 3.0

Adobe Experience Manager 에셋은 디자이너와 크리에이티브 사용자가 즐겨 사용하는 Adobe Creative Cloud 데스크탑 애플리케이션 내에서 사용할 수 있습니다.

Adobe Creative Cloud for enterprise용 Adobe Asset Link 확장은 Creative Cloud 애플리케이션 내에서 AEM 에셋의 메타데이터를 검색 및 찾아보기, 정렬, 미리보기, 업로드, 체크아웃, 수정, 체크인 및 보기 기능을 확장합니다.

>[!TIP]
>
> [Adobe XD Premium 교육 프로그램](https://helpx.adobe.com/support/xd.html)을 통해 Asset Link를 Adobe Experience Manager 워크플로와 통합하는 방법에 대해 자세히 알아보십시오.

## Adobe Asset Link 및 AEM creative 워크플로

다음 비디오에서는 Adobe Creative Cloud 애플리케이션에서 작업 중인 크리에이티브가 Adobe Asset Link를 사용하여 AEM과 직접 통합하는 데 사용되는 일반적인 워크플로를 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/335927?quality=12&learn=on)

## Adobe Asset Link 기능

+ Adobe Asset Link는 AEM Assets 및 Assets Essentials와 통합됩니다.
+ Adobe Asset Link는 클라우드 기반 AEM 환경(AEM Assets as a Cloud Service 및 Assets Essentials)에 대한 연결을 자동으로 구성합니다.
+ Adobe Asset Link는 Adobe Creative Cloud 애플리케이션 내에서 작동하는 확장입니다.

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Adobe Enterprise ID 또는 Federated ID을 사용한 AEM 자동 인증
+ AEM에서 디지털 에셋 검색 및 탐색
+ 패널을 사용하여에서 AEM에 있는 에셋의 파일 세부 정보에 액세스합니다.
   + 썸네일
   + 기본 메타데이터
   + 버전
+ 자산을 레이아웃에 배치, 다운로드 또는 드래그 앤 드롭
+ AEM에서 자산을 체크아웃하고 Creative Cloud Assets 계정 내에서 WIP(작업 중)를 통해 자산을 수정합니다
+ 에셋 수정을 완료한 후 AEM으로 다시 돌아가 새 버전이 AEM에 반영되는지 확인합니다
+ Adobe Asset Link 인앱 패널에서 AEM의 자산을 검색합니다
+ Asset Link 패널에서 직접 AEM Assets 컬렉션 및 스마트 컬렉션을 찾아보십시오
+ 새로 만든 에셋을 패널에서 AEM에 바로 추가
+ 에셋을 InDesign 프레임으로 직접 드래그 앤 드롭

## InDesign에 자산 배치

Adobe Asset Link는 Adobe Asset Link와 AEM 간의 InDesign 직접 연결 지원을 제공합니다. InDesign 직접 연결 지원을 사용하면 Adobe Asset Link 패널을 통해 AEM에서 InDesign으로 디지털 에셋을 배치(__연결 배치__ 또는 __복사 배치__)하거나 드래그하여 놓을 수 있습니다. 또한 *For Placement Only+(FPO) 표현물을 소개합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Adobe Creative Cloud Enterprise ID 또는 Federated ID만 사용하십시오. [Adobe Asset Link용 AEM을 구성](https://helpx.adobe.com/kr/enterprise/using/adobe-asset-link.html)했는지 확인하십시오.

아래 옵션 중 하나를 사용하여 InDesign 레이아웃에 자산을 배치할 수 있습니다.

+ **복사 배치** - 에셋을 포함하는 경우(복사 배치 옵션 사용) 로컬 시스템에 바이너리를 다운로드한 후 원본 에셋의 복사본을 InDesign 레이아웃에 배치합니다. Adobe Asset Link는 임베드된 사본과 원본 에셋 간의 링크를 유지 관리하지 않습니다. 원본 에셋이 AEM에서 수정된 경우, InDesign 파일에서 임베드된 에셋을 삭제하고 AEM에서 에셋을 다시 임베드해야 합니다.

+ **연결된 자산 배치** - InDesign 문서를 사용할 때 컨텍스트 메뉴에서 [복사 배치] 옵션을 사용하여 자산을 직접 포함할 뿐만 아니라 AEM에서 자산을 참조할 수 있는 옵션이 있습니다. 에셋을 참조하면 다른 사용자와 공동 작업을 수행하고 AEM의 원래 에셋에 대한 모든 업데이트를 통합할 수 있습니다. AEM의 에셋을 참조하려면 컨텍스트 메뉴에서 링크 배치 옵션을 사용합니다.

### 배치 이미지만

Adobe Asset Link를 사용하여 큰 에셋 파일을 AEM에서 InDesign 문서에 배치하면 크리에이티브 사용자는 배치 작업을 시작한 후 몇 초 동안 기다려야 합니다. 이는 전체 사용자 경험에 영향을 줍니다. Adobe Asset Link를 사용하면 AEM에서 원본 에셋의 저해상도 이미지를 일시적으로 배치할 수 있으므로 이미지를 배치하는 데 걸리는 시간을 줄일 수 있습니다. 동시에 전반적인 사용자 경험과 생산성을 높입니다. 저해상도 이미지는 임시로 배치되므로 인쇄 또는 게시에 최종 출력이 필요한 경우 FPO 렌디션을 원본으로 교체해야 합니다. 여러 FPO 이미지를 각각의 원본 이미지로 바꾸려면 **_Windows > 링크_** 패널로 이동한 다음 원본 에셋을 다운로드합니다. 원본 이미지가 다운로드되면 [모든 FPO를 원본으로 바꾸기]를 선택합니다.

FPO 렌디션은 원본 에셋을 소규모로 대체한 것입니다. 동일한 종횡비를 가지지만 원본 이미지에 비해 크기가 작습니다. 현재 InDesign에서는 다음 이미지 유형에 대해서만 FPO 렌디션 가져오기를 지원합니다.

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

AEM의 특정 에셋에 FPO 렌디션을 사용할 수 없는 경우 원본 고해상도 에셋이 대신 참조됩니다. FPO 이미지의 경우 InDesign 링크 패널에 FPO 상태가 표시됩니다.

## AEM Assets을 사용한 Adobe Asset Link 인증

Adobe IMS(Adobe Identity Management Services) 및 Adobe Experience Manager Author의 컨텍스트에서 Asset Link 인증이 작동하는 방식.

![Adobe Asset Link 아키텍처](assets/adobe-asset-link-article-understand.png)

1. Adobe Asset Link 확장은 Adobe Creative Cloud Desktop App을 통해 Adobe IMS(Identity Manage Service)에 승인을 요청하고 성공 시 전달자 토큰을 받습니다.
1. Adobe Asset Link 확장은 스키마(HTTP/HTTPS), 확장의 설정 JSON에 제공된 호스트 및 포트를 사용하여 **단계 1**&#x200B;에서 얻은 전달자 토큰을 포함하여 HTTP를 통해 AEM 작성자에 연결합니다.
1. AEM의 전달자 인증 핸들러가 요청에서 전달자 토큰을 추출하여 Adobe IMS로 확인합니다.
1. Adobe IMS에서 전달자 토큰의 유효성을 검사하면 사용자가 AEM에 만들어지고(아직 없는 경우) Adobe IMS의 프로필 및 그룹/멤버십 데이터를 동기화합니다. AEM 사용자에게 표준 AEM 로그인 토큰이 발급되며, 이 토큰은 HTTP(S) 응답의 쿠키로 Adobe Asset Link 확장에 다시 전송됩니다.
1. 후속 상호 작용(예: Adobe Asset Link 확장을 사용하여 에셋을 검색, 검색, 체크인/체크아웃 등)하면 표준 AEM 토큰 인증 핸들러를 사용하여 AEM 로그인 토큰을 사용하여 검증된 AEM Author에 대한 HTTP(S) 요청이 발생합니다.

>[!NOTE]
>
>로그인 토큰이 만료되면 **단계 1~5**&#x200B;이(가) 자동으로 호출되고, 전달자 토큰을 사용하여 Adobe Asset Link 확장을 인증하며, 유효한 새 로그인 토큰을 다시 발급합니다.

## 추가 리소스

+ [Adobe Asset Link 웹 사이트](https://www.adobe.com/kr/creativecloud/business/enterprise/adobe-asset-link.html)
