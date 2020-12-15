---
title: AEM Assets에서 메타데이터 가져오기 및 내보내기 사용
seo-title: AEM Assets에서 메타데이터 가져오기 및 내보내기 사용
description: AEM Assets 메타데이터 가져오기 및 내보내기 기능을 사용하면 컨텐츠 작성자는 AEM에서 에셋 메타데이터를 손쉽게 이동 및 내보낼 수 있으며 Microsoft Excel의 강력한 기능을 활용하여 메타데이터를 규모에 맞게 조작할 수 있으므로 AEM에서 기존 에셋에 대한 대량 업데이트 메타데이터를 활용할 수 있습니다.
seo-description: AEM Assets 메타데이터 가져오기 및 내보내기 기능을 사용하면 컨텐츠 작성자는 AEM에서 에셋 메타데이터를 손쉽게 이동 및 내보낼 수 있으며 Microsoft Excel의 강력한 기능을 활용하여 메타데이터를 규모에 맞게 조작할 수 있으므로 AEM에서 기존 에셋에 대한 대량 업데이트 메타데이터를 활용할 수 있습니다.
uuid: db7e57a4-b0c1-4a48-906d-802c19964313
discoiquuid: 72dd9230-73e1-454e-a3e0-9281e621d901
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 2%

---


# AEM Assets{#using-metadata-import-and-export-in-aem-assets}에서 메타데이터 가져오기 및 내보내기 사용

AEM Assets 메타데이터 가져오기 및 내보내기 기능을 사용하면 컨텐츠 작성자는 AEM에서 에셋 메타데이터를 손쉽게 이동 및 내보낼 수 있으며 Microsoft Excel의 강력한 기능을 활용하여 메타데이터를 규모에 맞게 조작할 수 있으므로 AEM에서 기존 에셋에 대한 대량 업데이트 메타데이터를 활용할 수 있습니다.

## 메타데이터 내보내기 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## 메타데이터 가져오기 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

[WeRetail 스포츠 폴더](assets/we-retail-sports.zip) 다운로드

[자산 메타데이터 패키지](assets/we-retail-sports-asset-metadata.zip) 다운로드

## 메타데이터 파일 형식 {#metadata-file-format}

### CSV 파일 형식

#### 첫 행

* CSV 파일의 첫 번째 행은 메타데이터 스키마를 정의합니다.
* 첫 번째 열의 기본값은 자산에 대한 절대 JCR 경로를 보유하는 `assetPath`입니다.

* 첫 번째 행의 후속 열은 자산의 다른 메타데이터 속성을 가리킵니다.

   * 예 : `dc:title, dc:description, jcr:title`

* 단일 값 속성 형식

   * `<metadata property name> {{<property type}}`
   * 속성 유형을 지정하지 않으면 기본적으로 String이 됩니다.
   * 예를 들어,`dc:title {{String}}`

* 속성 이름은 대/소문자를 구분합니다.
   * 정답:`dc:title {{String}}`
   * 오답:`Dc:Ttle {{String}}`

* 속성 유형은 대/소문자를 구분하지 않습니다.
* 모든 유효한 [JCR 속성 유형](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)이(가) 지원됩니다.

* 다중 값 속성 형식 - `<metadata property name> {{<property type : MULTI }}`

#### N행 두 번째 행

* 첫 번째 열에는 자산에 대한 절대 JCR 경로가 포함됩니다. 예:/content/dam/asset1.jpg
* 자산에 대한 메타데이터 속성에 CSV 파일에 누락된 값이 있을 수 있습니다. 특정 자산에 대한 메타데이터 속성이 누락되면 업데이트되지 않습니다.