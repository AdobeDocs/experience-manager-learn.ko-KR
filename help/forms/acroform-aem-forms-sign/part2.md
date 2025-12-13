---
title: AEM Forms이 포함된 Acroforms
description: Acroforms와 AEM Forms 통합의 2부. Acroform에서 스키마를 만듭니다.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 34
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---


# Acroform에서 스키마 만들기

다음 단계는 이전 단계에서 만든 Acroform에서 스키마를 만드는 것입니다. 샘플 애플리케이션은 이 자습서의 일부로 스키마를 만들기 위해 제공됩니다. 스키마를 만들려면 다음 지침을 따르십시오.

1. [CRXDE Lite](http://localhost:4502/crx/de)에 로그인
2. `/apps/AemFormsSamples/components/createxsd/POST.jsp` 파일을 엽니다.
3. `saveLocation`을(를) 하드 드라이브의 적절한 폴더로 변경합니다. 저장 중인 폴더가 이미 만들어져 있는지 확인하십시오.
4. 브라우저에서 AEM에서 호스팅된 [XSD 만들기](http://localhost:4502/content/DocumentServices/CreateXsd.html) 페이지를 가리킵니다.
5. Acroform을 끌어서 놓습니다.
6. 3단계에서 지정한 폴더를 확인합니다. 스키마 파일이 이 위치에 저장됩니다.

## Acroform 업로드

이 데모가 시스템에서 작동하려면 AEM Assets에서 `acroforms` 폴더를 만들어야 합니다. Acroform을 이 `acroforms` 폴더에 업로드합니다.

>[!NOTE]
>
>샘플 코드는 이 폴더에서 acroform을 찾습니다. Acroform은 적응형 양식의 제출된 데이터를 병합하는 데 필요합니다.
