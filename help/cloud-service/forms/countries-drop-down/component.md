---
title: 국가 구성 요소에 대한 구조 만들기
description: crx에서 구성 요소 구조 만들기
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: 87e790c9-6ef6-4337-90b8-687ca576b21a
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 2%

---

# 국가 구성 요소에 대한 구조 만들기

AEM Forms 인스턴스에 로그인하고 다음 단계에 따라 기본 드롭다운 구성 요소를 기반으로 새 구성 요소를 만듭니다.

1. CRXDE Lite에서 &#39;/apps/&lt;yourproject>/components/adaptiveForm/dropdown&#39;으로 이동합니다.
2. 드롭다운 구성 요소를 복사하여 동일한 디렉터리 수준에 붙여넣습니다.
3. 복사된 구성 요소의 이름을 국가로 변경합니다.
4. cq:template 노드의 jcr:title 속성을 국가로 업데이트합니다.
5. 변경 사항을 저장합니다.

이제 Countries 라는 이름의 새로운 구성 요소가 있으며, 이는 기본 드롭다운 구성 요소의 정확한 복제본입니다. 이는 추가적인 맞춤화를 위한 토대 역할을 합니다.

## HTL 파일 만들기

국가 구성 요소에 대한 HTL 파일을 생성하려면:

1. crx 저장소의 국가 폴더로 이동합니다
2. countries.html이라는 새 파일을 만듭니다.
3. crx 저장소에서 /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html 파일을 열고 내용을 복사합니다.
4. 복사한 컨텐츠를 countries.html에 붙여넣습니다.
5. 스크린샷에 표시된 대로 새 슬링 모델을 사용하도록 코드를 변경합니다
6. 변경 사항을 저장합니다.

![sling-model](assets/countriesdropdown.png)

마지막으로 프로젝트를 이러한 업데이트와 동기화하여 CRX 저장소의 변경 사항이 AEM 프로젝트에 반영되도록 합니다.


## 다음 단계

[cq 대화 상자 만들기](./dialog.md)
