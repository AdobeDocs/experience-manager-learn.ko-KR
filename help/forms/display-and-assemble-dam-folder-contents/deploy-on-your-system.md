---
title: DAM 폴더 컨텐츠 선택 및 다운로드
description: 로컬 AEM 인스턴스에 자습서 자산 배포
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# 시스템에 배포

로컬 AEM 인스턴스에서 작동하는 이 사용 사례를 가져오려면 아래 나열된 단계를 따르십시오.

* [이 문서에 언급된 단계에 따라 fd-service 사용자를 사용하도록 을 구성합니다](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). DevelopingWithServiceUser 번들을 배포했는지 확인합니다.

* [Newsletter 번들 배포](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 이 번들에는 폴더 컨텐츠를 나열하고 선택한 뉴스레터를 어셈블하는 코드가 포함되어 있습니다.

* [패키지 관리자를 사용하여 패키지 가져오기](assets/newsletter.zip). 이 패키지에는 솔루션을 테스트할 클라이언트 라이브러리와 샘플 pdf 파일이 포함되어 있습니다.

* [샘플 적응형 양식 가져오기](assets/sample-adaptive-form.zip). 이 양식에 선택할 수 있는 뉴스레터가 나열됩니다.

* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
다운로드할 뉴스레터 두 개를 선택하십시오.선택한 뉴스레터가 하나의 pdf로 결합되어 사용자에게 반환됩니다.




