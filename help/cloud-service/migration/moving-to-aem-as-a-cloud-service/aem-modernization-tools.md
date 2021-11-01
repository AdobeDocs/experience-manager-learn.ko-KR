---
title: AEM 현대화 도구를 사용하여 AEM as a Cloud Service으로 이동
description: AEM 현대화 도구를 사용하여 기존 AEM 프로젝트 및 콘텐츠를 AEM과 호환되도록 업그레이드하는 방법을 알아봅니다.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 1%

---


# AEM 현대화 도구

AEM 현대화 도구 를 사용하여 기존 AEM Sites 컨텐츠를 AEM과 호환되고 모범 사례에 맞게 업그레이드하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## AEM 현대화 도구 사용

![AEM 현대화 도구 수명 주기](./assets/aem-modernization-tools.png)

AEM 현대화 도구는 기존 정적 템플릿, 기초 구성 요소 및 parsys로 구성된 기존 AEM 페이지를 자동으로 변환하여 편집 가능한 템플릿, AEM Core WCM 구성 요소 및 레이아웃 컨테이너와 같은 최신 접근 방식을 사용합니다.

### 주요 활동

+ AEM 6.x 프로덕션을 클론하여 AEM 현대화 도구 실행
+ 를 다운로드하여 설치합니다. [최신 AEM 현대화 도구](https://github.com/adobe/aem-modernize-tools/releases/latest) 패키지 관리자를 통해 AEM 6.x 운영 클론에 대해

+ [페이지 구조 변환기](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) 레이아웃 컨테이너를 사용하여 정적 템플릿에서 매핑된 편집 가능한 템플릿으로 기존 페이지 컨텐츠를 업데이트합니다
   + OSGi 구성을 사용하여 전환 규칙 정의
   + 기존 페이지에 대해 페이지 구조 변환기 실행

+ [구성 요소 변환기](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) 레이아웃 컨테이너를 사용하여 정적 템플릿에서 매핑된 편집 가능한 템플릿으로 기존 페이지 컨텐츠를 업데이트합니다
   + JCR 노드 정의/XML을 통해 변환 규칙 정의
   + 기존 페이지에 대해 구성 요소 변환기 도구 실행

+ [정책 가져오기](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) 디자인 구성에서 정책 만들기
   + JCR 노드 정의/XML을 사용하여 변환 규칙 정의
   + 기존 디자인 정의에 대해 정책 가져오기 실행
   + 가져온 정책을 AEM 구성 요소 및 컨테이너에 적용

+ [대화 상자 변환기](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) 클래식(ExtJS) 및 CoralUI 2 기반 구성 요소 대화 상자를 CoralUI 3 TouchUI 기반 대화 상자로 변환.
   + 기존 ExtJS 또는 Coral2 UI 기반 대화 상자에 대해 대화 상자 변환기 도구를 실행합니다
   + 변환된 대화 상자를 Git 리포지토리에 다시 동기화

### 기타 리소스

+ [AEM Modernize 도구 다운로드](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM 현대화 도구 설명서](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - AEM 현대화 세트 소개](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
