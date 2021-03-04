---
title: 서명을 위한 Forms 만들기
description: 서명 패키지에 포함해야 하는 양식을 만듭니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: 개발
role: 비즈니스 전문가
level: 초급
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 1%

---


# 서명을 위한 양식 만들기

다음 단계는 패키지에 포함할 적응형 양식을 만드는 것입니다. 서명을 위한 양식을 만들 때는 다음 사항을 준수해야 합니다.

* 양식은 **SignMultipleForms** 템플릿을 기반으로 해야 합니다. 이렇게 하면 데이터베이스에서 가져온 데이터로 양식이 미리 채워집니다.

* Adobe Sign을 사용하도록 양식을 구성해야 하며 signer1 필드를 고객 이메일 필드와 연결해야 합니다
* 또한 양식을 **getnextform**&#x200B;이라는 clientLib와 연결해야 합니다.
* 양식은 서명 단계 구성 요소를 사용해야 합니다.
* 양식은 사용자 지정 **여러 양식 서명** 구성 요소도 사용해야 합니다. 이 구성 요소를 사용하면 다음 양식으로 이동하여 패키지에 로그인할 수 있습니다.
* 양식 제출은 AEM 작업 과정 **서명 상태 업데이트**&#x200B;를 트리거하도록 구성해야 합니다.
* 데이터 파일 경로가 **Data.xml**&#x200B;으로 설정되어 있는지 확인합니다. 이는 샘플 코드가 양식 제출 프로세스의 페이로드에서 Data.xml이라는 파일을 검색하므로 매우 중요합니다.

양식을 작성했으면 **commonfields** 적응형 양식 조각을 양식에 포함시킵니다. 조각은 숨김으로 표시됩니다. 이 조각에는 다음 필드가 포함되어 있습니다.

* **signed**  - 서명 상태를 유지하는 필드
* **guid**  - 패키지의 양식을 식별하는 고유 식별자
* **customerEmail**  - 이 필드에는 고객의 이메일이 저장됩니다.



>[!NOTE]
>양식의 데이터를 패키지에서 다른 양식으로 전달하려면 양식 필드의 이름을 모든 양식에서 동일하게 지정해야 합니다.

## 모두 완료 양식

패키지의 모든 양식을 작성하고 서명하면 해당 메시지를 표시해야 합니다. 이 메시지는 Aldone 양식의 도움말과 함께 표시됩니다. All 양식은 샘플 양식에 포함되어 있습니다.

## 자산

이 튜토리얼에서 사용되는 양식을 포함하는 샘플 양식은 여기에서 [다운로드할 수 있습니다](assets/forms-for-signing.zip)
