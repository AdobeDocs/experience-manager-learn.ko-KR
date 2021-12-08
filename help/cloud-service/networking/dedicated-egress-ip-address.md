---
title: 전용 송신 IP 주소
description: 전용 IP에서 AEM의 아웃바운드 연결을 시작할 수 있는 전용 송신 IP 주소를 설정하고 사용하는 방법을 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '1186'
ht-degree: 0%

---


# 전용 송신 IP 주소

전용 IP에서 AEM의 아웃바운드 연결을 시작할 수 있는 전용 송신 IP 주소를 설정하고 사용하는 방법을 알아봅니다.

## 전용 송신 IP 주소란 무엇입니까?

전용 송신 IP 주소를 사용하면 AEM as a Cloud Service의 요청에서 전용 IP 주소를 사용할 수 있으므로 외부 서비스가 이 IP 주소로 들어오는 요청을 필터링할 수 있습니다. 좋아요 [유연한 송신 포트](./flexible-port-egress.md)의 경우 전용 송신 IP를 사용하여 비표준 포트에서 수신 대기할 수 있습니다.

Cloud Manager 프로그램은 __단일__ 네트워크 인프라 유형. 전용 송신 IP 주소가 가장 많이 사용되는지 확인 [적절한 유형의 네트워크 인프라](./advanced-networking.md)  다음 명령을 실행하기 전에 AEM as a Cloud Service에 대해 설명합니다.

>[!MORELIKETHIS]
>
> AEM as a Cloud Service 읽기 [고급 네트워크 구성 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#dedicated-egress-IP-address) 전용 송신 IP 주소에 대한 자세한 내용

## 전제 조건

전용 송신 IP 주소를 설정할 때는 다음 사항이 필요합니다.

+ Cloud Manager API [Cloud Manager 비즈니스 소유자 권한](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ 액세스 권한 [Cloud Manager API 인증 자격 증명](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 조직 ID(IMS 조직 ID라고도 함)
   + 클라이언트 ID(API 키라고도 함)
   + 액세스 토큰(베어러 토큰이라고도 함)
+ Cloud Manager 프로그램 ID입니다
+ Cloud Manager 환경 ID

이 자습서에서는 을 사용합니다 `curl` 클라우드 관리자 API 구성을 설정하는 중입니다. 제공된 `curl` 명령은 Linux/macOS 구문을 가정합니다. Windows 명령 프롬프트를 사용하는 경우 `\` 줄 바꿈 문자 `^`.

## 프로그램에서 전용 송신 IP 주소 사용

먼저 AEM as a Cloud Service에서 전용 송신 IP 주소를 활성화하고 구성합니다.

1. 먼저 Cloud Manager API를 사용하여 고급 네트워킹을 설정할 영역을 결정합니다 [listRegions](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) 작업. 다음 `region name` 후속 Cloud Manager API 호출을 수행하려면 가 필요합니다. 일반적으로 프로덕션 환경이 상주하는 영역이 사용됩니다.

   __listRegions HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Cloud Manager API를 사용하여 Cloud Manager 프로그램의 전용 송신 IP 주소 활성화 [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) 작업. 적절한 `region` Cloud Manager API에서 가져온 코드 `listRegions` 작업.

   __createNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Cloud Manager 프로그램이 네트워크 인프라를 프로비저닝할 때까지 15분을 기다립니다.

1. 환경이 완료되었는지 확인합니다 __전용 송신 IP 주소__ cloud Manager API를 사용한 구성 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 작업, `id` 이전 단계의 createNetworkInfrastructure HTTP 요청에서 반환됩니다.

   __getNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 응답에 가 포함되어 있는지 확인합니다. __상태__ 의 __ready__. 아직 준비되지 않은 경우 몇 분마다 상태를 다시 확인합니다.

## 환경당 전용 송신 IP 주소 프록시 구성

1. 활성화 및 구성 __전용 송신 IP 주소__ cloud Manager API를 사용하여 각 AEM as a Cloud Service 환경에서 구성 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 작업.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   에서 JSON 매개 변수를 정의합니다 `dedicated-egress-ip-address.json` 컬을 통해 제공 `... -d @./dedicated-egress-ip-address.json`.

[dedicated-egress-ip-address.json 예제 다운로드](./assets/dedicated-egress-ip-address.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   전용 송신 IP 주소 구성의 HTTP 서명만 다릅니다. [유연한 송신 포트](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) 여기서 은 선택적 옵션도 지원합니다 `nonProxyHosts` 구성.

   `nonProxyHosts` 는 전용 송신 IP가 아닌 기본 공유 IP 주소 범위를 통해 포트 80 또는 443을 라우팅해야 하는 호스트 집합을 선언합니다. 이 기능은 공유 IP를 통한 트래픽 추적이 Adobe에 의해 자동으로 최적화될 수 있으므로 유용할 수 있습니다.

   각 `portForwards` 매핑 시 고급 네트워킹은 다음 전달 규칙을 정의합니다.

   | 프록시 호스트 | 프록시 포트 |  | 외부 호스트 | 외부 포트 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 각 환경에 대해 Cloud Manager API를 사용하여 송신 규칙이 적용되는지 확인합니다 [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) 작업.

   __getEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 전용 송신 IP 주소 구성은 Cloud Manager API를 사용하여 업데이트할 수 있습니다 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 작업. 기억 `enableEnvironmentAdvancedNetworkingConfiguration` is `PUT` 작업을 수행하므로 이 작업의 모든 호출과 함께 모든 규칙을 제공해야 합니다.

1. 를 얻습니다. __전용 송신 IP 주소__ DNS Resolver 사용(예: [DNSChecker.org](https://dnschecker.org/)) 내의 아무 곳이나 지정합니다. `p{programId}.external.adobeaemcloud.com`또는 실행 `dig` 명령줄에서 을(를) 클릭합니다.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   호스트 이름은 다음 중 하나일 수 없습니다 `pinged`- 송신 및 _not_ 그리고 강박.

1. 이제 사용자 지정 AEM 코드 및 구성에서 전용 송신 IP 주소를 사용할 수 있습니다. 종종 전용 송신 IP 주소를 사용할 때, 외부 서비스 AEM as a Cloud Service 연결이 이 전용 IP 주소의 트래픽만 허용하도록 구성됩니다.

## 전용 포트 포트를 통해 외부 서비스에 연결

전용 송신 IP 주소가 활성화되면 AEM 코드 및 구성은 전용 송신 IP를 사용하여 외부 서비스를 호출할 수 있습니다. AEM에서 다르게 처리하는 외부 호출에는 두 가지 방식이 있습니다.

1. 비표준 포트에서 외부 서비스에 대한 HTTP/HTTPS 호출
   + 표준 80 또는 443 포트 이외의 포트에서 실행되는 서비스에 대한 HTTP/HTTPS 호출을 포함합니다.
1. 외부 서비스에 대한 비HTTP/HTTPS 호출
   + 메일 서버와의 연결, SQL 데이터베이스 또는 다른 비HTTP/HTTPS 프로토콜에서 실행되는 서비스와 같은 HTTP가 아닌 모든 호출을 포함합니다.

표준 (80/443)에 있는 AEM의 HTTP/HTTPS 요청은 기본적으로 허용되며 추가 구성 또는 고려 사항이 필요하지 않습니다.

>[!TIP]
>
> 에 대해서는 AEM as a Cloud Service의 전용 송신 IP 주소 설명서를 참조하십시오 [전체 라우팅 규칙 세트](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing).


### 비표준 포트의 HTTP/HTTPS

AEM에서 비표준 포트(-80/443 아님)에 대한 HTTP/HTTPS 연결을 만들 때 자리 표시자를 통해 제공되는 특수 호스트 및 포트를 통해 연결을 만들어야 합니다.

AEM에서는 AEM HTTP/HTTPS 프록시에 매핑되는 두 개의 특별한 Java™ 시스템 변수 세트를 제공합니다.

| 변수 이름 | 사용 | Java™ 코드 | OSGi 구성 | | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | HTTP 연결을 위한 프록시 호스트 | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | | `AEM_HTTP_PROXY_PORT` | HTTP 연결을 위한 프록시 포트 | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` | | `AEM_HTTPS_PROXY_HOST` | HTTPS 연결용 프록시 호스트 | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | | `AEM_HTTPS_PROXY_PORT` | HTTPS 연결용 프록시 포트 | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` |

HTTP/HTTPS 외부 서비스에 대한 요청은 AEM 프록시 호스트/포트 값을 사용하여 Java™ HTTP 클라이언트의 프록시 구성을 구성하여 수행해야 합니다.

비표준 포트에서 외부 서비스에 HTTP/HTTPS를 호출하는 경우 해당 항목이 없습니다 `portForwards` Cloud Manager API를 사용하여 정의해야 합니다 `enableEnvironmentAdvancedNetworkingConfiguration` &quot;rules&quot;가 &quot;in code&quot;로 정의된 대로 작동합니다.

#### 코드 예

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="비표준 포트의 HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">비표준 포트의 HTTP/HTTPS</a></strong></div>
    <p>
        AEM에서 비 표준 HTTP/HTTPS 포트의 외부 서비스로 HTTP/HTTPS를 연결하는 Java™ 코드 예입니다.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### 외부 서비스에 대한 비HTTP/HTTPS 연결

비HTTP/HTTPS 연결을 만들 때(예: AEM의 SQL, SMTP 등)에서 AEM에서 제공하는 특수 호스트 이름을 통해 연결을 만들어야 합니다.

| 변수 이름 | 사용 | Java™ 코드 | OSGi 구성 | | - | - | - | - | | `AEM_PROXY_HOST` | 비HTTP/HTTPS 연결을 위한 프록시 호스트 | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


그런 다음 외부 서비스에 대한 연결을 `AEM_PROXY_HOST` 및 매핑된 포트(`portForwards.portOrig`). 그러면 AEM이 매핑된 외부 호스트 이름( )으로 라우팅됩니다.`portForwards.name`) 및 port( )`portForwards.portDest`).

| 프록시 호스트 | 프록시 포트 |  | 외부 호스트 | 외부 포트 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 코드 예

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="JDBC DataSourcePool을 사용한 SQL 연결" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">JDBC DataSourcePool을 사용한 SQL 연결</a></strong></div>
      <p>
            AEM JDBC 데이터 소스 풀을 구성하여 외부 SQL 데이터베이스에 연결하는 Java™ 코드 예입니다.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Java API를 사용한 SQL 연결" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Java™ API를 사용한 SQL 연결</a></strong></div>
      <p>
            Java™의 SQL API를 사용하여 외부 SQL 데이터베이스에 연결하는 Java™ 코드 예입니다.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="가상 사설 네트워크(VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">이메일 서비스</a></strong></div>
      <p>
        AEM을 사용하여 외부 이메일 서비스에 연결하는 OSGi 구성 예입니다.
      </p>
    </td>   
</tr></table>
