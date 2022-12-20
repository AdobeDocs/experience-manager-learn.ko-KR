---
title: 어셈블러 서비스를 사용한 XDP 결합
description: AEM Forms에서 어셈블러 서비스를 사용하여 xdp 결합
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
source-git-commit: 8f17e98c56c78824e8850402e3b79b3d47901c0b
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 2%

---

# 어셈블러 서비스를 사용한 XDP 결합

이 문서에서는 어셈블러 서비스를 사용하여 xdp 문서를 결합하는 기능을 보여 주는 자산을 제공합니다.
다음 jsp 코드가 **주소** xdp 문서에서 address.xdp라는 삽입 포인터가 **주소** master.xdp 문서에서 참조할 수 있습니다. 결과 xdp가 AEM 설치의 루트 폴더에 저장되었습니다.

어셈블러 서비스는 PDF 문서의 조작을 설명하기 위해 유효한 DDX 문서를 사용합니다. 을 참조할 수 있습니다. [DDX 참조 문서 여기](assets/ddxRef.pdf).Page 40에는 xdp 결합에 대한 정보가 있습니다.

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

다른 xdp에 조각을 삽입할 DDX 파일은 아래에 나와 있습니다. DDX는 하위 폼을 삽입합니다  **주소** address.xdp에서 호출된 삽입 지점으로 **주소** at.xdp. 이름이 지정된 결과 문서 **stitched.xdp** 가 파일 시스템에 저장됩니다.

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

AEM 서버에서 이 기능을 사용하려면

* 다운로드 [XDP 결합 패키지](assets/xdp-stitching.zip) 로컬 시스템으로 이동합니다.
* 를 사용하여 패키지를 업로드하고 설치합니다 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* [이 zip 파일의 컨텐츠를 추출합니다](assets/xdp-and-ddx.zip) 샘플 xdp 및 DDX 파일을 가져오려면 다음을 수행하십시오.

**패키지를 설치한 후에는 Granite CSRF 필터허용 목록에 추가하다에 다음 URL을 배치해야 합니다.**

1. 위에 언급된 경로를 추적하려면 허용 목록에 추가하다 아래 단계를 따르십시오.
1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Granite CSRF 필터 Adobe 검색
1. 제외된 섹션에 다음 경로를 추가하고 저장합니다 `/content/AemFormsSamples/assemblerservice`
1. Sling 레퍼러 필터 검색
1. &quot;Allow Empty&quot; 확인란을 선택합니다. (이 설정은 테스트용으로만 사용해야 합니다.) 샘플 코드를 테스트하는 방법에는 여러 가지가 있습니다. 가장 빠르고 가장 쉬운 것은 Postman 앱을 사용하는 것입니다. Postman을 사용하면 서버에 POST 요청을 수행할 수 있습니다. 시스템에 Postman 앱을 설치합니다.
앱을 실행하고 다음 URL을 입력하여 데이터 내보내기 API http://localhost:4502/content/AemFormsSamples/assemblerservice.html 를 테스트합니다.

스크린샷에 지정된 대로 다음 입력 매개 변수를 제공합니다. 이전에 다운로드한 샘플 문서를 사용할 수 있습니다.
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>AEM Forms 설치가 완료되었는지 확인합니다. 모든 번들은 활성 상태여야 합니다.
