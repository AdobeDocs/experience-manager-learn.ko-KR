---
title: 고급 네트워킹
description: AEM as a Cloud Service의 고급 네트워킹 옵션에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 157
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 2%

---

# 고급 네트워킹

AEM as a Cloud Service은 AEM as a Cloud Service 프로그램에 대한 연결 및 연결을 정확하게 관리할 수 있는 고급 네트워킹 기능을 제공합니다.

|                                                   | [프로덕션 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| 고급 네트워킹 지원 | ✔ | ✘ |


AEM 고급 네트워킹은 외부 서비스와의 연결을 관리하는 세 가지 옵션으로 구성됩니다. Cloud Manager 프로그램 및 해당 AEM as a Cloud Service 환경은 한 번에 단일 유형의 고급 네트워킹 구성만 사용할 수 있으므로 가장 적절한 유형이 선택되었는지 확인합니다.

|                                   | 표준 포트에서의 HTTP/HTTPS | 비표준 포트에서의 HTTP/HTTPS | 비 HTTP/HTTPS 연결 | 전용 이그레스 IP | &quot;프록시 호스트 없음&quot; 목록 | VPN으로 보호된 서비스에 연결 | IP별로 AEM 게시 트래픽 제한 |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __고급 네트워킹 없음__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__유연한 포트 이그레스__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__전용 이그레스 IP 주소__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__가상 사설망__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


적절한 고급 네트워킹 유형을 선택할 때 고려할 사항에 대한 자세한 내용은 [고급 네트워킹 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## 고급 네트워킹 자습서

조직의 필요에 따라 가장 적합한 고급 네트워킹 옵션이 식별되면 아래의 해당 튜토리얼을 클릭하여 단계별 지침 및 코드 샘플을 받으십시오.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="유연한 포트 이그레스" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">유연한 포트 이그레스</a></strong></div>
      <p>
          비표준 포트에서 아웃바운드 AEM as a Cloud Service 트래픽을 허용합니다.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="파일 전용 이그레스 IP 주소" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">전용 이그레스 IP 주소</a></strong></div>
      <p>
        전용 IP에서 아웃바운드 AEM as a Cloud Service 트래픽을 생성합니다.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Virtual Private Network(VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">가상 사설망(VPN)</a></strong></div>
      <p>
        고객 또는 공급업체 인프라와 AEM 간의 트래픽을 as a Cloud Service으로 보호합니다.
      </p>
    </td>   
  </tr>
</table>

## 코드 예

이 컬렉션은 특정 사용 사례에 대한 고급 네트워킹 기능을 활용하는 데 필요한 구성 및 코드의 예제를 제공합니다.

적절한지 확인 [고급 네트워킹 구성](#advanced-networking) 이 튜토리얼을 수행하기 전에 가 설정되었습니다.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network(VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">이메일 서비스</a></strong></div>
      <p>
        AEM을 사용하여 외부 이메일 서비스에 연결하는 OSGi 구성 예입니다.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            HTTP/HTTPS 프로토콜을 사용하여 AEM에서 외부 서비스로 as a Cloud Service으로 HTTP/HTTPS 연결을 만드는 Java™ 코드 예입니다.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="JDBC DataSourcePool을 사용한 SQL 연결" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">JDBC DataSourcePool을 사용한 SQL 연결</a></strong></div>
      <p>
            AEM JDBC 데이터 소스 풀을 구성하여 외부 SQL 데이터베이스에 연결하는 Java™ 코드 예입니다.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Java API를 사용한 SQL 연결" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Java™ API를 사용한 SQL 연결</a></strong></div>
      <p>
            Java™ 코드 예제 Java™의 SQL API를 사용하여 외부 SQL 데이터베이스에 연결.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="IP 허용 목록 적용" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">IP 허용 목록 적용</a></strong></div>
      <p>
            허용 목록에 추가하다 VPN 트래픽만 AEM에 액세스할 수 있도록 IP 트래픽만 구성합니다.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM 게시로의 경로 기반 VPN 액세스 제한" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">AEM 게시로의 경로 기반 VPN 액세스 제한</a></strong></div>
      <p>
            AEM Publish의 특정 경로에 대해 VPN 액세스 권한이 필요합니다.
      </p>
    </td>
</tr>
</table>
