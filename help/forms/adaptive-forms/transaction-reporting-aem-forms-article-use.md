---
title: AEM Forms에서 거래 보고 사용
description: AEM Forms의 트랜잭션 보고서를 사용하면 AEM Forms 배포에 지정된 날짜 이후 발생한 모든 트랜잭션 수를 유지할 수 있습니다.
feature: 적응형 양식
version: 6.4.1,6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 2%

---


# AEM Forms에서 거래 보고 사용{#using-transaction-reporting-in-aem-forms}

양식 제출 수를 캡처하기 위한 거래 보고, 문서 서비스를 사용한 문서 렌더링 및 대화형 커뮤니케이션(웹 및 인쇄 채널)이 AEM Forms 6.4.1에서 도입되었습니다. 이 기능은 주로 양식 제출 및/또는 렌더링된 문서의 수에 따라 소프트웨어 라이센스를 부여하려는 고객에게 제공됩니다. 이 기능은 현재 AEM Forms OSGI 스택에서만 사용할 수 있습니다.

## 트랜잭션 보고 활성화 {#enabling-transaction-reporting}

기본적으로 트랜잭션 기록은 비활성화되어 있습니다. 트랜잭션 기록을 사용하려면 아래 설명된 단계를 따르십시오.

* [configMgr 열기](http://localhost:4502/system/console/configMgr)
* &quot;Forms 거래 보고&quot;를 검색합니다.
* &quot;트랜잭션 기록&quot; 확인란을 선택합니다
* 변경 내용을 저장합니다

트랜잭션 보고가 활성화되면 적응형 Forms을 제출하거나, 문서 서비스를 사용하여 문서를 생성하거나, Interactive Communication 문서를 렌더링하여 트랜잭션 보고를 확인할 수 있습니다.

## 트랜잭션 보고서 보기 {#viewing-transaction-report}

트랜잭션 보고서를 보려면 AEM Forms에 관리자로 로그인하십시오. fd-Administrator 그룹의 구성원만 트랜잭션 보고서를 볼 수 있습니다.

도구 선택 | Forms | 트랜잭션 보고서 보기

또는 [여기](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)를 클릭하여 트랜잭션 보고서를 봅니다.

![트랜잭션 보고](assets/transactionreporting.gif)

Document Processed 위의 스크린샷에는 문서 서비스를 사용하여 생성된 문서의 수가 있습니다. 렌더링된 문서는 렌더링된 대화형 통신 문서(웹 및 인쇄)의 수입니다. Forms 제출됨 은 적응형 양식 제출 수입니다.

트랜잭션이 지정된 기간(플러시 버퍼 시간 + 역방향 복제 시간) 동안 버퍼에 남아 있습니다. 기본적으로 트랜잭션 수가 트랜잭션 보고서에 반영되려면 약 90초가 걸립니다.

PDF 양식 제출, 에이전트 UI를 사용하여 대화형 커뮤니케이션을 미리 보거나 비표준 양식 제출 방법 사용과 같은 작업은 트랜잭션으로 간주되지 않습니다. AEM Forms은 이러한 트랜잭션을 기록할 API를 제공합니다. 사용자 지정 구현에서 API를 호출하여 트랜잭션을 기록합니다.

작성자 인스턴스에서 트랜잭션 보고서를 보는 경우, 역복제가 모든 게시 인스턴스에 구성되어 있는지 확인하십시오.

트랜잭션 보고에 대한 자세한 내용을 보려면 [여기](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)를 클릭하십시오.

