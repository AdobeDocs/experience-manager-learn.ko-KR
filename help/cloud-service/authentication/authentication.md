---
title: AEM as a Cloud Service에서의 인증
description: AEM as a Cloud Service의 인증에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 3%

---

# AEM as a Cloud Service 인증

AEM as a Cloud Service은 여러 인증 옵션을 지원하며 서비스 유형에 따라 다릅니다.

|                       | AEM Author | AEM 게시 |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| · [Adobe IMS를 통한 SAML 2.0](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [SSO(Single Sign-On)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [OAuth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [토큰 인증](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |
| 기본 인증 | ✘ | ✘ |

## 인증 옵션

인증 접근 방식 설정 및 사용 방법에 대한 자세한 내용을 보려면 아래의 해당 링크를 클릭하십시오.

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
        AEM Publish 서비스의 SAML 2.0 통합을 사용하여 IDP에 웹 사이트 사용자를 인증합니다.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="토큰" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">토큰 인증</a></strong></div>
      <p>
        응용 프로그램 및 미들웨어가 API 서비스 토큰을 사용하여 AEM을 인증할 수 있도록 허용합니다.
      </p>
    </td>   
  </tr>
</table>
