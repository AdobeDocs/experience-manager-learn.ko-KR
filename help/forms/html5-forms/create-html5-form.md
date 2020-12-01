---
title: HTML5 Forms 만들기
description: HTML5 양식 작성 및 구성
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---


# HTML5 양식 만들기

HTML5 양식은 HTML5 형식으로 XFA 양식 템플릿(xdp)을 렌더링하는 Adobe Experience Manager의 새로운 기능입니다. 이 기능을 사용하면 XFA 기반 PDF가 지원되지 않는 모바일 디바이스 및 데스크탑 브라우저에서 양식을 렌더링할 수 있습니다. HTML5 양식은 XFA 양식 템플릿의 기존 기능을 지원할 뿐만 아니라 모바일 장치용 스크리블 서명과 같은 새로운 기능을 추가합니다.

## 전제 조건

AEM Forms의 작업 인스턴스가 있는지 확인하십시오. AEM Forms을 설치하고 구성하려면 [설치 안내서](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html)에 따르십시오.

## 간단한 HTML5 양식 만들기

1. [zip 파일의 내용을 다운로드하고 추출합니다](assets/assets.zip). zip 파일에 xdp 및 데이터 파일이 포함되어 있습니다.
2. [Forms 및 문서로 이동](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. 만들기 -> 파일 업로드를 클릭합니다.
4. 2단계에서 다운로드한 xdp 템플릿을 선택합니다.

## HTML로 미리 보기

xdp는 HTML5 형식 또는 PDF 형식으로 미리 볼 수 있습니다. HTML5 형식으로 xdp를 미리 보려면 다음 단계를 따르십시오

* 새로 업로드된 xdp를 누르고 _미리 보기 -> HTML로 미리 보기_&#x200B;를 클릭합니다. HTML5로 렌더링된 xdp를

>[!NOTE]
>_PDF로 미리 보기_ 옵션을 선택하면 AEM Forms에서 Acrobat 플러그인이 필요한 동적 pdf를 렌더링하므로 렌더링된 PDF가 브라우저에 표시되지 않습니다.PDF를 다운로드하고 Adobe Acrobat/Reader을 사용하여 열면


## 데이터를 포함한 미리 보기

데이터 파일과 함께 HTML5 형식으로 xdp를 미리 보려면 다음 단계를 수행하십시오.

* 새로 업로드된 xdp를 누르고 _미리 보기 -> 데이터를 사용하여 미리 보기_&#x200B;를 클릭합니다. 데이터 파일을 찾아 선택하고 _미리 보기_&#x200B;를 클릭합니다.
* 데이터가 미리 채워진 HTML5 형식으로 렌더링된 템플릿을 볼 수 있습니다

## xdp 템플릿의 고급 속성 살펴보기

xdp 템플릿의 고급 속성을 사용하면 게시 날짜, 제출 처리기, 양식의 렌더링 프로필, 자동 채우기 서비스 등을 지정할 수 있습니다. 템플릿의 고급 속성을 보려면 xdp를 누르고 _속성 -> 고급_&#x200B;을 클릭합니다. 다양한 속성을 확인할 수 있습니다. 이러한 속성 중 일부는 여기에서 다룹니다.

**전송 URL**  - HTML5 양식 제출을 처리하는 URL입니다. 다음 단원에서 이것을 다루겠습니다. 여기에서 제출 URL을 지정하지 않으면 양식 데이터를 브라우저에 반환하는 기본 제출 처리기가 호출됩니다.

**HTML 렌더링 프로필**  - HTML5 양식에는 양식 템플릿의 모바일 렌더링을 지원하기 위해 REST 끝점으로 표시되는 프로필의 개념이 있습니다. 대부분의 경우 기본 렌더링 프로필로도 양식을 렌더링하기에 충분합니다. 기본 렌더링 프로필이 사용자의 요구 사항을 충족하지 않을 경우, [사용자 지정 프로필](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html)을 만들고 양식과 연결할 수 있습니다.

**자동 완성 서비스**  - 자동 완성 기능은 일반적으로 백엔드 데이터 소스에서 가져온 데이터로 양식을 채우는 데 사용됩니다.

