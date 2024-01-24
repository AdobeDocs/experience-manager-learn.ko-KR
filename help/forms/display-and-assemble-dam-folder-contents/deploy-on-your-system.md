---
title: 로컬로 자산 배포
description: 로컬 AEM 인스턴스에 튜토리얼 자산 배포
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 32
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 시스템에 배포

아래 나열된 단계에 따라 로컬 AEM 인스턴스에서 이 사용 사례를 작업하십시오.

* [DevelopingWithServiceUser 번들 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) zip 파일에 포함되어 있습니다.

* Apache Sling Service User Mapper 서비스에 다음 항목 추가 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 사용 [configMgr](http://localhost:4502/system/console/configMgr).

* [뉴스레터 번들 배포](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 이 번들에는 폴더 콘텐츠를 나열하고 선택한 뉴스레터를 취합하는 코드가 포함되어 있습니다.

* [패키지 관리자를 사용하여 패키지 가져오기](assets/newsletter.zip). 이 패키지에는 솔루션을 테스트할 클라이언트 라이브러리 및 샘플 pdf 파일이 포함되어 있습니다.

* [샘플 적응형 양식 가져오기](assets/sample-adaptive-form.zip). 이 양식에는 선택할 수 있는 뉴스레터가 나열됩니다.

* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
다운로드할 뉴스레터 두 개를 선택합니다. 선택한 뉴스레터가 하나의 pdf로 결합되어 반환됩니다.
