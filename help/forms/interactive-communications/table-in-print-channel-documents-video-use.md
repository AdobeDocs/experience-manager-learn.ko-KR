---
title: AEM Forms 인쇄 채널 문서에서 표 구성 요소 사용
seo-title: Using Table Component in AEM Forms Print Channel Document
description: 다음 비디오에서는 Interactive Communications에서 인쇄 채널 문서에 테이블 구성 요소를 사용하는 데 필요한 단계를 안내합니다.
feature: Interactive Communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# AEM Forms 인쇄 채널 문서에서 표 구성 요소 사용 {#using-table-component-in-aem-forms-print-channel-document}

다음 비디오에서는 Interactive Communications에서 인쇄 채널 문서에 테이블 구성 요소를 사용하는 데 필요한 단계를 안내합니다.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

표는 데이터를 테이블 형식으로 표시하는 데 사용됩니다. 테이블의 행은 데이터 소스에서 반환한 데이터에 따라 증가하거나 축소해야 합니다. 인쇄 채널 문서에서 표를 사용하려면 AEM Forms 디자이너를 사용하여 레이아웃 파일(xdp 파일)을 만들어야 합니다. 이 레이아웃 파일에서 필요한 열 수가 있는 표를 추가합니다. 요구 사항에 따라 열 필드 개체 유형이 TextField 또는 Numeric 필드인지 확인하십시오. 각 열에 대해 필드가 데이터 바인딩이 Use Name으로 설정되어 있는지 확인합니다.

>[!NOTE]
>
>표를 동적으로 만들려면 행이 반복으로 표시되었는지 확인합니다.

**자체 서버에서 사용해 보십시오**

* [하드 드라이브에 자산 파일을 다운로드하여 압축을 해제합니다](assets/usingtablesinprintchannel.zip)

* 패키지 관리자를 사용하여 두 zip 파일을 AEM에 가져옵니다

* 이 문서와 연결된 자산에 포함되는 항목은 다음과 같습니다.

   * 레이아웃 단편

   * 양식 데이터 모달

   * 대화형 통신 문서
   * sampleretirementaccountdata.json

* Interactive Communication 문서 열기 [편집 모드](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* TableDemo 레이아웃 조각을 기여도 섹션에 추가합니다.
* 표 셀을 비디오에 표시된 대로 적절한 양식 데이터 모델 요소에 바인딩합니다

* 제공된 샘플 json 데이터 파일을 사용하여 대화형 통신 문서 미리 보기
