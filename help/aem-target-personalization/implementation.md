---
title: Adobe Experience Manager과 Adobe Target 통합
seo-title: 개인화된 콘텐츠를 제공하기 위해 Adobe Experience Manager(AEM)을 Adobe Target과 통합하는 다양한 방법을 다루는 문서입니다.
description: 다양한 시나리오에 대해 Adobe Target과 함께 Adobe Experience Manager을 설정하는 방법을 다루는 문서입니다.
seo-description: 다양한 시나리오에 대해 Adobe Target과 함께 Adobe Experience Manager을 설정하는 방법을 다루는 문서입니다.
feature: 경험 구성요소
topic: 개인화
role: Developer
level: Intermediate
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '700'
ht-degree: 4%

---


# Adobe Experience Manager과 Adobe Target 통합

이 섹션에서는 다양한 시나리오에 대해 Adobe Target과 함께 Adobe Experience Manager을 설정하는 방법에 대해 설명합니다. 시나리오 및 조직 요구 사항을 기반으로 합니다.

* **Adobe Target JavaScript 라이브러리 추가(모든 시나리오에 필요)**
 AEM에서 호스팅되는 사이트의 경우  [Launch](https://experienceleague.adobe.com/docs/launch/using/home.html)를 사용하여 Target 라이브러리를 사이트에 추가할 수 있습니다. Launch는 관련 고객 환경을 향상하는 데 필요한 모든 태그를 배포하고 관리하는 간단한 방법을 제공합니다.
* **Adobe Target Cloud Services 추가 (경험 조각 시나리오에 필요)**
 경험 조각 오퍼를 사용하여 Adobe Target 내에서 활동을 만들려는 AEM 고객의 경우 기존 Cloud Services을 사용하여 Adobe Target을 AEM과 통합해야 합니다. AEM에서 HTML/JSON 오퍼로 Target에 경험 구성요소를 푸시하고 오퍼를 AEM과 계속 동기화하는 데 이 통합이 필요합니다. 
*이 통합은 시나리오 1을 구현하는 데 필요합니다.*

## 전제 조건

* **AEM(Adobe Experience Manager){#aem}**
   * AEM 6.5(*최신 서비스 팩이 권장됨*)
   * AEM WKND 참조 사이트 패키지 다운로드
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [코어 구성 요소](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [디지털 데이터 계층](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 조직 Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com에 액세스
   * 다음 솔루션으로 제공된 Experience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O 콘솔](https://console.adobe.io)

* **환경**
   * Java 1.8 또는 Java 11(AEM 6.5+만 해당)
   * Apache Maven(3.3.9 이상)
   * Chrome

>[!NOTE]
>
> 고객은 [Adobe 지원](https://helpx.adobe.com/kr/contact/enterprise-support.ec.html)에서 Experience Platform Launch 및 Adobe I/O을 공급받거나 시스템 관리자에게 문의해야 합니다

### AEM{#set-up-aem} 설정

이 자습서를 완료하려면 AEM 작성자 및 게시 인스턴스가 필요합니다. `http://localhost:4502`에서 실행 중인 작성자 인스턴스와 `http://localhost:4503`에서 실행 중인 게시 인스턴스가 있습니다. 자세한 내용은 다음을 참조하십시오.[로컬 AEM 개발 환경 설정](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)

#### AEM 작성자 및 게시 인스턴스 설정

1. [AEM Quickstart Jar의 사본과 라이센스를 가져옵니다.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 다음과 같이 컴퓨터에 폴더 구조를 만듭니다.
   ![폴더 구조](assets/implementation/aem-setup-1.png)
3. Quickstart jar의 이름을 `aem-author-p4502.jar`로 변경하고 `/author` 디렉토리 아래에 배치합니다. `/author` 디렉토리 아래에 `license.properties` 파일을 추가합니다.
   ![AEM 작성자 인스턴스](assets/implementation/aem-setup-author.png)
4. Quickstart jar의 복사본을 만들고 이름을 `aem-publish-p4503.jar`로 변경하고 `/publish` 디렉토리 아래에 배치합니다. `/publish` 디렉토리 아래에 `license.properties` 파일의 복사본을 추가합니다.
   ![AEM 게시 인스턴스](assets/implementation/aem-setup-publish.png)
5. `aem-author-p4502.jar` 파일을 두 번 클릭하여 작성자 인스턴스를 설치합니다. 로컬 컴퓨터의 포트 4502에서 실행되는 작성자 인스턴스가 시작됩니다.
6. 아래 자격 증명을 사용하여 로그인하면 로그인 시 AEM 홈 페이지 화면으로 이동합니다.
사용자 이름 :**admin**
암호 :**admin**
   ![AEM 게시 인스턴스](assets/implementation/aem-author-home-page.png)
7. 게시 인스턴스를 설치하려면 `aem-publish-p4503.jar` 파일을 두 번 클릭합니다. 브라우저에 게시 인스턴스에 대해 새 탭이 열려 있고 포트 4503에서 실행되며 WeRetail 홈 페이지가 표시됩니다. 이 자습서에서는 WKND 참조 사이트를 사용하고 작성 인스턴스에 패키지를 설치하겠습니다.
8. `http://localhost:4502`에 있는 웹 브라우저에서 AEM 작성자로 이동합니다. AEM 시작 화면에서 *[도구 > 배포 > 패키지](http://localhost:4502/crx/packmgr/index.jsp)*&#x200B;로 이동합니다.
9. AEM용 패키지를 다운로드하여 업로드합니다( *[사전 요구 사항 > AEM](#aem)* 아래 나열됨)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0 zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. AEM Author에 패키지를 설치한 후 AEM Package Manager에서 업로드된 각 패키지를 선택하고 **자세히 > Replicate** 를 선택하여 패키지가 AEM Publish에 배포되는지 확인합니다.
11. 이 시점에서 이 자습서에 필요한 WKND 참조 사이트와 모든 추가 패키지를 성공적으로 설치했습니다.

[다음 장](./using-launch-adobe-io.md):다음 장에서는 Launch와 AEM을 통합하게 됩니다.
