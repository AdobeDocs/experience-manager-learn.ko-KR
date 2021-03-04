---
title: AEM에서 OKTA 구성
description: okta를 사용하여 단일 사인온을 사용하기 위한 다양한 구성 설정 이해
feature: 적응형 양식
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
topic: 관리
role: 관리자
level: 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 2%

---


# OKTA를 사용하여 AEM 작성자 인증

첫 번째 단계는 OKTA 포털에서 앱을 구성하는 것입니다. OKTA 관리자가 앱을 승인하면 IdP 인증서 및 SSO(Single Sign On) URL에 액세스할 수 있습니다. 다음은 새 응용 프로그램을 등록하는 데 일반적으로 사용되는 설정입니다.

* **응용 프로그램 이름:** 응용 프로그램 이름입니다. 애플리케이션에 고유한 이름을 지정해야 합니다.
* **SAML 수신자: OKTA에서** 인증 후 SAML 응답으로 AEM 인스턴스에서 히트할 URL입니다. SAML 인증 핸들러는 일반적으로 /saml_login을 사용하여 모든 URL을 가로채지만 응용 프로그램 루트 뒤에 추가하는 것이 좋습니다.
* **SAML 대상**:애플리케이션의 도메인 URL입니다. 도메인 URL에 프로토콜(http 또는 https)을 사용하지 마십시오.
* **SAML 이름 ID:** 드롭다운 목록에서 이메일을 선택합니다.
* **환경**:적합한 환경을 선택하십시오.
* **속성**:SAML 응답에서 사용자에 대해 얻는 속성입니다. 필요에 따라 지정합니다.


![okta 응용 프로그램](assets/okta-app-settings-blurred.PNG)


## AEM Trust Store에 OKTA(IdP) 인증서 추가

SAML 어설션이 암호화되므로 OKTA와 AEM 간의 안전한 통신이 가능하도록 AEM Trust Store에 OKTA(IdP) 인증서를 추가해야 합니다.
[아직 초기화되지 않은 경우 트러스트 저장소를](http://localhost:4502/libs/granite/security/content/truststore.html) 초기화합니다.
트러스트 저장소 암호를 기억하십시오. 이 프로세스는 나중에 이 암호를 사용해야 합니다.

* [전역 신뢰 저장소](http://localhost:4502/libs/granite/security/content/truststore.html)로 이동합니다.
* &quot;CER 파일에서 인증서 추가&quot;를 클릭합니다. OKTA에서 제공하는 IdP 인증서를 추가하고 제출을 클릭합니다.

   >[!NOTE]
   >
   >인증서를 어떤 사용자에게도 매핑하지 마십시오.

인증서를 신뢰 저장소에 추가할 때 아래 스크린샷에 나와 있는 것처럼 인증서 별칭을 가져와야 합니다. 별칭 이름은 대/소문자가 다를 수 있습니다.

![인증서 별칭](assets/cert-alias.PNG)

**인증서 별칭을 메모합니다. 이 작업은 이후 단계에 필요합니다.**

### SAML 인증 처리기 구성

[configMgr](http://localhost:4502/system/console/configMgr)로 이동합니다.
&quot;Adobe Granite SAML 2.0 Authentication Handler&quot;를 검색하고 엽니다.
아래에 지정된 대로 다음 속성을 제공합니다.
다음은 지정해야 하는 주요 속성입니다.

* **경로**  - 인증 핸들러가 트리거되는 경로입니다.
* **IdP URL**: OKTA에서 제공하는 IdP URL입니다.
* **IDP 인증서 별칭**: IdP 인증서를 AEM Trust Store에 추가할 때 받은 별칭입니다
* **서비스 공급자 엔티티 ID**: AEM 서버의 이름입니다.
* **키 저장소의** 암호: 사용한 트러스트 저장소 암호입니다.
* **기본 리디렉션**: 성공적인 인증 시 리디렉션할 URL입니다.
* **UserID 속성**:uid
* **암호화 사용**:false
* **CRX 사용자 자동 만들기**:true
* **그룹에 추가**:true
* **기본 그룹**:oktausers(사용자가 추가될 그룹입니다. AEM 내의 모든 기존 그룹을 제공할 수 있습니다.)
* **NamedIDPolicy**:요청한 제목을 나타내는 데 사용할 이름 식별자에 대한 제한을 지정합니다. 강조 표시된 문자열 **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**&#x200B;을 복사하여 붙여 넣습니다.
* **동기화된 속성**  - AEM 프로파일의 SAML 어설션에서 저장되는 속성입니다.

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Apache Sling 레퍼러 필터 구성

[configMgr](http://localhost:4502/system/console/configMgr)로 이동합니다.
&quot;Apache Sling 레퍼러 필터&quot;를 검색하고 엽니다.아래에 지정된 대로 다음 속성을 설정합니다.

* **비어 있음**:true
* **호스트 허용**:IdP의 호스트 이름(사용자의 경우 다를 수 있음)
* **등록 호스트 허용**:IdP의 호스트 이름(대/소문자가 다를 경우) Sling 레퍼러 필터 레퍼러 속성 스크린샷

![referrer-filter](assets/sling-referrer-filter.PNG)

#### OKTA 통합을 위한 DEBUG 로깅 구성

AEM에서 OKTA 통합을 설정할 때 AEM SAML 인증 핸들러의 DEBUG 로그를 검토하는 데 도움이 될 수 있습니다. 로그 수준을 DEBUG로 설정하려면 AEM OSGi 웹 콘솔을 통해 새 Sling Logger 구성을 만듭니다.

로그 노이즈를 줄이기 위해 스테이지와 프로덕션에서 이 로거를 제거하거나 비활성화해야 합니다.

AEM에서 OKTA 통합을 설정할 때 AEM SAML 인증 핸들러의 DEBUG 로그를 검토하는 데 도움이 될 수 있습니다. 로그 수준을 DEBUG로 설정하려면 AEM OSGi 웹 콘솔을 통해 새 Sling Logger 구성을 만듭니다.
**로그 노이즈를 줄이기 위해 스테이지와 프로덕션에서 이 로거를 제거하거나 비활성화해야 합니다.**
* [configMgr](http://localhost:4502/system/console/configMgr)로 이동합니다.

* &quot;Apache Sling Logging Configuration&quot; 검색 및 열기
* 다음 구성으로 로거를 만듭니다.
   * **로그 수준**:디버그
   * **로그 파일**:logs/saml.log
   * **로거**:com.adobe.granite.auth.saml
* 저장을 클릭하여 설정을 저장합니다.



#### OKTA 구성 테스트

AEM 인스턴스에서 로그아웃합니다. 링크에 액세스해 보십시오. OKTA SSO가 작동 중인 것을 볼 수 있습니다.
