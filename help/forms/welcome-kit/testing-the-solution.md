---
title: 시작 키트 솔루션 테스트
description: 솔루션 자산을 배포하여 솔루션 테스트
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# 샘플 자산 배포 및 테스트

[시작 키트 패키지 설치](assets/welcomekit.zip). 이 패키지에는 시작 키트에 포함할 자산, 샘플 워크플로우, 이메일 템플릿 및 샘플 pdf 문서를 나열하는 페이지 템플릿, 사용자 지정 구성 요소가 들어 있습니다.
[시작 키트 번들 설치](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). 이 번들에는 페이지를 만드는 코드와 웹 페이지에 표시할 자산을 반환할 Java 클래스가 포함되어 있습니다.
[샘플 적응형 양식 설치](assets/account-openeing-form.zip)
Day CQ Mail Service를 구성합니다. 워크플로우는 SMTP가 올바르게 구성되어 있어야 하는 양식 제출기에 시작 키트 링크를 보냅니다.
요구 사항에 따라 워크플로우의 이메일 보내기 구성 요소를 구성합니다
[양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
상세내역을 입력하고 하나 이상의 뮤추얼 펀드를 선택하고 양식을 제출합니다. 환영 키트에 대한 링크가 포함된 이메일을 받게 됩니다


