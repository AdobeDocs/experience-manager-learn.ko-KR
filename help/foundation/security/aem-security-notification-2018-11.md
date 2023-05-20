---
title: AEM 보안 알림(2018년 11월)
seo-title: AEM Security Notification (November 2018)
description: AEM Experience Manager 보안 알림 Dispatcher
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 15%

---

# AEM 보안 알림(2018년 11월)

## 요약

이 문서에서는 최근 AEM에 보고된 몇 가지 최신 및 오래된 취약점에 대해 설명합니다. 대부분의 식별된 취약성은 AEM 제품에 대해 알려진 문제였으며, 완화는 이전에 식별되었습니다. 새로운 취약성에 대해 새 Dispatcher 버전을 사용할 수 있습니다. Adobe은 또한 고객에게 다음을 완료하도록 촉구합니다. [AEM Security 검사 목록](https://helpx.adobe.com/kr/experience-manager/6-5/sites/administering/using/security-checklist.html) 관련 지침을 따르십시오.

## 작업 필요

* AEM 배포는 최신 Dispatcher 버전을 사용하여 시작해야 합니다.
* Dispatcher 보안 규칙은 권장 구성에 따라 적용해야 합니다.
* 다음 [AEM Security 검사 목록](https://helpx.adobe.com/kr/experience-manager/6-5/sites/administering/using/security-checklist.html) 은(는) AEM 배포에 대해 완료해야 합니다.

## 취약성 및 해결 방법

| 문제 | 해결 | 링크 |
|-------|------------|-------|
| AEM Dispatcher 규칙 우회 | 최신 버전의 Dispatcher(4.3.1)를 설치하고 권장 Dispatcher 구성을 따릅니다. | 다음을 참조하십시오 [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 및 [Dispatcher 구성](https://helpx.adobe.com/kr/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| URL 필터 Dispatcher 규칙을 우회하는 데 사용할 수 있는 취약성 우회 - CVE-2016-0957 | 이 문제는 이전 버전의 Dispatcher에서 해결되었지만 이제 최신 버전의 Dispatcher(4.3.1)를 설치하고 권장 Dispatcher 구성을 따르는 것이 좋습니다. | 다음을 참조하십시오 [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) 및 [Dispatcher 구성](https://helpx.adobe.com/kr/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| 저장된 SWF 파일과 관련된 XSS 취약성 | 이 문제는 이전에 릴리스된 보안 수정 사항을 통해 해결되었습니다. | 다음을 참조하십시오. [AEM 보안 게시판 APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| 암호 관련 악용 | 보안 체크리스트의 권장 사항을 따라 보다 강력한 암호를 확인하십시오. | 다음을 참조하십시오 [AEM Security 검사 목록](https://helpx.adobe.com/kr/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| 익명 사용자의 디스크 사용 노출 | 이 문제는 AEM 6.1 이상에서 해결되었습니다. AEM 6.0의 경우 기본 권한을 보다 제한적으로 수정할 수 있습니다. | 다음을 참조하십시오 [릴리스 정보](https://helpx.adobe.com/kr/experience-manager/aem-previous-versions.html)AEM 6.1 이상용 |
| 익명 사용자에 대한 Open Social 프록시 노출 | 이 문제는 6.0 SP2 부터 버전에서 해결되었습니다. | 다음을 참조하십시오 [릴리스 정보](https://helpx.adobe.com/kr/experience-manager/aem-previous-versions.html) AEM 6.1 이상용 |
| 프로덕션 인스턴스의 CRX 탐색기 액세스 | CRX 탐색기 액세스 관리는 보안 체크리스트에서 이미 다룹니다. CRX 탐색기는 프로덕션 작성자 및 게시에서 제거해야 하며 제거하지 않으면 보안 상태 검사 보고서에서 이를 보고합니다. | 다음을 참조하십시오 [AEM Security 검사 목록](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets 가 노출됨 | 이 문제는 AEM 6.2 이후 해결되었습니다. | 다음을 참조하십시오 [AEM 6.2 릴리스 노트](https://helpx.adobe.com/kr/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Dispatcher 사용 안내서](https://helpx.adobe.com/kr/experience-manager/dispatcher/user-guide.html)
>* [AEM Dispatcher 릴리스 노트](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM 보안 게시판](https://helpx.adobe.com/security.html#experience-manager)

