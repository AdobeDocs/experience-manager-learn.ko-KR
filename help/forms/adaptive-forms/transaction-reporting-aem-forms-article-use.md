---
title: AEM Forms에서 트랜잭션 보고 사용
description: AEM Forms의 트랜잭션 보고서를 사용하면 AEM Forms 배포에서 지정된 날짜 이후 발생한 모든 트랜잭션의 수를 유지할 수 있습니다.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# AEM Forms에서 트랜잭션 보고 사용{#using-transaction-reporting-in-aem-forms}

양식 제출, 문서 서비스를 사용한 문서 렌더링, 대화형 통신(웹 및 인쇄 채널) 렌더링을 캡처하는 트랜잭션 보고가 AEM Forms 6.4.1과 함께 도입되었습니다. 이 기능은 AEM Forms OSGi 스택용 AEM Forms 6.4.1 및 AEM Forms JEE 스택용 6.5.20에서 도입되었습니다.

## 거래 보고 활성화 {#enabling-transaction-reporting}

기본적으로 트랜잭션 기록은 비활성화되어 있습니다. 거래 기록을 활성화하려면 아래 단계를 따르십시오.

* [configMgr 열기](http://localhost:4502/system/console/configMgr)
* &quot;Forms 거래 보고&quot; 검색
* &quot;트랜잭션 기록&quot; 확인란을 선택합니다.
* 변경 사항 저장

거래 보고가 활성화되면 적응형 Forms을 제출하고, 문서 서비스를 사용하여 문서를 생성하거나, 대화형 통신 문서를 렌더링하여 거래 보고가 작동하도록 할 수 있습니다.

## 트랜잭션 보고서 보기 {#viewing-transaction-report}

트랜잭션 보고서를 보려면 AEM Forms에 관리자로 로그인합니다. fd-Administrator 그룹의 멤버만 트랜잭션 보고서를 볼 수 있습니다.

도구 선택 | Forms | 거래 보고서 보기

또는 다음을 눌러 트랜잭션 보고서를 봅니다. [여기](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![트랜잭션 보고](assets/transactionreporting.gif)

위 스크린샷의 Document Processed는 문서 서비스를 사용하여 생성된 문서 수입니다. Documents rendered는 렌더링된 대화형 통신 문서(웹 및 인쇄) 수입니다. Forms Submitted는 적응형 양식 제출 횟수입니다.

트랜잭션은 지정된 기간(버퍼 초기화 시간 + 역방향 복제 시간) 동안 버퍼에 남아 있습니다. 기본적으로 거래 수가 거래 보고서에 반영되는 데 약 90초가 소요됩니다.

PDF 양식 제출, 에이전트 UI를 사용하여 대화형 통신 미리 보기 또는 비표준 양식 제출 방법과 같은 작업은 트랜잭션으로 간주되지 않습니다. AEM Forms은 이러한 트랜잭션을 기록하는 API를 제공합니다. 사용자 지정 구현에서 API를 호출하여 트랜잭션을 기록합니다.

작성자 인스턴스에서 트랜잭션 보고서를 보는 경우 모든 게시 인스턴스에 역방향 복제가 구성되어 있는지 확인합니다.

거래 보고에 대해 자세히 알아보려면 [여기를 클릭하십시오.](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
