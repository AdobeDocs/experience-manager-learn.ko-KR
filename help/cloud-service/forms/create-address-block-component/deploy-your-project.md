---
title: 주소 구성 요소를 만드는 중
description: AEM Forms Cloud Service에서 새 주소 핵심 구성 요소 만들기
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 프로젝트 배포

AEM Forms Cloud Service에 프로젝트를 배포하기 전에 AEM Forms의 로컬 클라우드 준비 인스턴스에 프로젝트를 배포하는 것이 좋습니다.

## 변경 사항을 AEM 프로젝트와 동기화

IntelliJ를 실행하고 ``ui.apps`` 폴더(아래 표시)
![intellij](assets/intellij.png)

마우스 오른쪽 단추 클릭 ``adaptiveForm`` 노드를 선택하고 새로 만들기 선택 | 패키지 이름을 추가해야 합니다. **주소 블록** 패키지로

새로 만든 패키지를 마우스 오른쪽 단추로 클릭합니다. ``addressblock`` 및 선택 ``repo | Get Command`` 아래와 같이
![저장소 동기화](assets/sync-repo.png)

프로젝트를 로컬 클라우드 지원 AEM Forms 인스턴스와 동기화해야 합니다. .content.xml 파일을 확인하여 속성을 확인할 수 있습니다
![동기화 후](assets/after-sync.png)

## 로컬 인스턴스에 프로젝트 배포

새 명령 프롬프트 창을 시작하고 프로젝트의 루트 폴더로 이동한 다음 아래 표시된 명령을 사용하여 프로젝트를 빌드합니다.
![배포](assets/build-project.png)

프로젝트가 성공적으로 배포되면 이제 주소 구성 요소를 적응형 양식에서 사용할 수 있습니다

## 프로젝트를 클라우드 환경에 배포

모든 것이 로컬 개발 환경에서 잘 보이는 경우 다음 단계는 [cloud manager를 사용하는 클라우드 인스턴스.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



