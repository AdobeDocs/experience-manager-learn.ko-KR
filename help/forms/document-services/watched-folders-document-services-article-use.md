---
title: AEM Forms에서 감시 폴더 사용
description: AEM Forms에서 감시 폴더 구성 및 사용
feature: 출력 서비스
version: 6.4,6.5
topic: 개발
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# AEM Forms에서 감시 폴더 사용{#using-watched-folders-in-aem-forms}

관리자는 사용자가 미리 구성된 워크플로우, 서비스 또는 스크립트 작업을 시작하여 감시 폴더에 파일(예: PDF 파일)을 배치할 때 추가된 파일을 처리하도록 감시 폴더라는 네트워크 폴더를 구성할 수 있습니다. 서비스가 지정한 작업을 수행하면 결과 파일이 지정된 출력 폴더에 저장됩니다. 워크플로우, 서비스 및 스크립트에 대한 자세한 정보.

감시 폴더를 만드는 방법에 대한 자세한 내용을 보려면 [여기](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)를 클릭하십시오

감시 폴더는 일괄 처리 모드에서 문서를 생성하는 데 사용됩니다. 감시 폴더 메커니즘을 사용하여 인쇄 채널에 대한 대화형 커뮤니케이션을 생성하거나 출력 서비스를 사용하여 데이터를 템플릿과 병합할 수 있습니다.

이 문서에서는 감시 폴더 메커니즘을 통해 출력 서비스를 사용하여 템플릿과 데이터를 병합하는 사용 사례를 다룹니다.

출력 서비스는 AEM 문서 서비스의 일부인 OSGi 서비스입니다. 출력 서비스는 AEM Forms 디자이너의 다양한 출력 형식 및 출력 디자인 기능을 지원합니다. 출력 서비스는 XFA 템플릿 및 XML 데이터를 변환하여 다양한 형식으로 인쇄 문서를 생성할 수 있습니다.

출력 서비스에 대한 자세한 내용을 보려면 [여기](https://helpx.adobe.com/aem-forms/6/output-service.html)를 클릭하십시오.
시스템에서 감시 폴더를 설정하려면 아래 단계를 따르십시오.
* [zip 파일의 컨텐츠를 다운로드하고 추출합니다.](assets/outputservicewatchedfolderkt.zip) 이 zip 파일에는 감시 폴더 및 감시 폴더 메커니즘을 사용하여 출력 서비스를 테스트할 샘플 파일을 만드는 패키지가 들어 있습니다
   * Windows 시스템

      * 패키지 관리자를 사용하여 outputservicewatchedfolder.zip을 AEM에 가져옵니다.
      * 이렇게 하면 C 드라이브에 outputservicewatchedfolder라는 감시 폴더가 만들어집니다.
   * 비Windows 시스템
      * [감시 폴더의 구성 설정을 엽니다.](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 적절한 위치를 가리키도록 아웃서비스 노드의 폴더 경로 속성을 설정합니다.
      * 변경 내용을 저장합니다
      * 위에 언급된 위치는 감시 폴더입니다.

SamplePdfFile 및 SampleXdpFile 폴더를 감시 폴더의 입력 폴더에 놓습니다. 파일을 성공적으로 처리하면 결과가 감시 폴더의 결과 폴더 아래에 배치됩니다.


>[!NOTE]
>
>감시 폴더와 연관된 스크립트에 두 개 이상의 파일이 필요한 경우 폴더를 만들고 폴더에 모든 필수 파일을 배치하고 해당 폴더를 감시 폴더의 입력 폴더에 드롭해야 합니다.
>
>감시 폴더와 연결된 스크립트에 입력 파일이 하나만 필요한 경우 해당 파일을 감시 폴더의 입력 폴더에 직접 끌어 놓을 수 있습니다

