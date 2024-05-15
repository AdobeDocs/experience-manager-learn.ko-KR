---
title: HTML 5 Forms 만들기
description: HTML 5 양식 만들기 및 구성
feature: Mobile Forms
doc-type: article
version: 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
duration: 101
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# HTML 5 양식 만들기

HTML5 forms는 Adobe Experience Manager의 새로운 기능으로, HTML5 형식의 XFA 양식 템플릿(xdp) 렌더링을 제공합니다. 이 기능을 사용하면 XFA 기반 PDF이 지원되지 않는 모바일 장치 및 데스크탑 브라우저에서 양식을 렌더링할 수 있습니다. HTML5 양식은 XFA 양식 템플릿의 기존 기능을 지원할 뿐만 아니라 모바일 장치에 대한 스크리블 서명과 같은 새로운 기능을 추가합니다.

## 전제 조건

AEM Forms의 작업 인스턴스가 있는지 확인하십시오. 다음을 따르십시오. [설치 안내서](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) AEM Forms 설치 및 구성

## 첫 번째 HTML 5 양식 만들기

1. [zip 파일의 내용 다운로드 및 추출](assets/assets.zip). zip 파일에 xdp 및 데이터 파일이 포함되어 있습니다.
2. [Forms 및 문서 탐색](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 만들기 -> 파일 업로드 를 클릭합니다.
4. 2단계에서 다운로드한 xdp 템플릿 선택

## HTML으로 미리 보기

xdp는 HTML5 형식 또는 PDF 형식으로 미리 볼 수 있습니다. xdp를 HTML5 형식으로 미리 보려면 다음 단계를 따르십시오

* 새로 업로드한 xdp를 탭하고 을 클릭합니다. _미리 보기 -> HTML으로 미리 보기_. xdp가 HTML5로 렌더링되는 것이 표시됩니다.

>[!NOTE]
>다음을 선택할 때 _PDF으로 미리 보기_ AEM Forms은 Acrobat 플러그인이 필요한 동적 pdf를 렌더링하므로 렌더링된 PDF 옵션이 브라우저에 표시되지 않습니다. PDF을 다운로드하고 Adobe Acrobat/Reader을 사용하여 열어 확인해야 합니다


## 데이터를 포함한 미리 보기

데이터 파일로 HTML5 형식의 xdp를 미리 보려면 다음 단계를 따르십시오.

* 새로 업로드한 xdp를 탭하고 을 클릭합니다. _미리 보기 -> 데이터를 사용하여 미리 보기_. 데이터 파일을 찾아 선택하고 을(를) 클릭합니다. _미리 보기_.
* 데이터로 미리 채워진 HTML5 형식으로 렌더링된 템플릿이 표시됩니다

## xdp 템플릿의 고급 속성 살펴보기

xdp 템플릿의 고급 속성을 사용하면 게시 날짜, 제출 처리기, 양식에 대한 렌더링 프로필, 미리 채우기 서비스 등을 지정할 수 있습니다. 템플릿의 고급 속성을 보려면 xdp를 탭하고 을 클릭합니다. _속성 -> 고급_. 여기에서 여러 속성을 찾을 수 있습니다. 이러한 속성 중 일부는 여기에서 다룹니다.

**URL 제출** - HTML5 양식 제출을 처리하는 URL입니다. 다음 단원에서 이 내용을 다루겠습니다. 여기에 제출 URL을 지정하지 않으면 양식 데이터를 브라우저에 반환하는 기본 제출 처리기가 호출됩니다.

**HTML 렌더링 프로필** - HTML5 양식에는 양식 템플릿의 모바일 렌더링을 활성화하기 위해 REST 끝점으로 노출되는 프로필 개념이 있습니다. 대부분의 경우 기본 렌더링 프로필이 양식을 렌더링하는 데 충분해야 합니다. 기본 렌더링 프로필이 요구 사항을 충족하지 않으면 [사용자 지정 프로필](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) 을 만들고 양식과 연결할 수 있습니다.

**미리 채우기 서비스** - 미리 채우기 서비스는 일반적으로 백엔드 데이터 소스에서 가져온 데이터로 양식을 채우는 데 사용됩니다.
