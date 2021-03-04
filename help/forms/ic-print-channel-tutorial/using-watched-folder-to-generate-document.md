---
title: 감시 폴더를 사용하여 인쇄 채널 문서 생성
seo-title: 감시 폴더를 사용하여 인쇄 채널 문서 생성
description: 이 튜토리얼은 인쇄 채널용 첫 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 10개의 튜토리얼을 제공합니다. 이 부분에서는 감시 폴더 메커니즘을 사용하여 인쇄 채널 문서를 생성합니다.
seo-description: 이 튜토리얼은 인쇄 채널용 첫 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 10개의 튜토리얼을 제공합니다. 이 부분에서는 감시 폴더 메커니즘을 사용하여 인쇄 채널 문서를 생성합니다.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: 대화형 통신
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: 개발
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 1%

---


# 감시 폴더를 사용하여 인쇄 채널 문서 생성

이 부분에서는 감시 폴더 메커니즘을 사용하여 인쇄 채널 문서를 생성합니다.

인쇄 채널 문서를 만들고 테스트한 후 일괄 처리 모드 또는 On-Demand 방식으로 이러한 문서를 생성하는 메커니즘이 필요합니다. 일반적으로 이러한 유형의 문서는 일괄 처리 모드로 생성되며 가장 일반적인 메커니즘은 감시 폴더를 사용하는 것입니다.

AEM에서 감시 폴더를 구성할 때 파일을 감시 폴더에 드롭할 때 실행되는 ECMA 스크립트 또는 java 코드를 연결합니다. 이 문서에서는 인쇄 채널 문서를 생성하여 파일 시스템에 저장하는 ECMA 스크립트에 중점을 둡니다.

감시 폴더 구성 및 ECMA 스크립트는 이 자습서](introduction.md)의 시작 부분에서 가져온 에셋의 일부입니다[

감시 폴더에 드롭된 입력 파일의 구조는 다음과 같습니다. ECMA 스크립트는 계정 번호를 읽고 이러한 각 계정에 대한 인쇄 채널 문서를 생성합니다.

문서 생성을 위한 ECMA 스크립트에 대한 자세한 내용은 [이 문서](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)를 참조하십시오.

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

감시 폴더 메커니즘을 사용하여 인쇄 채널 문서를 생성하려면 아래 절차를 따르십시오.

* [이 문서에 언급된 단계를 따르십시오.](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* crx에 로그인하고 /etc/fd/watchfolder/scripts/PrintPDF.ecma으로 이동합니다.

* interactiveCommunicationsDocument 경로가 인쇄할 올바른 문서를 가리키는지 확인합니다.(1행)
* saveLocation(2행)을 메모해 두십시오. 필요에 따라 변경할 수 있습니다.
* 양식 데이터 모델에 대한 입력 매개 변수가 요청 속성에 바인딩되고 해당 바인딩 값이 &quot;계정번호&quot;로 설정되어 있는지 확인합니다. 아래 스크린샷을 참조하십시오.
   ![요청](assets/requestattributeprintchannel.gif)

* 다음 내용으로 accountnumbers.xml 파일을 만듭니다.

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* C:\RenderPrintChannel\input에 xml 파일 놓기

* ECMA 스크립트에 지정된 대로 저장 위치에 있는 pdf 파일을 확인합니다.




