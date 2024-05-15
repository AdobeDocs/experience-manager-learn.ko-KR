---
title: AEM Assets의 InDesign 파일 및 자산 템플릿 이해
description: 이 비디오 튜토리얼에서는 AEM Assets의 에셋 템플릿 기능에 사용할 InDesign 파일 및 모든 관련 고려 사항을 정의하는 과정을 안내합니다.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 909
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# AEM Assets의 InDesign 파일 및 자산 템플릿 이해 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

이 비디오 튜토리얼에서는 AEM Assets의 에셋 템플릿 기능에 사용할 InDesign 파일 및 모든 관련 고려 사항을 정의하는 과정을 안내합니다.

## InDesign 템플릿 파일 구성 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. 다운로드 및 열기 [**InDesign 파일 템플릿**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **태그 패널을 연 다음** 태그 이름 지정 규칙을 검토하고 InDesign 파일의 작성자 가능 요소에 이미 태그가 지정되어 있습니다. 태그가 지정된 요소만 AEM에서 편집할 수 있습니다.

   * **창 > 유틸리티 > 태그**

3. 페이지에서 새 텍스트 요소를 추가하고 &quot;Header&quot; 텍스트를 입력하고 를 적용합니다. **제목** 단락 스타일.

   * **[창] > [스타일] > [단락 스타일]**

   그런 다음 라는 새 태그를 만들어 적용합니다. **페이지 제목**

4. FPO 로고 이미지 추가([zip에 제공](assets/asset-templates-tutorial-video--supporting-files.zip))을 클릭하여 기본 페이지의 로고 요소에 추가합니다.

   * **마우스 오른쪽 버튼 클릭**&#x200B;및 선택&#x200B;**피팅(Fitting) > 프레임 피팅 옵션... > 컨텐트 피팅(Content Fitting) > 비례적으로 프레임 채우기(Fill Frame Proportional)**

   [프레임 맞춤 옵션에 대해 자세히 알아보기](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)및 사용 사례에 적합한 옵션입니다.

5. 바로 붙여넣기를 통해 페이지 및 페이지의 기본 템플릿에서 헤더(로고 및 회사 이름) 아래로 복사합니다.

   * 페이지 1에서 Shift 키를 누른 상태로 macOS을 클릭하거나 Shift 키를 누른 상태로 Windows를 누른 상태로 클릭하여 기본 페이지에서 노출된 헤더를 선택하고 삭제합니다.
   * 기본 페이지에서, 장소에 붙여넣기를 통해 헤더를 페이지 1에 복사합니다.
   * 페이지 2에 대해 단계를 반복합니다

6. [구조] 패널을 열고 각각을 두 번 클릭하여 모든 구조 요소가 InDesign 파일의 실제 요소와 일치하는지 확인합니다. 사용되지 않거나 필요하지 않은 요소를 제거합니다. 모든 태깅이 의미론적이고 요소에 태그가 올바르게 지정되었는지 확인합니다.

   >[!NOTE]
   >
   >잘못 구성된 InDesign 파일이 AEM Asset 템플릿 문제의 가장 일반적인 원인이므로 태깅과 구조가 깔끔하고 정확한지 확인하십시오.

## AEM Assets에서 에셋 템플릿 만들기 및 작성 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **InDesign Server 시작** 8080 포트에서.
2. 다음을 확인합니다. **AEM 작성자 인스턴스가 InDesign Server과 상호 작용하도록 구성되었습니다.**(그 반대의 경우도 가능).

   * [IDS 작업자 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [클라우드 프록시 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM 외부화 OSGi 구성](http://localhost:4502/system/console/configMgr)

3. **AEM Assets에 InDesign 파일을 업로드했습니다.** 또한 AEM 워크플로 및 InDesign Server이 에셋을 완전히 처리할 수 있습니다.
4. **새 템플릿 만들기** 아래에 **에셋 > 템플릿** #4단계에서 AEM에 업로드된 InDesign 파일을 선택합니다.
5. **자산 템플릿 편집** #5단계에서 만들고 편집 가능한 필드를 작성합니다.
6. 클릭 **완료** 에셋 템플릿의 최종 고화질 렌디션을 생성합니다.
7. 에셋 템플릿 카드를 클릭하여 열고 에셋 렌디션을 검토하여 고품질 렌디션을 다운로드합니다.

## 추가 리소스 {#additional-resources}

InDesign 템플릿 파일 및 지원 이미지

다운로드 [InDesign 템플릿 파일 및 지원 이미지](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesign CC 평가판 다운로드](https://creative.adobe.com/products/download/indesign)
* InDesign Server 체험판은에서 다운로드할 수 있습니다. [Adobe 프리릴리스 사이트](https://www.adobeprerelease.com/) 또는 [CC Enterprise 고객은 계정 담당자에게 문의하여 InDesign Server 평가판 라이센스를 요청할 수 있습니다.](https://www.adobe.com/products/indesignserver/faq.html)
