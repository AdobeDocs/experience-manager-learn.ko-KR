---
title: 스키마 만들기
description: 적응형 양식으로 가져와야 하는 데이터를 기반으로 스키마 만들기
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 61
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 1%

---

# 소개

첫 번째 단계는 적응형 양식을 채우는 데 사용할 데이터를 기반으로 스키마를 만드는 것입니다.

## XFA는 스키마를 기반으로 합니다

스키마를 사용하여 적응형 양식 만들기

## XFA는 스키마를 기반으로 하지 않음

* AEM Forms 디자이너에서 XDP를 엽니다.
* 파일 클릭 | 양식 속성 | 미리 보기.
* 미리 보기 데이터 생성을 누릅니다.
* 생성을 누릅니다.
* 다음과 같은 의미 있는 파일 이름을 제공하십시오. `form-data.xml`

무료 온라인 도구를 사용하여 다음을 수행할 수 있습니다. [xsd 생성](https://www.freeformatter.com/xsd-generator.html) 이전 단계에서 생성된 xml 데이터에서 가져온 것입니다.

이전 단계의 스키마를 기반으로 적응형 양식을 만듭니다.

>[!NOTE]
>항상 적응형 양식 제출 시 생성되는 데이터를 검사하는 것이 좋습니다. 적응형 양식과 병합해야 하는 데이터의 XML 형식을 잘 알 수 있습니다.

적응형 양식에서 제출된 데이터
![제출된 데이터](./assets/af-submitted-data.png)

PDF에서 내보낸 데이터
![내보낸 데이터](./assets/exported-data.png)

내보낸 데이터에서 다음을 추출해야 합니다. **_맨 위 하위 양식_** 데이터를 적응형 양식과 성공적으로 병합하기 위해 적절한 네임스페이스가 보존된 노드.

## 다음 단계

[OSGi 서비스 만들기](./create-osgi-service.md)
