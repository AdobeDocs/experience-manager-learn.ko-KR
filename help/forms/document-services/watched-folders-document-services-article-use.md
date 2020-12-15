---
title: AEM Forms에서 감시 폴더 사용
seo-title: AEM Forms에서 감시 폴더 사용
description: AEM Forms에서 감시 폴더 구성 및 사용
seo-description: AEM Forms에서 감시 폴더 구성 및 사용
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# AEM Forms{#using-watched-folders-in-aem-forms}에서 감시 폴더 사용

관리자는 미리 구성된 워크플로우, 서비스 또는 스크립트 작업을 시작할 때 감시 폴더(예: PDF 파일)에 파일을 배치할 때 추가된 파일을 처리하도록 감시됨 폴더라는 네트워크 폴더를 구성할 수 있습니다. 서비스가 지정된 작업을 수행하면 결과 파일이 지정된 출력 폴더에 저장됩니다. 워크플로우, 서비스 및 스크립트에 대한 자세한 내용을 살펴보십시오.

감시 폴더 만들기에 대한 자세한 내용을 보려면 [여기를 클릭](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

감시 폴더는 일괄 처리 모드에서 문서를 생성하는 데 사용됩니다. 감시 폴더 메커니즘을 사용하여 인쇄 채널용 대화형 통신을 생성하거나 출력 서비스를 사용하여 데이터를 템플릿과 병합할 수 있습니다.

이 문서에서는 감시 폴더 메커니즘을 통해 출력 서비스를 사용하여 템플릿으로 데이터를 병합하는 사용 사례를 다룹니다.

출력 서비스는 AEM 문서 서비스에 포함된 OSGi 서비스입니다. 출력 서비스는 AEM Forms Designer의 다양한 출력 포맷 및 출력 디자인 기능을 지원합니다. 출력 서비스는 XFA 템플릿과 XML 데이터를 변환하여 다양한 형식으로 인쇄 문서를 생성할 수 있습니다.

출력 서비스에 대한 자세한 내용을 보려면 [여기](https://helpx.adobe.com/aem-forms/6/output-service.html)를 클릭하십시오.
시스템에 감시 폴더를 설정하려면 아래 단계를 따르십시오.
* [zip 파일의 컨텐츠를 다운로드 및 추출합니다](assets/outputservicewatchedfolderkt.zip). 이 zip 파일에는 감시 폴더 메커니즘을 사용하여 출력 서비스를 테스트할 수 있는 감시 폴더 및 샘플 파일을 만드는 패키지가 들어 있습니다.
   * Windows 시스템

      * 패키지 관리자를 사용하여 outputservicewatchedfolder.zip을 AEM으로 가져옵니다.
      * 그러면 C 드라이브에 outputservicewatchedfolder라는 감시 폴더가 생성됩니다.
   * 비 Windows 시스템
      * [감시 폴더의 구성 설정을 엽니다.](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 외부 서비스 노드의 폴더 경로 속성을 적절한 위치를 가리키도록 설정합니다.
      * 변경 내용 저장
      * 위에 언급된 위치는 감시 폴더입니다.

SamplePdfFile 및 SampleXdpFile 폴더를 감시 폴더의 입력 폴더에 놓습니다. 파일을 성공적으로 처리하면 해당 결과가 감시 폴더의 results 폴더 아래에 배치됩니다.


>[!NOTE]
>
>감시 폴더와 연관된 스크립트에 파일이 두 개 이상 필요한 경우 폴더를 만들고 필요한 모든 파일을 폴더에 배치하고 해당 폴더를 감시 폴더의 입력 폴더에 저장해야 합니다.
>
>감시 폴더와 연관된 스크립트에 하나의 입력 파일만 있으면 해당 파일을 감시 폴더의 입력 폴더에 직접 드롭할 수 있습니다

