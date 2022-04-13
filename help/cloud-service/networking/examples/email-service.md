---
title: 이메일 서비스
description: 송신 포트를 사용하여 이메일 서비스와 연결하도록 AEM as a Cloud Service을 구성하는 방법을 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: 8da6d5470c702620ee1121fd2688eb8756f0cebd
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# 이메일 서비스

AEM 구성을 통해 AEM as a Cloud Service에서 이메일 전송 `DefaultMailService` 고급 네트워킹 송신 포트를 사용하려면

(대부분의) 메일 서비스는 HTTP/HTTPS에서 실행되지 않으므로 AEM as a Cloud Service의 메일 서비스에 대한 연결은 프록시되어야 합니다.

+ `smtp.host` 가 OSGi 환경 변수로 설정되어 있습니다 `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 그래서 그것은 피난을 통과합니다.
   + `$[env:AEM_PROXY_HOST]` AEM as a Cloud Service이 내부 변수에 매핑하는 예약된 변수입니다 `proxy.tunnel` 호스트.
   + 설정 안 함 `AEM_PROXY_HOST` Cloud Manager 사용.
+ `smtp.port` 이(가) `portForward.portOrig` 대상 이메일 서비스의 호스트 및 포트에 매핑되는 포트입니다. 이 예에서는 매핑을 사용합니다. `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.
   + 다음 `smpt.port` 이(가) `portForward.portOrig` 포트 및 SMTP 서버의 실제 포트가 아님 간의 매핑 `smtp.port` 그리고 `portForward.portOrig` 포트는 Cloud Manager에 의해 설정됩니다 `portForwards` 규칙(아래에 설명되어 있음).

암호는 코드에 저장하지 않아야 하므로 [보안 OSGi 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), AIO CLI 또는 Cloud Manager API를 사용하여 설정합니다.

일반적으로 [유연한 포트 송신](../flexible-port-egress.md) 는 `allowlist` Adobe IP(예: [전용 송신 ip 주소](../dedicated-egress-ip-address.md) 사용할 수 있습니다.

## 고급 네트워킹 지원

다음 코드 예는 다음과 같은 고급 네트워킹 옵션에서 지원됩니다.

다음을 확인합니다. [적절하](../advanced-networking.md#advanced-networking) 이 자습서를 따르기 전에 고급 네트워킹 구성을 설정했습니다.

| 고급 네트워킹 없음 | [유연한 포트 송신](../flexible-port-egress.md) | [전용 송신 IP 주소](../dedicated-egress-ip-address.md) | [가상 사설 네트워크](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 구성

이 OSGi 구성 예는 다음 Cloud Manager를 통해 외부 메일 서비스를 사용하도록 AEM Mail OSGi Service를 구성합니다 `portForwards` 규칙 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 작업.

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

AEM 구성 [기본 메일 서비스](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) 이메일 제공업체에서 요구하는 대로(예: `smtp.ssl`등)

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

다음 `EMAIL_USERNAME` 및 `EMAIL_PASSWORD` 다음 중 하나를 사용하여 환경별로 OSGi 변수 및 암호를 설정할 수 있습니다.

+ [Cloud Manager 환경 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 또는 `aio CLI` 명령

   ```shell
   $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
   ```
