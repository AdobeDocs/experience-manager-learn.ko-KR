---
title: AEM as a Cloud Service 인증
description: AEM as a Cloud Service의 인증에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.jpeg
source-git-commit: e666e38d6b2a7057f7016b35ad1034a4487e9bc7
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 4%

---


# AEM as a Cloud Service 인증

AEM as a Cloud Service은 다양한 인증 옵션을 지원하며 서비스 유형에 따라 달라집니다.

|  | AEM Author | AEM 게시 |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [토큰 인증](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## 인증 옵션

인증 접근 방식을 설정하고 사용하는 방법에 대한 자세한 내용을 보려면 아래 해당 링크를 클릭하십시오.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Adobe Admin Console을 통해 Adobe IMS를 사용하여 AEM 작성자 액세스를 관리합니다.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        AEM 게시 서비스의 SAML 2.0 통합을 사용하여 웹 사이트의 사용자를 IDP에 인증합니다.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="토큰" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">토큰 인증</a></strong></div>
      <p>
        API 서비스 토큰을 사용하여 응용 프로그램 및 미들웨어가 AEM에 인증되도록 허용합니다.
      </p>
    </td>   
  </tr>
</table>