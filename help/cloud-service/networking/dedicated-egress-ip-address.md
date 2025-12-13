---
title: 전용 이그레스 IP 주소
description: AEM의 아웃바운드 연결이 전용 IP에서 시작되도록 허용하는 전용 이그레스 IP 주소를 설정하고 사용하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
last-substantial-update: 2024-04-26T00:00:00Z
duration: 891
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1370'
ht-degree: 2%

---

# 전용 이그레스 IP 주소

AEM의 아웃바운드 연결이 전용 IP에서 시작되도록 허용하는 전용 이그레스 IP 주소를 설정하고 사용하는 방법에 대해 알아봅니다.

## 전용 이그레스 IP 주소란 무엇입니까?

전용 이그레스 IP 주소를 사용하면 AEM as a Cloud Service의 요청에서 전용 IP 주소를 사용할 수 있으므로 외부 서비스는 이 IP 주소로 들어오는 요청을 필터링할 수 있습니다. [유연한 이그레스 포트](./flexible-port-egress.md)와 같이 전용 이그레스 IP를 사용하면 비표준 포트에서의 이그레스를 사용할 수 있습니다.

Cloud Manager 프로그램에는 __single__ 네트워크 인프라 유형만 있을 수 있습니다. 다음 명령을 실행하기 전에 전용 이그레스 IP 주소가 AEM as a Cloud Service에 대해 가장 [적합한 유형의 네트워크 인프라](./advanced-networking.md)인지 확인하십시오.

>[!MORELIKETHIS]
>
> 전용 이그레스 IP 주소에 대한 자세한 내용은 AEM as a Cloud Service [고급 네트워크 구성 설명서](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)를 참조하십시오.

## 사전 요구 사항

Cloud Manager API를 사용하여 전용 이그레스 IP 주소를 설정할 때 필요한 사항은 다음과 같습니다.

+ [Cloud Manager 비즈니스 소유자 권한이 있는 Cloud Manager API](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ [Cloud Manager API 인증 자격 증명](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)에 액세스
   + 조직 ID(예: IMS 조직 ID)
   + 클라이언트 ID(예: API 키)
   + 액세스 토큰(예: 전달자 토큰)
+ Cloud Manager 프로그램 ID
+ Cloud Manager 환경 ID

자세한 내용을 보려면 [Cloud Manger API 자격 증명을 설정, 구성 및 얻는 방법](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/extensibility/app-builder/server-to-server-auth)을 검토하여 Cloud Manager API를 호출하는 데 사용하십시오.

이 자습서에서는 `curl`을(를) 사용하여 Cloud Manager API 구성을 만듭니다. 제공된 `curl` 명령은 Linux/macOS 구문을 사용합니다. Windows 명령 프롬프트를 사용하는 경우 `\` 줄 바꿈 문자를 `^`(으)로 바꾸십시오.

## 프로그램에서 전용 이그레스 IP 주소 활성화

AEM as a Cloud Service에서 전용 이그레스 IP 주소를 활성화하고 구성하여 시작합니다.

>[!BEGINTABS]

>[!TAB Cloud Manager]

Cloud Manager을 사용하여 전용 이그레스 IP 주소를 활성화할 수 있습니다. 다음 단계에서는 Cloud Manager을 사용하여 AEM as a Cloud Service에서 전용 이그레스 IP 주소를 활성화하는 방법에 대해 간략히 설명합니다.

1. [Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/)에 Cloud Manager 비즈니스 소유자로 로그인합니다.
1. 원하는 프로그램으로 이동합니다.
1. 왼쪽 메뉴에서 __서비스 > 네트워크 인프라__&#x200B;로 이동합니다.
1. __네트워크 인프라 추가__ 단추를 선택하십시오.

   ![네트워크 인프라 추가](./assets/cloud-manager__add-network-infrastructure.png)

1. __네트워크 인프라 추가__ 대화 상자에서 __전용 이그레스 IP 주소__ 옵션을 선택한 다음 __지역__&#x200B;을 선택하여 전용 이그레스 IP 주소를 만듭니다.

   ![전용 이그레스 IP 주소 추가](./assets/dedicated-egress-ip-address/select-type.png)

1. 전용 이그레스 IP 주소의 추가를 확인하려면 __저장__&#x200B;을(를) 선택하십시오.

   ![전용 이그레스 IP 주소 만들기 확인](./assets/dedicated-egress-ip-address/confirmation.png)

1. 네트워크 인프라가 만들어지고 __준비__(으)로 표시될 때까지 기다립니다. 이 프로세스는 최대 1시간 정도 소요될 수 있습니다.

   ![전용 이그레스 IP 주소 만들기 상태](./assets/dedicated-egress-ip-address/ready.png)

이제 만든 전용 이그레스 IP 주소에서 아래에 설명된 대로 Cloud Manager API를 사용하여 전용 이그레스 IP 주소를 구성할 수 있습니다.

>[!TAB Cloud Manager API]

Cloud Manager API를 사용하여 전용 이그레스 IP 주소를 활성화할 수 있습니다. 다음 단계에서는 Cloud Manager API를 사용하여 AEM as a Cloud Service에서 전용 이그레스 IP 주소를 활성화하는 방법에 대해 간략히 설명합니다.


1. 먼저 Cloud Manager API [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업을 사용하여 고급 네트워킹이 필요한 지역을 결정합니다. 후속 Cloud Manager API를 호출하려면 `region name`이(가) 필요합니다. 일반적으로 프로덕션 환경이 있는 영역이 사용됩니다.

   [환경의 세부 정보](https://my.cloudmanager.adobe.com)에서 [Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments)의 AEM as a Cloud Service 환경 지역을 찾으십시오. Cloud Manager에 표시되는 지역 이름은 Cloud Manager API에서 사용되는 지역 코드 [에 ](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments)매핑될 수 있습니다.

   __listRegions HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

2. Cloud Manager API [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업을 사용하여 Cloud Manager 프로그램에 대한 전용 이그레스 IP 주소를 사용하도록 설정합니다. Cloud Manager API `region` 작업에서 얻은 적절한 `listRegions` 코드를 사용합니다.

   __createNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Cloud Manager 프로그램이 네트워크 인프라를 프로비저닝할 때까지 15분 동안 기다립니다.

3. 이전 단계의 __HTTP 요청에서 반환된__&#x200B;을(를) 사용하여 Cloud Manager API [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 작업을 사용하여 프로그램이 `id`전용 이그레스 IP 주소`createNetworkInfrastructure` 구성을 완료했는지 확인하십시오.

   __getNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 응답에 __준비__&#x200B;의 __상태__&#x200B;가 포함되어 있는지 확인하십시오. 아직 준비되지 않은 경우 몇 분마다 상태를 다시 확인하십시오.

이제 만든 전용 이그레스 IP 주소에서 아래에 설명된 대로 Cloud Manager API를 사용하여 전용 이그레스 IP 주소를 구성할 수 있습니다.

>[!ENDTABS]


## 환경당 전용 이그레스 IP 주소 프록시 구성

1. Cloud Manager API __enableEnvironmentAdvancedNetworkingConfiguration__ 작업을 사용하여 각 AEM as a Cloud Service 환경에서 [전용 이그레스 IP 주소](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 구성을 구성합니다.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   `dedicated-egress-ip-address.json`에서 JSON 매개 변수를 정의하고 `... -d @./dedicated-egress-ip-address.json`을(를) 통해 curl에 제공합니다.

   [전용 이그레스-ip-address.json 예제를 다운로드합니다](./assets/dedicated-egress-ip-address.json). 이 파일은 예제일 뿐입니다. [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)에 문서화된 선택적/필수 필드를 기반으로 파일을 필요에 따라 구성합니다.

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

   전용 이그레스 IP 주소 구성의 HTTP 시그니처는 선택적 [ 구성도 지원한다는 점에서 ](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment)유연한 이그레스 포트`nonProxyHosts`와 다릅니다.

   `nonProxyHosts`은(는) 포트 80 또는 443이 전용 이그레스 IP가 아닌 기본 공유 IP 주소 범위를 통해 라우팅되어야 하는 호스트 집합을 선언합니다. 공유 IP를 통해 이그레스되는 트래픽이 Adobe에서 자동으로 최적화되므로 `nonProxyHosts`이(가) 유용할 수 있습니다.

   각 `portForwards` 매핑에 대해 고급 네트워킹은 다음 전달 규칙을 정의합니다.

   | 프록시 호스트 | 프록시 포트 |  | 외부 호스트 | 외부 포트 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 각 환경에 대해 Cloud Manager API [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업을 사용하여 이그레스 규칙이 적용되는지 확인하십시오.

   __getEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Cloud Manager API [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업을 사용하여 전용 이그레스 IP 주소 구성을 업데이트할 수 있습니다. `enableEnvironmentAdvancedNetworkingConfiguration`은(는) `PUT` 작업이므로 이 작업을 호출할 때마다 모든 규칙을 제공해야 합니다.

1. __호스트에서 DNS Resolver(예:__ DNSChecker.org[)를 사용하거나 명령줄에서 ](https://dnschecker.org/)을(를) 실행하여 `p{programId}.external.adobeaemcloud.com`전용 이그레스 IP 주소`dig`을(를) 가져옵니다.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   호스트 이름은 `pinged`일 수 없습니다. 이그레스이고 _not_ 및 인그레스입니다.

   전용 이그레스 IP 주소는 프로그램의 모든 AEM as a Cloud Service 환경에서 공유됩니다.

1. 이제 사용자 지정 AEM 코드 및 구성에서 전용 이그레스 IP 주소를 사용할 수 있습니다. 전용 이그레스 IP 주소를 사용할 때 AEM as a Cloud Service이 연결하는 외부 서비스는 이 전용 IP 주소의 트래픽만 허용하도록 구성되는 경우가 많습니다.

## 전용 이그레스 IP 주소를 통해 외부 서비스에 연결

전용 이그레스 IP 주소를 활성화한 상태에서 AEM 코드 및 구성은 전용 이그레스 IP를 사용하여 외부 서비스를 호출할 수 있습니다. AEM에서 다르게 처리하는 외부 호출에는 두 가지 유형이 있습니다.

1. 외부 서비스에 대한 HTTP/HTTPS 호출
   + 표준 80 또는 443 포트 이외의 포트에서 실행되는 서비스에 대한 HTTP/HTTPS 호출을 포함합니다.
1. 외부 서비스에 대한 비 HTTP/HTTPS 호출
   + 메일 서버, SQL 데이터베이스 또는 HTTP/HTTPS가 아닌 다른 프로토콜에서 실행되는 서비스와의 연결과 같은 HTTP가 아닌 호출을 포함합니다.

표준 포트(80/443)의 AEM에서 HTTP/HTTPS 요청은 기본적으로 허용되지만, 아래에 설명된 대로 적절하게 구성되지 않은 경우 전용 이그레스 IP 주소를 사용하지 않습니다.

>[!TIP]
>
> [전체 라우팅 규칙 집합](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)에 대해서는 AEM as a Cloud Service의 전용 이그레스 IP 주소 설명서를 참조하십시오.


### HTTP/HTTPS

AEM에서 HTTP/HTTPS 연결을 만들 때 전용 이그레스 IP 주소를 사용하는 경우 HTTP/HTTPS 연결은 전용 이그레스 IP 주소를 사용하여 AEM에서 자동으로 프록시됩니다. 전용 이그레스 IP 주소 고급 네트워킹을 설정하는 것 외에 HTTP/HTTPS 연결을 지원하는 데 추가적인 코드나 구성은 필요하지 않습니다.

#### 코드 예

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        HTTP/HTTPS 프로토콜을 사용하여 AEM as a Cloud Service에서 외부 서비스로의 HTTP/HTTPS 연결을 만드는 Java™ 코드 예입니다.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### 외부 서비스에 대한 비 HTTP/HTTPS 연결

비HTTP/HTTPS 연결을 만드는 경우(예: SQL, SMTP 등). AEM에서 연결은 AEM에서 제공하는 특수 호스트 이름을 통해 이루어져야 합니다.

| 변수 이름 | 사용 | Java™ 코드 | OSGi 구성 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | 비 HTTP/HTTPS 연결용 프록시 호스트 | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


외부 서비스에 대한 연결은 `AEM_PROXY_HOST` 및 매핑된 포트(`portForwards.portOrig`)를 통해 호출되며, AEM은 매핑된 외부 호스트 이름(`portForwards.name`) 및 포트(`portForwards.portDest`)로 라우팅됩니다.

| 프록시 호스트 | 프록시 포트 |  | 외부 호스트 | 외부 포트 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 코드 예

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="JDBC DataSourcePool을 사용한 SQL 연결" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">JDBC DataSourcePool을 사용한 SQL 연결</a></strong></div>
      <p>
            AEM의 JDBC 데이터 소스 풀을 구성하여 외부 SQL 데이터베이스에 연결하는 Java™ 코드 예입니다.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Java API를 사용한 SQL 연결" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Java™ API를 사용한 SQL 연결</a></strong></div>
      <p>
            Java™ 코드 예제 Java™의 SQL API를 사용하여 외부 SQL 데이터베이스에 연결.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="VPN (Virtual Private Network)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">전자 메일 서비스</a></strong></div>
      <p>
        AEM을 사용하여 외부 이메일 서비스에 연결하는 OSGi 구성 예입니다.
      </p>
    </td>   
</tr></table>
