---
title: AEM Assets에서 메타데이터 가져오기 및 내보내기 사용
seo-title: AEM Assets에서 메타데이터 가져오기 및 내보내기 사용
description: 컨텐츠 작성자는 AEM Assets 메타데이터 가져오기 및 내보내기 기능을 사용하여 에셋 메타데이터를 AEM 안팎으로 손쉽게 이동할 수 있고 강력한 Microsoft Excel을 활용하여 메타데이터를 규모에 맞게 조작할 수 있으므로 AEM에서 기존 에셋에 대한 일괄 업데이트 메타데이터를 활용할 수 있습니다.
seo-description: 컨텐츠 작성자는 AEM Assets 메타데이터 가져오기 및 내보내기 기능을 사용하여 에셋 메타데이터를 AEM 안팎으로 손쉽게 이동할 수 있고 강력한 Microsoft Excel을 활용하여 메타데이터를 규모에 맞게 조작할 수 있으므로 AEM에서 기존 에셋에 대한 일괄 업데이트 메타데이터를 활용할 수 있습니다.
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


# AEM Assets에서 메타데이터 가져오기 및 내보내기 사용{#using-metadata-import-and-export-in-aem-assets}

컨텐츠 작성자는 AEM Assets 메타데이터 가져오기 및 내보내기 기능을 사용하여 에셋 메타데이터를 AEM 안팎으로 손쉽게 이동할 수 있고 강력한 Microsoft Excel을 활용하여 메타데이터를 규모에 맞게 조작할 수 있으므로 AEM에서 기존 에셋에 대한 일괄 업데이트 메타데이터를 활용할 수 있습니다.

## 메타데이터 내보내기 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## 메타데이터 가져오기 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

WeRetail [스포츠 폴더 다운로드](assets/we-retail-sports.zip)

자산 [메타데이터 패키지 다운로드](assets/we-retail-sports-asset-metadata.zip)

## 메타데이터 파일 형식 {#metadata-file-format}

### CSV 파일 형식

#### 첫 행

* CSV 파일의 첫 번째 행은 메타데이터 스키마를 정의합니다.
* [첫 번째] 열의 기본값은 로 `assetPath`설정되며, 이 열은 자산의 절대 JCR 경로를 보유합니다.

* 첫 번째 행의 다음 열은 자산의 다른 메타데이터 속성을 가리킵니다.

   * 예 : `dc:title, dc:description, jcr:title`

* 단일 값 속성 형식

   * `<metadata property name> {{<property type}}`
   * 속성 유형을 지정하지 않으면 기본적으로 String이 됩니다.
   * 예를 들어,`dc:title {{String}}`

* 속성 이름은 대/소문자를 구분합니다.
   * 정답: `dc:title {{String}}`
   * 틀림: `Dc:Ttle {{String}}`

* 속성 유형은 대/소문자를 구분하지 않습니다.
* 모든 유효한 [JCR 속성 유형이](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) 지원됩니다.

* 다중 값 속성 형식 - `<metadata property name> {{<property type : MULTI }}`

#### 두 번째 행-N 행

* 첫 번째 열에는 자산의 절대 JCR 경로가 포함됩니다. 예:/content/dam/asset1.jpg
* 자산에 대한 메타데이터 속성에 CSV 파일에 값이 누락될 수 있습니다. 해당 특정 자산에 대한 누락된 메타데이터 속성은 업데이트되지 않습니다.