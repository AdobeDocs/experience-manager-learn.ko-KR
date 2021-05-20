---
title: AEM Forms을 사용한 Acrobat
seo-title: Acroform과 적응형 양식 데이터 병합
description: Acrobat과 AEM Forms 통합의 2부. Acrobat에서 스키마를 만듭니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---


# Acrobat에서 스키마 만들기

다음 단계는 이전 단계에서 만든 Acrobat에서 스키마를 만드는 것입니다. 이 자습서의 일부로 스키마를 생성하기 위해 샘플 응용 프로그램이 제공됩니다. 스키마를 만들려면 다음 지침을 따르십시오.

1. [CRXDE Lite](http://localhost:4502/crx/de)에 로그인합니다.
2. `/apps/AemFormsSamples/components/createxsd/POST.jsp` 파일로 열기
3. `saveLocation`을 하드 드라이브의 적절한 폴더로 변경합니다. 저장할 폴더가 이미 만들어졌는지 확인합니다.
4. AEM에 호스팅된 [XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) 페이지를 만들 브라우저를 가리킵니다.
5. Acrobat을 끌어다 놓습니다.
6. 3단계에서 지정한 폴더를 확인합니다. 스키마 파일이 이 위치에 저장됩니다.

## Acrobat 업로드

이 데모가 시스템에서 작동하려면 AEM Assets에서 `acroforms` 폴더를 만들어야 합니다. Acrobat을 이 `acroforms` 폴더에 업로드합니다.

>[!NOTE]
>
>샘플 코드는 이 폴더에서 Acroform을 찾습니다. 적응형 양식의 제출된 데이터를 병합하려면 Acrobat이 필요합니다.
