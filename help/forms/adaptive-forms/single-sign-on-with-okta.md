---
title: AEM으로 OKTA 구성
description: OKTA를 사용한 SSO(Single Sign-On) 사용을 위한 다양한 구성 설정 이해
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
last-substantial-update: 2021-06-09T00:00:00Z
duration: 193
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '733'
ht-degree: 0%

---

# OKTA를 사용하여 AEM 작성자 인증

첫 번째 단계는 OKTA 포털에서 앱을 구성하는 것입니다. OKTA 관리자가 앱을 승인하면 IdP 인증서와 SSO(Single Sign-On) URL에 액세스할 수 있습니다. 다음은 일반적으로 새 응용 프로그램을 등록할 때 사용되는 설정입니다.

* **응용 프로그램 이름:** 애플리케이션 이름입니다. 애플리케이션에 고유한 이름을 지정해야 합니다.
* **SAML 수신자:** OKTA에서 인증한 후 SAML 응답과 함께 AEM 인스턴스에서 적중되는 URL입니다. SAML 인증 처리기는 일반적으로 / saml_login을 사용하여 모든 URL을 가로채지만 애플리케이션 루트 뒤에 추가하는 것이 좋습니다.
* **SAML 대상**: 애플리케이션의 도메인 URL입니다. 도메인 URL에서 프로토콜(http 또는 https)을 사용하지 마십시오.
* **SAML 이름 ID:** 드롭다운 목록에서 이메일을 선택합니다.
* **환경**: 적절한 환경을 선택합니다.
* **속성**: SAML 응답에서 사용자에 대해 표시되는 속성입니다. 필요에 따라 지정합니다.


![okta-application](assets/okta-app-settings-blurred.PNG)


## AEM Trust Store에 OKTA(IdP) 인증서 추가

SAML 어설션은 암호화되므로 OKTA와 AEM 간에 보안 통신을 허용하려면 AEM Trust Store에 IdP(OKTA) 인증서를 추가해야 합니다.
[트러스트 저장소 초기화](http://localhost:4502/libs/granite/security/content/truststore.html), 아직 초기화되지 않은 경우.
Trust Store 암호를 기억하십시오. 이 암호는 나중에 이 프로세스에서 사용해야 합니다.

* 다음으로 이동 [글로벌 Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html).
* &quot;CER 파일에서 인증서 추가&quot;를 클릭합니다. OKTA에서 제공한 IdP 인증서를 추가하고 제출 을 클릭합니다.

  >[!NOTE]
  >
  >인증서를 사용자에게 매핑하지 마십시오.

인증서를 신뢰 저장소에 추가할 때는 아래 스크린샷과 같이 인증서 별칭을 가져와야 합니다. 사용자의 경우 별칭 이름이 다를 수 있습니다.

![인증서 별칭](assets/cert-alias.PNG)

**인증서 별칭을 기록합니다. 이 작업은 이후 단계에서 수행해야 합니다.**

### SAML 인증 처리기 구성

다음으로 이동 [configMgr](http://localhost:4502/system/console/configMgr).
&quot;Granite SAML 2.0 Authentication Handler Adobe&quot;를 검색하여 엽니다.
아래에 지정된 대로 다음 속성을 제공합니다. 다음은 지정해야 하는 주요 속성입니다.

* **경로** - 인증 처리기가 트리거되는 경로입니다.
* **IdP Url**:OKTA에서 제공하는 IdP URL입니다
* **IDP 인증서 별칭**:IdP 인증서를 AEM Trust Store에 추가할 때 얻은 앨리어스
* **서비스 공급자 엔티티 Id**:AEM 서버의 이름입니다.
* **키 저장소 암호**:사용한 Trust Store 암호입니다
* **기본 리디렉션**:인증 성공 시 리디렉션할 URL입니다
* **UserID 속성**:uid
* **암호화 사용**:false
* **CRX 사용자 자동 생성**:true
* **그룹에 추가**:true
* **기본 그룹**:oktausers(사용자가 추가되는 그룹입니다. AEM 내의 기존 그룹을 제공할 수 있습니다.)
* **NamedIDPolicy**: 요청된 제목을 표시하는 데 사용할 이름 식별자에 대한 제약 조건을 지정합니다. 강조 표시된 다음 문자열 복사 및 붙여넣기 **항아리:oasis:이름:tc:SAML:2.0:nameidformat:emailAddress**
* **동기화된 속성** - AEM 프로필의 SAML 어설션에 저장된 속성입니다.

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Apache Sling Referrer 필터 구성

다음으로 이동 [configMgr](http://localhost:4502/system/console/configMgr).
&quot;Apache Sling Referrer Filter&quot;를 검색하여 엽니다. 아래에 지정된 대로 다음 속성을 설정합니다.

* **빈 항목 허용**: false
* **호스트 허용**: IdP의 호스트 이름(사용자의 경우 다름)
* **Regexp 호스트 허용**: IdP의 호스트 이름(경우마다 다름) Sling Referrer Filter Referrer 속성 스크린샷

![referrer-filter](assets/okta-referrer.png)

#### OKTA 통합에 대한 디버그 로깅 구성

AEM에서 OKTA 통합을 설정할 때 AEM SAML 인증 핸들러에 대한 DEBUG 로그를 검토하는 것이 좋습니다. 로그 수준을 DEBUG로 설정하려면 AEM OSGi 웹 콘솔을 통해 새 Sling 로거 구성을 만드십시오.

스테이지 및 프로덕션에서 이 로거를 제거하거나 비활성화하여 로그 소음을 줄여야 합니다.

AEM에서 OKTA 통합을 설정할 때 AEM SAML 인증 핸들러에 대한 DEBUG 로그를 검토하는 것이 좋습니다. 로그 수준을 DEBUG로 설정하려면 AEM OSGi 웹 콘솔을 통해 새 Sling 로거 구성을 만드십시오.
**스테이지 및 프로덕션에서 이 로거를 제거하거나 비활성화하여 로그 소음을 줄여야 합니다.**
* 다음으로 이동 [configMgr](http://localhost:4502/system/console/configMgr)

* &quot;Apache Sling Logging Logger 구성&quot; 검색 및 열기
* 다음 구성으로 로거를 만듭니다.
   * **로그 수준**: 디버그
   * **로그 파일**: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* 설정을 저장하려면 저장 을 클릭합니다.

#### OKTA 구성 테스트

AEM 인스턴스에서 로그아웃 링크에 액세스해 보십시오. OKTA SSO가 작동 중입니다.
