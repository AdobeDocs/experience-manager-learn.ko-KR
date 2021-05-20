---
title: 하나의 데이터 파일에서 여러 pdf 생성
seo-title: 하나의 데이터 파일에서 여러 pdf 생성
feature: 출력 서비스
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 1%

---


# 하나의 xml 데이터 파일에서 PDF 문서 세트 생성

OutputService에서는 양식 디자인과 데이터를 사용하여 문서를 만드는 많은 방법을 양식 디자인에 병합합니다. 다음 문서에서는 여러 개의 개별 레코드가 포함된 하나의 큰 xml에서 여러 pdf를 생성하는 사용 사례에 대해 설명합니다.
다음은 여러 레코드가 포함된 xml 파일의 스크린샷입니다.

![multi-record-xml](assets/multi-record-xml.PNG)

데이터 xml에 2개의 레코드가 있습니다. 각 레코드는 form1 요소로 표시됩니다. 이 xml은 OutputService [generatePDFOutputBatch 메서드](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html)에 전달되며, pdf 문서 목록은 레코드당 하나씩 가져옵니다
generatePDFOutputBatch 메서드의 서명은 다음 매개 변수를 사용합니다

* 템플릿 - 키가 식별한 템플릿이 들어 있는 맵
* 데이터 - 키로 식별되는 xml 데이터 문서가 포함된 매핑
* pdfOutputOptions - pdf 생성을 구성하는 옵션
* batchOptions - 일괄 처리를 구성하는 옵션

>[!NOTE]
>
>이 사용 사례는 이 [웹 사이트](https://forms.enablementadobe.com/content/samples/samples.html?query=0)에서 라이브 예제로 사용할 수 있습니다.

## 사용 사례 세부 정보{#use-case-details}

이 사용 사례에서는 템플릿 및 데이터(xml) 파일을 업로드할 수 있는 간단한 웹 인터페이스를 제공하려고 합니다. 파일 업로드가 완료되고 POST 요청이 AEM 서블릿으로 전송되면. 이 서블릿은 문서를 추출하고 OutputService의 generatePDFOutputBatch 메서드를 호출합니다. 생성된 pdf는 zip 파일로 압축되어 최종 사용자가 웹 브라우저에서 다운로드할 수 있도록 합니다.

## 서블릿 코드{#servlet-code}

다음은 서블릿의 코드 조각입니다. 코드는 요청에서 템플릿(xdp) 및 데이터 파일(xml)을 추출합니다. 템플릿 파일이 파일 시스템에 저장됩니다. 템플릿과 xml(data) 파일이 각각 들어 있는 templateMap과 dataFileMap이라는 두 개의 맵이 만들어집니다. 그런 다음 DocumentServices 서비스의 generateMultipleRecords 메서드를 호출하게 됩니다.

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

다음 코드는 OutputService의 generatePDFOutputBatch를 사용하여 여러 pdf를 생성하고 pdf 파일이 들어 있는 zip 파일을 호출 서블릿에 반환합니다

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

* [zip 파일 내용을 다운로드하여 파일 시스템에 추출합니다](assets/mult-records-template-and-xml-file.zip). 이 zip 파일에는 템플릿과 xml 데이터 파일이 포함되어 있습니다.
* [브라우저를 Felix 웹 콘솔로 보냅니다.](http://localhost:4502/system/console/bundles)
* [DevelopingWithServiceUser 번들을 배포합니다](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [OutputService API를 사용하여 pdf를 생성하는 사용자 지정 AEMFormsDocumentServices Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Custom 번들을 배포합니다
* [브라우저를 패키지 관리자로 보냅니다.](http://localhost:4502/crx/packmgr/index.jsp)
* [패키지를 가져오고 설치합니다](assets/generate-multiple-pdf-from-xml.zip). 이 패키지에는 템플릿 및 데이터 파일을 삭제할 수 있는 html 페이지가 들어 있습니다.
* [브라우저를 MultiRecords.html에 보냅니다.](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* 템플릿과 xml 데이터 파일을 함께 끌어다 놓습니다
* 만든 zip 파일을 다운로드합니다. 이 zip 파일에는 출력 서비스에서 생성한 pdf 파일이 포함되어 있습니다.

>[!NOTE]
>이 기능을 트리거하는 방법에는 여러 가지가 있습니다. 이 예제에서는 웹 인터페이스를 사용하여 템플릿과 데이터 파일을 삭제하고 기능을 보여 주었습니다.

