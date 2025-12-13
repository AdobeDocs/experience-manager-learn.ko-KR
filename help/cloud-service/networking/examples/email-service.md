---
title: 이메일 서비스
description: 이그레스 포트를 사용하여 이메일 서비스에 연결하도록 AEM as a Cloud Service을 구성하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 이메일 서비스

고급 네트워킹 이그레스 포트를 사용하도록 AEM의 `DefaultMailService`을(를) 구성하여 AEM as a Cloud Service에서 이메일을 전송합니다.

대부분의 메일 서비스는 HTTP/HTTPS를 통해 실행되지 않으므로 AEM as a Cloud Service에서 메일 서비스에 대한 연결은 프록시아웃되어야 합니다.

+ `smtp.host`이(가) OSGi 환경 변수 `$[env:AEM_PROXY_HOST;default=proxy.tunnel]`(으)로 설정되어 있으므로 이그레스를 통해 라우팅됩니다.
   + `$[env:AEM_PROXY_HOST]`은(는) AEM as a Cloud Service이 내부 `proxy.tunnel` 호스트에 매핑하는 예약된 변수입니다.
   + Cloud Manager을 통해 `AEM_PROXY_HOST`을(를) 설정하지 마십시오.
+ `smtp.port`이(가) 대상 전자 메일 서비스의 호스트 및 포트에 매핑되는 `portForward.portOrig` 포트로 설정되어 있습니다. 이 예제에서는 `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465` 매핑을 사용합니다.
   + `smpt.port`이(가) SMTP 서버의 실제 포트가 아니라 `portForward.portOrig` 포트로 설정되어 있습니다. `smtp.port`과(와) `portForward.portOrig` 포트 간의 매핑은 Cloud Manager `portForwards` 규칙에 의해 설정됩니다(아래 참조).

암호는 코드에 저장할 수 없으므로 전자 메일 서비스의 사용자 이름과 암호는 [암호 OSGi 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)를 사용하여 제공되는 것이 가장 좋습니다. AIO CLI 또는 Cloud Manager API를 사용하여 설정합니다.

일반적으로 [유연한 포트 이그레스](../flexible-port-egress.md)는 Adobe IP를 `allowlist`할 필요가 없는 경우 이메일 서비스와 통합하는 데 사용됩니다. 이 경우 [전용 이그레스 IP 주소](../dedicated-egress-ip-address.md)를 사용할 수 있습니다.

또한 [전자 메일 보내기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email)에 대한 AEM 설명서를 검토하십시오.

## 고급 네트워킹 지원

다음 코드 예는 다음 고급 네트워킹 옵션에서 지원됩니다.

이 자습서를 수행하기 전에 [적절함](../advanced-networking.md#advanced-networking) 고급 네트워킹 구성이 설정되었는지 확인하십시오.

| 고급 네트워킹 없음 | [유연한 포트 이그레스](../flexible-port-egress.md) | [전용 이그레스 IP 주소](../dedicated-egress-ip-address.md) | [가상 개인 네트워크](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 구성

이 OSGi 구성 예제는 `portForwards`enableEnvironmentAdvancedNetworkingConfiguration[&#x200B; 작업의 다음 Cloud Manager &#x200B;](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 규칙을 사용하여 외부 메일 서비스를 사용하도록 AEM의 메일 OSGi 서비스를 구성합니다.

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

전자 메일 공급자(예: [&#x200B; 등)에 필요한 대로 AEM의 &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email)DefaultMailService`smtp.ssl`을 구성하십시오.

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
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

다음 중 하나를 사용하여 환경별로 `EMAIL_USERNAME` 및 `EMAIL_PASSWORD` OSGi 변수와 암호를 설정할 수 있습니다.

+ [Cloud Manager 환경 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 또는 `aio CLI` 명령 사용

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
