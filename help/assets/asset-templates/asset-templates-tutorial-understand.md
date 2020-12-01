---
title: 'AEM Assets의 InDesign 파일 및 자산 템플릿 이해 '
description: 이 비디오 자습서에서는 AEM Assets의 자산 템플릿 기능에 사용하기 위해 InDesign 파일 및 관련된 모든 고려 사항을 설명합니다.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# AEM Assets의 InDesign 파일 및 자산 템플릿 이해 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

이 비디오 자습서에서는 AEM Assets의 자산 템플릿 기능에 사용하기 위해 InDesign 파일 및 관련된 모든 고려 사항을 설명합니다.

## InDesign 템플릿 파일 {#constructing-the-indesign-template-file} 구성

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. [**InDesign 파일 템플릿 다운로드 및 열기**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **태그 패널을 열고 태그 명명 규칙을** 검토한 후 InDesign 파일에서 작성 가능한 요소에 이미 태그가 지정되어 있다는 점을 참고하십시오. AEM에서는 태그가 지정된 요소만 편집할 수 있습니다.

   * **창 > 유틸리티 > 태그**

3. 페이지에서 새 텍스트 요소를 추가하고 텍스트 &quot;Header&quot;를 제공하고 **Heading** 단락 스타일을 적용합니다.

   * **창 > 스타일 > 단락 스타일**

   그런 다음 **Page2Heading.**&#x200B;이라는 새 태그를 만들어 적용합니다.

4. FPO 로고 이미지(](assets/asset-templates-tutorial-video--supporting-files.zip)zip에 제공된[개)를 기본 페이지의 로고 요소에 추가합니다.

   * **마우스 오른쪽 버튼을 클릭하고 피팅(Fitting) > 프레임 맞춤 옵션(Frame Fitting Options...) > 컨텐츠 맞춤(Content Fitting)**> 비율에 맞게 프레임 채우기(Fill Frame Consequently)를 **선택합니다.**
   [사용 사례에 적합한 프레임 맞춤 옵션에](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) 대해 자세히 알아보십시오.

5. 페이지 및 페이지의 기본 템플릿에서 제자리에 붙여넣기를 통해 헤더(로고 및 회사 이름)를 복사합니다.

   * 1페이지의 경우, macOS에서는 Shift-Cmd-Click, Windows에서는 Shift-Alt-Click을 사용하여 페이지에서 기본 노출된 헤더를 선택하고 삭제합니다.
   * 페이지기본에서 제자리에 붙여넣기를 통해 머리글을 페이지 1로 복사
   * 2페이지에 대한 단계 반복

6. 각 구조물을 두 번 클릭하여 구조 패널을 열고 InDesign 파일의 실제 요소와 모든 구조적 요소가 일치하는지 확인합니다. 사용되지 않거나 불필요한 요소를 제거합니다. 모든 태그 지정 기능이 의미적이고 요소에 태그가 올바르게 지정되었는지 확인합니다.

   >[!NOTE]
   >
   >AEM 에셋 템플릿 관련 문제의 가장 일반적인 원인은 부실하게 구성된 InDesign 파일이므로 태그 지정 및 구조가 깨끗하고 정확한지 확인하십시오.

## AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}에서 자산 템플릿 만들기 및 작성

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **InDesign 서버** 포트 8080을 시작합니다.
2. **AEM 작성자 인스턴스가 InDesign Server**(및 그 반대로)과 상호 작용하도록 구성되어 있는지 확인합니다.

   * [IDS 작업자 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [클라우드 프록시 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi 구성](http://localhost:4502/system/console/configMgr)

3. **InDesign 파일을 AEM 자산에 업로드하고 AEM** 워크플로우 및 InDesign Server에서 자산을 완전히 처리할 수 있도록 허용합니다.
4. **자산 > 템플릿** 에서 새  **템플릿** 을 만들고 AEM에 업로드된 InDesign 파일을 #4단계에서 선택합니다.
5. **5단계에서 만든** 자산 템플릿을 편집하고 편집 가능한 필드를 작성합니다.
6. 자산 템플릿의 최종 고품질 변환을 생성하려면 **완료**&#x200B;를 클릭합니다.
7. 자산 템플릿 카드를 클릭하여 열고 자산 표현물을 검토하여 고품질의 표현물을 다운로드합니다.

## 추가 리소스 {#additional-resources}

InDesign 템플릿 파일 및 지원 이미지

[InDesign 템플릿 파일 및 지원 이미지](assets/asset-templates-tutorial-video--supporting-files-1.zip) 다운로드

* [InDesign CC 시험버전 다운로드](https://creative.adobe.com/products/download/indesign)
* [InDesign Server 시험버전 다운로드](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
