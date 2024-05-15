---
title: Virtual Private Network(VPN)
description: AEM AEM과 내부 서비스 간에 보안 통신 채널을 만들기 위해 VPN을 VPN에 as a Cloud Service으로 연결하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
last-substantial-update: 2024-04-26T00:00:00Z
duration: 919
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1472'
ht-degree: 1%

---

# Virtual Private Network(VPN)

AEM AEM과 내부 서비스 간에 보안 통신 채널을 만들기 위해 VPN을 VPN에 as a Cloud Service으로 연결하는 방법에 대해 알아봅니다.

## 가상 사설망이란?

AEM VPN(Virtual Private Network)을 사용하면 as a Cloud Service 고객이 연결할 수 있습니다 **AEM 환경** Cloud Manager 프로그램 내에서 기존 [지원됨](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) VPN. AEM VPN을 사용하면 고객 네트워크 내의 as a Cloud Service과 서비스 간에 안전하고 통제된 연결을 사용할 수 있습니다.

Cloud Manager 프로그램에는 __단일__ 네트워크 인프라 유형. Virtual Private Network가 가장 적합한지 확인 [적절한 유형의 네트워크 인프라](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) 다음 명령을 실행하기 전에 AEM에서 as a Cloud Service으로 확인하십시오.

>[!NOTE]
>
>Cloud Manager에서 VPN으로 빌드 환경을 연결하는 것은 지원되지 않습니다. 개인 저장소에서 바이너리 아티팩트에 액세스해야 하는 경우 공용 인터넷에서 사용할 수 있는 URL을 사용하여 암호로 보호된 안전한 저장소를 설정해야 합니다 [여기에 기재된 바와 같이](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project).

>[!MORELIKETHIS]
>
> AEM as a Cloud Service 읽기 [고급 네트워크 구성 설명서](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) 자세한 내용은 가상 개인 네트워크를 참조하십시오.

## 사전 요구 사항

Cloud Manager API를 사용하여 가상 개인 네트워크를 설정할 때 필요한 사항은 다음과 같습니다.

+ Adobe 계정 [Cloud Manager 비즈니스 소유자 권한](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 액세스 대상: [Cloud Manager API 인증 자격 증명](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + 조직 ID(예: IMS 조직 ID)
   + 클라이언트 ID(예: API 키)
   + 액세스 토큰(예: 전달자 토큰)
+ Cloud Manager 프로그램 ID
+ Cloud Manager 환경 ID
+ A **경로 기반** 필요한 모든 연결 매개 변수에 액세스할 수 있는 가상 사설망

자세한 내용은 다음 연습에서 Cloud Manager API 자격 증명을 설정, 구성 및 얻는 방법과 이를 사용하여 Cloud Manager API를 호출하는 방법을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

이 튜토리얼에서는 `curl` Cloud Manager API 구성을 만듭니다. 제공된 `curl` 명령은 Linux/macOS 구문을 사용합니다. Windows 명령 프롬프트를 사용하는 경우 `\` 줄 바꿈 문자 `^`.

## 프로그램당 Virtual Private Network 사용

AEMas a Cloud Service 에서 Virtual Private Network를 활성화하여 시작합니다.


>[!BEGINTABS]

>[!TAB Cloud Manager]

Cloud Manager를 사용하여 유연한 포트 이그레스를 활성화할 수 있습니다. 다음 단계에서는 Cloud Manager를 사용하여 AEM에서 as a Cloud Service으로 유연한 포트 이그레스를 활성화하는 방법에 대해 간략히 설명합니다.

1. 에 로그인합니다 [Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/) as a Cloud Manager 비즈니스 소유자.
1. 원하는 프로그램으로 이동합니다.
1. 왼쪽 메뉴에서 다음 위치로 이동합니다. __서비스 > 네트워크 인프라__.
1. 다음 항목 선택 __네트워크 인프라 추가__ 단추를 클릭합니다.

   ![네트워크 인프라 추가](./assets/cloud-manager__add-network-infrastructure.png)

1. 다음에서 __네트워크 인프라 추가__ 대화 상자에서 __가상 사설망__ 옵션을 선택합니다. 필드를 입력하고 선택 __계속__. 조직의 네트워크 관리자와 협력하여 정확한 값을 얻습니다.

   ![VPN 추가](./assets/vpn/select-type.png)

1. 최소 하나 이상의 VPN 연결을 만듭니다. 연결에 의미 있는 이름을 지정하고 __연결 추가__ 단추를 클릭합니다.

   ![VPN 연결 추가](./assets/vpn/add-connection.png)

1. VPN 연결을 구성합니다. 조직의 네트워크 관리자와 협력하여 정확한 값을 얻습니다. 선택 __저장__ 를 클릭하여 연결 추가를 확인합니다.

   ![VPN 연결 구성](./assets/vpn/configure-connection.png)

1. 여러 VPN 연결이 필요한 경우 필요에 따라 더 많은 연결을 해야 합니다. 모든 VPN 연결이 추가되면 다음을 선택합니다. __계속__.

   ![VPN 연결 구성](./assets/vpn/connections.png)

1. 선택 __저장__ vpn 및 구성된 모든 연결의 추가를 확인합니다.

   ![VPN 생성 확인](./assets/vpn/confirmation.png)

1. 네트워크 인프라가 만들어지고 다음으로 표시될 때까지 대기 __준비__. 이 프로세스는 최대 1시간 정도 소요될 수 있습니다.

   ![VPN 생성 상태](./assets/vpn/creating.png)

이제 VPN이 생성되면 아래 설명된 대로 Cloud Manager API를 사용하여 VPN을 구성할 수 있습니다.

>[!TAB Cloud Manager API]

Cloud Manager API를 사용하여 가상 사설망을 활성화할 수 있습니다. 다음 단계에서는 Cloud Manager API를 사용하여 AEM에서 as a Cloud Service으로 VPN을 활성화하는 방법에 대해 간략히 설명합니다.

1. 먼저 Cloud Manager API를 사용하여 고급 네트워킹이 필요한 지역을 결정합니다 [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 다음 `region name` 는 후속 Cloud Manager API를 호출하는 데 필요합니다. 일반적으로 프로덕션 환경이 있는 영역이 사용됩니다.

   에서 AEM as a Cloud Service 환경 지역 찾기 [Cloud Manager](https://my.cloudmanager.adobe.com) 다음 아래에 [환경 세부 정보](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments). Cloud Manager에 표시되는 지역 이름은 다음과 같을 수 있습니다. [지역 코드에 매핑됨](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) cloud Manager API에 사용됩니다.

   __listRegions HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Cloud Manager API를 사용하여 Cloud Manager 프로그램에 대한 Virtual Private Network 활성화 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 적절한 항목 사용 `region` Cloud Manager API에서 가져온 코드 `listRegions` 작업.

   __createNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   에서 JSON 매개 변수 정의 `vpn-create.json` 다음을 통해 curl에 제공 `... -d @./vpn-create.json`.

   [예제 vpn-create.json 다운로드](./assets/vpn-create.json).  이 파일은 예제일 뿐입니다. 다음 위치에 설명된 옵션/필수 필드를 기반으로 필요에 따라 파일을 구성합니다. [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   Cloud Manager 프로그램이 네트워크 인프라를 프로비저닝할 때까지 45~60분 동안 기다립니다.

1. 환경이 완료되었는지 확인합니다. __가상 사설망__ cloud Manager API를 사용한 구성 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 작업, 사용 `id` 에서 반환됨 `createNetworkInfrastructure` 이전 단계의 HTTP 요청입니다.

   __getNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 응답에 __상태__ / __준비__. 아직 준비되지 않은 경우 몇 분마다 상태를 다시 확인하십시오.


이제 VPN이 생성되면 아래 설명된 대로 Cloud Manager API를 사용하여 VPN을 구성할 수 있습니다.

>[!ENDTABS]

## 환경당 Virtual Private Network 프록시 구성

1. 활성화 및 구성 __가상 사설망__ cloud Manager API를 사용하여 각 AEM as a Cloud Service 환경에서 구성 [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   에서 JSON 매개 변수 정의 `vpn-configure.json` 다음을 통해 curl에 제공 `... -d @./vpn-configure.json`.

[vpn-configure.json 예제 다운로드](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   `nonProxyHosts` 포트 80 또는 443이 전용 이그레스 IP가 아닌 기본 공유 IP 주소 범위를 통해 라우팅되어야 하는 호스트 집합을 선언합니다. `nonProxyHosts` 공유 IP를 통해 이그레스되는 트래픽은 Adobe에 의해 자동으로 최적화되므로 유용할 수 있습니다.

   각 `portForwards` 매핑에서는 고급 네트워킹이 다음 전달 규칙을 정의합니다.

   | 프록시 호스트 | 프록시 포트 |  | 외부 호스트 | 외부 포트 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEM 배포의 경우 __전용__ 외부 서비스에 대한 HTTP/HTTPS 연결이 필요합니다. `portForwards` 이러한 규칙은 HTTP/HTTPS가 아닌 요청에만 필요하므로 배열이 비어 있습니다.


2. 각 환경에 대해 Cloud Manager API를 사용하여 VPN 라우팅 규칙이 적용되는지 확인합니다. [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업.

   __getEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

3. Cloud Manager API를 사용하여 가상 사설망 프록시 구성을 업데이트할 수 있습니다 [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 기억 `enableEnvironmentAdvancedNetworkingConfiguration` 다음 값: `PUT` 따라서 이 작업을 호출할 때마다 모든 규칙을 제공해야 합니다.

4. 이제 사용자 지정 AEM 코드 및 구성에서 Virtual Private Network 이그레스 구성을 사용할 수 있습니다.

## Virtual Private Network를 통해 외부 서비스에 연결

Virtual Private Network가 활성화되면 AEM 코드 및 구성은 이 코드를 사용하여 VPN을 통해 외부 서비스를 호출할 수 있습니다. AEM에서 다르게 처리하는 외부 호출에는 두 가지 유형이 있습니다.

1. 외부 서비스에 대한 HTTP/HTTPS 호출
   + 표준 80 또는 443 포트 이외의 포트에서 실행되는 서비스에 대한 HTTP/HTTPS 호출을 포함합니다.
1. 외부 서비스에 대한 비 HTTP/HTTPS 호출
   + 메일 서버, SQL 데이터베이스 또는 HTTP/HTTPS가 아닌 다른 프로토콜에서 실행되는 서비스와의 연결과 같은 HTTP가 아닌 호출을 포함합니다.

표준 포트(80/443)의 AEM에서 HTTP/HTTPS 요청은 기본적으로 허용되지만, 아래 설명된 대로 적절하게 구성되지 않은 경우 VPN 연결을 사용하지 마십시오.

### HTTP/HTTPS

AEM에서 HTTP/HTTPS 연결을 만들 때 VPN을 사용하면 HTTP/HTTPS 연결이 AEM에서 자동으로 프록시됩니다. HTTP/HTTPS 연결을 지원하는 데 추가적인 코드나 구성은 필요하지 않습니다.

>[!TIP]
>
> 다음에 대한 AEM as a Cloud Service의 Virtual Private Network 설명서 를 참조하십시오. [전체 라우팅 규칙 세트](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking).

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

### 비 HTTP/HTTPS 연결 코드 예

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

### VPN을 통한 AEM as a Cloud Service 액세스 제한

AEM 가상 사설망 구성은 VPN에 대한 as a Cloud Service 환경에 대한 액세스를 제한합니다.

#### 구성 예

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list"><img alt="IP 허용 목록 적용" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list">IP 허용 목록 적용</a></strong></div>
      <p>
            허용 목록에 추가하다 VPN 트래픽만 AEM에 액세스할 수 있도록 IP 트래픽만 구성합니다.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking"><img alt="AEM 게시로의 경로 기반 VPN 액세스 제한" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking">AEM 게시로의 경로 기반 VPN 액세스 제한</a></strong></div>
      <p>
            AEM Publish의 특정 경로에 대해 VPN 액세스 권한이 필요합니다.
      </p>
    </td>
   <td></td>
</tr></table>
