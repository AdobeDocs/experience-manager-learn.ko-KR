---
title: 고급 네트워킹
description: AEM as a Cloud Service의 고급 네트워킹 옵션에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: 6e7130cd98700bdb5e7f330ca0506fe89ea0eb94
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 4%

---

# 고급 네트워킹

AEM as a Cloud Service은 AEM as a Cloud Service 프로그램과의 연결을 정확하게 관리할 수 있는 고급 네트워킹 기능을 제공합니다.

|  | [제작 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------|
| 고급 네트워킹 지원 | ✔ | ✘ |


AEM 고급 네트워킹은 외부 서비스와의 연결을 관리하는 세 가지 옵션으로 구성됩니다. Cloud Manager 프로그램 및 AEM as a Cloud Service 환경에서는 한 번에 하나의 고급 네트워킹 구성만 사용할 수 있으므로 가장 적합한 유형을 선택했는지 확인하십시오.

|  | 표준 포트의 HTTP/HTTPS | 비표준 포트의 HTTP/HTTPS | 비HTTP/HTTPS 연결 | 전용 송신 IP | &quot;프록시가 없는 호스트&quot; 목록 | VPN 보호 서비스에 연결 | IP별 AEM 게시 트래픽 제한 |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __고급 네트워킹 없음__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__유연한 포트 송신__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__전용 송신 IP 주소__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__가상 사설 네트워크__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


적절한 고급 네트워킹 유형을 선택할 때 고려사항에 대한 자세한 내용은 다음을 참조하십시오 [고급 네트워킹 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## 고급 네트워킹 자습서

조직의 요구 사항에 따라 가장 적합한 고급 네트워킹 옵션을 식별했으면 아래 해당 자습서를 클릭하여 단계별 지침 및 코드 샘플을 확인하십시오.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="유연한 포트 송신" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">유연한 포트 송신</a></strong></div>
      <p>
          비표준 포트에서 아웃바운드 AEM as a Cloud Service 트래픽을 허용합니다.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="전용 송신 IP 주소" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">전용 송신 IP 주소</a></strong></div>
      <p>
        전용 IP에서 아웃바운드 AEM as a Cloud Service 트래픽을 가져옵니다.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Virtual Private Network(VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Virtual Private Network(VPN)</a></strong></div>
      <p>
        고객 또는 공급업체 인프라와 AEM as a Cloud Service 간의 트래픽을 보호합니다.
      </p>
    </td>   
  </tr>
</table>
