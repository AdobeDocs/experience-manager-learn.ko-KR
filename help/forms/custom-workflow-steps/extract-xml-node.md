---
title: 페이로드 문서를 파일 시스템에 쓰기
description: 페이로드 폴더 아래에 있는 쓰기 문서를 파일 시스템에 추가하는 사용자 지정 프로세스 단계
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# 제출된 데이터 xml에서 노드 추출

이 사용자 정의 프로세스 단계는 다른 xml 문서에서 노드를 추출하여 새 xml 문서를 만드는 것입니다. 제출된 데이터를 xdp 템플릿과 병합하여 pdf를 생성하려는 경우 이 기능을 사용해야 합니다. 예를 들어 적응형 양식을 제출할 때 xdp 템플릿과 병합해야 하는 데이터는 데이터 요소 내에 있습니다. 이 경우 적절한 데이터 요소를 추출하여 다른 xml 문서를 만들어야 합니다.

다음 스크린샷은 사용자 지정 프로세스 단계에 전달해야 하는 인수를 보여 줍니다
![프로세스 단계](assets/create-xml-process-step.png)
다음은 매개 변수입니다
* Data.xml - 노드를 추출할 xml 파일입니다
* datatomerge.xml - 추출된 노드로 생성된 새 xml
* /afData/afUnboundData/data - 추출할 노드입니다.


다음 스크린샷은 페이로드 폴더 아래에 만들어지는 datamerge.xml을 보여줍니다
![create-xml](assets/create-xml.png)

[여기에서 사용자 지정 번들을 다운로드할 수 있습니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)