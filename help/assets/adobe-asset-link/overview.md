---
title: Adobe Asset Link 및 AEM
description: Adobe Experience Manager 자산은 디자이너와 크리에이티브 사용자가 선호하는 Adobe Creative Cloud 데스크탑 애플리케이션 내에서 사용할 수 있습니다. Adobe Creative Cloud for enterprise용 Adobe Asset Link 확장 기능은 Adobe XD, Photoshop, InDesign 및 Illustrator와 같은 Creative Cloud 도구 내에서 AEM 자산에 대한 검색 및 탐색, 정렬, 미리보기, 업로드, 체크아웃, 수정, 체크인 및 메타데이터 조회 기능을 확장합니다.
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
duration: 673
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '983'
ht-degree: 100%

---


# Adobe Asset Link 3.0

Adobe Experience Manager 자산은 디자이너와 크리에이티브 사용자가 선호하는 Adobe Creative Cloud 데스크탑 애플리케이션 내에서 사용할 수 있습니다.

Adobe Creative Cloud for enterprise용 Adobe Asset Link 확장 기능은 Creative Cloud 애플리케이션 내에서 AEM 자산에 대한 검색 및 탐색, 정렬, 미리보기, 업로드, 체크아웃, 수정, 체크인 및 메타데이터 조회 기능을 확장합니다.

## Adobe Asset Link 기능

+ Adobe Asset Link는 AEM Assets 및 Assets Essentials와 통합됩니다.
+ Adobe Asset Link는 클라우드 기반 AEM 환경(AEM Assets as a Cloud Service 및 Assets Essentials)에 대한 연결을 자동으로 구성합니다.
+ Adobe Asset Link는 Adobe Creative Cloud 애플리케이션 내에서 작동하는 확장 기능입니다.

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Adobe Enterprise ID 또는 Federated ID를 사용한 AEM 자동 인증
+ AEM 내 디지털 자산 탐색 및 검색
+ 패널 내에서 AEM에 저장된 자산의 파일 세부 정보에 액세스:
   + 썸네일
   + 기본 메타데이터
   + 버전
+ 자산을 레이아웃에 배치, 다운로드 또는 드래그 앤 드롭
+ AEM에서 자산을 체크아웃하고 Creative Cloud Assets 계정 내에서 작업(WIP)하여 자산 수정
+ 수정 완료 후 자산을 AEM에 체크인하면 새로운 버전이 AEM에 반영됨
+ Adobe Asset Link 인앱 패널에서 AEM 자산 검색
+ Asset Link 패널에서 AEM Assets 컬렉션 및 스마트 컬렉션 직접 탐색
+ 새로 생성된 자산을 패널에서 AEM에 직접 추가
+ 자산을 InDesign 프레임으로 직접 드래그 앤 드롭

## InDesign에 자산 배치

Adobe Asset Link는 Adobe Asset Link와 AEM 간의 InDesign 직접 연결 지원을 제공합니다. InDesign 직접 연결 지원을 통해 Adobe Asset Link 패널에서 AEM의 디지털 자산을 배치(__연결된 배치__ 또는 __사본 배치__)하거나 끌어다 놓아 InDesign에 삽입할 수 있습니다. 또한 *FPO(배치 전용) 렌디션을 소개합니다.

>[!VIDEO](https://video.tv.adobe.com/v/37237?quality=12&learn=on&captions=kor)

>[!NOTE]
>
>Adobe Creative Cloud Enterprise ID 또는 Federated ID만 사용하십시오. [AEM에서 Adobe Asset Link가 작동하도록 구성](https://helpx.adobe.com/kr/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)되어 있는지 확인하십시오.

아래 옵션 중 하나를 사용하여 InDesign 레이아웃에 자산을 배치할 수 있습니다.

+ **사본 배치** - 사본 배치 옵션을 사용하여 자산을 배치하면 바이너리 파일을 로컬 시스템에 다운로드한 후 원본 자산의 사본을 InDesign 레이아웃에 삽입합니다. Adobe Asset Link에서는 임베드된 사본과 원본 자산 간의 링크가 유지되지 않습니다. AEM에서 원본 자산을 수정한 경우 InDesign 파일에서 임베드된 자산을 삭제하고 AEM에서 자산을 다시 임베드해야 합니다.

+ **연결된 배치** - InDesign 문서 작업 시, 컨텍스트 메뉴의 사본 배치 옵션을 사용하여 자산을 직접 임베드하는 대신 AEM의 자산을 참조할 수 있습니다. 자산을 참조하면 다른 사용자와 공동 작업을 수행할 수 있으며, AEM의 원본 자산에 적용된 모든 업데이트를 통합할 수 있습니다. AEM에서 자산을 참조하려면 컨텍스트 메뉴에서 연결된 배치 옵션을 사용하십시오.

### 배치 전용 이미지

Adobe Asset Link를 사용하여 AEM에서 InDesign 문서로 대용량 자산 파일을 배치하는 경우, 크리에이티브 사용자는 배치 작업을 시작한 후 몇 초간 대기해야 합니다. 이는 전반적인 사용자 경험에 영향을 미칩니다. Adobe Asset Link는 AEM 원본 자산의 저해상도 이미지를 임시로 배치하여 이미지 배치 시간을 단축합니다. 동시에 전반적인 사용자 경험과 생산성이 향상됩니다. 저해상도 이미지는 일시적으로만 배치되며, 인쇄나 출판 등의 최종 출력이 필요한 경우에는 해당 FPO 렌디션을 원본으로 교체해야 합니다. 여러 개의 FPO 이미지를 각각의 원본 이미지로 바꾸려면 **_Windows > 링크_** 패널로 이동한 다음 원본 자산을 다운로드합니다. 원본 이미지를 다운로드한 후 “모든 FPO를 원본으로 바꾸기”를 선택합니다.

FPO 렌디션은 원본 자산의 간소화된 대체물입니다. 두 이미지의 종횡비는 동일하지만 원본 이미지에 비해 크기가 더 작습니다. 현재 InDesign에서는 다음 이미지 유형에 대해서만 FPO 렌디션 가져오기를 지원합니다.

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

AEM에서 특정 자산에 대한 FPO 렌디션을 사용할 수 없는 경우 원본 고해상도 자산이 대신 참조됩니다. FPO 이미지의 경우 InDesign 링크 패널에 FPO 상태가 표시됩니다.

## AEM Assets를 사용한 Adobe Asset Link 인증

Adobe IMS(Identity Management Services) 및 Adobe Experience Manager 작성자 인스턴스에서 Adobe Asset Link 인증이 작동하는 방식을 소개합니다.

![Adobe Asset Link 아키텍처](assets/adobe-asset-link-article-understand.png)

1. Adobe Asset Link 확장 기능은 Adobe Creative Cloud 데스크탑 앱을 통해 Adobe IMS(Identity Management Services)에 인증을 요청하고, 성공하면 전달자 토큰을 받습니다.
1. Adobe Asset Link 확장 기능은 확장 기능의 설정 JSON에서 제공된 스키마(HTTP/HTTPS), 호스트 및 포트를 사용하여 **1단계**&#x200B;에서 획득한 전달자 토큰을 포함하여 HTTP(S)를 통해 AEM 작성자 인스턴스에 연결합니다.
1. AEM의 전달자 인증 핸들러는 요청에서 전달자 토큰을 추출하고 Adobe IMS를 통해 해당 토큰을 검증합니다.
1. Adobe IMS가 전달자 토큰을 검증하면 AEM에 사용자가 생성되고(아직 존재하지 않는 경우), Adobe IMS에서 프로필 및 그룹/멤버십 데이터가 동기화됩니다. 이후 AEM 사용자는 표준 AEM 로그인 토큰을 발급받으며, 이 토큰은 HTTP(S) 응답의 쿠키로 Adobe Asset Link 확장 기능에 전달됩니다.
1. 이후 Adobe Asset Link 확장 기능과의 상호 작용(예: 자산 탐색, 검색, 체크인/체크아웃 등)은 표준 AEM 토큰 인증 핸들러를 사용하여, AEM 로그인 토큰으로 검증되는 HTTP(S) 요청을 AEM 작성자 인스턴스로 전송합니다.

>[!NOTE]
>
>로그인 토큰이 만료되면 위 **1~5단계**&#x200B;가 자동으로 호출되어, 전달자 토큰을 사용하여 Adobe Asset Link 확장 기능을 인증하고 새롭고 유효한 로그인 토큰을 다시 발급합니다.

## 추가 리소스

+ [Adobe Asset Link 웹 사이트](https://www.adobe.com/kr/creativecloud/business/enterprise/adobe-asset-link.html)
