---
title: 서명용 Forms 만들기
description: 서명 패키지에 포함해야 하는 양식을 만듭니다.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 71
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 서명용 양식 만들기

다음 단계는 패키지에 포함할 적응형 양식을 만드는 것입니다. 서명용 양식을 작성할 때는 다음 사항을 준수해야 합니다.

* 양식이 **SignMultipleForms** 템플릿을 기반으로 하는지 확인하십시오. 이렇게 하면 데이터베이스에서 가져온 데이터로 양식이 미리 채워집니다.

* Acrobat Sign을 사용하도록 양식을 구성해야 하고 서명자 1 필드를 고객 이메일 필드와 연결해야 합니다.
* **getnextform**&#x200B;이라는 clientLib에도 양식을 연결해야 합니다.
* 양식에는 서명 단계 구성 요소를 사용해야 합니다.
* 양식에서도 사용자 지정 **여러 양식에 서명** 구성 요소를 사용해야 합니다. 이 구성 요소를 사용하면 다음 양식으로 이동하여 패키지에 로그인할 수 있습니다.
* 양식 제출은 AEM 워크플로 **서명 상태 업데이트**&#x200B;를 트리거하도록 구성해야 합니다.
* 데이터 파일 경로가 **Data.xml**(으)로 설정되어 있는지 확인하십시오. 이는 샘플 코드가 양식 제출을 처리하는 페이로드에서 Data.xml이라는 파일을 검색하기 때문에 매우 중요합니다.

양식을 작성한 후 양식에 **공통필드** 적응형 양식 단편을 포함하십시오. 조각이 숨겨진 것으로 표시됩니다. 이 조각에는 다음 필드가 포함되어 있습니다.

* **서명됨** - 서명 상태를 유지하는 필드
* **guid** - 패키지에서 양식을 식별하는 고유 식별자입니다.
* **customerEmail** - 이 필드에는 고객의 이메일이 보관됩니다.



>[!NOTE]
>한 양식에서 패키지의 다른 양식으로 데이터를 전달하려면 양식 필드 이름이 모든 양식에서 동일하게 지정되었는지 확인하십시오.

## 모든 양식 완료

패키지에 있는 모든 양식을 작성하고 서명하면 적절한 메시지를 표시해야 합니다. 이 메시지는 Alldone 양식의 도움으로 표시됩니다. Alldone 양식은 샘플 양식에 포함됩니다.

## 자산

이 자습서에서 사용되는 가 포함된 샘플 양식은 [여기에서 다운로드](assets/forms-for-signing.zip)할 수 있습니다.

## 다음 단계

[로컬 시스템에서 솔루션 테스트](./testing-and-trouble-shooting.md)
