---
title: 이메일 서비스
description: 이그레스 포트를 사용하여 이메일 서비스에 연결하도록 AEM을 as a Cloud Service으로 구성하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 98
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# 이메일 서비스

AEM을 구성하여 AEMas a Cloud Service 에서 이메일 보내기 `DefaultMailService` 고급 네트워킹 이그레스 포트를 사용합니다.

(대부분의) 메일 서비스는 HTTP/HTTPS를 통해 실행되지 않으므로 AEM as a Cloud Service에서 메일 서비스에 대한 연결은 프록시 처리되어야 합니다.

+ `smtp.host` OSGi 환경 변수로 설정됩니다. `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 따라서 이그레스를 통해 라우팅됩니다.
   + `$[env:AEM_PROXY_HOST]` 는 AEM이 내부에 as a Cloud Service으로 매핑하는 예약된 변수입니다. `proxy.tunnel` 호스트.
   + 을(를) 설정하지 마십시오 `AEM_PROXY_HOST` cloud Manager를 통해
+ `smtp.port` 이(가) (으)로 설정됩니다. `portForward.portOrig` 대상 이메일 서비스의 호스트 및 포트에 매핑되는 포트입니다. 이 예에서는 매핑을 사용합니다. `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + 다음 `smpt.port` 이(가) (으)로 설정됩니다. `portForward.portOrig` SMTP 서버의 실제 포트가 아닌 포트. 다음 사이의 매핑 `smtp.port` 및 `portForward.portOrig` Cloud Manager에서 포트를 설정합니다. `portForwards` 규칙(아래 설명 참조).

암호는 코드에 저장해서는 안 되므로 이메일 서비스의 사용자 이름과 암호를 사용하는 것이 가장 좋습니다. [비밀 OSGi 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), AIO CLI 또는 Cloud Manager API를 사용하여 설정합니다.

일반적으로 [유연한 포트 이그레스](../flexible-port-egress.md) 필요한 경우가 아니면 이메일 서비스와 통합하는 데 사용됩니다. `allowlist` Adobe IP(이 경우 [전용 이그레스 ip 주소](../dedicated-egress-ip-address.md) 를 사용할 수 있습니다.

또한 다음에 대한 AEM 설명서를 검토하십시오. [전자 메일 보내기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## 고급 네트워킹 지원

다음 코드 예는 다음 고급 네트워킹 옵션에서 지원됩니다.

다음을 확인합니다. [적당하](../advanced-networking.md#advanced-networking) 이 자습서를 수행하기 전에 고급 네트워킹 구성이 설정되었습니다.

| 고급 네트워킹 없음 | [유연한 포트 이그레스](../flexible-port-egress.md) | [전용 이그레스 IP 주소](../dedicated-egress-ip-address.md) | [가상 사설망](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 구성

이 OSGi 구성 예제는 다음 Cloud Manager를 통해 외부 메일 서비스를 사용하도록 AEM Mail OSGi 서비스를 구성합니다 `portForwards` 규칙 [enableEnvironmentAdvancedNetworkConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 작업.

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

AEM 구성 [기본 메일 서비스](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) 이메일 공급자의 필요에 따라(예: `smtp.ssl`등).

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

다음 `EMAIL_USERNAME` 및 `EMAIL_PASSWORD` 다음 중 하나를 사용하여 환경별로 OSGi 변수 및 암호를 설정할 수 있습니다.

+ [Cloud Manager 환경 구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ 또는 `aio CLI` 명령

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
