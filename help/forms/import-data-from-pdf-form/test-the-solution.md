---
title: PDF 파일에서 적응형 양식으로 데이터 가져오기
description: PDF 파일을 가져와서 적응형 양식을 채우는 자습서
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 21
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 샘플 자산 배포

샘플 자산을 배포하여 로컬 AEM Forms 인스턴스에서 이 솔루션을 작동할 수 있습니다

* [패키지 관리자를 통해 pdf 양식을 업로드하기 위한 클라이언트 라이브러리 및 사용자 정의 구성 요소 가져오기](./assets/client-libs-custom-component.zip)
* OSGi 웹 콘솔을 사용하여 번들을 다운로드하고 배포합니다[사용자 지정 문서 서비스 번들](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* OSGi 웹 콘솔을 사용하여 번들을 다운로드하고 배포합니다. [서비스 사용자 번들로 개발](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* OSGi 웹 콘솔을 사용하여 번들을 다운로드하여 배포[pdf 파일에서 데이터 가져오기](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* _**Apache Sling 서비스 사용자 매퍼 서비스**_ OSGi 구성 콘솔에서 _**DevelopingWithServiceUser.core:getresourceresolver=data**_ 항목을 추가합니다.
