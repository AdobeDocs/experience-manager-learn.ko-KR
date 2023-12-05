---
title: 감시 폴더 메커니즘을 사용하여 인쇄 채널용 대화형 통신 문서 생성
description: 감시 폴더를 사용하여 인쇄 채널 문서 생성
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
duration: 161
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# 감시 폴더 메커니즘을 사용하여 인쇄 채널용 대화형 통신 문서 생성

인쇄 채널 문서를 디자인하고 테스트한 후에는 일반적으로 REST 호출을 수행하여 문서를 생성하거나 감시 폴더 메커니즘을 사용하여 인쇄 문서를 생성해야 합니다.

이 문서에서는 감시 폴더 메커니즘을 사용하여 인쇄 채널 문서를 생성하는 사용 사례를 설명합니다.

파일을 감시 폴더에 놓으면 감시 폴더와 연결된 스크립트가 실행됩니다. 이 스크립트는 아래 문서에 설명되어 있습니다.

감시 폴더에 드롭된 파일은 다음과 같은 구조를 갖습니다. 이 코드는 XML 문서에 나열된 모든 계정 번호에 대한 문을 생성합니다.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

아래 코드 목록은 다음을 수행합니다.

1행 - InteractiveCommunicationsDocument 경로

15-20행: 감시 폴더로 드롭된 XML 문서에서 계정 번호 목록 가져오기

24-25행: 문서와 연결된 PrintChannelService 및 인쇄 채널을 가져옵니다.

30행: 양식 데이터 모델에 accountnumber를 키 요소로 전달합니다.

32-36행: 생성할 문서에 대한 데이터 옵션을 설정합니다.

38행: 문서를 렌더링합니다.

39-40행 - 생성된 문서를 파일 시스템에 저장합니다.

양식 데이터 모델의 REST 끝점에서는 ID를 입력 매개 변수로 예상합니다. 이 id는 아래 스크린샷에 표시된 대로 accountnumber라는 요청 속성에 매핑됩니다.

![requestattribute](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**로컬 시스템에서 테스트하려면 다음 지침을 따르십시오.**

* 이에 설명된 대로 Tomcat 설정 [기사.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat에는 샘플 데이터를 생성하는 war 파일이 있습니다.
* 이에 설명된 대로 시스템 사용자라고도 하는 서비스 설정 [기사](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
이 시스템 사용자에게 다음 노드에 대한 읽기 권한이 있는지 확인하십시오. 권한 로그인 대상: [사용자 관리자](https://localhost:4502/useradmin) 시스템 사용자 &quot;data&quot;를 검색하고 권한 탭으로 이동하여 다음 노드에 대한 읽기 권한을 부여합니다
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* 패키지 관리자를 사용하여 다음 패키지를 AEM으로 가져옵니다. 이 패키지에는 다음 항목이 포함되어 있습니다.


* [샘플 대화형 통신 문서](assets/retirementstatementprint.zip)
* [감시 폴더 스크립트](assets/printchanneldocumentusingwatchedfolder.zip)
* [데이터 소스 구성](assets/datasource.zip)

* /etc/fd/watchfolder/scripts/PrintPDF.ecma 파일을 엽니다. 1행의 interactiveCommunicationsDocument 경로가 인쇄할 올바른 문서를 가리키는지 확인합니다

* 라인 2의 기본 설정에 따라 saveLocation을 수정합니다.

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


* accountnumbers.xml을 C:\RenderPrintChannel\input 폴더에 놓습니다.

* 생성된 PDF 파일은 ecma 스크립트에 지정된 대로 saveLocation에 기록됩니다.

>[!NOTE]
>
>Windows가 아닌 운영 체제에서 사용할 계획이라면 다음 위치로 이동하십시오.
>
>/etc/fd/watchfolder /config/PrintChannelDocument 를 실행하고 원하는 대로 folderPath를 변경합니다
