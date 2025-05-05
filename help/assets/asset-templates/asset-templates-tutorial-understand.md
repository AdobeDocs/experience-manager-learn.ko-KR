---
title: AEM Assets의 InDesign 파일 및 자산 템플릿 이해
description: 이 비디오 튜토리얼에서는 InDesign의 자산 템플릿 기능에 사용할 AEM Assets 파일 및 모든 관련 고려 사항을 정의하는 과정을 안내합니다.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 909
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# AEM Assets의 InDesign 파일 및 자산 템플릿 이해 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

이 비디오 튜토리얼에서는 InDesign의 자산 템플릿 기능에 사용할 AEM Assets 파일 및 모든 관련 고려 사항을 정의하는 과정을 안내합니다.

## InDesign 템플릿 파일 구성 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. [**InDesign 파일 템플릿**](assets/asset-templates-tutorial-video--supporting-files.zip) 다운로드 및 열기
2. **태그 패널을 열고** 태그 명명 규칙을 검토하고 InDesign 파일의 작성자 가능 요소에 이미 태그가 지정되어 있습니다. 태그가 지정된 요소만 AEM에서 편집할 수 있습니다.

   * **창 > 유틸리티 > 태그**

3. 페이지에서 새 텍스트 요소를 추가하고 &quot;Header&quot; 텍스트를 입력한 다음 **제목** 단락 스타일을 적용합니다.

   * **창 > 스타일 > 단락 스타일**

   그런 다음 **Page2Heading.**(이)라는 새 태그를 만들어 적용합니다.

4. FPO 로고 이미지([zip에서 제공](assets/asset-templates-tutorial-video--supporting-files.zip))를 기본 페이지의 로고 요소에 추가합니다.

   * **마우스 오른쪽 단추로**&#x200B;을 클릭하고&#x200B;**맞춤 > 프레임 맞춤 옵션... > 내용 맞춤 > 프레임 비율 채우기**

   [프레임 맞춤 옵션에 대해 자세히 알아보세요](https://helpx.adobe.com/kr/indesign/using/frames-objects.html#fitting_objects_to_frames). 사용 사례에 적합한 옵션입니다.

5. 바로 붙여넣기를 통해 페이지 및 페이지의 기본 템플릿에서 헤더(로고 및 회사 이름) 아래로 복사합니다.

   * 페이지 1에서 Shift 키를 누른 상태로 macOS을 클릭하거나 Shift 키를 누른 상태로 Windows를 누른 상태로 클릭하여 기본 페이지에서 노출된 헤더를 선택하고 삭제합니다.
   * 기본 페이지에서, 장소에 붙여넣기를 통해 헤더를 페이지 1에 복사합니다.
   * 페이지 2에 대해 단계를 반복합니다

6. [구조] 패널을 열고 각각을 두 번 클릭하여 모든 구조 요소가 InDesign 파일의 실제 요소와 일치하는지 확인합니다. 사용되지 않거나 필요하지 않은 요소를 제거합니다. 모든 태깅이 의미론적이고 요소에 태그가 올바르게 지정되었는지 확인합니다.

   >[!NOTE]
   >
   >잘못 구성된 InDesign 파일이 AEM 자산 템플릿 문제의 가장 일반적인 원인이므로 태깅과 구조가 깔끔하고 올바른지 확인하십시오.

## AEM Assets에서 에셋 템플릿 만들기 및 작성 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. 포트 8080에서 **InDesign Server 시작**.
2. **AEM 작성자 인스턴스가 InDesign Server과 상호 작용하도록 구성되었는지**(또는 그 반대) 확인합니다.

   * [IDS 작업자 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [클라우드 프록시 Cloud Service 구성](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM 외부화 OSGi 구성](http://localhost:4502/system/console/configMgr)

3. **InDesign 파일을 AEM Assets에 업로드했습니다**. AEM Workflow 및 InDesign Server에서 자산을 완전히 처리할 수 있도록 허용합니다.
4. **Assets > 템플릿**&#x200B;에서 **새 템플릿을 만들고** #4단계에서 AEM에 업로드된 InDesign 파일을 선택합니다.
5. **#5단계에서 만든 자산 템플릿을 편집**&#x200B;하고 편집 가능한 필드를 작성합니다.
6. 자산 템플릿의 최종 고화질 변환을 생성하려면 **완료**&#x200B;를 클릭하십시오.
7. 에셋 템플릿 카드를 클릭하여 열고 에셋 렌디션을 검토하여 고품질 렌디션을 다운로드합니다.

## 추가 리소스 {#additional-resources}

InDesign 템플릿 파일 및 지원 이미지

[InDesign 템플릿 파일 및 지원 이미지 다운로드](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesign CC 평가판 다운로드](https://creative.adobe.com/products/download/indesign)
* InDesign Server 평가판은 [Adobe 프리릴리스 사이트](https://www.adobeprerelease.com/)에서 다운로드하거나 [CC Enterprise 고객은 계정 담당자에게 문의하여 오전 InDesign Server 평가판 라이선스를 요청할 수 있습니다](https://www.adobe.com/products/indesignserver/faq.html)
