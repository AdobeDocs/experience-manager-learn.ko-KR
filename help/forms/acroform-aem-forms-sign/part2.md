---
title: AEM Forms을 사용한 Acrobat
seo-title: Acrobat과 적응형 양식 데이터 병합
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 1ba56ad44df4dc327cf37d39ac72539b5c7af4a2
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---


# Acrobat에서 스키마 만들기

다음 단계는 이전 단계에서 생성된 Acrobat에서 스키마를 만드는 것입니다. 이 자습서의 일부로 스키마를 만들기 위한 샘플 응용 프로그램이 제공됩니다. 스키마를 만들려면 다음 지침을 따르십시오.

1. CRXDE Lite에 [로그인](http://localhost:4502/crx/de)
2. 파일에 열기 `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. 하드 드라이브 `saveLocation` 의 해당 폴더로 변경합니다. 저장할 폴더가 이미 생성되어 있는지 확인합니다.
4. AEM에서 호스팅되는 [XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) 페이지를 브라우저에서 가리킵니다.
5. Acrobat을 드래그하여 놓습니다.
6. 3단계에서 지정한 폴더를 확인합니다. 스키마 파일이 이 위치에 저장됩니다.

## Acrobat 업로드

이 데모를 시스템 작업을 수행하려면 AEM Assets에 있는 폴더를 만들어야 `acroforms` 합니다. Acrobat을 이 `acroforms` 폴더에 업로드합니다.

>[!NOTE]
샘플 코드는 이 폴더에서 Acrobat을 찾습니다. 적응형 양식의 제출된 데이터를 병합하려면 Acrobat이 필요합니다.
