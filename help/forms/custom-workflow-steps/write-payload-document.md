---
title: 파일 시스템에 페이로드 문서 작성
description: 페이로드 폴더 아래에 있는 쓰기 문서를 파일 시스템에 추가하는 맞춤형 프로세스 단계
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 파일 시스템에 문서 작성

일반적인 사용 사례는 워크플로에서 생성된 문서를 파일 시스템에 작성하는 것입니다.
이 맞춤형 워크플로 프로세스 단계를 통해 파일 시스템에 워크플로 문서를 쉽게 작성할 수 있습니다.
사용자 지정 프로세스에는 쉼표로 구분된 다음 인수가 사용됩니다

```java
ChangeBeneficiary.pdf,c:\confirmation
```

첫 번째 인수는 파일 시스템에 저장할 문서의 이름입니다. 두 번째 인수는 문서를 저장할 폴더 위치입니다. 예를 들어 위의 사용 사례에서 문서는 `c:\confirmation\ChangeBeneficiary.pdf`

다음 스크린샷은 사용자 지정 프로세스 단계에 전달하는 데 필요한 인수를 보여 줍니다
![write-payload-file-system](assets/write-payload-file-system.png)

[사용자 지정 번들은 여기에서 다운로드할 수 있습니다.](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
