---
title: 사이트 유지 관리 팁과 트릭
description: 사이트 유지 관리 팁과 트릭
hide: true
hidefromtoc: true
source-git-commit: 3eb429039589ae26a81bc6d24f020a77517133e8
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 5%

---


# 사이트 유지 관리 팁과 트릭

AEM 인스턴스를 설치하고 유지 관리하는 데에는 세 가지 옵션이 있습니다

* AEMaaCS(클라우드 서비스) - 시스템이 항상 켜져 있으며 최신 상태로 유지되며 필요에 따라 동적으로 크기가 조절됩니다
* 고객 서비스 엔지니어 Adobe이 매일/매주/매월 모든 유지 관리를 수행하고 모든 서비스 팩이 설치되고 시스템이 항상 안전하며 원활하게 실행되는 Adobe Managed Services입니다
* 온-프레미스에서 실행 - 백업, 업그레이드, 보안 등 전체 시스템을 관리해야 합니다.

온프레미스에서 시스템을 구현하도록 선택하는 경우 몇 가지 보안 성능 시스템을 사용할 수 있도록 주의해야 할 사항이 있습니다. 이 백서에서는 &quot;관리 및 공급&quot; 항목 외에도 AEM 개발자들이 시스템을 제대로 작동시키기 위해 주의해야 하는 몇 가지 사항을 설명합니다.

## 관리자

백업 - 자주 스케줄 지정 시 전체 백업 및/또는 부분 백업 완료:

* 매일
* 매주
* 월별

많은 고객이 스냅샷 백업을 수행합니다. 이 작업은 기본 운영 체제가 이러한 백업을 지원한다고 가정할 경우 몇 분 정도 걸립니다. 이러한 백업이 제대로 저장되었는지 확인합니다(AEM 시스템에서 꺼짐). 백업이 제대로 작동하는지 확인하고 작업 시스템을 주기적으로 다시 만드는 데 사용할 수 있습니다. 시스템 충돌이 발생하고 어떤 이유로 백업이 손상되는 것보다 더 나쁜 것은 없습니다.

문제 없는 작업을 위해 모니터링해야 하는 몇 가지 항목이 있습니다.

### 일상적인 유지 관리

#### [색인 유지 관리](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=ko)

인덱스를 통해 쿼리를 가능한 한 빨리 실행하여 다른 작업에 대한 리소스를 확보할 수 있습니다. 색인이 끝단 모양인지 확인하십시오! AEM에서 인덱스를 사용하여 잘못된 쿼리 하나가 전체 AEM 성능에 영향을 주지 않도록 하는 쿼리를 취소합니다.

#### [Tar 압축/개정 정리](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

저장소에 대한 각 업데이트에서는 새 컨텐츠 개정을 만듭니다. 따라서 각 업데이트 시 저장소의 크기가 증가합니다. 저장소 증가를 제어하지 않으려면 사용 가능한 디스크 리소스를 위해 이전 버전을 정리해야 합니다.

#### [Lucene 이진 파일 정리](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-dashboard.html?lang=en#automated-maintenance-tasks)

lucene 바이너리를 제거하고 실행 중인 데이터 저장소 크기 요구 사항을 줄입니다.

#### [데이터 저장소 가비지](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/data-store-garbage-collection.html?lang=en)

AEM의 자산이 삭제되면 기본 데이터 저장소 레코드에 대한 참조가 노드 계층에서 제거될 수 있지만 데이터 저장소 레코드 자체는 유지됩니다. 이 참조되지 않은 데이터 저장소 레코드는 보존할 필요가 없는 &quot;가비지&quot;가 됩니다. 참조되지 않은 자산이 많은 경우 자산을 제거하여 공간을 유지하고 백업 및 파일 시스템 유지 관리 성능을 최적화하는 것이 좋습니다.

#### [워크플로 삭제](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/workflows-administering.html?lang=en)

워크플로 인스턴스 수를 최소화하면 워크플로 엔진의 성능이 향상되므로 완료되었거나 실행 중인 워크플로 인스턴스를 저장소에서 정기적으로 제거할 수 있습니다.

#### [감사 로그 유지 관리](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-audit-log.html?lang=en)

감사 로깅에 적합한 AEM 이벤트는 아카이브된 데이터를 많이 생성합니다. 복제, 자산 업로드 및 기타 시스템 작업으로 인해 시간이 지남에 따라 이러한 데이터가 빠르게 증가할 수 있습니다.

#### [보안](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en)

Security Checklist Best Practice가 잘 준수되도록 AEM의 가장 안전한 인스턴스를 보장합니다.

#### 디스크 공간

디스크 공간을 모니터링하여 JCR 리포지토리를 위한 충분한 공간이 확보되었는지 확인하고 절반 정도의 공간을 확보할 수 있습니다. tar 압축은 실행 시 추가 공간을 사용합니다. 디스크 공간이 부족하면 JCR이 손상된 가장 큰 이유가 바로 JCR입니다.

## 개발자

사용자 지정 구성 요소를 사용하지 마십시오. [핵심 구성 요소](https://www.aemcomponents.dev/). 시간의 80~90%에 해당하는 핵심 구성 요소와 사용자 지정 구성 요소를 드물게 사용하는 것이 목표입니다. 이렇게 하려면 페이지에서 구성 요소를 보는 새로운 방법이 필요한 경우가 많습니다. CSS를 사용하는 프런트 엔드 개발자가 구성 요소를 쉽게 복원할 수 있음을 깨달아야 합니다. 또한 이러한 핵심 구성 요소를 서로 임베드하여 매우 복잡한 결과를 얻을 수 있음을 염두에 두십시오. 창의력을 발휘하세요!

### [스타일 시스템](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

스타일 시스템을 사용하여 코어 구성 요소 및 사용자 지정 구성 요소도 작성자의 재량에 따라 변경되고 완전히 새로운 디자인 구성 요소를 만들 수 있습니다. 이러한 스타일 변경 사항은 일반적으로 프런트 엔드 디자이너와 전문 작성자(종종 &#39;슈퍼 작성자&#39;라고도 함)만 포함합니다

### [론치](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

론치를 사용하면 현재 배포된 페이지에 영향을 주지 않고 새 홍보, 판매 또는 웹 사이트 롤아웃에 대한 작업을 완료할 수 있습니다. 또한 참석이나 감독 없이 자동으로 라이브로 전환되도록 예약할 수 있으므로 작성자는 다음 주(또는 다음 분기) 작업을 오늘 수행할 수 있으며 페이지 개발에 뛰어들지 않고 실제로 적용할 수 있는 TIME의 선물입니다.

### [콘텐츠 조각](https://experienceleague.adobe.com/docs/experience-manager-64/assets/fragments/content-fragments.html?lang=en)

컨텐츠 조각은 사이트 전체에서 쉽게 재사용할 수 있는 사용자 정의 가능한 &quot;청크&quot; 정보입니다. 변경이 필요한 경우 원래 청크를 변경하고 업데이트가 사용되는 모든 곳에서 즉시 표시됩니다.

### [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

컨텐츠 조각과 거의 동일한 사운딩을 수행하는 동안 경험 조각은 페이지의 작고, 표시되며, 또한 사이트 전체에서 이러한 구성 요소를 광범위하게 재사용하고 AEM 내의 중앙 위치에서 유지 관리하여 며칠 또는 몇 주가 아닌 몇 초 내에 사이트 전체에서 잠재적으로 전역 변경 작업을 손쉽게 수행할 수 있습니다.

미리 생각해 보고 재사용할 수 있는 것을 확인해 보십시오. 바닥글이요? 면책조항? 헤더요? 특정 유형의 컨텐츠? 이러한 모든 구성 요소는 최소한으로 유지 관리하면서 전체 사이트에서 공유할 수 있습니다. 면책조항에 있는 날짜를 업데이트해야 하지만, 사이트의 1,000페이지에 게시되어 있습니까? 경험 조각을 사용한 경우 5초 작업입니다.

## 일반

계속해서 배우면서 AEM의 변화를 따라갈 수 있습니다. 과거에 익숙해지지 마십시오. 사용 [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) 및 [ADLS(디지털 학습 서비스) Adobe](https://learning.adobe.com/) 기술을 연마하기 위해

## 결론

AEM은 큰 시스템이 될 수 있고, 많은 종류의 사람들이 그것을 &quot;노래&quot;로 만들 수 있습니다. 관리자부터 개발자(프런트 엔드 및 하드 Java 개발자 모두)부터 작성자에 이르기까지 모든 사용자를 위한 다양한 기능이 있습니다. 그리고 여러분이 일상적인 관리를 하고 싶지 않다면, 항상 AMS와 AEM이 as a Cloud Service으로 있습니다.
