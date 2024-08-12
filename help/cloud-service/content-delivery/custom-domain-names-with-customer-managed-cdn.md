---
title: 고객 관리 CDN을 사용한 사용자 정의 도메인 이름
description: 고객 관리 CDN을 사용하는 AEM as a Cloud Service 웹 사이트에 사용자 정의 도메인 이름을 구현하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-21T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
source-git-commit: 13657903c37b90c6d854dcba317dc1801d869de0
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# 고객 관리 CDN을 사용한 사용자 정의 도메인 이름

**고객 관리 CDN**&#x200B;을(를) 사용하는 AEM as a Cloud Service 웹 사이트에 사용자 지정 도메인 이름을 추가하는 방법을 알아봅니다.

이 자습서에서는 고객 관리 CDN을 사용하여 TLS(전송 계층 보안)가 있는 HTTPS 주소 지정 가능한 사용자 지정 도메인 이름 `wkndviaawscdn.enablementadobe.com`을(를) 추가하여 샘플 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 사이트의 브랜딩을 개선합니다. 이 자습서에서는 AWS CloudFront를 고객 관리 CDN으로 사용하지만 모든 CDN 공급자는 AEM as a Cloud Service과 호환되어야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3432561?quality=12&learn=on)

높은 수준의 단계는 다음과 같습니다.

고객 CDN을 사용한 ![사용자 지정 도메인 이름](./assets/add-custom-domain-name-with-customer-CDN.png){width="800" zoomable="yes"}

## 사전 요구 사항

>[!VIDEO](https://video.tv.adobe.com/v/3432562?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) 및 [dig](https://www.isc.org/blogs/dns-checker/)이(가) 로컬 컴퓨터에 설치되어 있습니다.
- 서드파티 서비스에 대한 액세스:
   - CA(인증 기관) - [DigitCert](https://www.digicert.com/)와 같이 사이트 도메인에 대해 서명된 인증서를 요청합니다.
   - 고객 CDN - 고객 CDN을 설정하고 SSL 인증서 및 AWS CloudFront, Azure CDN 또는 Akamai와 같은 도메인 세부 사항을 추가합니다.
   - DNS(Domain Name System) 호스팅 서비스 - Azure DNS 또는 AWS Route 53과 같은 사용자 정의 도메인에 대한 DNS 레코드를 추가합니다.
- HTTP 헤더 유효성 검사 CDN 규칙을 AEM as a Cloud Service 환경에 배포하기 위해 [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)에 액세스합니다.
- 샘플 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 사이트가 [프로덕션 프로그램](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs) 유형의 AEM as a Cloud Service 환경에 배포되었습니다.

서드파티 서비스에 액세스할 수 없는 경우 _보안 또는 호스팅 팀과 협력하여 단계를 완료합니다_.

## SSL 인증서 생성

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

다음 두 가지 옵션이 있습니다.

1. `openssl` 명령줄 도구를 사용하여 사이트 도메인에 대한 개인 키 및 CSR(인증서 서명 요청)을 생성할 수 있습니다. 서명된 인증서를 요청하려면 CSR을 인증 기관(CA)에 제출하십시오.
1. 호스팅 팀은 사이트에 필요한 개인 키와 서명된 인증서를 제공합니다.

첫 번째 옵션에 대한 단계를 검토해 보겠습니다.

개인 키 및 CSR을 생성하려면 다음 명령을 실행하고 메시지가 표시되면 필요한 정보를 제공합니다.

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

서명된 인증서를 요청하려면 문서에 따라 생성된 CSR을 CA에 제공하십시오. CA가 CSR에 서명하면 서명된 인증서 파일을 받게 됩니다.

### 서명된 인증서 검토

서명된 인증서를 Cloud Manager에 추가하기 전에 검토하는 것이 좋습니다. 다음 명령을 사용하여 인증서 세부 사항을 검토할 수 있습니다.

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

서명된 인증서에는 최종 엔티티 인증서와 함께 루트 및 중간 인증서를 포함하는 인증서 체인이 포함될 수 있습니다.

Adobe Cloud Manager은 별도의 양식 필드 _에서 최종 엔티티 인증서 및 인증서 체인_&#x200B;을(를) 허용하므로 서명된 인증서에서 최종 엔티티 인증서 및 인증서 체인을 추출해야 합니다.

이 자습서에서는 `*.enablementadobe.com` 도메인에 대해 발급된 [DigitCert](https://www.digicert.com/) 서명 인증서를 예로 사용합니다. 텍스트 편집기에서 서명된 인증서를 열고 `-----BEGIN CERTIFICATE-----`과(와) `-----END CERTIFICATE-----` 마커 사이의 콘텐츠를 복사하여 최종 엔터티 및 인증서 체인을 추출합니다.

## 고객 관리 CDN 설정

>[!VIDEO](https://video.tv.adobe.com/v/3432563?quality=12&learn=on)

AWS CloudFront, Azure CDN 또는 Akamai와 같은 고객 CDN을 설정하고 SSL 인증서 및 도메인 세부 사항을 추가합니다. 이 자습서에서는 AWS CloudFront를 예로 사용합니다. 그러나 CDN 공급업체에 따라 단계가 달라질 수 있습니다. 주요 설명선은 다음과 같습니다.

- CDN에 SSL 인증서를 추가합니다.
- CDN에 사용자 정의 도메인 이름을 추가합니다.
- 이미지, CSS 및 JavaScript 파일과 같은 콘텐츠를 캐시하도록 CDN을 구성합니다.
- CDN이 AEMCD 원본으로 보내는 모든 요청에 이 헤더를 포함하도록 `X-Forwarded-Host` HTTP 헤더를 CDN 설정에 추가합니다.
- `Host` 헤더 값이 프로그램 및 환경 ID를 포함하고 `adobeaemcloud.com`(으)로 끝나는 기본 AEM as a Cloud Service 도메인으로 설정되어 있는지 확인하십시오. 고객 CDN에서 Adobe CDN으로 전달되는 HTTP 호스트 헤더 값은 기본 AEM as a Cloud Service 도메인이어야 합니다. 다른 값은 오류 상태를 초래합니다.

## DNS 레코드 구성

>[!VIDEO](https://video.tv.adobe.com/v/3432564?quality=12&learn=on)

사용자 정의 도메인에 대한 DNS 레코드를 구성하려면 다음 단계를 따르십시오.

1. CDN 도메인 이름을 가리키는 사용자 정의 도메인에 대한 CNAME 레코드를 추가합니다.

이 자습서에서는 사용자 지정 도메인 `wkndviaawscdn.enablementadobe.com`에 대한 CNAME 레코드를 Azure DNS에 추가하고 AWS CloudFront 배포 도메인 이름을 가리킵니다.

### 사이트 확인

사용자 정의 도메인 이름을 사용하여 사이트에 액세스하여 사용자 정의 도메인 이름을 확인합니다.
AEM as a Cloud Service 환경의 vhhost 구성에 따라 작동할 수도 있고 작동하지 않을 수도 있습니다.

중요한 보안 단계는 HTTP 헤더 유효성 검사 CDN 규칙을 AEM as a Cloud Service 환경에 배포하는 것입니다. 규칙은 요청이 다른 소스가 아닌 고객 CDN에서 오고 있는지 확인합니다.

## HTTP 헤더 유효성 검사 CDN 규칙 없이 현재 작업 상태

>[!VIDEO](https://video.tv.adobe.com/v/3432565?quality=12&learn=on)

HTTP 헤더 유효성 검사 CDN 규칙이 없으면 `Host` 헤더 값이 프로그램 및 환경 ID를 포함하고 `adobeaemcloud.com`(으)로 끝나는 기본 AEM as a Cloud Service 도메인으로 설정됩니다. Adobe CDN은 HTTP 헤더 유효성 검사 CDN 규칙이 배포된 경우에만 `Host` 헤더 값을 고객 CDN에서 받은 `X-Forwarded-Host` 값으로 변환합니다. 그렇지 않으면 `Host` 헤더 값이 그대로 AEM as a Cloud Service 환경에 전달되고 `X-Forwarded-Host` 헤더가 사용되지 않습니다.

### 호스트 헤더 값을 인쇄하는 샘플 서블릿 코드

다음 서블릿 코드는 JSON 응답에 `Host`, `X-Forwarded-*`, `Referer` 및 `Via` HTTP 헤더 값을 인쇄합니다.

```java
package com.adobe.aem.guides.wknd.core.servlets;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.ServletResolverConstants;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = Servlet.class, property = {
        ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/verify-headers",
        ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
})
public class VerifyHeadersServlet extends SlingSafeMethodsServlet {

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");

        // Create JSON response
        StringBuilder jsonResponse = new StringBuilder();
        jsonResponse.append("{");

        Enumeration<String> headerNames = request.getHeaderNames();
        boolean firstHeader = true;

        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();

            if (headerName.startsWith("X-Forwarded-") || headerName.startsWith("Host")
                    || headerName.startsWith("Referer") || headerName.startsWith("Via")) {
                if (!firstHeader) {
                    jsonResponse.append(",");
                }
                jsonResponse.append("\"").append(headerName).append("\": \"").append(request.getHeader(headerName))
                        .append("\"");
                firstHeader = false;
            }
        }

        jsonResponse.append("}");

        response.getWriter().write(jsonResponse.toString());
    }
}
```

서블릿을 테스트하려면 `../dispatcher/src/conf.dispatcher.d/filters/filters.any` 파일을 다음 구성으로 업데이트하십시오. 또한 CDN이 `/bin/*` 경로를 **캐싱하지 않음**&#x200B;하도록 구성되어 있는지 확인하십시오.

```plaintext
# Testing purpose bin
/0300 { /type "allow" /extension "json" /path "/bin/*"}
/0301 { /type "allow" /path "/bin/*"}
/0302 { /type "allow" /url "/bin/*"}
```

## HTTP 헤더 유효성 검사 CDN 규칙 구성 및 배포

>[!VIDEO](https://video.tv.adobe.com/v/3432566?quality=12&learn=on)

HTTP 헤더 유효성 검사 CDN 규칙을 구성하고 배포하려면 다음 단계를 수행합니다.

- `cdn.yaml` 파일에 HTTP 헤더 유효성 검사 CDN 규칙을 추가합니다. 예가 아래에 나와 있습니다.

  ```yaml
  kind: "CDN"
  version: "1"
  metadata:
  envTypes: ["prod"]
  data:
  authentication:
      authenticators:
      - name: edge-auth
          type: edge
          edgeKey1: ${{CDN_EDGEKEY_080124}}
          edgeKey2: ${{CDN_EDGEKEY_110124}}
      rules:
      - name: edge-auth-rule
          when: { reqProperty: tier, equals: "publish" }
          action:
          type: authenticate
          authenticator: edge-auth
  ```

- Cloud Manager UI를 사용하여 비밀 유형 환경 변수(CDN_EDGEKEY_080124, CDN_EDGEKEY_110124)를 만듭니다.
- Cloud Manager 파이프라인을 사용하여 AEM as a Cloud Service 환경에 HTTP 헤더 유효성 검사 CDN 규칙을 배포합니다.

## X-AEM-Edge-Key HTTP 헤더에 암호 전달

>[!VIDEO](https://video.tv.adobe.com/v/3432567?quality=12&learn=on)

고객 CDN을 업데이트하여 `X-AEM-Edge-Key` HTTP 헤더에서 암호를 전달합니다. 이 암호는 Adobe CDN에서 고객 CDN으로부터 요청이 오고 있는지 확인하고 `Host` 헤더 값을 고객 CDN으로부터 받은 `X-Forwarded-Host`의 값으로 변환하는 데 사용됩니다.

## 비디오 전체

고객 관리 CDN을 사용하는 사용자 정의 도메인 이름을 AEM as a Cloud Service 호스팅 사이트에 추가하는 위의 단계를 보여 주는 종단간 비디오를 볼 수도 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3432568?quality=12&learn=on)
