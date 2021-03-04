---
title: AEM Forms에서 트랜잭션 보고 사용
seo-title: AEM Forms에서 트랜잭션 보고 사용
description: AEM Forms의 트랜잭션 보고서를 사용하면 AEM Forms 배포의 지정된 날짜 이후 발생한 모든 트랜잭션 수를 유지할 수 있습니다.
seo-description: AEM Forms의 트랜잭션 보고서를 사용하면 AEM Forms 배포의 지정된 날짜 이후 발생한 모든 트랜잭션 수를 유지할 수 있습니다.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: 적응형 양식
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---


# AEM Forms{#using-transaction-reporting-in-aem-forms}에서 트랜잭션 보고 사용

양식 제출 수를 캡처하기 위한 거래 보고, 문서 서비스를 사용한 문서 렌더링 및 인터랙티브한 통신(웹 및 인쇄 채널)의 렌더링이 AEM Forms 6.4.1에서 도입되었습니다. 이 기능은 주로 양식 제출 및/또는 렌더링된 문서의 수에 따라 소프트웨어 라이선스를 부여하려는 고객에게 적합합니다. 이 기능은 현재 AEM Forms OSGI 스택에서만 사용할 수 있습니다.

## 거래 보고 활성화 {#enabling-transaction-reporting}

기본적으로 트랜잭션 레코딩은 비활성화됩니다. 거래 기록을 활성화하려면 아래 단계를 따르십시오.

* [configMgr 열기](http://localhost:4502/system/console/configMgr)
* &quot;Forms 트랜잭션 보고&quot; 검색
* &quot;트랜잭션 기록&quot; 확인란을 선택합니다.
* 변경 내용 저장

거래 보고가 활성화되면 적응형 Forms을 실행하고, 문서 서비스를 사용하여 문서를 생성하거나, Interactive Communication 문서를 렌더링하여 트랜잭션 보고를 확인할 수 있습니다.

## 거래 보고서 보기 {#viewing-transaction-report}

거래 보고서를 보려면 AEM Forms에 관리자로 로그인합니다. fd-Administrator 그룹의 구성원만 트랜잭션 보고서를 볼 수 있습니다.

도구 선택 | Forms | 거래 보고서 보기

또는 [여기](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)를 클릭하여 거래 보고서를 봅니다.

![거래 보고](assets/transactionreporting.gif)

[처리된 문서] 위의 스크린샷에는 문서 서비스를 사용하여 생성된 문서의 수가 있습니다. 렌더링된 문서는 렌더링된 대화형 통신 문서(웹 및 인쇄)의 수입니다. 제출된 Forms은 적응형 양식 제출 수입니다.

트랜잭션은 지정된 기간 동안 버퍼에 남아 있습니다(플러시 버퍼 시간 + 역방향 복제 시간). 기본적으로 트랜잭션 수가 트랜잭션 보고서에 반영되는 데 약 90초가 소요됩니다.

PDF 양식 제출, 에이전트 UI를 사용하여 대화형 통신을 미리 보거나, 비표준 양식 제출 방법 사용과 같은 작업은 거래로 간주되지 않습니다. AEM Forms은 이러한 트랜잭션을 기록할 API를 제공합니다. 사용자 지정 구현에서 API를 호출하여 트랜잭션을 기록합니다.

작성 인스턴스에서 트랜잭션 보고서를 보는 경우 모든 게시 인스턴스에 역 복제가 구성되어 있는지 확인합니다.

거래 보고에 대한 자세한 내용을 보려면 [여기를 클릭하십시오](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

