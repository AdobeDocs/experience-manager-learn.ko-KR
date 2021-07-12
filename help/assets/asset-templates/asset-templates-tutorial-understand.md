---
title: 'AEM Assets의 InDesign 파일 및 자산 템플릿 이해 '
description: 이 비디오 자습서에서는 AEM Assets의 자산 템플릿 기능에 사용할 InDesign 파일 및 추가 고려 사항을 모두 정의하는 과정을 안내합니다.
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---


# AEM Assets의 InDesign 파일 및 자산 템플릿 이해 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

이 비디오 자습서에서는 AEM Assets의 자산 템플릿 기능에 사용할 InDesign 파일 및 추가 고려 사항을 모두 정의하는 과정을 안내합니다.

## InDesign 템플릿 파일 구성 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. [**InDesign 파일 템플릿**](assets/asset-templates-tutorial-video--supporting-files.zip)&#x200B;을 다운로드하여 엽니다.
2. **InDesign 패널을 열고** 태그 이름 지정 규칙을 검토하고 태그 파일의 작성 가능한 요소에 이미 태그가 지정되어 있습니다. AEM에서는 태그가 지정된 요소만 편집할 수 있습니다.

   * **창 > 유틸리티 > 태그**

3. 페이지에서 새 텍스트 요소를 추가하고 &quot;Header&quot; 텍스트를 제공하고 **Heading** Paragraph Style을 적용합니다.

   * **창 > 스타일 > 단락 스타일**

   그런 다음 **Page2Heading.** 라는 새 태그를 만들어 적용합니다.

4. FPO 로고 이미지(](assets/asset-templates-tutorial-video--supporting-files.zip)zip에 제공된[)를 기본 페이지의 로고 요소에 추가합니다.

   * **마우스 오른쪽 버튼**&#x200B;을 클릭하고 피팅(Fitting) > 프레임 피팅 옵션... > 컨텐츠 피팅(Content Fitting) > 비례적으로 프레임 채우기(Fill Frame Alignly)를 **선택합니다.**
   [사용 사례에 적합한 프레임 피팅](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) 옵션에 대해 자세히 알아보십시오.

5. 제 자리에 붙여넣기를 통해 페이지 및 페이지의 기본 템플릿에서 헤더(로고 및 회사 이름)를 복사합니다.

   * 페이지 1에서 Shift 키를 누른 상태로 macOS를 클릭하거나 Shift 키를 누른 상태로 Windows를 클릭하여 기본 페이지에서 노출된 헤더를 선택하고 삭제합니다.
   * 기본 페이지에서 제 자리에 붙여넣기를 통해 헤더를 1페이지에 복사합니다
   * 2페이지에 대한 단계를 반복합니다

6. 각 구조를 두 번 클릭하여 구조 패널을 열고 모든 구조적 요소가 InDesign 파일의 실제 요소에 해당하는지 확인합니다. 사용되지 않거나 필요하지 않은 요소를 제거합니다. 모든 태깅이 의미적이고 요소에 올바르게 태그가 지정되어 있는지 확인합니다.

   >[!NOTE]
   >
   >AEM 자산 템플릿 문제의 가장 일반적인 원인은 잘못 구성된 InDesign 파일이므로 태깅 및 구조가 깨끗하고 정확한지 확인하십시오.

## AEM Assets에서 자산 템플릿 만들기 및 작성 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **InDesign 서버** 포트 8080을 시작합니다.
2. **AEM 작성자 인스턴스가 InDesign Server**&#x200B;과 상호 작용하도록 구성되어 있는지 확인합니다(또는 그 반대로).

   * [IDS 작업자 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [클라우드 프록시 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi 구성](http://localhost:4502/system/console/configMgr)

3. **InDesign 파일을 AEM Assets에** 업로드하고, AEM 워크플로우 및 InDesign Server에서 자산을 완전히 처리할 수 있습니다.
4. **자산 >** 템플릿 **에서 새 템플릿** 을 만들고 #4 단계에서 AEM에 업로드된 InDesign 파일을 선택합니다.
5. **단계 #5에서** 만든 자산 템플릿을 편집하고 편집 가능한 필드를 작성합니다.
6. **완료** 를 클릭하여 자산 템플릿의 최종 고화질 표현물을 생성합니다.
7. 자산 템플릿 카드를 클릭하여 열고 자산 표현물 을 검토하여 고화질 표현물을 다운로드합니다.

## 추가 리소스 {#additional-resources}

InDesign 템플릿 파일 및 지원 이미지

[InDesign 템플릿 파일 및 지원 이미지](assets/asset-templates-tutorial-video--supporting-files-1.zip) 다운로드

* [InDesign CC 평가판 다운로드](https://creative.adobe.com/products/download/indesign)
* [CC Enterprise 고객은 계정 담당자에게 문의하여 am InDesign Server 평가판 라이센스를 요청할 수 있습니다](https://www.adobe.com/products/indesignserver/faq.html)
