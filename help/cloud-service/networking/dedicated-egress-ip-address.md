---
title: 전용 이그레스 IP 주소
description: AEM의 아웃바운드 연결이 전용 IP에서 시작되도록 허용하는 전용 이그레스 IP 주소를 설정하고 사용하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
duration: 961
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 1%

---

# 전용 이그레스 IP 주소

AEM의 아웃바운드 연결이 전용 IP에서 시작되도록 허용하는 전용 이그레스 IP 주소를 설정하고 사용하는 방법에 대해 알아봅니다.

## 전용 이그레스 IP 주소란 무엇입니까?

전용 이그레스 IP 주소를 사용하면 AEMas a Cloud Service 의 요청이 전용 IP 주소를 사용할 수 있으므로 외부 서비스가 이 IP 주소로 들어오는 요청을 필터링할 수 있습니다. 좋아요 [유연한 이그레스 포트](./flexible-port-egress.md), 전용 이그레스 IP를 사용하면 비표준 포트에서의 이그레스를 사용할 수 있습니다.

Cloud Manager 프로그램에는 __단일__ 네트워크 인프라 유형. 전용 이그레스 IP 주소가 가장 큰지 확인합니다. [적절한 유형의 네트워크 인프라](./advanced-networking.md)  다음 명령을 실행하기 전에 AEM에서 as a Cloud Service으로 확인하십시오.

>[!MORELIKETHIS]
>
> AEM as a Cloud Service 읽기 [고급 네트워크 구성 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) 전용 이그레스 IP 주소에 대한 자세한 내용.

## 사전 요구 사항

전용 이그레스 IP 주소를 설정할 때 필요한 사항은 다음과 같습니다.

+ 을 사용한 Cloud Manager API [Cloud Manager 비즈니스 소유자 권한](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 액세스 대상: [Cloud Manager API 인증 인증서](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 조직 ID(예: IMS 조직 ID)
   + 클라이언트 ID(예: API 키)
   + 액세스 토큰(예: 전달자 토큰)
+ Cloud Manager 프로그램 ID
+ Cloud Manager 환경 ID

자세한 내용은 다음 연습에서 Cloud Manager API 자격 증명을 설정, 구성 및 얻는 방법과 이를 사용하여 Cloud Manager API를 호출하는 방법을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

이 튜토리얼에서는 `curl` Cloud Manager API 구성을 만듭니다. 제공된 `curl` 명령은 Linux/macOS 구문을 사용합니다. Windows 명령 프롬프트를 사용하는 경우 `\` 줄 바꿈 문자 `^`.

## 프로그램에서 전용 이그레스 IP 주소 활성화

AEMas a Cloud Service 에서 전용 이그레스 IP 주소를 활성화하고 구성하여 시작합니다.

1. 먼저 Cloud Manager API를 사용하여 고급 네트워킹이 필요한 지역을 결정합니다 [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 다음 `region name` 는 후속 Cloud Manager API를 호출하는 데 필요합니다. 일반적으로 프로덕션 환경이 있는 영역이 사용됩니다.

   에서 AEM as a Cloud Service 환경 지역 찾기 [Cloud Manager](https://my.cloudmanager.adobe.com) 다음 아래에 [환경 세부 정보](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager에 표시되는 지역 이름은 다음과 같을 수 있습니다. [지역 코드에 매핑됨](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) cloud Manager API에 사용됩니다.

   __listRegions HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Cloud Manager API를 사용하여 Cloud Manager 프로그램에 대한 전용 이그레스 IP 주소 활성화 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 적절한 항목 사용 `region` Cloud Manager API에서 가져온 코드 `listRegions` 작업.

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

1. 프로그램이 완료되었는지 확인 __전용 이그레스 IP 주소__ cloud Manager API를 사용한 구성 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 작업, 사용 `id` 이전 단계의 createNetworkInfrastructure HTTP 요청에서 반환되었습니다.

   __getNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 응답에 __상태__ / __준비__. 아직 준비되지 않은 경우 몇 분마다 상태를 다시 확인하십시오.

## 환경당 전용 이그레스 IP 주소 프록시 구성

1. 구성 __전용 이그레스 IP 주소__ cloud Manager API를 사용하여 각 AEM as a Cloud Service 환경에서 구성 [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   에서 JSON 매개 변수 정의 `dedicated-egress-ip-address.json` 다음을 통해 curl에 제공 `... -d @./dedicated-egress-ip-address.json`.

   [전용-이그레스-ip-address.json 예 다운로드](./assets/dedicated-egress-ip-address.json). 이 파일은 예제일 뿐입니다. 다음 위치에 설명된 옵션/필수 필드를 기반으로 필요에 따라 파일을 구성합니다. [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   전용 이그레스 IP 주소 구성의 HTTP 서명은 [유연한 송신 포트](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) 는 또한 선택 사항을 지원합니다. `nonProxyHosts` 구성.

   `nonProxyHosts` 포트 80 또는 443이 전용 이그레스 IP가 아닌 기본 공유 IP 주소 범위를 통해 라우팅되어야 하는 호스트 집합을 선언합니다. `nonProxyHosts` 공유 IP를 통해 이그레스되는 트래픽은 Adobe에 의해 자동으로 더 최적화될 수 있으므로 유용할 수 있습니다.

   각 `portForwards` 매핑에서는 고급 네트워킹이 다음 전달 규칙을 정의합니다.

   | 프록시 호스트 | 프록시 포트 |  | 외부 호스트 | 외부 포트 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 각 환경에 대해 Cloud Manager API를 사용하여 이그레스 규칙이 적용되는지 확인합니다 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업.

   __getEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 전용 이그레스 IP 주소 구성은 Cloud Manager API를 사용하여 업데이트할 수 있습니다 [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 기억 `enableEnvironmentAdvancedNetworkingConfiguration` 다음 값: `PUT` 따라서 이 작업을 호출할 때마다 모든 규칙을 제공해야 합니다.

1. 다음을 획득 __전용 이그레스 IP 주소__ DNS Resolver 사용(예: [DNSChecker.org](https://dnschecker.org/))를 사용할 수 없습니다. `p{programId}.external.adobeaemcloud.com`또는 를 실행하여 `dig` 명령줄에서 을 클릭합니다.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   호스트 이름은 다음과 같을 수 없습니다. `pinged`, 이그레스 및 _아님_ 인그레스

   다음을 참고하십시오. __전용 이그레스 IP 주소__ 는 프로그램의 모든 AEM as a Cloud Service 환경에서 공유됩니다.

1. 이제 사용자 지정 AEM 코드 및 구성에서 전용 이그레스 IP 주소를 사용할 수 있습니다. AEM 전용 이그레스 IP 주소를 사용하는 경우 일반적으로 as a Cloud Service으로 연결하는 외부 서비스는 이 전용 IP 주소의 트래픽만 허용하도록 구성됩니다.

## 전용 이그레스 IP 주소를 통해 외부 서비스에 연결

전용 이그레스 IP 주소를 활성화한 상태에서 AEM 코드 및 구성은 전용 이그레스 IP를 사용하여 외부 서비스를 호출할 수 있습니다. AEM에서 다르게 처리하는 외부 호출에는 두 가지 유형이 있습니다.

1. 외부 서비스에 대한 HTTP/HTTPS 호출
   + 표준 80 또는 443 포트 이외의 포트에서 실행되는 서비스에 대한 HTTP/HTTPS 호출을 포함합니다.
1. 외부 서비스에 대한 비 HTTP/HTTPS 호출
   + 메일 서버, SQL 데이터베이스 또는 HTTP/HTTPS가 아닌 다른 프로토콜에서 실행되는 서비스와의 연결과 같은 HTTP가 아닌 호출을 포함합니다.

표준 포트(80/443)의 AEM에서 HTTP/HTTPS 요청은 기본적으로 허용되지만, 아래 설명된 대로 적절하게 구성되지 않은 경우 전용 이그레스 IP 주소를 사용하지 않습니다.

>[!TIP]
>
> 다음에 대한 AEM as a Cloud Service의 전용 이그레스 IP 주소 설명서 를 참조하십시오. [전체 라우팅 규칙 세트](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

AEM에서 HTTP/HTTPS 연결을 만들 때 전용 이그레스 IP 주소를 사용하는 경우 HTTP/HTTPS 연결은 전용 이그레스 IP 주소를 사용하여 AEM에서 자동으로 프록시됩니다. HTTP/HTTPS 연결을 지원하는 데 추가적인 코드나 구성은 필요하지 않습니다.

#### 코드 예

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        HTTP/HTTPS 프로토콜을 사용하여 AEM에서 외부 서비스로 as a Cloud Service으로 HTTP/HTTPS 연결을 만드는 Java™ 코드 예입니다.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### 외부 서비스에 대한 비 HTTP/HTTPS 연결

비HTTP/HTTPS 연결을 만드는 경우(예: SQL, SMTP 등) AEM에서 AEM이 제공하는 특수 호스트 이름을 통해 연결해야 합니다.

| 변수 이름 | 사용 | Java™ 코드 | OSGi 구성 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | 비 HTTP/HTTPS 연결용 프록시 호스트 | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


외부 서비스에 대한 연결은 `AEM_PROXY_HOST` 및 매핑된 포트(`portForwards.portOrig`): AEM이 매핑된 외부 호스트 이름( )으로 라우팅합니다.`portForwards.name`) 및 포트(`portForwards.portDest`).

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
            Java™ 코드 예제 Java™의 SQL API를 사용하여 외부 SQL 데이터베이스에 연결.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network(VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">이메일 서비스</a></strong></div>
      <p>
        AEM을 사용하여 외부 이메일 서비스에 연결하는 OSGi 구성 예입니다.
      </p>
    </td>   
</tr></table>
