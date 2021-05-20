---
title: AEM 보안 알림(2018년 11월)
seo-title: AEM 보안 알림(2018년 11월)
description: AEM Experience Manager 보안 알림 Dispatcher
seo-description: AEM Experience Manager 보안 알림 Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: 보안
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 9%

---


# AEM 보안 알림(2018년 11월)

## 요약

이 문서에서는 최근 AEM에서 보고된 몇 가지 최신 및 이전 취약점을 해결합니다. 가장 식별된 취약점이 AEM 제품에 대한 알려진 문제 및 완화가 이전에 식별되었으며, 새로운 취약점에 대한 새 Dispatcher 버전을 사용할 수 있습니다. 또한 Adobe은 고객이 [AEM Security Checklist](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)를 완료하고 관련 지침을 따르도록 권장합니다.

## 작업 필요

* AEM 배포은 최신 Dispatcher 버전을 사용하여 시작해야 합니다.
* Dispatcher 보안 규칙은 권장 구성에 따라 적용되어야 합니다.
* AEM 배포에 대해 [AEM Security 검사 목록](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)을 완료해야 합니다.

## 취약점 및 해결 방법

| 문제 | 해상도 | 링크 |
|-------|------------|-------|
| AEM Dispatcher 규칙 무시 | 최신 버전의 Dispatcher(4.3.1)를 설치하고 권장 Dispatcher 구성을 따릅니다. | [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 및 [Dispatcher 구성](https://helpx.adobe.com/kr/experience-manager/dispatcher/using/dispatcher-configuration.html을 참조하십시오.)을 참조하십시오. |
| 디스패처 규칙을 우회하는 데 사용할 수 있는 URL 필터 바이패스 취약성 - CVE-2016-0957 | 이 문제는 이전 버전의 Dispatcher에서 해결되었지만 이제 최신 버전의 Dispatcher(4.3.1)를 설치하고 권장 Dispatcher 구성을 따르는 것이 좋습니다. | [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 및 [Dispatcher 구성](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)을 참조하십시오. |
| 저장된 SWF 파일과 관련된 XSS 취약성 | 이 문제는 앞서 릴리스된 보안 수정 사항으로 해결되었습니다. | [AEM 보안 게시판 APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)을 참조하십시오. |
| 암호 관련 사용 | 보안 확인 목록의 권장 사항을 따라 더 강력한 암호를 확인합니다. | [AEM Security 검사 목록](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) 참조 |
| 익명 사용자에 대한 디스크 사용량 노출 | 이 문제는 AEM 6.1 이상에서 해결되었습니다. AEM 6.0의 경우 기본 권한을 수정하여 더 제한적인 권한을 만들 수 있습니다. | AEM 6.1 이상용 [릴리스 노트](https://helpx.adobe.com/kr/experience-manager/aem-previous-versions.html)를 참조하십시오. |
| 익명 사용자에 대한 Open Social 프록시 노출 | 이 문제는 6.0 SP2부터 시작되는 버전에서 해결되었습니다. | AEM 6.1 이상의 경우 [릴리스 노트](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)를 참조하십시오. |
| 프로덕션 인스턴스의 CRX 탐색기 액세스 | CRX Explorer 액세스 관리는 보안 체크리스트에서 이미 다룹니다. 제거되지 않은 경우 CRX Explorer를 프로덕션 작성 및 게시에서 제거하고 보안 상태 확인 보고서에서 제거해야 합니다. | [AEM Security 검사 목록](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)을 참조하십시오. |
| BGServlet이 노출됨 | 이 문제는 AEM 6.2 이후 해결되었습니다. | [AEM 6.2 릴리스 노트](https://helpx.adobe.com/kr/experience-manager/6-2/release-notes.html) 를 참조하십시오. |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher 사용 안내서](https://helpx.adobe.com/kr/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM 보안 게시판](https://helpx.adobe.com/security.html#experience-manager)

