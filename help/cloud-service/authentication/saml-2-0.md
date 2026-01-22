---
title: AEM as a Cloud Service의 SAML 2.0
description: AEM에서 Cloud Service Publish 서비스로 SAML 2.0 인증을 구성하는 방법에 대해 알아보십시오.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 2ed303e316577363f6d1c265ef7f9cd6d81491d8
workflow-type: tm+mt
source-wordcount: '4277'
ht-degree: 1%

---

# SAML 2.0 인증{#saml-2-0-authentication}

최종 사용자(AEM 작성자 아님)를 선택한 SAML 2.0 호환 IDP로 설정하고 인증하는 방법을 알아봅니다.

## AEM as a Cloud Service 용 SAML은 무엇입니까?

SAML 2.0과 AEM Publish(또는 미리 보기)를 통합하면 AEM 기반 웹 경험의 최종 사용자가 비 Adobe Systems IDP(ID 공급자)에 인증하고 이름이 지정된 인증된 사용자로 AEM에 액세스할 수 있습니다.

|                       | AEM Author | AEM 게시 인스턴스 |
|-----------------------|:----------:|:-----------:|
| SAML 2.0 지원 | ✘ | ✔ |

+++ AEM를 통한 SAML 2.0 흐름 이해

AEM Publish SAML 통합의 일반적인 흐름은 다음과 같습니다.

1. 사용자가 AEM에 요청 Publish 인증이 필요함을 나타냅니다.
   + 사용자가 CUG/ACL 보호 리소스를 요청합니다.
   + 사용자가 Authentication 요구 사항이 적용되는 리소스를 요청합니다.
   + 사용자가 로그인 작업을 명시적으로 요청하는 AEM의 로그인 종단점(즉 `/system/sling/login`, )에 대한 링크를 따라갑니다.
1. AEM은 IDP에 AuthnRequest를 만들어 IDP에 인증 프로세스를 시작하도록 요청합니다.
1. 사용자가 IDP를 인증합니다.
   + IDP에서 자격 증명을 입력하라는 메시지가 표시됩니다.
   + 사용자가 이미 IDP로 인증되었으므로 추가 자격 증명을 제공할 필요가 없습니다.
1. IDP는 사용자의 데이터가 포함된 SAML 어설션을 생성하고 IDP의 개인 인증서를 사용하여 서명합니다.
1. IDP는 사용자의 웹 브라우저(EACH_PROTECTED_PATH/saml_login)를 통해 HTTP POST를 통해 SAML 어설션을 AEM Publish로 보냅니다.
1. AEM Publish는 SAML 어설션을 수신하고 IDP 공개 인증서를 사용하여 SAML 어설션의 무결성 및 신뢰성을 확인합니다.
1. AEM Publish는 SAML 2.0 OSGi 구성 및 SAML 어설션의 컨텐츠를 기반으로 AEM 사용자 레코드를 관리합니다.
   + 사용자 생성
   + 사용자 특성을 동기화합니다.
   + AEM 사용자 그룹 멤버십 업데이트
1. AEM Publish HTTP 응답에 AEM `login-token` 쿠키를 설정하며, 이 쿠키는 AEM Publish에 대한 후속 요청을 인증하는 데 사용됩니다.
1. AEM 게시는 `saml_request_path` 쿠키에 지정된 대로 사용자를 AEM 게시의 URL로 리디렉션합니다.

+++

## 구성 연습

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

이 비디오는 AEM as a Cloud Service Publish 서비스와 SAML 2.0 통합을 설정하고 Okta를 IDP로 사용하는 방법에 대해 안내합니다.

## 사전 요구 사항

SAML 2.0 인증을 설정할 때 필요한 사항은 다음과 같습니다.

+ Cloud Manager에 대한 배포 관리자 액세스
+ AEM 관리자가 Cloud Service 환경으로서의 AEM 액세스
+ IDP에 대한 관리자 액세스
+ 선택적으로, SAML 페이로드를 암호화하는 데 사용되는 공개/개인 키 쌍에 대한 액세스
+ AEM Sites 페이지(또는 페이지 트리), AEM Publish에 게시되고 [CUG(폐쇄된 사용자 그룹)에 의해 보호됨](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties#permissions)

SAML 2.0은 AEM Publish 또는 미리 보기 사용을 인증하는 용도로만 지원됩니다. 및 IDP [를 사용하여 AEM 작성자 인증을 관리하려면 IDP를 Adobe Systems IMS](https://helpx.adobe.com/kr/enterprise/using/set-up-identity.html)와 통합하십시오.


## AEM 에 IDP 공개 인증서 설치

IDP의 공개 인증서는 AEM Global Trust Store에 추가되며, IDP에서 전송한 SAML 어설션이 유효한지 확인하는 데 사용됩니다.

+++SAML 어설션 서명 흐름

![SAML 2.0 - IDP SAML 어설션 서명](./assets/saml-2-0/idp-signing-diagram.png)

1. 사용자가 IDP에 인증합니다.
1. IDP는 사용자 데이터가 포함된 SAML 어설션을 생성합니다.
1. IDP는 IDP의 사설 인증서를 사용하여 SAML 어설션에 서명합니다.
1. IDP는 서명된 SAML 어설션을 포함하는 AEM Publish의 SAML 종단점(`.../saml_login`)에 대해 클라이언트측 HTTP POST를 시작합니다.
1. AEM Publish 는 서명된 SAML 어설션이 포함된 HTTP POST를 수신하고 IDP 공용 인증서를 사용하여 서명의 유효성을 검사할 수 있습니다.

+++

![글로벌 Trust Store에 IDP 공용 인증서 추가](./assets/saml-2-0/global-trust-store.png)

1. IDP에서 __공용 인증서__ 파일을 가져옵니다. 이 인증서를 사용하면 AEM에서 IDP가 AEM에 제공한 SAML 어설션의 유효성을 검사할 수 있습니다.

   인증서는 PEM 포맷 형식이며 다음과 유사해야 합니다.

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. AEM 작성자에 AEM 관리자로 로그인합니다.
1. > Security > Trust Store __도구__&#x200B;이동합니다.
1. 글로벌 Trust Store를 만들기 또는 엽니다. 글로벌 Trust Store를 만드는 경우 암호 안전한 곳에 스토어하십시오.
1. Add certificate from CER file(CER 파일에서 인증서 추가)을 확장 __합니다__.
1. 인증서 파일&#x200B;__선택을 선택하고__ IDP에서 제공한 인증서 파일을 업로드합니다.
1. Map Certificate to User(사용자에&#x200B;__인증서 매핑)를 비워 둡니다__.
1. __제출__&#x200B;을 선택합니다.
1. 새로 추가된 인증서가 CRT 파일에서&#x200B;__인증서 추가 섹션 위에__&#x200B;나타납니다.
1. 이 값은 SAML 2.0 Authentication Handler OSGi 구성&#x200B;__에서__&#x200B;사용되므로 별칭[을 기록](#saml-2-0-authentication-handler-osgi-configuration)해 둡니다.
1. __저장 후 닫기__&#x200B;를 선택합니다.

글로벌 Trust Store는 AEM 작성자에 있는 IDP의 공용 인증서로 구성되지만, SAML은 AEM Publish에서만 사용되므로 IDP 공용 인증서에 액세스할 수 있으려면 글로벌 Trust Store를 AEM Publish에 복제해야 합니다.

![글로벌 Trust Store를 AEM Publish에 복제](./assets/saml-2-0/global-trust-store-replicate.png)

1. 도구 > Deployment > Packages __(배포 패키지)로__&#x200B;이동합니다.
1. 패키지 만들기
   + 패키지 이름: `Global Trust Store`
   + 버전: `1.0.0`
   + 그룹: `com.your.company`
1. 새 __글로벌 Trust Store__ 패키지를 편집 합니다.
1. __필터__ 탭 을 선택하고 루트 경로 `/etc/truststore`에 대한 필터를 추가합니다.
1. __완료__&#x200B;를 선택한 다음 __저장__&#x200B;을 선택합니다.
1. 글로벌 Trust Store __패키지에__&#x200B;대한 __빌드__ 버튼 을 선택합니다.
1. 빌드가 완료되면 추가 > 복제를 선택하여 __글로벌 Trust Store 노드(__)을 활성화하여 Publish AEM합니다.____`/etc/truststore`

## authentication-service 키 저장소 만들기{#authentication-service-keystore}

_SAML 2.0 인증 핸들러 OSGi 구성 속성 [ 로 설정 `handleLogout` 되거나 AuthnRequest 서명/SAML 어설션 암호화`true`](#saml-20-authenticationsaml-2-0-authentication)가 필요한 경우 [](#install-aem-public-private-key-pair)authentication-service에 대한 키 저장소를 작성해야 합니다_

1. AEM 작성자에 AEM 관리자로 로그인하여 개인 키를 업로드합니다.
1. __도구 > 보안 > 사용자__(으)로 이동하고 __인증 서비스__ 사용자를 선택한 다음 상단 작업 표시줄에서 __속성__&#x200B;을 선택합니다.
1. __키 저장소__ 탭을 선택합니다.
1. 키 저장소를 생성하거나 엽니다. 키 저장소를 만드는 경우 암호를 안전하게 유지합니다.
   + AuthnRequest 서명/SAML 어설션 암호화가 필요한 경우에만 [공용/개인 키 저장소가 이 키 저장소에 설치됩니다](#install-aem-public-private-key-pair).
   + 이 SAML 통합이 로그아웃은 지원하지만 AuthnRequest 서명/SAML 어설션은 지원하지 않는 경우 빈 키 저장소로 충분합니다.
1. __저장 후 닫기__&#x200B;를 선택합니다.
1. 업데이트된 __authentication-service__ 사용자 포함 패키지를 만들기.

   패키지를 사용하여 다음과 같은 임시 해결 방법을 _Use.:_

   1. 도구 > Deployment > Packages __(배포 패키지)로__&#x200B;이동합니다.
   1. 패키지 만들기
      + 패키지 이름: `Authentication Service`
      + 버전: `1.0.0`
      + 그룹: `com.your.company`
   1. 새 __Authentication Service Key Store__ 패키지를 편집합니다.
   1. __필터__ 탭 을 선택하고 루트 경로 `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`에 대한 필터를 추가합니다.
      + `<AUTHENTICATION SERVICE UUID>` 도구 > Security > Users로 __이동하고 authentication-service__ 사용자를 선택하여 __찾을 수 있습니다__. UUID는 URL 마지막 부분입니다.
   1. 완료(Done __)를 선택한 다음__&#x200B;저장(Save __)을 선택합니다__.
   1. __Authentication Service Key Store__ 패키지에 대한 __빌드__ 버튼 선택
   1. 빌드가 완료되면 추가&#x200B;__>__&#x200B;복제&#x200B;__를 선택하여__ Authentication 서비스 키 스토어 활성화하여 Publish AEM합니다.

## AEM 공개/개인 키 쌍 설치{#install-aem-public-private-key-pair}

_AEM 공개/개인 키 쌍 설치는 선택 사항입니다_

AuthnRequests(IDP에) 서명하고 SAML 어설션을 암호화(AEM에 연결)하도록 AEM Publish를 구성할 수 있습니다. 이는 개인 키를 AEM Publish에 제공함으로써 수행되며, IDP에 공개 키를 일치시킵니다.

+++ AuthnRequest 서명 흐름 이해(선택 사항)

AuthnRequest(로그인 프로세스를 시작하는 AEM Publish에서 IDP에 대한 요청)는 AEM Publish로 서명할 수 있습니다. 이를 위해 AEM Publish 가 개인 키를 사용하여 AuthnRequest에 서명하면 IDP가 공개 키를 사용하여 서명의 유효성을 검사합니다. 이렇게 하면 IDP에 AuthnRequest가 악의적인 타사가 아닌 AEM Publish 에서 시작되고 요청되었음을 알 수 있습니다.

![SAML 2.0 - SP AuthnRequest 서명](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. 사용자가 AEM Publish에 HTTP 요청하면 SAML 인증이 IDP에 요청됩니다.
1. AEM Publish 는 IDP에 보낼 SAML 요청을 생성합니다.
1. AEM Publish 는 AEM의 개인 키를 사용하여 SAML 요청에 서명합니다.
1. AEM Publish 는 서명된 SAML 요청이 포함된 IDP에 대한 HTTP 클라이언트측 리디렉션인 AuthnRequest를 시작합니다.
1. IDP는 AuthnRequest를 수신하고 AEM의 공개 키를 사용하여 서명의 유효성을 검사하여 AEM Publish 가 AuthnRequest를 시작하도록 보장합니다.
1. 그런 다음 AEM Publish 는 IDP 공개 인증서를 사용하여 암호 해독된 SAML 어설션의 무결성 및 신뢰성을 확인합니다.

+++

+++ SAML 어설션 암호화 흐름 이해(선택 사항)

IDP와 AEM Publish 간의 모든 HTTP 통신은 HTTPS를 통해 이루어져야 하므로 기본적으로 안전합니다. 그러나 필요에 따라 HTTPS에서 제공하는 것 외에 추가 기밀성이 필요한 경우 SAML 어설션을 암호화할 수 있습니다. 이를 위해 IDP는 개인 키를 사용하여 SAML 어설션 데이터를 암호화하고, AEM Publish 개인 키를 사용하여 SAML 어설션의 암호를 해독합니다.

![SAML 2.0 - SP SAML Assertion 암호화](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. 사용자가 IDP에 인증합니다.
1. IDP는 사용자 데이터가 포함된 SAML 어설션을 생성하고 IDP의 사설 인증서를 사용하여 서명합니다.
1. 그런 다음 IDP는 AEM의 공개 키를 사용하여 SAML 어설션을 암호화하며, 이 경우 암호를 해독하려면 AEM 개인 키가 필요합니다.
1. 암호화된 SAML 어설션은 사용자의 웹 브라우저를 통해 AEM Publish로 전송됩니다.
1. AEM Publish 는 SAML 어설션을 수신하고 AEM의 개인 키를 사용하여 암호를 해독합니다.
1. 침입 탐지 및 방지(IDP) 사용자 인증 메시지를 표시합니다.

+++

AuthnRequest 서명 및 SAML 어설션 암호화는 모두 선택 사항이지만 SAML 2.0 인증 핸들러 OSGi 구성 속성 [을 사용하여 `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)둘 다 활성화되며, 이는 둘 다 사용하거나 둘 다 사용할 수 없음을 의미합니다.

![인증 서비스 키 스토어 AEM](./assets/saml-2-0/authentication-service-key-store.png)

1. AuthnRequest에 서명하는 데 사용되는 공개 키, 개인 키(DER 포맷 PKCS#8) 및 인증서 체인 파일(공개 키일 수 있음)을 가져오고 SAML 어설션을 암호화합니다. 키는 일반적으로 IT 조직의 보안 팀 에서 제공합니다.

   + 자체 서명된 키 쌍은 openssl __을 사용하여__&#x200B;생성할 수 있습니다.

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. 공개 키를 IDP에 업로드합니다.
   + `openssl` 위의 방법을 사용하는 경우 공개 키는 파일입니다`aem-public.crt`.
1. AEM 작성자에 AEM 관리자로 로그인하여 개인 키를 업로드합니다.
1. 도구 > Security > Trust Store __로__&#x200B;이동하고 authentication-service __사용자를 선택한__&#x200B;다음 상단 작업 표시줄에서 속성&#x200B;__선택합니다__.
1. 도구 > Security > Users(보안 사용자)로 __이동하고 authentication-service__ 사용자를 선택한 __다음 상단 작업 표시줄에서 속성__ 선택합니다&#x200B;__.__
1. [키 저장소&#x200B;__]__&#x200B;탭 을 선택합니다.
1. 키 저장소를 만들기 또는 여십시오. 키 저장소를 만드는 경우 암호를 안전하게 유지합니다.
1. DER 파일에서&#x200B;__개인 키 추가를 선택하고__&#x200B;개인 키 및 체인 파일을 AEM에 추가합니다.
   + ____&#x200B;별칭: 의미 있는 이름(종종 IDP의 이름)을 제공합니다.
   + __개인 키 파일__: 개인 키 파일(DER 포맷 PKCS#8)을 업로드합니다.
      + `openssl` 위의 방법을 사용하면 다음과 같은 파일이 있습니다`aem-private-pkcs8.der`.
   + __인증서 체인 파일 선택__: 함께 제공되는 체인 파일을 업로드합니다(공개 키일 수 있음).
      + 위의 `openssl` 메서드를 사용하면 `aem-public.crt` 파일입니다.
   + 제출을 선택합니다 __.__
1. 새로 추가된 인증서가 CRT 파일에서&#x200B;__인증서 추가 섹션 위에__&#x200B;나타납니다.
   + 별칭&#x200B;__은__ SAML 2.0 인증 처리기 OSGi 구성에서 [사용되므로 기록해 둡니다](#saml-20-authentication-handler-osgi-configuration)
1. __저장 후 닫기__&#x200B;를 선택합니다.
1. 업데이트된 __authentication-service__ 사용자 포함 패키지를 만들기.

   패키지를 사용하여 다음과 같은 임시 해결 방법을 _Use.:_

   1. 도구 > Deployment > Packages __(배포 패키지)로__&#x200B;이동합니다.
   1. 패키지 만들기
      + 패키지 이름: `Authentication Service`
      + 버전: `1.0.0`
      + 그룹: `com.your.company`
   1. 새 __Authentication Service Key Store__ 패키지를 편집합니다.
   1. __필터__ 탭 을 선택하고 루트 경로 `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`에 대한 필터를 추가합니다.
      + `<AUTHENTICATION SERVICE UUID>` 도구 > Security > Users로 __이동하고 authentication-service__ 사용자를 선택하여 __찾을 수 있습니다__. UUID는 URL 마지막 부분입니다.
   1. 완료(Done __)를 선택한 다음__&#x200B;저장(Save __)을 선택합니다__.
   1. __Authentication Service Key Store__ 패키지에 대한 __빌드__ 버튼 선택
   1. 빌드가 완료되면 추가&#x200B;__>__&#x200B;복제&#x200B;__를 선택하여__ Authentication 서비스 키 스토어 활성화하여 Publish AEM합니다.

## SAML 2.0 인증 처리기 구성{#configure-saml-2-0-authentication-handler}

AEM의 SAML 구성은 Adobe Systems Granite SAML 2.0 Authentication Handler __OSGi 구성을 통해__수행됩니다.
구성은 OSGi 공장 구성이며, 이는 단일 AEM as a Cloud Service Publish 서비스로, 저장소의 개별 리소스 트리를 포함하는 여러 SAML 구성이 있을 수 있음을 의미합니다. 이 기능은 다중 사이트 AEM 배포에 유용합니다.

+++ SAML 2.0 Authentication Handler OSGi 구성 용어집

### Adobe Systems Granite SAML 2.0 Authentication Handler OSGi 구성{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi 속성 | 필수 | 값 포맷 | 기본 값 | 설명 |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| 경로 | `path` | ✔ | string형 배열 | `/` | 이 인증 처리기가 사용되는 AEM 경로입니다. |
| IDP URL | `idpUrl` | ✔ | 문자열 |                           | IDP URL SAML 인증 요청이 전송됩니다. |
| IDP 인증서 별칭 | `idpCertAlias` | ✔ | 문자열 |                           | AEM Global Trust Store에 있는 IDP 인증서의 별칭 |
| IDP HTTP 리디렉션 | `idpHttpRedirect` | ✘ | 부울 | `false` | HTTP가 AuthnRequest를 보내는 대신 IDP URL 리디렉션되는지 여부를 나타냅니다. 침입 탐지 및 방지(IDP) 시작 인증에 대해 로 `true` 설정합니다. |
| IDP 식별자 | `idpIdentifier` | ✘ | 문자열 |                           | AEM 사용자 및 그룹 고유성을 보장하기 위한 고유 IDP ID. 비어 있으면 가 `serviceProviderEntityId` 대신 사용됩니다. |
| 어설션 소비자 서비스 URL | `assertionConsumerServiceURL` | ✘ | 문자열 |                           | `AssertionConsumerServiceURL` 메시지를 AEM으로 보내야 하는 위치를 `<Response>` 지정하는 AuthnRequest의 URL 속성입니다. |
| SP 엔티티 ID | `serviceProviderEntityId` | ✔ | 문자열 |                           | IDP에 대해 AEM을 고유하게 식별합니다. 일반적으로 AEM 호스트 이름입니다. |
| SP 암호화 | `useEncryption` | ✘ | 부울 | `true` | IDP가 SAML 어설션을 암호화하는지 여부를 나타냅니다. Requires `spPrivateKeyAlias` 및 `keyStorePassword` To Be Set. |
| SP 개인 키 별칭 | `spPrivateKeyAlias` | ✘ | 문자열 |                           | 사용자의 키 스토어에 있는 `authentication-service` 개인 키의 별칭입니다. 로 설정된 경우 `useEncryption` 필수입니다 `true`. |
| SP 키 스토어 암호 | `keyStorePassword` | ✘ | 문자열 |                           | &#39;authentication-service&#39; 사용자의 키 암호이 스토어. 로 설정된 경우 `useEncryption` 필수입니다 `true`. |
| 기본 리디렉션 | `defaultRedirectUrl` | ✘ | 문자열 | `/` | 인증 성공 후의 기본 리디렉션 URL. AEM 호스트(예: `/content/wknd/us/en/html`)에 상대적일 수 있습니다. |
| 사용자 ID 속성 | `userIDAttribute` | ✘ | 문자열 | `uid` | AEM 사용자의 사용자 ID를 포함하는 SAML 어설션 속성의 이름입니다. 를 `Subject:NameId`사용하려면 비워 둡니다. |
| AEM 사용자 자동 생성 | `createUser` | ✘ | 부울 | `true` | 인증 성공 시 AEM 사용자가 생성되는지 여부를 나타냅니다. |
| AEM 사용자 중간 경로 | `userIntermediatePath` | ✘ | 문자열 |                           | AEM 사용자를 만들 때 이 값은 중간 경로로 사용됩니다(예 `/home/users/<userIntermediatePath>/jane@wknd.com`: ). `createUser` 로 설정해야 합니다`true`. |
| AEM 사용자 속성 | `synchronizeAttributes` | ✘ | string형 배열 |                           | AEM 사용자, 포맷 `[ "saml-attribute-name=path/relative/to/user/node" ]` 에 스토어 SAML 속성 매핑 목록입니다(예: `[ "firstName=profile/givenName" ]`). [기본 AEM 속성의 전체 목록을 참조하십시오](#aem-user-attributes). |
| AEM 그룹에 사용자 추가 | `addGroupMemberships` | ✘ | 부울 | `true` | 인증 성공 후 AEM 사용자가 AEM 사용자 그룹에 자동으로 추가되는지 여부를 나타냅니다. |
| AEM 그룹 멤버십 속성 | `groupMembershipAttribute` | ✘ | 문자열 | `groupMembership` | 사용자를 추가해야 하는 AEM 사용자 그룹 목록을 포함하는 SAML 어설션 속성의 이름입니다. `addGroupMemberships` 로 설정해야 합니다`true`. |
| 기본 AEM 그룹 | `defaultGroups` | ✘ | string형 배열 |                           | 인증된 사용자 AEM 사용자 그룹 목록은 항상 에 추가됩니다(예: `[ "wknd-user" ]`). `addGroupMemberships` 로 설정해야 합니다`true`. |
| NameIDPolicy 형식 | `nameIdFormat` | ✘ | 문자열 | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | AuthnRequest 메시지에서 보낼 NameIDPolicy 포맷 매개 변수의 값입니다. |
| SAML 응답 저장 | `storeSAMLResponse` | ✘ | 부울 | `false` | 값이 AEM `samlResponse` 노드 상에 저장되는지 여부를 `cq:User` 나타냅니다. |
| 로그아웃 처리 | `handleLogout` | ✘ | 부울 | `false` | 로그아웃 요청 사항이 이 SAML 인증 처리기에 의해 처리되는지 여부를 나타냅니다. `logoutUrl` 설정해야 합니다. |
| 로그아웃 URL | `logoutUrl` | ✘ | 문자열 |                           | SAML 로그아웃 요청이 전송되는 IDP의 URL. 로 설정된 경우 `handleLogout` 필수입니다 `true`. |
| 클럭 허용 오차 | `clockTolerance` | ✘ | 정수 | `60` | SAML 어설션의 유효성을 검사할 때 IDP 및 AEM(SP) 클럭 오차 허용치. |
| 다이제스트 방법 | `digestMethod` | ✘ | 문자열 | `http://www.w3.org/2001/04/xmlenc#sha256` | 침입 탐지 및 방지(IDP) SAML 메시지에 서명할 때 사용하는 다이제스트 알고리즘입니다. |
| 서명 방법 | `signatureMethod` | ✘ | 문자열 | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | SAML 메시지에 서명할 때 IDP가 사용하는 서명 알고리즘입니다. |
| ID 동기화 유형 | `identitySyncType` | ✘ | `default` 또는 `idp` | `default` | Cloud Service as a AEM의 기본값을 변경 `from` 하지 마십시오. |
| 서비스 등급 | `service.ranking` | ✘ | 정수 | `5002` | 동일한 `path`에 대해 더 높은 등급 구성이 선호됩니다. |

### AEM 사용자 속성{#aem-user-attributes}

AEM에서는 Adobe Systems Granite SAML 2.0 Authentication Handler OSGi 구성의 속성 통해 `synchronizeAttributes` 채울 수 있는 다음 사용자 속성을 사용합니다.  모든 침입 탐지 및 방지(IDP) 속성은 AEM 사용자 속성에 동기화할 수 있지만, AEM 사용 속성 속성(아래 나열됨)에 매핑하면 AEM 자연스럽게 사용할 수 있습니다.

| 사용자 속성 | 노드의 상대 속성 경로 `rep:User` |
|--------------------------------|--------------------------|
| 제목(예: `Mrs`) | `profile/title` |
| 이름(예: 이름) | `profile/givenName` |
| 성(예: 성) | `profile/familyName` |
| 직책 | `profile/jobTitle` |
| 이메일 주소 | `profile/email` |
| 상세 주소 | `profile/street` |
| 도시 | `profile/city` |
| 우편 번호 | `profile/postalCode` |
| 국가 | `profile/country` |
| 전화번호 | `profile/phoneNumber` |
| 내 정보 | `profile/aboutMe` |

+++

1. 프로젝트에서 OSGi 구성 파일을 만들기 하고 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` IDE에서 엽니다.
   + 변경 `/wknd-examples/` 사항을 `/<project name>/`
   + 파일 이름 뒤 `~` 의 식별자는 이 구성을 고유하게 식별해야 하므로 IDP의 이름(예 `...~okta.cfg.json`: )일 수 있습니다. 값은 하이픈이 있는 영숫자여야 합니다.
1. 다음 JSON을 `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` 파일에 붙여넣기 하고 필요에 따라 참조를 업데이트합니다 `wknd` .

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. 프로젝트에 필요한 대로 값을 업데이트합니다. __구성 속성 설명은 위의 SAML 2.0 Authentication Handler OSGi 구성 용어집을__ 참조하십시오. `path` CUG(Closed User Groups)로 보호되고 인증이 필요한 컨텐츠 트리를 포함해야 하며 이 인증 처리기는 보호를 담당해야 합니다.
1. 값이 릴리스 주기와 동기화 해제되어 변경될 수 있는 경우 또는 유사한 환경 유형/서비스 계층 간에 값이 다른 경우 OSGi 환경 변수 및 시크릿을 사용하는 것이 좋지만 필수는 아닙니다. 기본값은 위에 표시된 구문을 `$[env:..;default=the-default-value]"` 사용하여 설정할 수 있습니다.

환경별 OSGi 구성(`config.publish.dev`, `config.publish.stage`, 및 `config.publish.prod`)은 SAML 구성이 환경마다 다른 경우 특정 속성으로 정의할 수 있습니다.

### 암호화 사용

AuthnRequest 및 SAML 어설션[](#encrypting-the-authnrequest-and-saml-assertion)을 암호화할 때 `useEncryption`, `spPrivateKeyAlias`, 및 `keyStorePassword`. 에 `keyStorePassword` 암호 포함 따라서 값은 OSGi 구성 파일에 저장되지 않고 비밀 구성 값을 사용하여 [삽입되어야 합니다](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++선택적으로, 암호화를 사용하도록 OSGi 구성을 업데이트하십시오

1. IDE에서 엽니다 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` .
1. 아래와 같이 세 개의 속성 `useEncryption`, `spPrivateKeyAlias`및 `keyStorePassword` 를 추가합니다.

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. 암호화에 필요한 세 가지 OSGi 구성 속성은 다음과 같습니다.

+ `useEncryption` 로 설정 `true`
+ `spPrivateKeyAlias` SAML 통합에서 사용하는 개인 키에 대한 키 저장소 항목 별칭이 포함되어 있습니다.
+ `keyStorePassword`[에는 사용자 키 저장소의 암호를 포함하는 ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) OSGi 시크릿 구성 변수`authentication-service`가 포함되어 있습니다.

+++

## 레퍼러 필터 구성

SAML 인증 프로세스 중에 IDP는 AEM Publish `.../saml_login` 의 끝점에 대한 클라이언트측 HTTP POST를 시작합니다. IDP 및 AEM Publish가 서로 다른 출처에 있는 경우 AEM Publish __의 레퍼러 필터링__ 은 OSGi 구성을 통해 IDP 출처에서 HTTP POST를 허용하도록 구성됩니다.

1. 의 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`프로젝트에서 OSGi 구성 파일을 만들기(또는 편집)합니다.
   + 변경 `/wknd-examples/` 사항을 `/<project name>/`
1. `allow.empty` 값이 으로 설정되어 `true`있는지 확인합니다. (또는 원하는 `allow.hosts`경우 )에는 `allow.hosts.regexp` IDP의 원본이 포함되고 `filter.methods` 가 포함됩니다`POST`. OSGi 구성은 다음과 유사해야 합니다.

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM Publish 는 단일 레퍼러 필터 구성을 지원하므로 SAML 구성 요구 사항을 기존 구성과 병합하십시오.

환경마다 `config.publish.dev`(또는 `config.publish.stage`)이 다른 경우 환경당 OSGi 구성(`config.publish.prod`, `allow.hosts` 및 `allow.hosts.regex`)을 특정 특성으로 정의할 수 있습니다.

## CORS(원본 간 리소스 공유) 구성

SAML 인증 프로세스 중에 IDP는 AEM 게시의 `.../saml_login` 끝점에 대한 클라이언트측 HTTP POST를 시작합니다. IDP와 AEM 게시가 다른 호스트/도메인에 있는 경우 AEM 게시의 __CRoss-Origin Resource Sharing(CORS)__&#x200B;이(가) IDP의 호스트/도메인에서 HTTP POST를 허용하도록 구성되어 있어야 합니다.

이 HTTP POST 요청의 `Origin` 헤더에 일반적으로 AEM 게시 호스트와 다른 값이 있으므로 CORS 구성이 필요합니다.

로컬 AEM SDK(`localhost:4503`)에서 SAML 인증을 테스트할 때 IDP는 헤더`Origin`를 `null` 로 설정할 수 있습니다. `"null"` 목록에 `alloworigin`을(를) 추가합니다.

1. 프로젝트의 OSGi 구성 파일 만들기: `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + 프로젝트 이름으로 변경 `/wknd-examples/`
   + 파일 이름 뒤 `~` 의 식별자는 이 구성을 고유하게 식별해야 하므로 IDP의 이름(예 `...CORSPolicyImpl~okta.cfg.json`: )일 수 있습니다. 값은 하이픈이 있는 영숫자여야 합니다.
1. `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` 파일에 다음 JSON을 붙여 넣습니다.

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

`config.publish.dev` 및 `config.publish.stage`이(가) 환경마다 다른 경우 환경당 OSGi 구성(`config.publish.prod`, `alloworigin` 및 `allowedpaths`)을 특정 특성으로 정의할 수 있습니다.

## SAML HTTP POST를 허용하도록 AEM Dispatcher 구성

IDP에 성공적으로 인증되면 IDP는 AEM의 등록된 `/saml_login` 끝점(IDP에 구성됨)으로 HTTP POST를 다시 오케스트레이션합니다. `/saml_login`에 대한 이 HTTP POST는 Dispatcher에서 기본적으로 차단되므로 다음 Dispatcher 규칙을 사용하여 명시적으로 허용해야 합니다.

1. IDE에서 `dispatcher/src/conf.dispatcher.d/filters/filters.any`을(를) 엽니다.
1. 파일 맨 아래에 로 끝나는 `/saml_login`URL에 대한 HTTP POST에 대한 허용 규칙 을 추가합니다.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>다양한 보호 경로 및 고유한 침입 탐지 및 방지(IDP) 엔드포인트에 대해 AEM 여러 SAML 구성을 구축할 때 침입 탐지 및 방지(IDP) 엔드포인트에 RESPECTIVE_PROTECTED_PATH/saml_로그인 게시하여 AEM 측에서 적절한 SAML 구성을 선택해야 합니다. 동일한 보호 경로에 대해 중복된 SAML 구성이 있는 경우 SAML 구성이 임의로 선택됩니다.

Apache 웹 서버에서 URL 재작성이 구성된 경우(`dispatcher/src/conf.d/rewrites/rewrite.rules`) 엔드포인트에 대한 `.../saml_login` 요청이 실수로 중단되지 않도록 합니다.

## 동적 그룹 구성원

동적 그룹 멤버십은 그룹 평가 및 프로비저닝의 성능을 향상시키는 Apache Jackrabbit Oak[ 기능입니다](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). 이 섹션에서는 이 기능이 활성화될 때 사용자 및 그룹이 저장되는 방법과 SAML Authentication Handler의 구성을 수정하여 신규 또는 기존 환경에서 활성화하는 방법에 대해 설명합니다.

### 새로운 환경에서 SAML 사용자에 대해 동적 그룹 멤버십을 활성화하는 방법

새로운 AEM as a Cloud Service 환경에서 그룹 평가 성능을 크게 향상시키려면 새 환경에서 동적 그룹 멤버십 기능을 활성화하는 것이 좋습니다.
이 단계는 데이터 동기화가 활성화될 때도 필요합니다. 자세한 내용은 [여기](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier) .
이렇게 하려면 OSGI 구성 파일에 다음 속성 추가하십시오.

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

이 구성을 사용하면 사용자 및 그룹이 Oak 외부 사용자[로 ](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html)만들어집니다. AEM에서 외부 사용자 및 그룹의 기본값은 `rep:principalName` 또는 `[user name];[idp]`로 구성됩니다`[group name];[idp]`.
ACL(액세스 제어 목록)은 사용자 또는 그룹의 PrincipalName과 연결되어 있습니다.
이전에 `identitySyncType` 지정되지 않았거나 로 설정 `default`되지 않은 기존 배포 환경에서 이 구성을 배포하면 새 사용자 및 그룹이 생성되고 ACL을 이러한 새 사용자 및 그룹에 적용해야 합니다. 외부 그룹에는 로컬 사용자가 포함될 수 없습니다. [](https://sling.apache.org/documentation/bundles/repository-initialization.html) 보고서는 SAML 외부 그룹에 대한 ACL을 생성하는 데 사용할 수 있으며, 사용자가 로그인을 수행할 때만 생성되는 경우 ACL이 균일 가능합니다.
ACL에서 이러한 리팩토링을 방지하기 위해 표준 [마이그레이션 기능이](#automatic-migration-to-dynamic-group-membership-for-existing-environments) 구현되었습니다.

### 동적 그룹 멤버십를 사용하여 로컬 및 외부 그룹에 멤버십을 저장하는 방법How memberships are stored in local and external groups with dynamic

로컬 그룹에서 그룹 구성원은 oak 속성 `rep:members`에 저장됩니다. 이 속성에는 그룹 내 모든 구성원의 uid 목록이 들어 있습니다. 자세한 내용은 여기에서 확인할 [수 있습니다](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository).
예:

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

동적 그룹 멤버십가 있는 외부 그룹은 그룹 항목에 구성원을 스토어하지 않습니다.
대신 그룹 멤버십 항목이 사용자 항목에 저장됩니다. 추가 설명서는 여기에서 찾을 [수 있습니다](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). 예를 들면 다음은 그룹에 대한 OAK 노드입니다.

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

해당 그룹의 사용자 구성원에 대한 노드입니다.

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### 기존 환경에서 SAML 사용자에 대해 동적 그룹 멤버십을 활성화하는 방법

이전 섹션에서 설명한 것처럼 외부 사용자 및 그룹의 포맷 형식은 로컬 사용자 및 그룹에 사용되는 형식과 약간 다릅니다. 외부 그룹에 대한 새 ACL을 정의하고 새 외부 사용자를 프로비전하거나 아래에 설명된 대로 마이그레이션 도구 를 사용할 수 있습니다.

#### 외부 사용자가 있는 기존 환경에 대한 동적 그룹 멤버십 사용

SAML Authentication 처리기는 다음 속성이 지정 `"identitySyncType": "idp"`될 때 외부 사용자를 만듭니다. 이 경우 이 속성 다음과 같이 `"identitySyncType": "idp_dynamic"`수정하여 동적 그룹 멤버십 활성화할 수 있습니다. 마이그레이션이 필요하지 않습니다.

#### 로컬 사용자가 있는 기존 환경에 대해 다이내믹 그룹 멤버십로 자동 마이그레이션

SAML Authentication 처리기는 다음 속성이 지정 `"identitySyncType": "default"`될 때 로컬 사용자를 만듭니다. 속성 지정이 지정되지 않은 경우에도 기본값입니다. 이 섹션에서는 자동 마이그레이션 절차에서 수행하는 단계에 대해 설명합니다.

이 마이그레이션이 활성화되면 사용자 인증 중에 수행되며 다음 단계로 구성됩니다.
1. 로컬 사용자는 원래 사용자 이름을 유지한 채 외부 사용자로 마이그레이션됩니다. 즉, 이제 외부 사용자 역할을 하는 마이그레이션된 로컬 사용자는 이전 섹션에서 언급한 명명 구문을 따르는 대신 원래 사용자 이름을 유지합니다. 라는 하나의 추가 속성 이 추가됩니다 `rep:externalId` `[user name];[idp]`. 사용자 `PrincipalName` 수는 수정되지 않습니다.
2. SAML 어설션에서 수신된 각 외부 그룹에 대해 외부 그룹이 생성됩니다. 해당 로컬 그룹이 있는 경우 외부 그룹이 로컬 그룹에 구성원으로 추가됩니다.
3. 사용자가 외부 그룹 구성원으로 추가됩니다.
4. 그런 다음 로컬 사용자 사용자가 속해 있던 모든 Saml 로컬 그룹에서 제거됩니다. Saml 로컬 그룹은 OAK 속성: 으로 식별됩니다 `rep:managedByIdp`. 이 속성은 특성 `syncType` 이 지정되지 않았거나 로 설정 `default`되지 않은 경우 Saml Authentication 처리기에 의해 설정됩니다.

인스턴스의 경우, 마이그레이션 `user1` 전에 로컬 사용자 및 로컬 그룹 `group1`의 구성원인 경우 마이그레이션 후에 다음과 같은 변경 사항이 발생합니다.
`user1` 외부 사용자. 속성이 `rep:externalId` 그의 프로필에 추가됩니다.
`user1` 외부 그룹의 구성원이 됨: `group1;idp`
`user1` 더 이상 로컬 그룹의 직접 구성원이 아님: `group1`
`group1;idp` 로컬 그룹의 `group1`구성원입니다.
`user1` 는 로컬 그룹 `group1` (상속)의 구성원입니다

외부 그룹의 그룹 멤버십은 속성의 사용자 프로필에 저장됩니다 `rep:externalPrincipalNames`

### 다이내믹 그룹 멤버십로 자동 마이그레이션을 구성하는 방법

1. SAML OSGi 구성 파일에서 속성 `"identitySyncType": "idp_dynamic_simplified_id"` 활성화: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` :
2. 로 시작하는 `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`공장 PID로 새 OSGi 서비스를 구성합니다. 예를 들어 PID는 다음과 같을 `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`수 있습니다. 다음 속성 설정:

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

여러 SAML 구성을 마이그레이션하려면 `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration`에 대한 여러 OSGi 팩토리 구성을 만들어야 하며, 각 구성은 마이그레이션할 `idpIdentifier`을(를) 지정합니다.

## SAML 구성 배포

OSGi 구성은 Git에 커밋하고 Cloud Manager을 사용하여 AEM as a Cloud Service에 배포해야 합니다.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

전체 스택 배포 파이프라인을 사용하여 대상 Cloud Manager Git 분기(이 예에서는 `develop`)를 배포합니다.

## SAML 인증 호출

SAML 인증 흐름은 특별히 제작된 링크나 단추를 만들어 AEM 사이트 웹 페이지에서 호출할 수 있습니다. 아래에 설명된 매개 변수는 필요에 따라 프로그래밍 방식으로 설정할 수 있으므로 인스턴스의 경우 로그인 버튼은 버튼의 컨텍스트에 따라 SAML 인증 성공 시 사용자가 이동하는 위치를 다른 AEM 페이지로 설정할 `saml_request_path`수 있습니다.

## SAML을 사용하는 동안 보안 캐싱

AEM 게시 인스턴스에서 대부분의 페이지는 일반적으로 캐시됩니다. 그러나 SAML로 보호되는 경로의 경우 캐싱 사용하지 않도록 설정하거나 auth_checker 구성을 사용하여 사용하도록 설정캐싱 보호해야 합니다. 자세한 내용은 여기에 제공된 [세부 정보를 참조하십시오](https://experienceleague.adobe.com/ko/docs/experience-manager-dispatcher/using/configuring/permissions-cache)

auth_checker 활성화하지 않고 보호된 경로를 캐시하면 예측할 수 없는 동작이 경험 수 있습니다.

### GET 요청

SAML 인증은 다음 형식으로 HTTP GET 요청을 만들어 호출할 수 있습니다.

`HTTP GET /system/sling/login`

쿼리 매개 변수를 제공하는 방법:

| 쿼리 매개 변수 이름 | 쿼리 매개 변수 값 |
|----------------------|-----------------------|
| `resource` | Adobe Systems Granite SAML 2.0 Authentication Handler OSGi 구성의[ ](#configure-saml-2-0-authentication-handler) 속성에 정의된 `path`대로 SAML 인증 핸들러가 수신하는 모든 JCR 경로 또는 하위 경로입니다. |
| `saml_request_path` | SAML 인증에 성공한 후 사용자 이동해야 하는 URL 경로입니다. |

예를 들어, 이 HTML 링크는 SAML 로그인 흐름을 트리거하고, 성공하면 사용자 `/content/wknd/us/en/protected/page.html`를 . 이러한 쿼리 매개 변수는 필요에 따라 프로그래밍 방식으로 설정할 수 있습니다.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## POST 요청

SAML 인증은 포맷 형식으로 HTTP POST 요청을 생성하여 호출할 수 있습니다.

`HTTP POST /system/sling/login`

양식 데이터 제공 :

| 양식 데이터 이름 | 양식 데이터 값 |
|----------------------|-----------------------|
| `resource` | Adobe Systems Granite SAML 2.0 Authentication Handler OSGi 구성의[ ](#configure-saml-2-0-authentication-handler) 속성에 정의된 `path`대로 SAML 인증 핸들러가 수신하는 모든 JCR 경로 또는 하위 경로입니다. |
| `saml_request_path` | SAML 인증에 성공한 후 사용자 이동해야 하는 URL 경로입니다. |


예를 들어 이 HTML 버튼은 HTTP POST를 사용하여 SAML 로그인 흐름을 트리거하고, 성공하면 사용자 `/content/wknd/us/en/protected/page.html`를 . 이러한 양식 데이터 매개 변수는 필요에 따라 프로그래밍 방식으로 설정할 수 있습니다.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Dispatcher 구성

HTTP GET 및 POST 메서드 모두 클라이언트가 AEM의 `/system/sling/login` 끝점에 액세스해야 하므로 AEM Dispatcher를 통해 허용해야 합니다.

GET 또는 POST가 사용되는지에 따라 필요한 URL 패턴을 허용합니다.

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
