---
title: 로컬로 자산 배포
description: 로컬 AEM 인스턴스에 튜토리얼 에셋 배포
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 시스템에 배포

아래 나열된 단계에 따라 로컬 AEM 인스턴스에서 이 사용 사례를 작업하십시오.

* zip 파일에 포함된 [DevelopingWithServiceUser 번들 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=ko).

* [configMgr](http://localhost:4502/system/console/configMgr)을 사용하여 Apache Sling 서비스 사용자 매퍼 서비스 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**&#x200B;에 다음 항목을 추가하십시오.

* [뉴스레터 번들 배포](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 이 번들에는 폴더 콘텐츠를 나열하고 선택한 뉴스레터를 취합하는 코드가 포함되어 있습니다.

* [패키지 관리자를 사용하여 패키지를 가져옵니다](assets/newsletter.zip). 이 패키지에는 솔루션을 테스트할 클라이언트 라이브러리 및 샘플 pdf 파일이 포함되어 있습니다.

* [샘플 적응형 양식 가져오기](assets/sample-adaptive-form.zip). 이 양식에는 선택할 수 있는 뉴스레터가 나열됩니다.

* [양식을 미리 봅니다](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
다운로드할 뉴스레터 두 개를 선택합니다. 선택한 뉴스레터가 하나의 pdf로 결합되어 반환됩니다.
