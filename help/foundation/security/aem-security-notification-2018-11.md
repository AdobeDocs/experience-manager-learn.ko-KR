---
title: AEM 보안 알림(2018년 11월)
seo-title: AEM 보안 알림(2018년 11월)
description: AEM Experience Manager 보안 알림 디스패처
seo-description: AEM Experience Manager 보안 알림 디스패처
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: 보안
role: 건축가
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 9%

---


# AEM 보안 알림(2018년 11월)

## 요약

이 문서에서는 AEM에서 최근 보고된 몇 가지 최신 취약점을 해결합니다. 가장 식별된 취약점이 AEM 제품에 대해 알려진 문제이며 이전에 식별된 문제를 발견했으며 새 디스패처 버전을 새 취약점에 사용할 수 있습니다. 또한 Adobe은 고객이 [AEM 보안 검사 목록](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)을 완료하고 관련 지침을 따르도록 권장합니다.

## 작업 필요

* AEM 배포은 최신 Dispatcher 버전을 사용하여 시작해야 합니다.
* 권장 구성에 따라 디스패처 보안 규칙을 적용해야 합니다.
* AEM 배포에 대해 [AEM 보안 검사 목록](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html)을 완료해야 합니다.

## 취약점 및 해결 방법

| 문제 | 해상도 | 링크 |
|-------|------------|-------|
| AEM Dispatcher 규칙 무시 | 최신 버전의 Dispatcher(4.3.1)을 설치하고 권장 Dispatcher 구성을 따릅니다. | [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 및 [Dispatcher 구성](https://helpx.adobe.com/kr/experience-manager/dispatcher/using/dispatcher-configuration.html을 참조하십시오.)을 참조하십시오. |
| 발송자 규칙을 우회하는 데 사용할 수 있는 URL 필터 우회 취약점 - CVE-2016-0957 | 이 문제는 이전 버전의 Dispatcher에서 해결되었지만 이제 최신 버전의 Dispatcher(4.3.1)을 설치하고 권장 Dispatcher 구성을 따르는 것이 좋습니다. | [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 및 [Dispatcher 구성](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)을 참조하십시오. |
| 저장된 SWF 파일과 관련된 XSS 취약성 | 이 문제는 앞서 릴리스된 보안 수정으로 해결되었습니다. | [AEM 보안 게시판 APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html)을(를) 참조하십시오. |
| 암호 관련 악용 | 보안 체크리스트의 권장 사항을 따라 더 강력한 암호를 확인합니다. | [AEM 보안 검사 목록](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) 참조 |
| 익명의 사용자를 위한 디스크 사용량 노출 | AEM 6.1 이상 버전에서 이 문제가 해결되었습니다. AEM 6.0의 경우 즉시 사용 권한을 수정하여 보다 제한적인 것으로 만들 수 있습니다. | AEM 6.1 이하 버전의 경우 [릴리스 노트](https://helpx.adobe.com/kr/experience-manager/aem-previous-versions.html)를 참조하십시오. |
| 익명 사용자를 위한 Open Social 프록시 노출 | 이 문제는 6.0 SP2부터 시작되는 버전에서 해결되었습니다. | AEM 6.1 이하 버전의 경우 [릴리스 노트](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)를 참조하십시오. |
| 프로덕션 인스턴스에서 CRX 탐색기 액세스 | CRX 탐색기 액세스 관리는 이미 보안 체크리스트에서 다룹니다. CRX 탐색기는 프로덕션 작성자에서 제거되어야 하며 제거되지 않으면 보안 상태 확인 보고서도 게시해야 합니다. | [AEM 보안 검사 목록](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)을 참조하십시오. |
| BGServlets가 노출됨 | AEM 6.2 이후 해결되었습니다. | [AEM 6.2 릴리스 노트](https://helpx.adobe.com/kr/experience-manager/6-2/release-notes.html)를 참조하십시오. |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher 사용자 안내서](https://helpx.adobe.com/kr/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM 보안 게시판](https://helpx.adobe.com/security.html#experience-manager)

