---
title: 일상적인 사이트 유지 관리 안내서
description: 관리자, 작성자 또는 개발자이든 사이트 유지 관리는 AEM Sites 인스턴스의 모든 측면에 영향을 줍니다. 이 안내서를 사용하여 성공을 위한 전략을 수립할 수 있습니다.
role: Admin
level: Beginner, Intermediate
topic: Administration
feature: Learn From Your Peers
jira: KT-14255
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
duration: 209
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 3%

---

# 사이트 유지 관리 팁 및 요령

AEM 인스턴스의 설치 및 유지 관리에 대한 세 가지 옵션이 있습니다

* AEMaaCS(클라우드 서비스) - 시스템이 항상 최신 상태로 유지되며 필요에 따라 동적으로 확장됩니다
* Adobe 고객 서비스 엔지니어가 모든 일별/주별/월별 유지 관리를 수행하고 모든 서비스 팩이 설치되어 있고 시스템이 항상 안전하게 실행되고 있는지 확인하는 Managed Services Adobe
* 백업, 업그레이드, 보안 등 전체 시스템을 관리해야 하는 온프레미스에서 실행.

온프레미스에서 자체 시스템을 구현하기로 선택한 경우, 안전하고 성능이 좋은 시스템을 갖추기 위해 몇 가지 주의해야 할 사항이 있습니다. &quot;관리 및 공급&quot; 항목 외에도, 이 논문에서는 AEM 개발자가 시스템이 잘 작동하도록 돕기 위해 염두에 두어야 할 몇 가지 항목도 지적할 것입니다.

## 관리자

백업 - 빈번한 일정에 따라 전체 및/또는 부분 백업을 수행해야 합니다.

* 매일
* 매주
* 월별

많은 고객이 스냅샷 백업을 수행하는데, 이는 기본 운영 체제가 이러한 백업을 지원한다고 가정할 때 몇 분 밖에 걸리지 않습니다. 이러한 백업이 AEM 시스템 외부에 올바르게 저장되었는지 확인합니다. 백업이 정상적으로 작동하고 작동 중인 시스템을 주기적으로 다시 만드는 데 사용할 수 있는지 확인하십시오. 시스템 충돌이 발생한 것보다 더 나쁜 것은 없으며 어떤 이유로든 백업이 손상되어 있습니다.

문제 없는 작업을 보장하기 위해 모니터링해야 하는 몇 가지 항목이 있습니다.

### 일상적인 유지 관리

#### [인덱스 유지 관리](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=en)

인덱스를 통해 쿼리를 최대한 신속하게 실행할 수 있으므로 다른 작업에 필요한 리소스를 확보할 수 있습니다. 색인이 맨 위 셰이프인지 확인합니다. AEM은 인덱스를 사용하는 대신 트래버스하는 쿼리를 취소하여 잘못된 쿼리 하나가 전체 AEM 성능에 영향을 주지 않도록 합니다.

#### [Tar 압축/수정 정리](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

저장소에 대한 각 업데이트는 새 콘텐츠 개정을 만듭니다. 결과적으로 각 업데이트 시 저장소의 크기가 커집니다. 저장소 증가를 제어하지 않으려면 디스크 리소스를 확보하기 위해 이전 버전을 정리해야 합니다.

#### [Lucene 바이너리 정리](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

Lucene 바이너리를 제거하고 실행 중인 데이터 저장소 크기 요구 사항을 줄입니다.

#### [데이터 저장소 가비지](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

AEM의 에셋이 삭제되면 기본 데이터 저장소 레코드에 대한 참조가 노드 계층 구조에서 제거될 수 있지만 데이터 저장소 레코드 자체는 유지됩니다. 참조되지 않은 이 데이터 저장소 레코드는 보존할 필요가 없는 &quot;가비지&quot;가 됩니다. 참조되지 않은 자산이 많이 존재하는 경우 해당 자산을 제거하고 공간을 보존하며 백업 및 파일 시스템 유지 관리 성능을 최적화하는 것이 좋습니다.

#### [워크플로 제거](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html)

워크플로 인스턴스 수를 최소화하면 워크플로 엔진의 성능이 향상되므로 완료되었거나 실행 중인 워크플로 인스턴스를 저장소에서 정기적으로 제거할 수 있습니다.

#### [감사 로그 유지 관리] (https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

감사 로깅에 적합한 AEM 이벤트는 많은 보관된 데이터를 생성합니다. 이 데이터는 복제, 에셋 업로드 및 기타 시스템 활동으로 인해 시간이 지남에 따라 빠르게 증가할 수 있습니다.

#### [보안](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en)

AEM의 가장 안전한 인스턴스를 보장하기 위해 보안 체크리스트 모범 사례 를 철저히 준수해야 합니다.

#### 디스크 공간

디스크 공간을 모니터링하여 JCR 리포지토리에 충분한 공간과 약 절반의 공간을 다시 확보하십시오. tar 압축은 실행될 때 추가 공간을 사용합니다. 디스크 공간 부족은 JCR 손상의 가장 큰 원인입니다!

## 개발자

사용자 지정 구성 요소를 사용하지 마십시오. [핵심 구성 요소](https://www.aemcomponents.dev/)를 사용하십시오. 핵심 구성 요소는 시간의 80~90%, 사용자 지정 구성 요소는 제한적으로만 사용해야 합니다. 이렇게 하려면 페이지의 구성 요소를 보는 새로운 방법이 필요할 수 있습니다. CSS를 사용하는 프론트엔드 개발자가 구성 요소를 쉽게 변경할 수 있음을 알고 있어야 합니다. 또한 이러한 핵심 구성 요소는 서로 임베드되어 매우 복잡한 결과를 얻을 수 있습니다. 창의력을 발휘하십시오!

### [스타일 시스템](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

스타일 시스템을 사용하면 핵심 구성 요소 및 사용자 지정 구성 요소도 완전히 새로운 모양의 구성 요소를 만들 수 있도록 작성자 재량에 따라 핵심 구성 요소의 모양과 느낌을 변경할 수 있습니다. 이러한 스타일 변경은 일반적으로 프론트엔드 디자이너와 지식이 있는 작성자(&#39;슈퍼 작성자&#39;라고도 함)만 포함합니다

### [론치](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

론치를 사용하면 현재 배포된 페이지에 영향을 주지 않고 새로운 프로모션, 판매 또는 웹 사이트 롤아웃에 대한 작업을 완료할 수 있습니다. 또한 출석 또는 감독 없이 자동으로 라이브로 예약할 수 있으므로 작성자는 다음 주(또는 다음 분기) 작업을 오늘 수행하고 라이브로 전환되기 하루 전에 페이지 개발에 서두르지 않아도 됩니다. 이는 참으로 TIME의 선물입니다.)

### [콘텐츠 조각](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

콘텐츠 조각은 사용자 정의 가능한 &quot;청크&quot; 정보의 덩어리로, 사이트 전체에서 쉽게 재사용할 수 있습니다. 변경이 필요한 경우 원본 청크만 변경하면 업데이트가 사용되는 모든 곳에서 즉시 표시됩니다.

### [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

콘텐츠 조각과 거의 동일하게 들리는 동안 경험 조각은 페이지의 작고 보이는 조각입니다. 또한 사이트 전체에서 광범위하게 재사용하고 AEM 내의 중앙 위치에서 유지 관리하여 사이트 전체에서 일 또는 주가 아닌 몇 초 만에 잠재적으로 전역 변경 작업을 완화할 수 있습니다.

미리 생각해 보고 무엇이 재사용될지 확인하십시오. 바닥글? 면책조항이요? 머리말? 특정 유형의 콘텐츠? 이러한 모든 기능은 최소한으로 유지 관리를 유지하면서 전체 사이트에서 공유할 수 있습니다. 면책조항에서 날짜를 업데이트해야 하는데, 사이트의 1,000페이지에 있습니까? 경험 조각을 사용한 경우 5초 길이의 작업입니다.

## 일반

지속적인 학습을 통해 AEM의 변화에 당황하지 마십시오. 과거에 얽매이지 마십시오. [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) 및 [ADLS(디지털 학습 서비스) Adobe](https://learning.adobe.com/)을(를) 사용하여 기술을 연마하세요.

## 결론

AEM은 큰 시스템이 될 수 있으며, &quot;노래&quot;를 부르려면 다양한 유형의 사람들이 필요합니다. 관리자부터 개발자(프론트엔드 및 하드 코어 Java 개발자 모두)까지 - 작성자까지 모든 사용자를 위한 항목이 있습니다. 일상적인 관리를 처리하고 싶지 않다면 항상 AMS와 AEM as a Cloud Service이 있습니다.
