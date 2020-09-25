---
title: HTM5 양식 제출에서 PDF 생성
seo-title: HTML5 양식 제출에서 PDF 생성
description: 모바일 양식 제출 시 PDF 생성
seo-description: 모바일 양식 제출 시 PDF 생성
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# HTM5 양식 제출에서 PDF 생성 {#generate-pdf-from-htm-form-submission}

이 문서에서는 HTML5(일명 모바일 Forms) 양식 제출 시 pdf를 생성하는 데 필요한 단계를 안내합니다. 이 데모에서는 HTML5 양식에 이미지를 추가하고 이미지를 최종 pdf로 병합하는 데 필요한 단계도 설명합니다.

이 기능의 라이브 데모를 보려면 [샘플 서버를](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 방문하여 &quot;Mobile Form To PDF&quot;를 검색하십시오.

제출된 데이터를 xdp 템플릿에 병합하려면 다음을 수행합니다

HTML5 양식 제출을 처리할 서블릿 작성

* 이 서블릿 내에서 제출된 데이터를 확보합니다.
* 이 데이터를 xdp 템플릿과 병합하여 pdf 생성
* 호출 응용 프로그램으로 PDF 다시 스트리밍

다음은 요청에서 제출된 데이터를 추출하는 서블릿 코드입니다. 그런 다음 사용자 정의 documentServices .mobileFormToPDF 메서드를 호출하여 pdf를 가져옵니다.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
   response.setContentType("application/pdf");
   response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
   response.setContentLength((int) fileInputStream.available());
   OutputStream responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    responseOutputStream.write(bytes);
   }
   responseOutputStream.flush();
   responseOutputStream.close();

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

모바일 양식에 이미지를 추가하고 이 이미지를 pdf에 표시하려면 다음을 사용했습니다.

XDP 템플릿 - xdp 템플릿에서 btnAddImage라는 이미지 필드와 단추를 추가했습니다. 다음 코드는 사용자 지정 프로필에서 btnAddImage의 클릭 이벤트를 처리합니다. 보시다시피 file1 클릭 이벤트를 트리거합니다. xdp에서 이러한 사용 사례를 처리하는 데 코딩 작업이 필요하지 않습니다.

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[사용자 지정 프로필](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). 사용자 정의 프로파일을 사용하면 모바일 양식의 HTML DOM 개체를 보다 손쉽게 조작할 수 있습니다. 숨겨진 파일 요소가 HTML.jsp에 추가됩니다. 사용자가 &quot;사진 추가&quot;를 클릭하면 파일 요소의 클릭 이벤트가 트리거됩니다. 따라서 사용자가 첨부할 사진을 찾아 선택할 수 있습니다. 그런 다음 javascript FileReader 개체를 사용하여 이미지의 base64 인코딩 문자열을 가져옵니다. base64 이미지 문자열은 양식의 텍스트 필드에 저장됩니다. 양식이 제출되면 이 값을 추출하여 XML의 img 요소에 삽입합니다. 이 XML은 xdp와 병합하여 최종 pdf를 생성하는 데 사용됩니다.

이 아티클에 사용된 사용자 지정 프로필을 이 아티클 에셋의 일부로 사용할 수 있게 되었습니다.

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

위의 코드는 파일 요소의 클릭 이벤트를 트리거할 때 실행됩니다. 5행 업로드한 파일의 컨텐츠를 base64 문자열로 추출하여 텍스트 필드에 저장합니다. 그런 다음 양식이 Adobe 서블릿에 제출되면 이 값이 추출됩니다.

그런 다음 AEM에서 모바일 양식의 다음 속성(고급)을 구성합니다

* 제출 URL - http://localhost:4502/bin/handlemobileformsubmission. 전송된 데이터를 xdp 템플릿과 병합하는 Adobe 서블릿입니다.
* HTML 렌더링 프로필 - &quot;AddImageToMobileForm&quot;을 선택해야 합니다. 그러면 코드에 이미지가 추가되도록 트리거됩니다.

자체 서버에서 이 기능을 테스트하려면 다음 단계를 수행하십시오.

* [AemFormsDocumentServices 번들 배포](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [서비스 사용자 번들로 개발](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [이 아티클과 관련된 패키지를 다운로드하고 설치합니다.](assets/pdf-from-mobile-form-submission.zip)

* xdp의 속성 페이지를 보고 제출 URL 및 HTML 렌더링 프로필이 올바르게 설정되었는지 [확인합니다](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [XDP를 html로 미리 보기](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 양식에 이미지를 추가하고 제출합니다. 이미지를 사용하여 PDF를 다시 제출해야 합니다.

