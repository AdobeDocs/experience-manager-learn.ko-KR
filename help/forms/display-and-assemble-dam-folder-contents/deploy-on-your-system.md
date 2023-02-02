---
title: 로컬로 자산 배포
description: 로컬 AEM 인스턴스에 자습서 자산 배포
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---

# 시스템에 배포

로컬 AEM 인스턴스에서 작동하는 이 사용 사례를 가져오려면 아래 나열된 단계를 따르십시오.

* [DevelopingWithServiceUser 번들 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) zip 파일에 들어 있습니다.

* Apache Sling Service User Mapper 서비스에 다음 항목을 추가합니다 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 사용 [configMgr](http://localhost:4502/system/console/configMgr).

* [Newsletter 번들 배포](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 이 번들에는 폴더 컨텐츠를 나열하고 선택한 뉴스레터를 어셈블하는 코드가 포함되어 있습니다.

* [패키지 관리자를 사용하여 패키지 가져오기](assets/newsletter.zip). 이 패키지에는 솔루션을 테스트할 클라이언트 라이브러리와 샘플 pdf 파일이 포함되어 있습니다.

* [샘플 적응형 양식 가져오기](assets/sample-adaptive-form.zip). 이 양식에 선택할 수 있는 뉴스레터가 나열됩니다.

* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
다운로드할 뉴스레터 두 개를 선택하십시오.선택한 뉴스레터가 하나의 pdf로 결합되어 사용자에게 반환됩니다.




