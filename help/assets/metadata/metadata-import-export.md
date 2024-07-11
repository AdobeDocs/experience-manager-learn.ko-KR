---
title: AEM Assets에서 메타데이터 가져오기 및 내보내기 사용
description: Adobe Experience Manager Assets의 메타데이터 가져오기 및 내보내기 기능을 사용하는 방법에 대해 알아봅니다. 가져오기 및 내보내기 기능을 사용하여 콘텐츠 작성자는 기존 에셋에 대한 메타데이터를 대량으로 업데이트할 수 있습니다.
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 419
source-git-commit: 726715890d997ba3bb85f4833e220ac2222b3a42
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---

# AEM Assets에서 메타데이터 가져오기 및 내보내기 사용 {#metadata-import-and-export}

Adobe Experience Manager Assets의 메타데이터 가져오기 및 내보내기 기능을 사용하는 방법에 대해 알아봅니다. 가져오기 및 내보내기 기능을 사용하여 콘텐츠 작성자는 기존 에셋에 대한 메타데이터를 대량으로 업데이트할 수 있습니다.

## 메타데이터 내보내기 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

>[!TIP]
>
> Excel에서 메타데이터 내보내기 CSV 파일을 열 때 [Excel Importer](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) UTF-8 인코딩 CSV 파일 문제를 방지하기 위해 파일을 두 번 클릭하는 대신
>
> Excel에서 메타데이터 내보내기 CSV 파일을 열려면 다음 단계를 수행합니다.
> 
> 1. Microsoft Excel 열기
> 1. 선택 __파일 > 새로 만들기__ 빈 스프레드시트를 만들려면
> 1. 빈 스프레드시트를 연 상태에서 을 선택합니다. __파일 > 가져오기__
> 1. 선택 __텍스트__ 파일 및 클릭 __가져오기__
> 1. 파일 시스템에서 내보낸 CSV 파일을 선택하고 __데이터 가져오기__
> 1. 가져오기 마법사의 1단계에서 다음을 선택합니다 __구분됨__ 및 설정 __파일 원본__ 끝 __유니코드(UTF-8)__, 및 클릭 __다음__
> 1. 2단계에서 __구분 기호__ 끝 __쉼표__, 및 클릭 __다음__
> 1. 3단계에서 __열 데이터 형식__ 있는 그대로 및 클릭 __완료__
> 1. 선택 __가져오기__ 스프레드시트에 데이터를 추가하려면

## 메타데이터 가져오기 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> 가져올 CSV 파일을 준비할 때 메타데이터 내보내기 기능을 사용하여 에셋 목록으로 CSV를 더 쉽게 생성할 수 있습니다. 그런 다음 생성된 CSV 파일을 수정하고 가져오기 기능을 사용하여 가져올 수 있습니다.

## 메타데이터 CSV 파일 형식 {#metadata-file-format}

### 첫 행

* CSV 파일의 첫 번째 행은 메타데이터 스키마를 정의합니다.
* 첫 번째 열의 기본값은 입니다. `assetPath`에셋에 대한 절대 JCR 경로를 보유합니다.

* 첫 번째 행의 후속 열은 자산의 다른 메타데이터 속성을 가리킵니다.
   * 예 : `dc:title, dc:description, jcr:title`

* 단일 값 속성 형식

   * `<metadata property name> {{<property type}}`
   * 속성 유형을 지정하지 않으면 기본값은 String입니다.
   * 예를 들어`dc:title {{String}}`

* 속성 이름은 대소문자를 구분합니다.
   * 올바른 : `dc:title {{String}}`
   * 잘못됨: `Dc:Title {{String}}`

* 속성 유형은 대/소문자를 구분하지 않습니다.
* 모두 유효 [JCR 속성 유형](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) 지원됨

* 다중 값 속성 형식 - `<metadata property name> {{<property type : MULTI }}`

### 두 번째 행에서 N개 행

* 첫 번째 열에는 자산에 대한 절대 JCR 경로가 있습니다. 예: /content/dam/asset1.jpg
* 에셋의 메타데이터 속성은 CSV 파일에서 누락된 값을 가질 수 있습니다. 해당 특정 자산에 대한 메타데이터 속성이 누락되어도 업데이트되지 않습니다.
