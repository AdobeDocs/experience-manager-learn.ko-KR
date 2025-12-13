---
title: Adobe 관리형 CDN을 사용한 사용자 정의 도메인 이름
description: Adobe 관리 CDN을 사용하는 AEM as a Cloud Service 웹 사이트에 사용자 정의 도메인 이름을 구현하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 1042
last-substantial-update: 2024-08-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
exl-id: 8936c3ae-2daf-4d0f-b260-28376ae28087
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 1%

---

# Adobe CDN을 사용한 사용자 정의 도메인 이름

Adobe CDN(Content Delivery Network)을 사용하는 AEM as a Cloud Service 웹 사이트에 대한 사용자 정의 도메인 이름을 구현하는 방법을 알아봅니다.

이 자습서에서는 TLS(전송 계층 보안)를 사용하여 HTTPS 주소 지정 가능한 사용자 지정 도메인 이름 [을(를) 추가하여 샘플 &#x200B;](https://github.com/adobe/aem-guides-wknd)AEM WKND`wknd.enablementadobe.com` 사이트의 브랜딩이 향상되었습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

높은 수준의 단계는 다음과 같습니다.

Adobe CDN을 사용하는 ![사용자 지정 도메인 이름](./assets/add-custom-domain-name-with-Adobe-CDN.png){width="800" zoomable="yes"}

## 사전 요구 사항

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) 및 [dig](https://www.isc.org/blogs/dns-checker/)이(가) 로컬 컴퓨터에 설치되어 있습니다.
- 서드파티 서비스에 대한 액세스:
   - CA(인증 기관) - [DigitCert](https://www.digicert.com/)와 같이 사이트 도메인에 대해 서명된 인증서를 요청합니다.
   - DNS(Domain Name System) 호스팅 서비스 - Azure DNS 또는 AWS Route 53과 같은 사용자 정의 도메인에 대한 DNS 레코드를 추가합니다.
- [비즈니스 소유자](https://my.cloudmanager.adobe.com/) 또는 **배포 관리자** 역할로 **Adobe Cloud Manager**&#x200B;에 액세스합니다.
- 샘플 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 사이트가 [프로덕션 프로그램](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs) 유형의 AEM as a Cloud Service 환경에 배포되었습니다.

서드파티 서비스에 액세스할 수 없는 경우 _보안 또는 호스팅 팀과 협력하여 단계를 완료합니다_.

## SSL 인증서 생성

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

다음 두 가지 옵션이 있습니다.

1. `openssl` 명령줄 도구를 사용하여 사이트 도메인에 대한 개인 키 및 CSR(인증서 서명 요청)을 생성합니다. 서명된 인증서를 요청하려면 CSR을 인증 기관(CA)에 제출하십시오.
1. 호스팅 팀은 사이트에 필요한 개인 키와 서명된 인증서를 제공합니다.

첫 번째 옵션에 대한 단계를 검토해 보겠습니다.

개인 키 및 CSR을 생성하려면 다음 명령을 실행하고 메시지가 표시되면 필요한 정보를 제공합니다.

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

서명된 인증서를 요청하려면 CA의 설명서에 따라 생성된 CSR을 CA에 제공하십시오. CA가 CSR에 서명하면 서명된 인증서 파일을 받게 됩니다.

### 서명된 인증서 검토

서명된 인증서를 Cloud Manager에 추가하기 전에 검토하십시오. 다음 명령을 사용하여 인증서 세부 사항을 검토합니다.

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

서명된 인증서에는 최종 엔티티 인증서와 함께 루트 및 중간 인증서를 포함하는 인증서 체인이 포함될 수 있습니다.

Adobe Cloud Manager은 별도의 양식 필드 _에서 최종 엔티티 인증서 및 인증서 체인_&#x200B;을(를) 허용하므로 서명된 인증서에서 최종 엔티티 인증서 및 인증서 체인을 추출해야 합니다.

이 자습서에서는 [&#x200B; 도메인에 대해 발급된 &#x200B;](https://www.digicert.com/)DigitCert`*.enablementadobe.com` 서명 인증서를 예로 사용합니다. 텍스트 편집기에서 서명된 인증서를 열고 `-----BEGIN CERTIFICATE-----`과(와) `-----END CERTIFICATE-----` 마커 사이의 콘텐츠를 복사하여 최종 엔터티 및 인증서 체인을 추출합니다.

## Cloud Manager에 SSL 인증서 추가

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

Cloud Manager에서 SSL 인증서를 추가하려면 [SSL 인증서 추가](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate) 설명서를 따르십시오.

## 도메인 이름 확인

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

도메인 이름을 확인하려면 다음 단계를 수행하십시오.

- [사용자 지정 도메인 이름 추가](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name) 설명서에 따라 Cloud Manager에서 도메인 이름을 추가하십시오.
- DNS 호스팅 서비스에 AEM 관련 [TXT 레코드](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record)를 추가합니다.
- `dig` 명령을 사용하여 DNS 서버를 쿼리하여 위의 단계를 확인합니다.

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

성공적인 샘플 응답은 다음과 같습니다.

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

이 자습서에서는 Azure DNS를 사용하지만 모든 DNS 공급자를 사용할 수 있습니다. TXT 레코드를 추가하려면 DNS 호스팅 서비스의 설명서를 따라야 합니다.

문제가 있는 경우 [도메인 이름 상태 확인](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status) 설명서를 검토하십시오.

## DNS 레코드 구성

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

사용자 정의 도메인에 대한 DNS 레코드를 구성하려면 다음 단계를 따르십시오.

1. 루트 도메인(APEX) 또는 하위 도메인(CNAME)과 같은 도메인 유형에 따라 DNS 레코드 유형(CNAME 또는 APEX)을 결정하고 [DNS 설정 구성](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings) 설명서를 따릅니다.
1. DNS 호스팅 서비스에 DNS 레코드를 추가합니다.
1. [DNS 레코드 상태 확인](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status) 설명서에 따라 DNS 레코드 유효성 검사를 트리거합니다.

이 자습서에서는 **하위 도메인** `wknd.enablementadobe.com`이(가) 사용되면 `cdn.adobeaemcloud.com`을(를) 가리키는 CNAME 레코드 종류가 추가됩니다.

그러나 **루트 도메인**&#x200B;을 사용하는 경우 Adobe에서 제공하는 특정 IP 주소를 가리키는 APEX 레코드 유형(예: A, ALIAS 또는 ANAME)을 추가해야 합니다.

## 사이트 확인

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

사용자 지정 도메인 이름을 사용하여 사이트에 액세스할 수 있는지 확인하려면 웹 브라우저를 열고 사용자 지정 도메인 URL로 이동합니다. 사이트에 액세스할 수 있고 브라우저에 자물쇠 아이콘과의 보안 연결이 표시되어 있는지 확인하십시오.

## 엔드투엔드 비디오

AEM as a Cloud Service 호스팅 사이트에 사용자 정의 도메인 이름을 추가하기 위한 개요, 사전 요구 사항 및 위 단계를 설명하는 엔드 투 엔드 비디오를 시청할 수도 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)
