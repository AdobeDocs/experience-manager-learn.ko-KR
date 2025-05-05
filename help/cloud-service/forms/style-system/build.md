---
title: AEM Forms에서 스타일 시스템 사용
description: 테마 프로젝트 빌드
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 4a02f494-ca0e-42d4-bbb9-6223ff8685e3
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# 변경 사항 테스트

**&quot;핵심 구성 요소가 있는 빈 구성 요소&quot;** 템플릿을 기반으로 적응형 양식을 만듭니다. 양식에 3개의 버튼을 드래그 앤 드롭하고 &quot;Corporate&quot;, &quot;Marketing&quot; 및 &quot;Default&quot;로 레이블을 지정합니다.
아래 그림과 같이 페인트 브러시를 선택하여 회사 및 마케팅 버튼에 적절한 스타일 변형을 할당합니다.

![스타일](assets/marketing-variation.png)

세 번째 버튼에는 기본 스타일이 적용됩니다.

## 테마 프로젝트 빌드

다음 단계는 테마 프로젝트를 빌드하는 것입니다. 테마 프로젝트의 루트 폴더로 이동하여 아래 스크린샷과 같이 _&#x200B;**npm run build**&#x200B;_ 명령을 실행합니다.

![테마 빌드](assets/build-theme.png)

테마 프로젝트가 정상적으로 빌드되면 변경 사항을 테스트할 준비가 되었습니다.

## CSS를 빠르고 쉽게 테스트하는 방법

* 테마 프로젝트의 dist 폴더 아래에 있는 theme.css 파일을 엽니다.전체 파일 내용을 선택하고 복사합니다.
* 이전 단계에서 만든 양식을 미리 봅니다.
* 버튼 중 하나를 마우스 오른쪽 버튼으로 클릭하고 검사 를 선택하여 개발자 콘솔을 엽니다.
* 개발자 콘솔에서 theme.css를 클릭하여 theme.css를 엽니다.
* CTR-A를 사용하여 theme.css의 전체 콘텐츠를 선택하고 삭제 버튼을 누릅니다.
* 이전 단계에서 빌드한 theme.css의 콘텐츠를 복사하여 붙여넣습니다.
* 버튼은 아래와 같이 적절한 스타일로 업데이트됩니다.

![final-buttons](assets/final-state-buttons.png)

## 변경 사항 푸시

변경 사항에 만족하면 [프론트엔드 파이프라인](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/enable-frontend-pipeline-devops/create-frontend-pipeline)을 사용하여 변경 사항을 클라우드 인스턴스에 푸시할 수 있습니다.
