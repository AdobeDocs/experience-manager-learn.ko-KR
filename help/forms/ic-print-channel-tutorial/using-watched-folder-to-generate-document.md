---
title: 감시 폴더를 사용하여 인쇄 채널 문서 생성
description: 인쇄 채널용 첫 번째 대화형 통신 문서를 만들기 위한 10단계 튜토리얼의 일부입니다. 이 부분에서는 감시 폴더 메커니즘을 사용하여 인쇄 채널 문서를 생성합니다.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
duration: 87
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# 감시 폴더를 사용하여 인쇄 채널 문서 생성

이 부분에서는 감시 폴더 메커니즘을 사용하여 인쇄 채널 문서를 생성합니다.

인쇄 채널 문서를 만들고 테스트한 후 배치 모드 또는 온디맨드로 이러한 문서를 생성하는 메커니즘이 필요합니다. 일반적으로 이러한 종류의 문서는 일괄 처리 모드에서 생성되며 가장 일반적인 메커니즘은 감시 폴더를 사용하는 것입니다.

AEM에서 감시 폴더를 구성할 때는 감시 폴더에 파일을 놓을 때 실행되는 ECMA 스크립트 또는 Java 코드를 연결합니다. 이 문서에서는 인쇄 채널 문서를 생성하여 파일 시스템에 저장하는 ECMA 스크립트에 중점을 둡니다.

감시 폴더 구성 및 ECMA 스크립트는 [이 자습서의 시작](introduction.md)

감시 폴더에 드롭되는 입력 파일은 다음과 같은 구조를 가진다. ECMA 스크립트는 계정 번호를 읽고 이러한 각 계정에 대한 인쇄 채널 문서를 생성합니다.

문서 생성을 위한 ECMA 스크립트에 대한 자세한 내용은 [이 문서 참조](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

감시 폴더 메커니즘을 사용하여 인쇄 채널 문서를 생성하려면 아래 단계를 수행하십시오.

* [이 문서에 언급된 단계 수행](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* crx에 로그인하고 /etc/fd/watchfolder/scripts/PrintPDF.ecma 로 이동합니다.

* interactiveCommunicationsDocument 경로가 인쇄할 올바른 문서를 가리키는지 확인합니다.( 라인 1)
* saveLocation(2행)을 기록해 두십시오.필요에 따라 변경할 수 있습니다.
* 양식 데이터 모델에 대한 입력 매개 변수가 요청 속성에 바인딩되어 있고 해당 바인딩 값이 &quot;accountnumber&quot;로 설정되어 있는지 확인하십시오. 아래 스크린샷을 참조하십시오.
  ![요청](assets/requestattributeprintchannel.gif)

* 다음 내용으로 accountnumbers.xml 파일을 만듭니다

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

* xml 파일을 C:\RenderPrintChannel\input에 놓습니다.

* ECMA 스크립트에 지정된 대로 저장 위치에서 pdf 파일을 확인합니다.

## 다음 단계

[양식 제출에서 에이전트 UI 열기](./opening-agent-ui-on-form-submission.md)