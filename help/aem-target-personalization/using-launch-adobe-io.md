---
title: Experience Platform Launch 및 Adobe I/O을 사용한 Adobe Experience Manager과 Adobe Target 통합
seo-title: Experience Platform Launch 및 Adobe I/O을 사용한 Adobe Experience Manager과 Adobe Target 통합
description: Experience Platform Launch 및 Adobe I/O을 사용하여 Adobe Experience Manager과 Adobe Target을 통합하는 방법 살펴보기
seo-description: Experience Platform Launch 및 Adobe I/O을 사용하여 Adobe Experience Manager과 Adobe Target을 통합하는 방법 살펴보기
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 2%

---


# Adobe I/O 콘솔을 통해 Adobe Experience Platform Launch 사용

## 전제 조건

* [로컬 호스트 포트 4502 및 ](./implementation.md#set-up-aem) 4503에 AEM 작성자 및 게시 설치
* **Experience Cloud**
   * 조직 Adobe Experience Cloud 액세스 - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 다음 솔루션으로 제공된 Experience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O 콘솔](https://console.adobe.io)

      >[!NOTE]
      >Launch에서 확장 기능을 개발, 승인, 게시, 관리 및 관리할 수 있는 권한이 있어야 합니다. 사용자 인터페이스 옵션을 사용할 수 없어 이러한 단계를 완료할 수 없는 경우 Experience Cloud 관리자에게 문의하여 액세스 권한을 요청하십시오. 시작 권한에 대한 자세한 내용은 [설명서](https://docs.adobelaunch.com/administration/user-permissions)를 참조하십시오.


* **브라우저 플러그인**
   * Adobe Experience Cloud 디버거([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * 시작 및 DTM 스위치([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 관련된 사용자

이러한 통합을 위해 다음 대상이 관련되어 있고 일부 작업을 수행하려면 관리자 액세스 권한이 필요할 수 있습니다.

* 개발자
* AEM 관리자
* Experience Cloud 관리자

## 소개

AEM은 Experience Platform Launch과 간편하게 통합할 수 있습니다. 이 통합을 통해 AEM 관리자는 사용하기 쉬운 인터페이스를 통해 Experience Platform Launch을 쉽게 구성할 수 있으므로 이러한 두 도구를 구성할 때 작업 수준과 오류 수를 줄일 수 있습니다. 또한 Experience Platform Launch에 Adobe Target 익스텐션을 추가하면 AEM 웹 페이지에서 Adobe Target의 모든 기능을 사용할 수 있습니다.

이 섹션에서는 다음 통합 단계를 다룹니다.

* 시작
   * 론치 속성 만들기
   * 타겟 확장 추가
   * 데이터 요소 만들기
   * 페이지 규칙 만들기
   * 설정 환경
   * 빌드 및 게시
* AEM
   * Cloud Service 만들기
   * 만들기

### 시작

#### 론치 속성 만들기

속성은 사이트에 태그를 배포할 때 확장 기능, 규칙, 데이터 요소 및 라이브러리로 채우는 컨테이너입니다.

1. 조직 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)으로 이동합니다.
2. Adobe ID을 사용하여 로그인하고 올바른 조직에 속해 있는지 확인하십시오.
3. 솔루션 전환기에서 **Launch**&#x200B;을 클릭한 다음 **Go To Launch** 단추를 선택합니다.

   ![Experience Cloud - 실행](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 올바른 조직에 있는지 확인한 다음 론치 속성 만들기를 계속합니다.
   ![Experience Cloud - 실행](assets/using-launch-adobe-io/launch-create-property.png)

   *속성 만들기에 대한 자세한 내용은 제품 설명서의  [속성 ](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) 만들기를 참조하십시오.*
5. **새 속성** 단추를 클릭합니다.
6. 속성의 이름을 입력합니다(예: *AEM Target 자습서*).
7. 도메인으로 WKND 데모 사이트가 실행 중인 도메인이므로 *localhost.com*&#x200B;을 입력합니다. &#39;*도메인*&#39; 필드가 필요하지만 Launch 속성은 구현된 모든 도메인에서 작동합니다. 이 필드의 기본 목적은 규칙 빌더에서 메뉴 옵션을 미리 채우는 것입니다.
8. **저장** 단추를 클릭합니다.

   ![론치 - 새 속성](assets/using-launch-adobe-io/exc-launch-property.png)

9. 방금 만든 속성을 열고 확장 탭을 클릭합니다.

#### 타겟 확장 추가

Adobe Target 익스텐션은 최신 웹인 `at.js`용 Target JavaScript SDK를 사용하여 클라이언트측 구현을 지원합니다. 여전히 Target 이전 라이브러리인 `mbox.js`, [을 사용하고 있는 고객은 Launch를 사용하려면 at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html)로 업그레이드해야 합니다.

Target 익스텐션은

* 핵심 라이브러리 설정을 관리하는 확장 구성
* 다음을 수행할 규칙 작업:
   * Target 로드(at.js)
   * 모든 mbox에 매개 변수 추가
   * 전역 mbox에 매개 변수 추가
   * Fire Global Mbox

1. **확장**&#x200B;에서 Launch 속성에 이미 설치되어 있는 확장 프로그램 목록을 볼 수 있습니다. ([Experience Platform Launch 코어 익스텐션](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html)이(가) 기본적으로 설치됨)
2. **확장 카탈로그** 옵션을 클릭하고 필터에서 Target을 검색합니다.
3. 최신 버전의 Adobe Target at.js를 선택하고 **설치** 옵션을 클릭합니다.
   ![Launch -New 속성](assets/using-launch-adobe-io/launch-target-extension.png)

4. **구성** 단추를 클릭하면 Target 계정 자격 증명을 가져온 구성 창과 이 확장의 at.js 버전이 있는 것을 확인할 수 있습니다.
   ![Target - 확장 구성](assets/using-launch-adobe-io/launch-target-extension-2.png)

   비동기 론치 포함 코드를 통해 Target이 배포되는 경우 컨텐츠 깜박임을 관리하기 위해 론치 포함 코드 전에 페이지에 미리 숨겨진 코드 조각을 하드 코딩해야 합니다. 우리는 나중에 숨기 전에 스나입니퍼에 대해 더 많이 배울 것이다. 사전 숨겨진 코드 조각 [여기](assets/using-launch-adobe-io/prehiding.js)를 다운로드할 수 있습니다.

5. **저장**&#x200B;을 클릭하여 Target 확장 기능을 Launch 속성에 추가를 완료하면 이제 **Installed** 확장 목록 아래에 나열된 Target 확장을 볼 수 있습니다.

6. 위의 단계를 반복하여 &quot;Experience Cloud ID 서비스&quot; 익스텐션을 검색하고 설치합니다.
   ![확장 - Experience Cloud ID 서비스](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 설정 환경

1. 사이트 속성에 대한 **환경** 탭을 클릭하면 사이트 속성에 대해 생성되는 환경 목록을 볼 수 있습니다. 기본적으로 개발, 스테이징 및 프로덕션을 위해 각각 하나의 인스턴스가 만들어집니다.

![데이터 요소 - 페이지 이름](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 빌드 및 게시

1. 사이트 속성에 대한 **게시** 탭을 클릭하고, 빌드할 라이브러리를 만들어 변경 사항(데이터 요소, 규칙)을 개발 환경에 배포합니다.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 개발의 변경 사항을 스테이징 환경에 게시합니다.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. **스테이징용 빌드 옵션**&#x200B;을 실행합니다.
4. 빌드가 완료되면 **게시 승인**을 실행합니다. 그러면 변경 내용이 스테이징 환경에서 프로덕션 환경으로 이동합니다.
   ![프로덕션 스테이징](assets/using-launch-adobe-io/build-staging.png)
5. 마지막으로 **제작 및 제작**에 게시 옵션을 실행하여 프로덕션 변경 내용을 푸시합니다.
   ![제작 및 제작 게시](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 중앙 팀이 몇 개의 작업 영역](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)에서만 API 기반 변경을 수행할 수 있도록 적절한 [역할이 있는 일부 작업 영역에 대한 액세스 권한을 Adobe I/O과 통합하여 부여합니다.

1. Adobe I/O의 자격 증명을 사용하여 AEM에서 IMS 통합을 만듭니다(01:12~03:55).
2. Experience Platform Launch에서 속성을 만듭니다. (위 [위](#create-launch-property) 포함)
3. 1단계에서 IMS 통합을 사용하여 Experience Platform Launch 통합을 만들어 Launch 속성을 가져옵니다.
4. AEM에서 브라우저 구성을 사용하여 Experience Platform Launch 통합을 사이트에 매핑합니다. (05:28 ~ 06:14)
5. 통합 유효성을 수동으로 확인합니다. (06:15 ~ 06:33)
6. Launch/DTM 브라우저 플러그인 사용. (06:34 ~ 06:50)
7. Adobe Experience Cloud 디버거 브라우저 플러그인 사용 (06:51 ~ 07:22)

이 시점에 옵션 1에 자세히 설명된 대로 Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options)을 사용하여 [AEM을 Adobe Target과 통합했습니다.

AEM 경험 조각 오퍼를 사용하여 개인화 활동을 강화하려면 다음 장으로 진행하여 레거시 클라우드 서비스를 사용하여 AEM을 Adobe Target과 통합하십시오.
