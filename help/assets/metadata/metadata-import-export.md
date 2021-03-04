---
title: AEM Assets에서 메타데이터 가져오기 및 내보내기 사용
description: Adobe Experience Manager Assets의 메타데이터 기능을 가져오고 내보내는 방법을 알아봅니다. 가져오기 및 내보내기 기능을 통해 컨텐츠 작성자는 기존 에셋에 대한 메타데이터를 일괄 업데이트할 수 있습니다.
version: 6.3, 6.4, 6.5, cloud-service
topic: 컨텐츠 관리
feature: 메타데이터
role: 관리자
level: 중간
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 4%

---


# AEM Assets {#metadata-import-and-export}에서 메타데이터 가져오기 및 내보내기 사용

Adobe Experience Manager Assets의 메타데이터 기능을 가져오고 내보내는 방법을 알아봅니다. 가져오기 및 내보내기 기능을 통해 컨텐츠 작성자는 기존 에셋에 대한 메타데이터를 일괄 업데이트할 수 있습니다.

## 메타데이터 내보내기 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## 메타데이터 가져오기 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> 가져올 CSV 파일을 준비할 때 메타데이터 내보내기 기능을 사용하여 자산 목록을 포함하는 CSV를 쉽게 생성할 수 있습니다. 그런 다음 생성된 CSV 파일을 수정하고 가져오기 기능을 사용하여 가져올 수 있습니다.

## 메타데이터 CSV 파일 형식 {#metadata-file-format}

### 첫 행

* CSV 파일의 첫 번째 행은 메타데이터 스키마를 정의합니다.
* 첫 번째 열의 기본값은 자산에 대한 절대 JCR 경로를 보유하는 `assetPath`입니다.

* 첫 번째 행의 후속 열은 자산의 다른 메타데이터 속성을 가리킵니다.
   * 예를 들어,`dc:title, dc:description, jcr:title`

* 단일 값 속성 형식

   * `<metadata property name> {{<property type}}`
   * 속성 유형을 지정하지 않으면 기본값은 String입니다.
   * 예를 들어,`dc:title {{String}}`

* 속성 이름은 대/소문자를 구분합니다.
   * 정답:`dc:title {{String}}`
   * 오답:`Dc:Title {{String}}`

* 속성 유형은 대/소문자를 구분하지 않습니다.
* 모든 유효한 [JCR 속성 유형](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)이(가) 지원됩니다.

* 다중 값 속성 형식 - `<metadata property name> {{<property type : MULTI }}`

### N행 두 번째 행

* 첫 번째 열에는 자산에 대한 절대 JCR 경로가 포함됩니다. 예:/content/dam/asset1.jpg
* 자산에 대한 메타데이터 속성에 CSV 파일에 누락된 값이 있을 수 있습니다. 특정 자산에 대한 누락된 메타데이터 속성은 업데이트되지 않습니다.
