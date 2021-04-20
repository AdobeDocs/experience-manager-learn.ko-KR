---
title: 'AEM Assets의 InDesign 파일 및 자산 템플릿 이해 '
description: 이 비디오 자습서에서는 InDesign 파일을 정의하고 AEM Assets의 자산 템플릿 기능에서 사용할 수 있는 모든 관련 고려 사항을 안내합니다.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 1%

---


# AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}의 InDesign 파일 및 자산 템플릿 이해

이 비디오 자습서에서는 InDesign 파일을 정의하고 AEM Assets의 자산 템플릿 기능에서 사용할 수 있는 모든 관련 고려 사항을 안내합니다.

## InDesign 템플릿 파일 {#constructing-the-indesign-template-file} 구성

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. [**InDesign 파일 템플릿 다운로드 및 열기**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **[태그] 패널을 열고 [태그 이름 지정] 규칙을** 검토하여 InDesign 파일의 작성 가능 요소에 태그가 이미 지정되어 있음을 확인합니다. AEM에서는 태그가 있는 요소만 편집할 수 있습니다.

   * **창 > 유틸리티 > 태그**

3. 페이지에서 새 텍스트 요소를 추가하고 &quot;머리글&quot; 텍스트를 입력하고 **머리글** 단락 스타일을 적용합니다.

   * **창 > 스타일 > 단락 스타일**

   그런 다음 **Page2Heading이라는 새 태그를 만들고 적용합니다.**

4. FPO 로고 이미지(](assets/asset-templates-tutorial-video--supporting-files.zip)zip에 제공된[) 를 페이지의 로고 요소에 기본 추가합니다.

   * **마우스 오른쪽 단추**&#x200B;를 클릭하고 [맞춤] > [프레임 맞춤 옵션...] > [내용 맞춤] > [비율에 맞게 프레임 채우기]를 **선택합니다.**
   [프레임 맞춤 옵션에 대한 자세한](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) 내용을 살펴보십시오. 이러한 옵션이 사용 사례에 적합합니다.

5. [제자리에 붙여넣기]를 통해 페이지 및 페이지기본의 템플릿에서 머리글(로고 및 회사 이름)을 복사합니다.

   * 페이지 1에서 macOS의 경우 Shift-Cmd-Click, Windows의 경우 Shift-Alt-Click을 기본 사용하여 페이지에서 노출된 헤더를 선택하고 삭제합니다.
   * 페이지에서 기본 제자리에 붙여넣기를 통해 헤더를 페이지 1에 복사
   * 2페이지에 대해 단계를 반복합니다.

6. 각 요소를 두 번 클릭하여 구조 패널을 열고 모든 구조적 요소가 InDesign 파일의 실제 요소에 해당하는지 확인합니다. 사용되지 않거나 필요하지 않은 요소를 제거합니다. 모든 태그 지정이 의미적이고 요소에 태그가 올바르게 지정되었는지 확인합니다.

   >[!NOTE]
   >
   >제대로 구성되지 않은 InDesign 파일은 AEM 에셋 템플릿 관련 문제의 가장 일반적인 원인이므로 태그 지정 및 구조가 분명하고 올바른지 확인하십시오.

## AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}에서 자산 템플릿 만들기 및 작성

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **InDesign Server** 포트 8080을 시작합니다.
2. **AEM 작성자 인스턴스가 InDesign Server**(및 그 반대로)과 상호 작용하도록 구성되어 있는지 확인합니다.

   * [IDS 작업자 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [클라우드 프록시 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi 구성](http://localhost:4502/system/console/configMgr)

3. **InDesign 파일을 AEM 자산에 업로드하고 AEM** 워크플로우 및 InDesign Server에서 자산을 완전히 처리할 수 있도록 허용합니다.
4. **자산 > 템플릿** 에서 새  **템플릿을 만들고 단계** 에서 AEM에 업로드된 InDesign 파일을 #4.
5. **#5단계에서** 만든 에셋 템플릿을 편집하고 편집 가능한 필드를 작성합니다.
6. 자산 템플릿의 최종 고품질 변환을 생성하려면 **완료**&#x200B;를 클릭합니다.
7. 자산 템플릿 카드를 클릭하여 열고 자산 표현물을 검토하여 고품질의 표현물을 다운로드합니다.

## 추가 리소스 {#additional-resources}

InDesign 템플릿 파일 및 지원 이미지

[InDesign 템플릿 파일 및 지원 이미지](assets/asset-templates-tutorial-video--supporting-files-1.zip) 다운로드

* [InDesign CC 시험버전 다운로드](https://creative.adobe.com/products/download/indesign)
* [InDesign Server 시험버전 다운로드](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
