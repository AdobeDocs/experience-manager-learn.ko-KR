---
title: 하나의 데이터 파일에서 여러 PDF 생성
description: OutputService는 폼 디자인과 데이터를 사용하여 문서를 만들고 폼 디자인과 병합하는 여러 메서드를 제공합니다. 여러 개의 개별 레코드가 포함된 하나의 큰 xml에서 여러 PDF를 생성하는 방법을 알아봅니다.
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
last-substantial-update: 2020-01-07T00:00:00Z
duration: 138
jira: KT-16142
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# 하나의 xml 데이터 파일에서 PDF 문서 세트 생성

OutputService는 폼 디자인과 데이터를 사용하여 문서를 만들고 폼 디자인과 병합하는 여러 메서드를 제공합니다. 다음 문서에서는 여러 개별 레코드가 포함된 하나의 큰 xml에서 여러 pdf를 생성하는 사용 사례에 대해 설명합니다.
다음은 여러 레코드가 포함된 xml 파일의 스크린샷입니다.

![multi-record-xml](assets/multi-record-xml.PNG)

데이터 xml에는 2개의 레코드가 있습니다. 각 레코드는 form1 요소에 의해 표시됩니다. 이 XML은 OutputService [generatePDFOutputBatch 메서드](https://helpx.adobe.com/kr/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html)에 전달됩니다. PDF 문서 목록(레코드당 하나)이 제공됩니다.
generatePDFOutputBatch 메서드의 서명은 다음 매개 변수를 사용합니다

* 템플릿 - 키로 식별되는 템플릿이 포함된 맵
* 데이터 - 키로 식별되는 xml 데이터 문서가 포함된 맵
* pdfOutputOptions - pdf 생성 구성 옵션
* batchOptions - 일괄 처리를 구성하는 옵션



## 사용 사례 세부 정보{#use-case-details}

이 사용 사례에서는 템플릿과 데이터(xml) 파일을 업로드할 수 있는 간단한 웹 인터페이스를 제공합니다. 파일 업로드가 완료되고 POST 요청이 AEM 서블릿으로 전송되면 이 서블릿은 문서를 추출하고 OutputService의 generatePDFOutputBatch 메서드를 호출합니다. 생성된 PDF는 zip 파일로 압축되어 최종 사용자가 웹 브라우저에서 다운로드할 수 있습니다.

## 서블릿 코드{#servlet-code}

다음은 서블릿의 코드 조각입니다. 코드는 요청에서 템플릿(xdp) 및 데이터 파일(xml)을 추출합니다. 템플릿 파일이 파일 시스템에 저장됩니다. 템플릿과 xml(데이터) 파일을 각각 포함하는 templateMap과 dataFileMap이라는 두 개의 맵이 만들어집니다. 그런 다음 DocumentServices 서비스의 generateMultipleRecords 메서드를 호출합니다.

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### 인터페이스 구현 코드{#Interface-Implementation-Code}

다음 코드는 OutputService의 generatePDFOutputBatch를 사용하여 여러 pdf를 생성하고 pdf 파일이 포함된 zip 파일을 호출 서블릿에 반환합니다

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### 서버에 배포{#Deploy-on-your-server}

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오.

* [샘플 자산을 다운로드합니다](assets/mult-records-template-and-xml-file.zip).이 zip 파일에는 템플릿과 xml 데이터 파일이 들어 있습니다.
* [브라우저를 Felix 웹 콘솔로 지정](http://localhost:4502/system/console/bundles)
* [DevelopingWithServiceUser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* configMgr을 사용하여 Apache Sling 서비스 사용자 매퍼 서비스에 다음 항목을 추가합니다.

```java
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```



![user-mapper-service](assets/user-mapper-service-fd-service.png)

* OutputService API를 사용하여 pdf를 생성하는 [사용자 지정 AEMFormsDocumentServices 번들 배포](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).사용자 지정 번들
* [브라우저에서 패키지 관리자를 가리키도록 지정](http://localhost:4502/crx/packmgr/index.jsp)
* [패키지 가져오기 및 설치](assets/generate-multiple-pdf-from-xml.zip). 이 패키지에는 템플릿과 데이터 파일을 삭제할 수 있는 html 페이지가 포함되어 있습니다.
* [브라우저에서 MultiRecords.html을 가리킵니다](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* 템플릿과 xml 데이터 파일을 함께 끌어서 놓습니다.
* 생성된 zip 파일을 다운로드합니다. 이 zip 파일에는 출력 서비스에서 생성한 pdf 파일이 포함되어 있습니다.

>[!NOTE]
>이 기능을 트리거하는 방법에는 여러 가지가 있습니다. 이 예제에서는 웹 인터페이스를 사용하여 템플릿과 데이터 파일을 끌어 놓아 기능을 보여 주었습니다.
