---
title: AEM as a Cloud Service 컨텐츠 마이그레이션 FAQ
description: AEM as a Cloud Service으로 컨텐츠 마이그레이션에 대한 FAQ 답변을 얻습니다.
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
source-git-commit: b2656329270ac90458dbc25bb05f39bf76921f26
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 0%

---


# AEM as a Cloud Service 컨텐츠 마이그레이션 FAQ

AEM as a Cloud Service으로 컨텐츠 마이그레이션에 대한 FAQ 답변을 얻습니다.

## 용어

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**: [모범 사례 분석기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [컨텐츠 전송 도구](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **캠**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Identity Management 시스템](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

CTT 관련 Adobe 지원 티켓을 만드는 동안 자세한 내용을 보려면 아래 템플릿을 사용하십시오.

![콘텐츠 마이그레이션 Adobe 지원 티켓 템플릿](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## 일반 컨텐츠 마이그레이션 질문

### Q: 컨텐츠를 Cloud Services으로 AEM에 마이그레이션하는 다양한 방법은 무엇입니까?

사용할 수 있는 방법에는 세 가지가 있습니다

+ 컨텐츠 전송 도구 사용(AEM 6.3+ → AEMaaCS)
+ 패키지 관리자 사용(AEM → AEMaaCS)
+ 즉시 사용 가능한 자산에 대한 벌크 가져오기 서비스(S3/Azure → AEMaaCS)

### Q: CTT를 사용하여 전송할 수 있는 컨텐츠 양에 제한이 있습니까?

아니요. CTT 는 AEM 소스에서 추출하여 AEMaaCS로 수집할 수 있습니다. 그러나 AEMaaCS 플랫폼은 마이그레이션 전에 고려해야 하는 특정한 제한이 있습니다.

자세한 내용은 [클라우드 마이그레이션 사전 요구 사항](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### Q: 소스 시스템에서 최신 BPA 보고서가 있습니다. 어떻게 해야 합니까?

보고서를 CSV로 내보낸 다음 Cloud Acceleration Manager에 업로드합니다. [IMS 조직과 연결](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html). 그런 다음 검토 프로세스를 진행합니다. [준비 단계에 요약됨](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

도구에서 제공하는 코드 및 컨텐츠 복잡성 평가를 검토하고 코드 리팩터링 백로그 또는 클라우드 마이그레이션 평가를 시작하는 관련 작업 항목을 기록해 두십시오.

### Q: 소스 작성자에 대해 추출하고 AEMaaCS 작성자 및 게시에 수집하는 것이 좋습니다.

항상 작성자와 게시 계층 간에 1:1 추출 및 수집을 수행하는 것이 좋습니다. 즉, 소스 프로덕션 작성자를 추출하고 개발, 스테이지 및 프로덕션 CS에 수집하는 것은 허용됩니다.

### Q: CTT를 사용하여 소스 AEM에서 AEMaaCS로 컨텐츠를 마이그레이션하는 데 걸리는 시간을 예상할 수 있습니까?

마이그레이션 프로세스는 인터넷 대역 폭, CTT 프로세스에 할당된 heap, 사용 가능한 여유 메모리 및 각 소스 시스템에 대해 주관적인 디스크 IO에 따라 다르므로 마이그레이션 증명 을 일찍 실행하고 데이터 포인트를 추정에 따라 추정을 수행하는 것이 좋습니다.

### Q: CTT 추출 프로세스를 시작할 때 소스 AEM 성능에 어떤 영향을 줍니까?

CTT 도구는 OSGi 구성을 통해 구성할 수 있는 최대 4gb 힙이 소요되는 자체 Java™ 프로세스에서 실행됩니다. 이 번호는 변경될 수 있지만 Java™ 프로세스에 대해 감사하고 확인할 수 있습니다.

AZCopy가 설치되어 있고 Pre Copy 옵션/유효성 검사 기능이 활성화된 경우 AZCopy 프로세스는 CPU 주기를 사용합니다.

jvm 외에도 이 도구는 디스크 IO를 사용하여 과도 임시 공간에 데이터를 저장하며 추출 주기 후에 정리됩니다. CTT 도구는 RAM, CPU 및 디스크 IO와 별도로 소스 시스템의 네트워크 대역 너비를 사용하여 데이터를 Azure Blob Store에 업로드합니다.

CTT 추출 프로세스에서 사용하는 리소스의 양은 노드 수, BLOB 수 및 집계된 크기에 따라 다릅니다. 수식을 제공하기가 어려우므로 소규모 마이그레이션 증명을 실행하여 소스 서버 업크기 요구 사항을 결정하는 것이 좋습니다.

클론 환경이 마이그레이션에 사용되면 라이브 프로덕션 서버 리소스 활용에는 영향을 주지 않지만 라이브 프로덕션과 클론 간의 컨텐츠 동기화에 대한 자체 다운사이드가 제공됩니다

### Q: 소스 작성자 시스템에서 사용자가 작성자 인스턴스에 인증하도록 SSO가 구성되었습니다. 이 경우 CTT의 사용자 매핑 기능을 사용해야 합니까?

답은 &quot; 입니다.**예**&quot;.

CTT 추출 및 섭취 **사용 안 함** 사용자 매핑은 소스 AEM에서 AEMaaCS로 콘텐츠, 관련 원칙(사용자, 그룹)만 마이그레이션합니다. 그러나 Adobe IMS에 있는 이러한 사용자(ID)에 대한 요구 사항이 있으며 AEMaaCS 인스턴스에 대한 액세스 권한이 있어야 성공적으로 인증됩니다. 작업 [사용자 매핑 도구](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 는 인증과 인증이 함께 작동하도록 로컬 AEM 사용자를 IMS 사용자에게 매핑합니다.

이 경우 SAML ID 공급자는 인증 핸들러를 사용하여 AEM에 직접 사용되지 않고 Federated / Enterprise ID을 사용하도록 Adobe IMS에 대해 구성됩니다.

### Q: 소스 작성자 시스템에서는 사용자가 로컬 AEM 사용자로 작성자 인스턴스에 인증하도록 기본 인증이 구성되어 있습니다. 이 경우 CTT의 사용자 매핑 기능을 사용해야 합니까?

답은 &quot; 입니다.**예**&quot;.

사용자 매핑 없이 CTT 추출 및 섭취는 컨텐츠를 소스 AEM에서 AEMaaCS로 마이그레이션하는 데 도움이 됩니다. 관련 원칙(사용자, 그룹)은 소스에서 AEMaaCS로 마이그레이션됩니다. 그러나 Adobe IMS에 있는 이러한 사용자(ID)에 대한 요구 사항이 있으며 AEMaaCS 인스턴스에 대한 액세스 권한이 있어야 성공적으로 인증됩니다. 작업 [사용자 매핑 도구](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 는 인증과 인증이 함께 작동하도록 로컬 AEM 사용자를 IMS 사용자에게 매핑합니다.

이 경우 사용자는 개인 Adobe ID을 사용하고 IMS 관리자는 Adobe ID을 사용하여 AEMaaCS에 액세스할 수 있습니다.

### Q: CTT 컨텍스트에서 &quot;지우기&quot;와 &quot;덮어쓰기&quot;라는 용어는 무엇을 의미합니까?

의 컨텍스트에서 [추출 단계](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html)를 지정하는 옵션은 이전 추출 주기에 있는 스테이징 컨테이너의 데이터를 덮어쓰거나 차등(추가/업데이트/삭제)을 스테이징 컨테이너에 추가하는 것입니다. 스테이징 컨테이너는 아무것도 아니지만 마이그레이션 세트와 연결된 Blob 저장소 컨테이너입니다. 각 마이그레이션 세트는 자체 스테이징 컨테이너를 가져옵니다.

의 컨텍스트에서 [수집 단계](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html)를 지정하는 옵션은 + 입니다. AEMaaCS의 전체 컨텐츠 저장소를 바꾸거나 스테이징 마이그레이션 컨테이너에서 차등(추가/업데이트/삭제) 컨텐츠를 동기화하는 것입니다.

### Q: 소스 시스템에는 여러 웹 사이트, 관련 자산, 사용자, 그룹이 있습니다. AEMaaCS로 단계적으로 마이그레이션할 수 있습니까?

예, 가능하지만 다음 사항에 대해서는 신중한 계획이 필요합니다.

+ 사이트를 가정할 마이그레이션 세트를 만들면 자산이 각 계층 구조로 표시됩니다
   + 모든 자산을 하나의 마이그레이션 세트의 일부로 마이그레이션한 다음 단계적으로 사용 중인 사이트를 가져올 수 있는지 확인합니다
+ 현재 상태에서는 게시 계층이 컨텐츠를 제공할 수 있어도 작성 수집 프로세스를 통해 작성 인스턴스를 컨텐츠 작성에 사용할 수 없게 됩니다
   + 즉, 수집이 작성자로 완료될 때까지 컨텐츠 작성 활동이 동결됩니다

마이그레이션을 계획하기 전에 설명된 대로 추출 및 수집 추가 프로세스를 검토하십시오.

### Q: AEMaaCS 작성자 또는 게시 인스턴스에서 발생하는 수집이라도 최종 사용자가 내 웹 사이트를 사용할 수 있습니까?

예. 최종 사용자 트래픽은 컨텐츠 마이그레이션 활동으로 중단되지 않습니다. 그러나 작성 수집은 컨텐츠 작성이 완료될 때까지 컨텐츠 작성을 중지합니다.

### Q: BPA 보고서에는 누락된 원래 표현물과 관련된 항목이 표시됩니다. 추출하기 전에 소스에 정리해야 합니까?

예. 누락된 원래 표현물은 자산 바이너리가 처음에 제대로 업로드되지 않음을 의미합니다. 잘못된 데이터로 간주하려면 패키지 관리자를 사용하여 백업하고(필요에 따라) 추출을 실행하기 전에 소스 AEM에서 제거하십시오. 잘못된 데이터는 자산 처리 단계에서 음수 결과를 가져옵니다.

### Q: BPA 보고서에 누락과 관련된 항목이 있습니다 `jcr:content` 노드 아래에 나열됩니다. 어떻게 해야 하죠?

When `jcr:content` 이 폴더 수준에서 누락되어 처리 프로필 등과 같은 설정을 전파하는 작업이 없습니다. 부모로부터 헤어지는 것은 이 정도입니다. 누락된 이유를 검토하십시오 `jcr:content`. 이러한 폴더를 마이그레이션할 수 있더라도 이러한 폴더는 사용자 경험을 저하시키고 나중에 불필요한 문제 해결 주기를 초래할 수 있습니다.

### Q: 마이그레이션 세트를 만들었습니다. 사이즈를 확인할 수 있나요?

예, [크기 확인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) ctt에 포함된 기능입니다.

### Q: 마이그레이션(추출, 수집)을 수행하고 있습니다. 추출된 모든 콘텐츠가 target에 수집되는지 확인할 수 있습니까?

예, [유효성 검사](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) ctt에 포함된 기능입니다.

### Q: 고객은 AEMaaCS Dev에서 AEMaaCS Stage나 AEMaaCS Prod로 등의 AEMaaCS 환경 간에 컨텐츠를 이동해야 합니다. 이러한 사용 사례에 컨텐츠 전송 도구를 사용할 수 있습니까?

안타깝지만 아니요 CTT의 사용 사례는 온-프레미스/AMS에서 호스팅하는 AEM 6.3 이상에서 AEMaaCS 클라우드 환경으로 컨텐츠를 마이그레이션하는 것입니다. [CTT 설명서를 참조하십시오](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### Q: 추출 중에 어떤 문제가 예상됩니까?

추출 단계는 여러 측면이 필요한 관련 프로세스입니다. 발생할 수 있는 다양한 유형의 문제를 파악하고 이를 완화하는 방법을 통해 컨텐츠 마이그레이션의 전반적인 성공을 높일 수 있습니다.

공개 설명서는 지식을 바탕으로 지속적으로 개선되지만, 몇 가지 높은 수준의 문제 카테고리와 가능한 기본 이유가 있습니다.

![AEM as a Cloud Service 컨텐츠 마이그레이션 추출 문제](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### Q: 섭취 중에 어떤 문제가 예상됩니까?

수집 단계는 클라우드 플랫폼에서 완전히 수행되며 AEMaaCS 인프라에 액세스할 수 있는 리소스의 도움이 필요합니다. 자세한 도움말을 보려면 지원 티켓을 만드십시오.

가능한 문제 범주는 다음과 같습니다(이것을 제외 목록으로 간주하지 마십시오)

![AEM as a Cloud Service 컨텐츠 마이그레이션 수집 문제](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### Q: CTT가 작동하려면 소스 서버에 아웃바운드 인터넷 연결이 있어야 합니까?

답은 &quot; 입니다.**예**&quot;.

CTT 프로세스는 아래 리소스에 연결해야 합니다.

+ 대상 AEM as a Cloud Service 환경: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Azure Blob 저장소 서비스: `casstorageprod.blob.core.windows.net`
+ 사용자 매핑 IO 끝점: `usermanagement.adobe.io`

자세한 내용은 설명서 를 참조하십시오 [소스 연결](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## 자산 처리 Dynamic Media 관련 질문

### Q: AEMaaCS에서 수집하면 자산이 자동으로 다시 처리됩니까?

아니요. 자산을 처리하려면 재처리 요청이 시작되어야 합니다.

### Q: AEMaaCS에서 수집하면 자산이 자동으로 다시 색인화됩니까?

예. 자산은 AEMaaCS에서 사용할 수 있는 색인 정의를 기반으로 다시 색인화됩니다.

### Q: 소스 AEM은 Dynamic Media과 통합됩니다. 컨텐츠 마이그레이션 전에 고려해야 하는 특별한 사항이 있습니까?

예. 소스 AEM에 Dynamic Media 통합이 있는 경우 다음 사항을 고려하십시오.

+ AemAaCS는 Dynamic Media Scene7 모드만 지원합니다. 소스 시스템이 하이브리드 모드에 있는 경우 Scene7 모드로 DM 마이그레이션이 필요합니다.
+ 소스 클론 인스턴스에서 마이그레이션하는 방법이 있는 경우 CTT에 사용할 클론에 대해 DM 통합을 비활성화하는 것이 좋습니다. 이 단계는 DM에 대한 쓰기를 방지하거나 DM 트래픽에 대한 로드를 방지하기 위한 것입니다.
+ CTT는 소스 AEM에서 AEMaaCS로 마이그레이션 세트의 노드, 메타데이터를 마이그레이션합니다. DM에서 직접 작업을 수행하지 않습니다.

### Q: Source AEM에 DM 통합이 있을 때 서로 다른 마이그레이션 접근 방식은 무엇입니까?

위의 질문을 읽고 먼저 대답하세요

(두 가지 가능한 옵션이지만 이 두 가지 옵션에만 제한되지 않습니다.) 고객이 UAT, 성능 테스트, 사용 가능한 환경 및 클론을 마이그레이션에 사용하는지 여부에 따라 달라집니다. 이 두 가지를 토론의 시작점으로 고려하십시오

**옵션 1**

소스 환경의 자산/노드 수가 하단(~100K)에 있는 경우, 추출 및 수집을 포함하여 24 + 72시간 동안 마이그레이션할 수 있다고 가정할 경우, 더 나은 접근 방법은 다음과 같습니다

+ 프로덕션에서 직접 마이그레이션 수행
+ 을 사용하여 AEMaaCS에 초기 추출 및 섭취 실행 `wipe=true`
   + 이 단계에서는 모든 노드 및 바이너리를 마이그레이션합니다
+ 온-프레미스/AMS Prod 작성자에서 계속 작업
+ 지금부터 다음을 통해 다른 모든 마이그레이션 주기 증명을 실행합니다. `wipe=true`
   + 이 작업은 전체 노드 저장소를 마이그레이션하지만 전체 Blob과 반대되는 수정된 Blob만 마이그레이션합니다. 이전 blob 집합은 대상 AEMaaCS 인스턴스의 Azure blob 저장소에 있습니다.
   + 마이그레이션 기간, 사용자 매핑, 테스트, 기타 모든 기능의 유효성 검사를 측정하는 데 이 마이그레이션 증명을 사용합니다
+ 마지막으로, go-live가 진행되는 한 주 전에 Wipe=true 마이그레이션을 수행합니다
   + AEMaaCS에서 Dynamic Media 연결
   + AEM 온-프레미스 소스에서 DM 구성 연결 끊기

이 옵션을 사용하여 마이그레이션을 하나씩 실행할 수 있습니다. 즉, On-Prem Dev → AEMaaCS Dev 등이 있습니다. 각 환경에서 DM 구성 이동

클론에서 마이그레이션을 수행할 예정인 경우

**옵션 2**

+ 운영 작성자의 클론 생성, 클론에서 DM 구성 제거
+ AemAaCS Dev/Stage→ 온-프레미스 복제 마이그레이션
   + 유효성 검사를 위해 프로덕션 DM 회사를 AEMaaCS 개발/스테이지에 간단히 연결합니다
   + DM 연결이 활성화되는 동안 AEMaaCS에 자산을 수집하지 마십시오
   + 이를 통해 CTT, DM 특정 유효성 검사의 유효성을 검사할 수 있습니다
+ AEMaaCS에서 테스트가 완료되면
   + 온-프레미스 스테이지에서 AEMaaCS 스테이지로 지우기 마이그레이션 실행

온-프레미스 Dev에서 AEMaaCS Dev로 지우기 마이그레이션을 실행합니다.

위의 접근 방식은 마이그레이션 기간을 측정하는 데 사용할 수 있지만 나중에 정리해야 합니다.

## 추가 리소스

+ [클라우드의 Experience Manager으로 마이그레이션하기 위한 팁과 트릭(Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT 전문가 시리즈 비디오](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [다른 AEMaaCS 항목에 대한 전문가 시리즈 비디오](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)
