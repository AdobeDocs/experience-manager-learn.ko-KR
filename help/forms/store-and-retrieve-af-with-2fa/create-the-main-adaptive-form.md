---
title: 기본 적응형 양식 만들기
description: 지원자 정보를 캡처하는 적응형 양식 및 저장된 적응형 양식을 검색하는 적응형 양식 만들기
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 46
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# 기본 적응형 양식 만들기

양식 **StoreAFWithAttachments** 는 기본 적응형 양식입니다. 이 적응형 양식은 사용 사례의 진입점입니다. 이 양식에서는 모바일 번호를 포함한 사용자 세부 사항이 캡처됩니다. 이 양식에는 일부 첨부 파일을 추가할 수도 있습니다. 저장 및 종료 단추를 클릭하면 서버측 코드가 실행되어 양식 데이터가 데이터베이스에 저장되고 안전한 보관을 위해 고유한 애플리케이션 ID가 생성되어 사용자에게 표시됩니다. 이 애플리케이션 ID는 애플리케이션과 연결된 모바일 번호를 검색하는 데 사용됩니다.

![기본 애플리케이션 양식](assets/6552.JPG)

이 양식은 과(와) 연결되어 있습니다. **bootboxjs540,storeAFWithAttachments** 이전 과정에서 생성된 클라이언트 라이브러리와 양식 제출 시 트리거되는 AEM 워크플로우입니다.


* 샘플 양식은 다음을 기반으로 합니다. [사용자 지정 적응형 양식 템플릿](assets/custom-template-with-page-component.zip) 샘플 양식을 올바르게 렌더링하려면 AEM으로 가져와야 합니다.

* 완료됨 [StoreAfWithAttachments 양식](assets/store-af-with-attachments-form.zip) 다운로드하여 AEM 인스턴스로 가져올 수 있습니다.

* 다음 [이 양식과 연결된 AEM 워크플로](assets/workflow-model-store-af-with-attachments.zip) 양식을 사용하려면 AEM 인스턴스로 가져와야 합니다.


## 다음 단계

[저장된 양식을 검색하는 양식 만들기](./retrieve-saved-form.md)