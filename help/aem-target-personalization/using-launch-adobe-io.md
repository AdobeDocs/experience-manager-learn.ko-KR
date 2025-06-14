---
title: 태그 및 Adobe Developer을 사용하여 Adobe Experience Manager과 Adobe Target 통합
description: 태그 및 Adobe Developer을 사용하여 Adobe Experience Manager을 Adobe Target과 통합하는 방법에 대한 단계별 안내
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 663
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 2%

---

# Adobe Developer Console을 통해 태그 사용

## 사전 요구 사항

* localhost 포트 4502 및 4503에서 각각 실행되는 [AEM 작성자 및 게시 인스턴스](./implementation.md#set-up-aem)
* **Experience Cloud**
   * 조직에 대한 액세스 권한 Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 다음 솔루션으로 Experience Cloud 프로비저닝
      * [데이터 수집](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe Developer Console](https://developer.adobe.com/console/)

     >[!NOTE]
     >데이터 수집에서 개발, 승인, Publish, 확장 관리 및 환경 관리 권한이 있어야 합니다. 사용자 인터페이스 옵션을 사용할 수 없어서 이러한 단계를 완료할 수 없는 경우 Experience Cloud 관리자에게 연락하여 액세스 권한을 요청하십시오. 태그 권한에 대한 자세한 내용은 [설명서를 참조](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html?lang=ko)하십시오.

* **Chrome 브라우저 확장**
   * Adobe Experience Cloud Debugger(https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

## 관련 사용자

이 통합을 위해 다음 대상이 포함되어야 하며 일부 작업을 수행하려면 관리 액세스 권한이 필요할 수 있습니다.

* 개발자
* AEM 관리자
* Experience Cloud 관리자

## 소개

AEM은 태그와 즉시 통합됩니다. 이 통합을 통해 AEM 관리자는 사용하기 쉬운 인터페이스를 통해 태그를 쉽게 구성할 수 있으므로 이러한 두 도구를 구성할 때 노력 수준과 오류 수를 줄일 수 있습니다. 태그에 Adobe Target 확장 기능을 추가하는 것만으로도 AEM 웹 페이지에서 Adobe Target의 모든 기능을 사용하는 데 도움이 됩니다.

이 섹션에서는 다음 통합 단계를 다룹니다.

* 태그
   * 태그 속성 만들기
   * Target 확장 추가
   * 데이터 요소 만들기
   * 페이지 규칙 만들기
   * 환경 설정
   * 빌드 및 게시
* AEM
   * Cloud Service 만들기
   * 만들기

### 태그

#### 태그 속성 만들기

속성은 사이트에 태그를 배포할 때 확장, 규칙, 데이터 요소 및 라이브러리로 채우는 컨테이너입니다.

1. 조직 [Adobe Experience Cloud](https://experiencecloud.adobe.com/)(`https://<yourcompany>.experiencecloud.adobe.com`)으로 이동
1. Adobe ID을 사용하여 로그인하고 올바른 조직에 속해 있는지 확인하십시오.
1. 솔루션 전환기에서 **Experience Platform**&#x200B;을 클릭한 다음 **데이터 수집** 섹션을 클릭하고 **태그**&#x200B;를 선택합니다.

![Experience Cloud - 태그](assets/using-launch-adobe-io/exc-cloud-launch.png)

1. 올바른 조직에 속해 있는지 확인한 다음 태그 속성을 계속 만듭니다.
   ![Experience Cloud - 태그](assets/using-launch-adobe-io/launch-create-property.png)

   *속성 만들기에 대한 자세한 내용은 제품 설명서에서 [속성 만들기](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=ko#create-or-configure-a-property)를 참조하세요.*
1. **새 속성** 단추 클릭
1. 속성의 이름을 지정하십시오(예: *AEM Target 자습서*).
1. WKND 데모 사이트가 실행되고 있는 도메인이므로 도메인으로는 *localhost.com*&#x200B;을(를) 입력하십시오. &#39;*도메인*&#39; 필드는 필수이지만 태그 속성은 구현된 모든 도메인에서 작동합니다. 이 필드의 기본 목적은 규칙 빌더에서 메뉴 옵션을 미리 채우는 것입니다.
1. **저장** 단추를 클릭합니다.

   ![태그 - 새 속성](assets/using-launch-adobe-io/exc-launch-property.png)

1. 방금 만든 속성을 열고 확장 탭을 클릭합니다.

#### Target 확장 추가

Adobe Target 확장은 최신 웹 `at.js`에 Target JavaScript SDK를 사용하여 클라이언트측 구현을 지원합니다. 여전히 Target 이전 라이브러리인 `mbox.js`, [을(를) 사용하는 고객은 태그를 사용하려면 at.js로 업그레이드](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html?lang=ko)해야 합니다.

Target 확장은 다음 두 가지 주요 부분으로 구성됩니다.

* 코어 라이브러리 설정을 관리하는 확장 구성
* 규칙 작업: 다음을 수행합니다.
   * Target 로드(at.js)
   * 모든 Mbox에 매개 변수 추가
   * 글로벌 Mbox에 매개 변수 추가
   * 글로벌 Mbox 실행

1. **확장**&#x200B;에서 태그 속성에 대해 이미 설치된 확장 목록을 볼 수 있습니다. ([Adobe 실행 코어 확장](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension)이(가) 기본적으로 설치됨)
2. **확장 카탈로그** 옵션을 클릭하고 필터에서 Target을 검색합니다.
3. 최신 버전의 Adobe Target at.js를 선택하고 **설치** 옵션을 클릭합니다.
   ![태그 - 새 속성](assets/using-launch-adobe-io/launch-target-extension.png)

4. **구성** 단추를 클릭하면 가져온 Target 계정 자격 증명이 포함된 구성 창과 이 확장에 대한 at.js 버전이 표시됩니다.
   ![대상 - 확장 구성](assets/using-launch-adobe-io/launch-target-extension-2.png)

   비동기 태그 포함 코드를 통해 Target이 배포되면 컨텐츠 깜박임을 관리하기 위해 태그 포함 코드 앞에 페이지에 사전 숨김 코드 조각을 하드 코딩해야 합니다. 나중에 사전 숨김 스니퍼에 대해 자세히 알아보겠습니다. 사전 숨김 코드 조각 [여기](assets/using-launch-adobe-io/prehiding.js)를 다운로드할 수 있습니다.

5. **저장**&#x200B;을 클릭하여 Target 확장을 태그 속성에 추가합니다. 이제 **설치됨** 확장 목록에 나열된 Target 확장을 볼 수 있습니다.

6. 위의 단계를 반복하여 &quot;Experience Cloud ID 서비스&quot; 확장을 검색하고 설치하십시오.
   ![확장 - Experience Cloud ID 서비스](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 환경 설정

1. 사이트 속성에 대한 **환경** 탭을 클릭하면 사이트 속성에 대해 만들어지는 환경 목록을 볼 수 있습니다. 기본적으로 개발, 스테이징 및 프로덕션을 위해 각각 하나의 인스턴스가 생성됩니다.

![데이터 요소 - 페이지 이름](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 빌드 및 게시

1. 사이트 속성에 대한 **게시** 탭을 클릭하고 라이브러리를 만들어 변경 사항(데이터 요소, 규칙)을 개발 환경에 배포해 보겠습니다.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 개발에서 스테이징 환경으로의 변경 사항을 Publish 합니다.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. **스테이징용 빌드 옵션**&#x200B;을(를) 실행합니다.
4. 빌드가 완료되면 **게시용으로 승인**&#x200B;을 실행하여 변경 내용을 스테이징 환경에서 프로덕션 환경으로 이동합니다.
   ![프로덕션에 스테이징](assets/using-launch-adobe-io/build-staging.png)
5. 마지막으로 **프로덕션에 빌드 및 Publish** 옵션을 실행하여 변경 내용을 프로덕션에 푸시합니다.
   ![프로덕션에 빌드 및 Publish](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Adobe Developer 통합에 적절한 [역할이 있는 작업 영역을 선택할 수 있는 액세스 권한을 부여하여 중앙 팀이 일부 작업 영역에서만 API 기반 변경을 수행할 수 있도록 합니다](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html?lang=ko).

1. Adobe Developer의 자격 증명을 사용하여 AEM에서 IMS 통합을 만듭니다. (01:12~03:55)
2. 데이터 수집에서 속성을 만듭니다. ([위](#create-launch-property)에서 적용)
3. 1단계의 IMS 통합을 사용하여 태그 통합을 만들어 태그 속성을 가져옵니다.
4. AEM에서 브라우저 구성을 사용하여 태그 통합을 사이트에 매핑합니다. (05:28~06:14)
5. 통합의 유효성을 수동으로 검사합니다. (06:15~06:33)
6. Adobe Experience Cloud Debugger 브라우저 플러그인 사용. (06:51~07:22)

이 시점에서 옵션 1에 자세히 설명된 대로 태그를 사용하여 [AEM과 Adobe Target을 통합](./using-aem-cloud-services.md#integrating-aem-target-options)했습니다.

AEM Experience Fragment 오퍼를 사용하여 개인화 활동을 향상시키려면 다음 장으로 이동하고 레거시 클라우드 서비스를 사용하여 AEM을 Adobe Target과 통합합니다.
