---
title: AEM의 인증 지원 이해
description: 'AEM에서 지원하는 인증(및 가끔 인증) 메커니즘에 대한 통합 보기. '
version: 6.3, 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
translation-type: tm+mt
source-git-commit: 703321483454435e33c0aa26d66110271fc095d8
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---


# AEM 6.x에서 인증 지원 이해

AEM에서 지원하는 인증(및 가끔 인증) 메커니즘에 대한 통합 보기.

*다음 표에서는 사용자가 AEM에 인증할 수 있는 방법을 설명합니다.*

<table>
    <tbody>
        <tr>
            <td><strong>인증</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>표준 ID로 AEM 사용</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>기본 인증</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Forms 기반</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>토큰 기반(포함/ <a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">캡슐화된 토큰</a>)</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>표준 ID 공급자로 AEM이 아닌 시스템</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html" target="_blank">OAuth 1.0a 및 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-authenticationhandler/openid-authenticationhandler.html" target="_blank">OpenID</a></td>
                <td>⁕</td>
                <td>⁕</td>
                <td>*</td>
            </tr>
    </tbody>
</table>

*⁕ 커뮤니티 프로젝트를 통해 제공되지만 Adobe에서 직접 지원되지 않습니다.*
