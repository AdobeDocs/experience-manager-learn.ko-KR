---
title: AEM Sites과 Adobe Target 통합
description: 다양한 시나리오를 위해 Adobe Target과 함께 Adobe Experience Manager을 설정하는 방법을 다루는 문서입니다.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# AEM Sites과 Adobe Target 통합

이 섹션에서는 다양한 시나리오에 대해 Adobe Target을 사용하여 Adobe Experience Manager Sites을 설정하는 방법에 대해 설명합니다. 시나리오 및 조직 요구 사항 기반.

* **Adobe Target JavaScript 라이브러리 추가(모든 시나리오에 필요)**
AEM에서 호스팅되는 사이트의 경우 [Adobe Experience Platform의 태그](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ko)를 사용하여 Target 라이브러리를 사이트에 추가할 수 있습니다. 태그는 관련 고객 환경을 향상하는 데 필요한 모든 태그를 배포하고 관리하는 간단한 방법을 제공합니다.
* **Adobe Target Cloud Service 추가(경험 조각 시나리오에 필요)**
경험 조각 오퍼를 사용하여 Adobe Target 내에서 활동을 만들려는 AEM 고객의 경우 이전 Cloud Service을 사용하여 Adobe Target을 AEM과 통합해야 합니다. 이 통합은 AEM에서 Target으로 경험 조각을 HTML/JSON 오퍼로 푸시하고 오퍼를 AEM과 동기화 상태로 유지하는 데 필요합니다. *시나리오 1을 구현하려면 이 통합이 필요합니다.*

## 사전 요구 사항

* **Adobe Experience Manager(AEM){#aem}**
   * AEM 6.5(*최신 서비스 팩이 권장됨*)
   * AEM WKND 참조 사이트 패키지 다운로드
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [핵심 구성 요소](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [디지털 데이터 레이어](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 조직에 대한 액세스 권한 Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 다음 솔루션으로 프로비저닝된 Experience Cloud
      * [데이터 수집](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O 콘솔](https://console.adobe.io)

* **환경**
   * Java 1.8 또는 Java 11(AEM 6.5+ 전용)
   * Apache Maven (3.3.9 이상)
   * Chrome

>[!NOTE]
>
> 고객에게 [Adobe 지원](https://helpx.adobe.com/kr/contact/enterprise-support.ec.html)에서 데이터 수집 및 Adobe I/O을 제공하거나 시스템 관리자에게 연락해야 합니다.

### AEM 설정{#set-up-aem}

이 자습서를 완료하려면 AEM 작성자 및 게시 인스턴스가 필요합니다. 작성자 인스턴스가 `http://localhost:4502`에서 실행 중이고 게시 인스턴스가 `http://localhost:4503`에서 실행 중입니다. 자세한 내용은 [로컬 AEM 개발 환경 설정](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)을 참조하십시오.

#### AEM 작성자 및 Publish 인스턴스 설정

1. [AEM Quickstart Jar 및 라이선스 복사본을 가져옵니다.](https://helpx.adobe.com/kr/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 다음과 같이 컴퓨터에 폴더 구조를 만듭니다.
   ![폴더 구조](assets/implementation/aem-setup-1.png)
3. Quickstart jar 이름을 `aem-author-p4502.jar`(으)로 바꾸고 `/author` 디렉터리 아래에 놓습니다. `/author` 디렉터리 아래에 `license.properties` 파일을 추가합니다.
   ![AEM 작성자 인스턴스](assets/implementation/aem-setup-author.png)
4. Quickstart jar를 복사하고 이름을 `aem-publish-p4503.jar`(으)로 바꾸고 `/publish` 디렉터리 아래에 놓습니다. `/publish` 디렉터리 아래에 `license.properties` 파일의 복사본을 추가합니다.
   ![AEM Publish 인스턴스](assets/implementation/aem-setup-publish.png)
5. 작성자 인스턴스를 설치하려면 `aem-author-p4502.jar` 파일을 두 번 클릭하십시오. 이렇게 하면 로컬 컴퓨터의 포트 4502에서 실행되는 작성자 인스턴스가 시작됩니다.
6. 아래의 자격 증명을 사용하여 로그인하면 AEM 홈 페이지 화면으로 이동합니다.
사용자 이름: **관리자**
암호: **관리자**
   ![AEM Publish 인스턴스](assets/implementation/aem-author-home-page.png)
7. 게시 인스턴스를 설치하려면 `aem-publish-p4503.jar` 파일을 두 번 클릭하십시오. 브라우저에서 열린 게시 인스턴스의 새 탭이 포트 4503에서 실행되고 WeRetail 홈 페이지가 표시되는 것을 확인할 수 있습니다. 이 자습서에서는 WKND 참조 사이트를 사용하고 있으며 작성자 인스턴스에 패키지를 설치하겠습니다.
8. 웹 브라우저(`http://localhost:4502`)에서 AEM 작성자로 이동합니다. AEM 시작 화면에서 *[도구 > 배포 > 패키지](http://localhost:4502/crx/packmgr/index.jsp)*(으)로 이동합니다.
9. AEM용 패키지 다운로드 및 업로드(*[사전 요구 사항 > AEM](#aem)* 아래에 나열됨)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. AEM 작성자에 패키지를 설치한 후 AEM 패키지 관리자에서 업로드된 각 패키지를 선택하고 **자세히 > 복제**&#x200B;를 선택하여 패키지가 AEM Publish에 배포되도록 합니다.
11. 이 시점에서 이 자습서에 필요한 WKND 참조 사이트 및 모든 추가 패키지를 정상적으로 설치했습니다.

[다음 챕터](./using-launch-adobe-io.md): 다음 챕터에서 태그를 AEM과 통합합니다.
