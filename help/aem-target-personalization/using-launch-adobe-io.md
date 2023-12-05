---
title: Experience Platform Launch 및 Adobe Developer을 사용하여 Adobe Experience Manager과 Adobe Target 통합
description: Experience Platform Launch 및 Adobe Developer을 사용하여 Adobe Experience Manager을 Adobe Target과 통합하는 방법에 대한 단계별 안내
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 748
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1021'
ht-degree: 4%

---

# Adobe Developer 콘솔을 통해 Adobe Experience Platform Launch 사용

## 사전 요구 사항

* [AEM 작성자 및 게시 인스턴스](./implementation.md#set-up-aem) localhost 포트 4502 및 4503에서 각각 실행
* **Experience Cloud**
   * 조직에 대한 액세스 Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 다음 솔루션으로 Experience Cloud 프로비저닝
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe Developer 콘솔](https://developer.adobe.com/console/)

     >[!NOTE]
     >Launch에서 개발, 승인, 게시, 확장 관리 및 환경 관리 권한이 있어야 합니다. 사용자 인터페이스 옵션을 사용할 수 없어서 이러한 단계를 완료할 수 없는 경우 Experience Cloud 관리자에게 연락하여 액세스 권한을 요청하십시오. Launch 권한에 대한 자세한 내용은 [설명서 참조](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).

* **브라우저 플러그인**
   * Adobe Experience Cloud Debugger ([크롬](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob))
   * Launch 및 DTM 스위치([크롬](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 관련 사용자

이 통합을 위해 다음 대상이 포함되어야 하며 일부 작업을 수행하려면 관리 액세스 권한이 필요할 수 있습니다.

* 개발자
* AEM 관리자
* Experience Cloud 관리자

## 소개

AEM은 Experience Platform Launch와의 획기적인 통합 기능을 제공합니다. 이 통합을 통해 AEM 관리자는 사용하기 쉬운 인터페이스를 통해 Experience Platform Launch을 쉽게 구성할 수 있으므로 이러한 두 도구를 구성할 때 노력 수준과 오류 수를 줄일 수 있습니다. Adobe Target 확장을 Experience Platform Launch에 추가하기만 하면 AEM 웹 페이지에서 Adobe Target의 모든 기능을 사용할 수 있습니다.

이 섹션에서는 다음 통합 단계를 다룹니다.

* 시작
   * Launch 속성 만들기
   * Target 확장 추가
   * 데이터 요소 만들기
   * 페이지 규칙 만들기
   * 환경 설정
   * 빌드 및 게시
* AEM
   * Cloud Service 만들기
   * 만들기

### 시작

#### Launch 속성 만들기

속성은 사이트에 태그를 배포할 때 확장, 규칙, 데이터 요소 및 라이브러리로 채우는 컨테이너입니다.

1. 내 조직으로 이동 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
2. Adobe ID을 사용하여 로그인하고 올바른 조직에 속해 있는지 확인하십시오.
3. 솔루션 전환기에서 을 클릭합니다. **시작** 을(를) 선택한 다음 **Launch로 이동** 단추를 클릭합니다.

   ![Experience Cloud - 시작](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 올바른 조직에 속해 있는지 확인한 다음 Launch 속성을 계속 만듭니다.
   ![Experience Cloud - 시작](assets/using-launch-adobe-io/launch-create-property.png)

   *속성 만들기에 대한 자세한 내용은 [속성 만들기](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) 를 참조하십시오.*
5. 을(를) 클릭합니다 **새 속성** 단추
6. 속성의 이름을 입력합니다(예: *AEM Target 자습서*)
7. 도메인으로 을 입력합니다. *localhost.com* WKND 데모 사이트가 실행 중인 도메인입니다. 비록 &#39;*도메인*&#39; 필드는 필수입니다. Launch 속성은 이 필드가 구현된 모든 도메인에서 작동합니다. 이 필드의 기본 목적은 규칙 빌더에서 메뉴 옵션을 미리 채우는 것입니다.
8. 다음을 클릭합니다. **저장** 단추를 클릭합니다.

   ![Launch - 새 속성](assets/using-launch-adobe-io/exc-launch-property.png)

9. 방금 만든 속성을 열고 확장 탭을 클릭합니다.

#### Target 확장 추가

Adobe Target 확장은 최신 웹, `at.js`. 여전히 Target 이전 라이브러리를 사용하는 고객, `mbox.js`, [at.js로 업그레이드해야 함](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) Launch를 사용합니다.

Target 확장은 다음 두 가지 주요 부분으로 구성됩니다.

* 코어 라이브러리 설정을 관리하는 확장 구성
* 규칙 작업: 다음을 수행합니다.
   * Target 로드(at.js)
   * 모든 Mbox에 매개 변수 추가
   * 글로벌 Mbox에 매개 변수 추가
   * 글로벌 Mbox 실행

1. 아래 **확장**&#x200B;에서는 Launch 속성에 대해 이미 설치된 확장 의 목록을 볼 수 있습니다. ([Experience Platform Launch 코어 확장](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) 기본적으로 설치됨)
2. 을(를) 클릭합니다 **확장 카탈로그** 을 클릭하고 필터에서 Target을 검색합니다.
3. 최신 버전의 Adobe Target at.js를 선택하고 **설치** 옵션을 선택합니다.
   ![Launch -새 속성](assets/using-launch-adobe-io/launch-target-extension.png)

4. 클릭 **구성** 을 누르면 가져온 Target 계정 자격 증명이 포함된 구성 창이 표시되고 이 확장에 대한 at.js 버전이 표시됩니다.
   ![Target - 확장 구성](assets/using-launch-adobe-io/launch-target-extension-2.png)

   비동기 Launch 포함 코드를 통해 Target이 배포되면 콘텐츠 깜박임을 관리하기 위해 Launch 포함 코드 앞에 페이지에 사전 숨김 코드 조각을 하드 코딩해야 합니다. 나중에 사전 숨김 스니퍼에 대해 자세히 알아보겠습니다. 사전에 숨기는 코드 조각을 다운로드할 수 있습니다 [여기](assets/using-launch-adobe-io/prehiding.js)

5. 클릭 **저장** Launch 속성에 Target 확장 프로그램 추가를 완료하면 이제 아래의 Target 확장 프로그램을 확인할 수 있습니다. **설치됨** 확장 목록입니다.

6. 위의 단계를 반복하여 &quot;Experience Cloud ID 서비스&quot; 확장을 검색하고 설치하십시오.
   ![확장 - Experience Cloud ID 서비스](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 환경 설정

1. 을(를) 클릭합니다 **환경** 를 탭으로 설정하고, 사이트 속성에 대해 만들어지는 환경 목록을 볼 수 있습니다. 기본적으로 개발, 스테이징 및 프로덕션을 위해 각각 하나의 인스턴스가 생성됩니다.

![데이터 요소 - 페이지 이름](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 빌드 및 게시

1. 을(를) 클릭합니다 **게시** 사이트 속성에 대한 탭으로, 라이브러리를 만들어 변경 사항(데이터 요소, 규칙)을 개발 환경에 배포해 보겠습니다.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 개발의 변경 사항을 스테이징 환경에 게시합니다.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 실행 **스테이징용 빌드 옵션**.
4. 빌드가 완료되면 를 실행합니다. **게시를 위해 승인**: 스테이징 환경에서 프로덕션 환경으로 변경 사항을 이동합니다.
   ![프로덕션에 스테이징](assets/using-launch-adobe-io/build-staging.png)
5. 마지막으로 **빌드 및 프로덕션에 게시** 옵션을 사용하여 변경 사항을 프로덕션에 푸시할 수 있습니다.
   ![빌드 및 프로덕션에 게시](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Adobe Developer 통합에 적절한 권한이 있는 작업 공간을 선택할 수 있는 액세스 권한을 부여합니다 [중앙 팀이 소수의 작업 영역에서만 API 기반 변경을 수행할 수 있도록 하는 역할](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Adobe Developer의 자격 증명을 사용하여 AEM에서 IMS 통합을 만듭니다. (01:12~03:55)
2. Experience Platform Launch에서 속성을 만듭니다. (적용) [위](#create-launch-property))
3. 1단계의 IMS 통합을 사용하여 Experience Platform Launch 통합을 만들어 Launch 속성을 가져옵니다.
4. AEM에서 브라우저 구성을 사용하여 Experience Platform Launch 통합을 사이트에 매핑합니다. (05:28~06:14)
5. 통합의 유효성을 수동으로 검사합니다. (06:15~06:33)
6. Launch/DTM 브라우저 플러그인 사용. (06:34~06:50)
7. Adobe Experience Cloud Debugger 브라우저 플러그인 사용. (06:51~07:22)

이 시점에서 을(를) 통합했습니다. [Adobe Experience Platform Launch을 사용하여 Adobe Target을 사용하는 AEM](./using-aem-cloud-services.md#integrating-aem-target-options) 옵션 1에 자세히 설명되어 있습니다.

AEM Experience Fragment 오퍼를 사용하여 개인화 활동을 향상시키려면 다음 장으로 이동하고 레거시 클라우드 서비스를 사용하여 AEM을 Adobe Target과 통합합니다.
