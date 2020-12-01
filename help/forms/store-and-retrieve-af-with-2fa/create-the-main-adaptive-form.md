---
title: 주 응용 양식 만들기
description: 적응형 양식을 만들어 지원자 정보 및 적응형 양식을 캡처하여 저장된 적응형 양식을 검색합니다
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# 주 응용 양식 만들기

양식 **StoreAFWwithAttachments**&#x200B;은 주 응용 양식입니다. 이 응용 양식이 사용 사례에 대한 시작 지점입니다. 이 양식에서는 모바일 번호를 비롯한 사용자 세부 사항이 캡처됩니다. 이 양식에는 일부 첨부 파일을 추가할 수도 있습니다. 저장 및 종료 단추를 클릭하면 서버측 코드가 실행되어 양식 데이터를 데이터베이스에 저장되고 고유 응용 프로그램 ID가 생성되어 사용자에게 안전하게 보관됩니다. 이 응용 프로그램 ID는 응용 프로그램과 연결된 모바일 번호를 검색하는 데 사용됩니다.

![기본 애플리케이션 양식](assets/6552.JPG)

이 양식은 교육 과정에서 이전에 만든 **bootboxjs540,storeAFWitures** 클라이언트 라이브러리와 양식 제출 시 트리거되는 AEM 워크플로우와 관련이 있습니다.


* 샘플 양식은 샘플 양식이 올바로 렌더링되도록 AEM으로 가져와야 하는 [사용자 지정 적응형 양식 템플릿](assets/custom-template-with-page-component.zip)을 기반으로 합니다.

* 완료된 [StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip)을(를) 다운로드하여 AEM 인스턴스로 가져올 수 있습니다.

* 양식이 작동하려면 이 양식](assets/workflow-model-store-af-with-attachments.zip)과 연결된 [AEM 작업 흐름을 AEM 인스턴스로 가져와야 합니다.



