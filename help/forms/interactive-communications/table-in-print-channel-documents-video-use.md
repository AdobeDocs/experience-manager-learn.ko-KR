---
title: AEM Forms 인쇄 채널 문서에서 표 구성 요소 사용
seo-title: AEM Forms 인쇄 채널 문서에서 표 구성 요소 사용
description: 다음 비디오에서는 인쇄 채널 문서에 대해 대화형 통신에서 표 구성 요소를 사용하는 데 필요한 단계를 안내합니다.
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# AEM Forms 인쇄 채널 문서에서 표 구성 요소 사용 {#using-table-component-in-aem-forms-print-channel-document}

다음 비디오에서는 인쇄 채널 문서에 대해 대화형 통신에서 표 구성 요소를 사용하는 데 필요한 단계를 안내합니다.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

표는 데이터를 표 형식으로 표시하는 데 사용됩니다. 테이블의 행은 데이터 소스에서 반환된 데이터에 따라 확대하거나 축소해야 합니다. 인쇄 채널 문서에서 표를 사용하려면 AEM Forms Designer를 사용하여 레이아웃 파일(xdp 파일)을 만들어야 합니다. 이 레이아웃 파일에서 필요한 열 수와 함께 표를 추가합니다. 요구 사항에 따라 열 필드 개체 유형이 TextField 또는 Numeric 필드인지 확인합니다. 각 열에 대해 필드는 데이터 바인딩이 [이름 사용]으로 설정되어 있는지 확인합니다.

>[!NOTE]
>
>표를 동적으로 만들려면 행을 반복으로 표시했는지 확인하십시오.

**자체 서버에서 사용해 보기**

* [하드 드라이브에 에셋 파일을 다운로드 및 압축 해제](assets/usingtablesinprintchannel.zip)

* 패키지 관리자를 사용하여 2개의 zip 파일을 AEM으로 가져오기

* 이 아티클과 관련된 에셋에 포함된 내용은 다음과 같습니다.

   * 레이아웃 단편

   * 양식 데이터 양식

   * 인터랙티브 커뮤니케이션 문서
   * sampleretirementaccountdata.json

* [편집 모드](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)에서 대화형 통신 문서를 엽니다.

* 기여도 섹션에 TableDemo 레이아웃 조각을 추가합니다.
* 표 셀을 비디오에 표시된 대로 적절한 양식 데이터 모델 요소에 바인딩합니다.

* 제공된 샘플 json 데이터 파일을 사용하여 대화형 통신 문서 미리 보기

