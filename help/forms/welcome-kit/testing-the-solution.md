---
title: 시작 키트 솔루션 테스트
description: 솔루션 자산을 배포하여 솔루션 테스트
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
duration: 29
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# 샘플 자산 배포 및 테스트

[시작 키트 패키지를 설치합니다](assets/welcomekit.zip). 이 패키지에는 시작 키트에 포함할 페이지 템플릿, 에셋을 나열하는 사용자 지정 구성 요소, 샘플 워크플로, 이메일 템플릿 및 샘플 pdf 문서가 포함되어 있습니다.
[시작 키트 번들을 설치합니다](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). 이 번들에는 페이지를 만드는 코드와 웹 페이지에 표시될 자산을 반환하는 java 클래스가 포함되어 있습니다.
[샘플 적응형 양식 설치](assets/account-openeing-form.zip)
일별 CQ 메일 서비스를 구성합니다. 워크플로우는 SMTP를 올바르게 구성해야 하는 양식 제출자에게 시작 키트 링크를 보냅니다.
요구 사항에 따라 워크플로우의 이메일 전송 구성 요소 구성
[양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
상세내역을 입력하고 하나 이상의 뮤추얼 펀드를 선택하고 양식을 제출합니다.
시작 키트에 대한 링크가 포함된 이메일을 받습니다
