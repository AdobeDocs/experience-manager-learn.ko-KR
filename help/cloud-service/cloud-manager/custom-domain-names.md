---
title: 사용자 정의 도메인 이름 추가
description: AEM as a cloud service 호스팅 사이트에 사용자 정의 도메인 이름을 추가하는 방법을 알아봅니다.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: null
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
source-git-commit: 466a19a30dd5f81d50c28cb57034800494255d4b
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---


# 사용자 정의 도메인 이름 추가

사용자 정의 도메인 이름을 AEM as a Cloud Service 웹 사이트에 추가하는 방법을 알아봅니다.

이 자습서에서는 샘플 브랜딩 [AEM WKND](https://github.com/adobe/aem-guides-wknd) HTTPS 주소 지정 가능한 사용자 정의 도메인 이름을 추가하여 사이트 개선 `wknd.enablementadobe.com` 전송 계층 보안(TLS) 사용.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)

높은 수준의 단계는 다음과 같습니다.

![높은 사용자 정의 도메인 이름](./assets/add-custom-domain-name-steps.png){width="800" zoomable="yes"}

## 사전 요구 사항

- [Openssl](https://www.openssl.org/) 및 [파기](https://www.isc.org/blogs/dns-checker/) 로컬 컴퓨터에 설치됩니다.
- 서드파티 서비스에 대한 액세스:
   - CA(인증 기관) - 다음과 같이 사이트 도메인에 대해 서명된 인증서를 요청합니다 [숫자 인증서](https://www.digicert.com/)
   - DNS(Domain Name System) 호스팅 서비스 - Azure DNS 또는 AWS Route 53과 같은 사용자 정의 도메인에 대한 DNS 레코드를 추가합니다.
- 액세스 대상: [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) 비즈니스 소유자 또는 배포 관리자 역할로 사용됩니다.
- 샘플 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 사이트가 의 AEMCS 환경에 배포됩니다. [제작 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) 유형.

서드파티 서비스에 액세스할 수 없는 경우 _보안 또는 호스팅 팀과 협력하여 단계를 완료합니다_.

## SSL 인증서 생성

다음 두 가지 옵션이 있습니다.

- 사용 `openssl` 명령줄 도구 - 사이트 도메인에 대한 개인 키 및 CSR(인증서 서명 요청)을 생성할 수 있습니다. 서명된 인증서를 요청하려면 CSR을 인증 기관(CA)에 제출하십시오.

- 호스팅 팀은 사이트에 필요한 개인 키와 서명된 인증서를 제공합니다.

첫 번째 옵션에 대한 단계를 검토해 보겠습니다.

개인 키 및 CSR을 생성하려면 다음 명령을 실행하고 메시지가 표시되면 필요한 정보를 제공합니다.

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

서명된 인증서를 요청하려면 문서에 따라 생성된 CSR을 CA에 제공하십시오. CA가 CSR에 서명하면 서명된 인증서 파일을 받게 됩니다.

### 서명된 인증서 검토

Cloud Manager에 추가하기 전에 서명된 인증서를 검토하는 것이 좋습니다. 다음 명령을 사용하여 인증서 세부 사항을 검토할 수 있습니다.

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

서명된 인증서에는 최종 엔티티 인증서와 함께 루트 및 중간 인증서를 포함하는 인증서 체인이 포함될 수 있습니다.

Adobe Cloud Manager는 최종 엔티티 인증서 및 인증서 체인을 허용합니다 _별도의 양식 필드에서_&#x200B;서명된 인증서에서 최종 엔티티 인증서 및 인증서 체인을 추출해야 합니다.

이 자습서에서는 [숫자 인증서](https://www.digicert.com/) 서명된 인증서 발급 대상 `*.enablementadobe.com` 도메인이 예로 사용됩니다. 최종 엔티티 및 인증서 체인은 텍스트 편집기에서 서명된 인증서를 열고 사이에 컨텐츠를 복사함으로써 추출됩니다. `-----BEGIN CERTIFICATE-----` 및 `-----END CERTIFICATE-----` 마커.

## Cloud Manager에 SSL 인증서 추가

Cloud Manager에서 SSL 인증서를 추가하려면 다음을 수행합니다. [SSL 인증서 추가](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate.html) 설명서를 참조하십시오.

## 도메인 이름 확인

도메인 이름을 확인하려면 다음 단계를 수행하십시오.

- 다음을 수행하여 Cloud Manager에 도메인 이름 추가 [사용자 정의 도메인 이름 추가](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name.html) 설명서를 참조하십시오.
- AEM 관련 추가 [TXT 레코드](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record.html) DNS 호스팅 서비스에서
- 다음을 사용하여 DNS 서버를 쿼리하여 위의 단계를 확인합니다. `dig` 명령입니다.

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

이 자습서에서는 Azure DNS를 예로 사용합니다. TXT 레코드를 추가하려면 DNS 호스팅 서비스의 설명서를 따라야 합니다.

리뷰 [도메인 이름 상태 확인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status.html) 문제가 있는 경우 설명서를 참조하십시오.

## DNS 레코드 구성

사용자 정의 도메인에 대한 DNS 레코드를 구성하려면 다음 단계를 따르십시오.

- 루트 도메인(APEX) 또는 하위 도메인(CNAME)과 같은 도메인 유형을 기반으로 DNS 레코드 유형(CNAME 또는 APEX)을 결정하고 다음을 따릅니다. [DNS 설정 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings.html) 설명서를 참조하십시오.
- DNS 호스팅 서비스에 DNS 레코드를 추가합니다.
- 다음을 수행하여 DNS 레코드 유효성 검사 트리거 [DNS 레코드 상태 확인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status.html) 설명서를 참조하십시오.

이 튜토리얼에서 **하위 도메인** `wknd.enablementadobe.com` 는 을 가리키고 있는 CNAME 레코드 유형인 를 사용합니다. `cdn.adobeaemcloud.com` 가 추가됩니다.

그러나 를 사용하는 경우에는 **루트 도메인**&#x200B;에서 Adobe이 제공하는 특정 IP 주소를 가리키는 APEX 레코드 유형(예: A, ALIAS 또는 ANAME)을 추가해야 합니다.

## 사이트 확인

사용자 지정 도메인 이름을 사용하여 사이트에 액세스할 수 있는지 확인하려면 웹 브라우저를 열고 사용자 지정 도메인 URL로 이동합니다. 사이트에 액세스할 수 있고 브라우저에 자물쇠 아이콘과의 보안 연결이 표시되어 있는지 확인하십시오.


