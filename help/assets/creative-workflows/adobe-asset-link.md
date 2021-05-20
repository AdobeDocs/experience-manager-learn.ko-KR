---
title: AEM Assets에서 Adobe Asset Link Extension 사용
description: '이제 디자이너와 크리에이티브 사용자가 선호하는 Adobe Creative Cloud 데스크탑 애플리케이션 내에서 Adobe Experience Manager 자산을 사용할 수 있습니다. Adobe Creative Cloud Enterprise용 Adobe Asset Link 확장 기능은 Adobe Photoshop, InDesign 및 Illustrator과 같은 Creative Cloud 도구 내에서 AEM 자산의 메타데이터를 검색 및 탐색, 정렬, 미리 보기, 업로드, 체크아웃, 수정, 체크인 및 볼 수 있는 기능을 확장합니다. '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: 컨텐츠 관리
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 1%

---


# AEM Assets에서 Adobe Asset Link 확장 사용{#using-adobe-asset-link-extension-with-aem-assets}

이제 디자이너와 크리에이티브 사용자가 선호하는 Adobe Creative Cloud 데스크탑 애플리케이션 내에서 Adobe Experience Manager 자산을 사용할 수 있습니다. Adobe Creative Cloud Enterprise용 Adobe Asset Link 확장 기능은 Adobe Photoshop, InDesign 및 Illustrator과 같은 Creative Cloud 도구 내에서 AEM 자산의 메타데이터를 검색 및 탐색, 정렬, 미리 보기, 업로드, 체크아웃, 수정, 체크인 및 볼 수 있는 기능을 확장합니다.


## Adobe Asset Link 1.1

이제 Adobe Asset Link v1.1에서는 Adobe Asset Link와 AEM Assets 간에 InDesign 직접 연결 지원을 제공합니다. 이제 InDesign 직접 연결 지원을 통해 자산 링크 패널을 통해 AEM Assets의 InDesign에 디지털 자산을 배치(연결 또는 복사 배치)하거나 드래그 앤 드롭할 수 있습니다. 또한 에서는 *For Placement Only* (FPO) 변환을 도입합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Adobe Creative Cloud Enterprise ID 또는 Federated ID만 사용합니다. [Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)에 대해 AEM을 구성해야 합니다.


### Adobe 자산 링크 기능

* Adobe Asset Link는 PS, AI, ID 내에서 작동하며 AEM Assets에 있는 디지털 자산에 직접 액세스할 수 있는 확장입니다
* 광고 팀은 Adobe IMS Enterprise ID 또는 Federated ID을 사용하여 자동으로 AEM에 로그인됩니다
* 광고 팀은 AEM Assets 및 Creative Cloud 자산에서 검색하는 것 외에도 AEM Assets에 있는 디지털 자산을 검색할 수 있습니다
* 광고 팀은 AEM Assets에 있는 자산에 대한 파일 세부 사항에 액세스할 수 있습니다.패널 내에서 축소판, 기본 메타데이터 및 버전
* 광고 팀은 자산을 레이아웃에 배치, 다운로드 또는 드래그하여 놓을 수 있습니다
* 크리에이티브는 AEM Assets에서 자산을 확인하고 Creative Cloud 자산 계정 내에서 WIP(Assets)로 작업하여 자산을 수정할 수 있습니다
* 광고 팀은 자산 수정을 완료한 후 자산을 다시 AEM Assets에 확인할 수 있으며 새 버전이 AEM Assets에 반영됩니다
* Creative Cloud 2020, 2019 및 2018 InDesign, Photoshop 및 Illustrator 데스크탑 앱을 지원합니다
* 사용자는 Adobe 자산 링크 인앱 패널에서 자산 검색을 수행하고 크기, 알파벳순 및 관련성을 기준으로 정렬할 수 있습니다
* 사용자는 Asset Link 패널에서 직접 AEM Assets 컬렉션과 스마트 컬렉션을 액세스하고 검색할 수 있습니다
* 패널에서 AEM Assets에 바로 새로 만든 자산 추가
* InDesign 프레임으로 자산을 직접 끌어다 놓을 수 있습니다

### InDesign에 AEM Assets 배치

아래 옵션 중 하나를 사용하여 자산을 InDesign 레이아웃에 배치할 수 있습니다.

* **복사 배치**  - 자산을 포함(복사 배치 옵션 사용)하면 바이너리를 로컬 시스템에 다운로드한 후 원래 자산의 사본을 InDesign 레이아웃에 배치합니다. Adobe 자산 링크에서는 포함된 사본과 원래 자산 간의 링크가 유지되지 않습니다. 원본 자산이 AEM Assets에서 수정된 경우 InDesign 파일에서 포함된 자산을 삭제하고 AEM Assets에서 자산을 다시 포함해야 합니다.

* **연결 배치**  - InDesign 문서로 작업할 때 이제 자산을 직접 포함할 뿐 아니라 AEM Assets에서 자산을 참조할 수 있는 옵션이 제공됩니다(컨텍스트 메뉴에서 복사 배치 옵션 사용). 자산을 참조하면 다른 사용자와 공동 작업을 수행하고 AEM Assets에서 원래 자산에 대한 업데이트를 통합할 수 있습니다. AEM Assets에서 자산을 참조하려면 컨텍스트 메뉴에서 연결 배치 옵션을 사용합니다.

### 배치만(FPO) 해상도

Adobe Asset Link를 사용하여 AEM Assets에서 InDesign 문서에 대용량 자산 파일을 배치하는 경우 크리에이티브 사용자는 배치 작업을 시작한 후 몇 초 동안 기다려야 합니다. 이것은 전체 사용자 경험에 영향을 줍니다. 이제 Adobe 자산 링크를 사용하여 AEM Assets에서 원래 자산의 저해상도 이미지를 일시적으로 배치할 수 있으므로, 이미지를 배치하는 데 걸리는 시간을 줄일 수 있습니다. 동시에 전체 사용자 경험과 생산성을 높입니다. 저해상도 이미지가 일시적으로 배치되며, 인쇄하거나 게시하기 위해 최종 출력이 필요한 경우 FPO 변환을 원본으로 바꿔야 합니다. 여러 FPO 이미지를 각각의 원본 이미지로 바꾸려면 **_Windows > 링크_** 패널로 이동한 다음 원래 자산을 다운로드합니다. 원본 이미지를 다운로드한 후 모든 FPO의 원본을 원본으로 바꾸기를 선택합니다.

>[!NOTE]
>
> *FPO(Placement Only)*  표현물은 연결된 배치 옵션에만 작동합니다. 또한 AEM Assets *Dam 자산 업데이트* 워크플로우에서 FPO 변환 지원을 활성화해야 합니다.

FPO 표현물은 원래 자산의 가벼운 대체입니다. 종횡비는 동일하지만 원본 이미지보다 크기가 작습니다. 현재 InDesign은 다음 이미지 유형에 대한 FPO 표현물 가져오기만 지원합니다.

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

AEM Assets의 특정 자산에 FPO 렌디션을 사용할 수 없는 경우 원본 고해상도 자산을 대신 참조합니다. FPO 이미지의 경우 InDesign 링크 패널에 상태 FPO가 표시됩니다.

## AEM Assets을 사용한 Adobe 자산 링크 인증 이해{#understanding-adobe-asset-link-authentication-with-aem-assets}

IMS(Adobe Identity Management Services) 및 Adobe Experience Manager 작성자 컨텍스트에서 자산 링크 인증이 작동하는 방식.

![Adobe 자산 링크 아키텍처](assets/adobe-asset-link-article-understand.png)

[Adobe 자산 링크 아키텍처 다운로드](assets/adobe-asset-link-article-understand-1.png)

1. Adobe 자산 링크 확장은 Adobe Creative Cloud 데스크탑 앱을 통해 IMS(Identity Manager Service) Adobe에 대한 권한 부여를 요청하고, 성공하면 베어러 토큰을 받습니다.
2. Adobe Asset Link 확장은 확장의 설정 JSON에 제공된 스키마(HTTP/HTTPS)를 사용하여 **1단계에서 얻은 베어러 토큰을 포함하여 HTTP를 통해 AEM Author에 연결합니다.**
3. AEM Bearer Authentication Handler가 요청에서 Bearer 토큰을 추출하고 Adobe IMS에 대해 확인합니다.
4. Adobe IMS가 베어러 토큰을 확인하면, 사용자가 AEM에 만들어지고(아직 없는 경우) Adobe IMS에서 프로필 및 그룹/멤버십 데이터를 동기화합니다. AEM 사용자에게는 표준 AEM 로그인 토큰이 발급되며, 이 토큰은 HTTP(S) 응답에서 쿠키로 Adobe Asset Link 확장으로 다시 전송됩니다.
5. 후속 상호 작용(예: 자산 검색, 검색, 체크인/체크아웃 등) 자산 Adobe 링크 확장을 사용하면 표준 AEM 토큰 인증 핸들러를 사용하여 AEM 로그인 토큰을 사용하여 확인되는 AEM 작성자에게 HTTP(S) 요청이 발생합니다.

>[!NOTE]
>
>로그인 토큰이 만료되면 **단계 1-5**&#x200B;이 자동으로 호출되고, 베어러 토큰을 사용하여 Adobe 자산 링크 확장을 인증하고, 유효한 새 로그인 토큰을 다시 발행합니다.

## 추가 리소스{#additional-resources}

* [Adobe Asset Link 웹 사이트](https://www.adobe.com/kr/creativecloud/business/enterprise/adobe-asset-link.html)