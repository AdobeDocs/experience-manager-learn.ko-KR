---
title: 유연한 포트 전송
description: 유연한 포트 이그레스를 설정하고 사용하여 AEMas a Cloud Service 에서 외부 서비스로의 외부 연결을 지원하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
duration: 906
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 2%

---

# 유연한 포트 전송

유연한 포트 이그레스를 설정하고 사용하여 AEMas a Cloud Service 에서 외부 서비스로의 외부 연결을 지원하는 방법에 대해 알아봅니다.

## 유연한 포트 이그레스란 무엇입니까?

유연한 포트 이그레스에서는 사용자 지정 특정 포트 전달 규칙을 AEM에 as a Cloud Service으로 첨부할 수 있으므로 AEM에서 외부 서비스에 연결할 수 있습니다.

Cloud Manager 프로그램에는 __단일__ 네트워크 인프라 유형. 전용 이그레스 IP 주소가 가장 큰지 확인합니다. [적절한 유형의 네트워크 인프라](./advanced-networking.md)  다음 명령을 실행하기 전에 AEM에서 as a Cloud Service으로 확인하십시오.

>[!MORELIKETHIS]
>
> AEM as a Cloud Service 읽기 [고급 네트워크 구성 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) 유연한 포트 이그레스에 대한 자세한 내용.

## 사전 요구 사항

유연한 포트 이그레스를 설정할 때 필요한 사항은 다음과 같습니다.

+ Cloud Manager API가 활성화된 Adobe Developer 콘솔 프로젝트 및 [Cloud Manager 비즈니스 소유자 권한](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 액세스 대상: [Cloud Manager API 인증 자격 증명](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + 조직 ID(예: IMS 조직 ID)
   + 클라이언트 ID(예: API 키)
   + 액세스 토큰(예: 전달자 토큰)
+ Cloud Manager 프로그램 ID
+ Cloud Manager 환경 ID

자세한 내용은 다음 연습에서 Cloud Manager API 자격 증명을 설정, 구성 및 얻는 방법과 이를 사용하여 Cloud Manager API를 호출하는 방법을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

이 튜토리얼에서는 `curl` Cloud Manager API 구성을 만듭니다. 제공된 `curl` 명령은 Linux/macOS 구문을 사용합니다. Windows 명령 프롬프트를 사용하는 경우 `\` 줄 바꿈 문자 `^`.

## 프로그램당 유연한 포트 이그레스 활성화

AEMas a Cloud Service 에서 유연한 포트 이그레스를 활성화하여 시작합니다.

1. 먼저 Cloud Manager API를 사용하여에서 고급 네트워킹이 설정된 영역을 확인합니다 [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 다음 `region name` 는 후속 Cloud Manager API를 호출하는 데 필요합니다. 일반적으로 프로덕션 환경이 있는 영역이 사용됩니다.

   에서 AEM as a Cloud Service 환경 지역 찾기 [Cloud Manager](https://my.cloudmanager.adobe.com) 다음 아래에 [환경 세부 정보](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager에 표시되는 지역 이름은 다음과 같을 수 있습니다. [지역 코드에 매핑됨](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) cloud Manager API에 사용됩니다.

   __listRegions HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Cloud Manager API를 사용하여 Cloud Manager 프로그램에 대해 유연한 포트 이그레스 활성화 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 적절한 항목 사용 `region` Cloud Manager API에서 가져온 코드 `listRegions` 작업.

   __createNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Cloud Manager 프로그램이 네트워크 인프라를 프로비저닝할 때까지 15분 동안 기다립니다.

1. 환경이 완료되었는지 확인합니다. __유연한 포트 이그레스__ cloud Manager API를 사용한 구성 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 작업, 사용 `id` 이전 단계의 createNetworkInfrastructure HTTP 요청에서 반환되었습니다.

   __getNetworkInfrastructure HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 응답에 __상태__ / __준비__. 아직 준비되지 않은 경우 몇 분마다 상태를 다시 확인하십시오.

## 환경별로 유연한 포트 이그레스 프록시 구성

1. 활성화 및 구성 __유연한 포트 이그레스__ cloud Manager API를 사용하여 각 AEM as a Cloud Service 환경에서 구성 [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   에서 JSON 매개 변수 정의 `flexible-port-egress.json` 다음을 통해 curl에 제공 `... -d @./flexible-port-egress.json`.

   [유연한 port-egress.json 예제 다운로드](./assets/flexible-port-egress.json). 이 파일은 예제일 뿐입니다. 다음 위치에 설명된 옵션/필수 필드를 기반으로 필요에 따라 파일을 구성합니다. [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
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

   각 `portForwards` 매핑에서는 고급 네트워킹이 다음 전달 규칙을 정의합니다.

   | 프록시 호스트 | 프록시 포트 |  | 외부 호스트 | 외부 포트 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEM 배포의 경우 __전용__ 외부 서비스에 대한 HTTP/HTTPS 연결(포트 80/443)이 필요하므로 `portForwards` 이러한 규칙은 HTTP/HTTPS가 아닌 요청에만 필요하므로 배열이 비어 있습니다.

1. 각 환경에 대해 Cloud Manager API를 사용하여 이그레스 규칙이 적용되는지 확인합니다 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업.

   __getEnvironmentAdvancedNetworkingConfiguration HTTP 요청__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Cloud Manager API를 사용하여 유연한 포트 이그레스 구성을 업데이트할 수 있습니다 [enableEnvironmentAdvancedNetworkConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 작업. 기억 `enableEnvironmentAdvancedNetworkingConfiguration` 다음 값: `PUT` 따라서 이 작업을 호출할 때마다 모든 규칙을 제공해야 합니다.

1. 이제 사용자 지정 AEM 코드 및 구성에서 유연한 포트 이그레스 구성을 사용할 수 있습니다.


## 유연한 포트 이그레스를 통해 외부 서비스에 연결

유연한 포트 이그레스 프록시가 활성화되면 AEM 코드 및 구성은 이를 사용하여 외부 서비스를 호출할 수 있습니다. AEM에서 다르게 처리하는 외부 호출에는 두 가지 유형이 있습니다.

1. 비표준 포트의 외부 서비스에 대한 HTTP/HTTPS 호출
   + 표준 80 또는 443 포트 이외의 포트에서 실행되는 서비스에 대한 HTTP/HTTPS 호출을 포함합니다.
1. 외부 서비스에 대한 비 HTTP/HTTPS 호출
   + 메일 서버, SQL 데이터베이스 또는 HTTP/HTTPS가 아닌 다른 프로토콜에서 실행되는 서비스와의 연결과 같은 HTTP가 아닌 호출을 포함합니다.

표준 포트(80/443)의 AEM에서 HTTP/HTTPS 요청은 기본적으로 허용되며 추가 구성이나 고려 사항이 필요하지 않습니다.


### 비표준 포트에서의 HTTP/HTTPS

AEM에서 비표준 포트(80/443이 아닌 포트)에 대한 HTTP/HTTPS 연결을 만드는 경우 자리 표시자를 통해 제공되는 특수 호스트 및 포트를 통해 연결을 만들어야 합니다.

AEM은 AEM HTTP/HTTPS 프록시에 매핑되는 두 세트의 특수 Java™ 시스템 변수를 제공합니다.

| 변수 이름 | 사용 | Java™ 코드 | OSGi 구성 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | HTTP/HTTPS 연결 모두에 대한 프록시 호스트 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | HTTPS 연결을 위한 프록시 포트(폴백 설정) `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | HTTPS 연결을 위한 프록시 포트(폴백 설정) `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

비표준 포트에서 외부 서비스에 HTTP/HTTPS를 호출하는 경우 해당 포트 없음 `portForwards` 는 Cloud Manager API를 사용하여 정의해야 합니다 `enableEnvironmentAdvancedNetworkingConfiguration` 포트 전달 &quot;rules&quot;가 &quot;in code&quot;로 정의되므로 작업입니다.

>[!TIP]
>
> 다음에 대한 AEM as a Cloud Service의 유연한 포트 이그레스 설명서 를 참조하십시오. [전체 라우팅 규칙 세트](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### 코드 예

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="비표준 포트에서의 HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">비표준 포트에서의 HTTP/HTTPS</a></strong></div>
    <p>
        AEM에서 비표준 HTTP/HTTPS 포트의 외부 서비스에 as a Cloud Service으로 HTTP/HTTPS 연결을 만드는 Java™ 코드 예입니다.
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
| `AEM_PROXY_HOST` | 비 HTTP/HTTPS 연결용 프록시 호스트 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


외부 서비스에 대한 연결은 `AEM_PROXY_HOST` 및 매핑된 포트(`portForwards.portOrig`): AEM이 매핑된 외부 호스트 이름( )으로 라우팅합니다.`portForwards.name`) 및 포트(`portForwards.portDest`).

| 프록시 호스트 | 프록시 포트 |  | 외부 호스트 | 외부 포트 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 코드 예

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="JDBC DataSourcePool을 사용한 SQL 연결" src="./assets/code-examples__sql-osgi.png"/></a>
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
