---
title: HTML5 양식 제출에서 PDF 생성
description: 모바일 양식 제출에서 PDF 생성
feature: Mobile Forms
version: 6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%

---


# HTML5 양식 제출에서 PDF 생성 {#generate-pdf-from-htm-form-submission}

이 문서에서는 HTML5(모바일 Forms라고도 함) 양식 제출에서 pdf를 생성하는 단계를 안내합니다. 이 데모에서는 HTML5 양식에 이미지를 추가하고 이미지를 최종 pdf에 병합하는 데 필요한 단계도 설명합니다.

이 기능의 라이브 데모를 보려면 [샘플 서버](https://forms.enablementadobe.com/content/samples/samples.html?query=0)를 방문하여 &quot;Mobile Form To PDF&quot;를 검색하십시오.

제출된 데이터를 xdp 템플릿에 병합하기 위해 다음을 수행합니다

HTML5 양식 제출을 처리할 서블릿 쓰기

* 이 서블릿 내에 제출된 데이터가 포함됩니다
* 이 데이터를 xdp 템플릿과 병합하여 pdf 생성
* PDF를 호출 응용 프로그램으로 다시 스트리밍합니다

다음은 요청에서 제출된 데이터를 추출하는 서블릿 코드입니다. 그런 다음 사용자 지정 documentServices .mobileFormToPDF 메서드를 호출하여 pdf를 가져옵니다.

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

모바일 양식에 이미지를 추가하고 해당 이미지를 pdf에 표시하려면 다음을 사용했습니다

XDP 템플릿 - xdp 템플릿에 btnAddImage라는 이미지 필드와 단추를 추가했습니다. 다음 코드는 사용자 지정 프로필에서 btnAddImage의 클릭 이벤트를 처리합니다. 알 수 있듯이 file1 클릭 이벤트를 트리거합니다. 이 사용 사례를 수행하기 위해 xdp에는 코딩이 필요하지 않습니다

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[사용자 지정 프로필](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). 사용자 지정 프로필을 사용하면 모바일 양식의 HTML DOM 개체를 보다 쉽게 조작할 수 있습니다. 숨겨진 파일 요소가 HTML.jsp에 추가됩니다. 사용자가 &quot;사진 추가&quot;를 클릭하면 파일 요소의 클릭 이벤트가 트리거됩니다. 이렇게 하면 사용자가 첨부할 사진을 찾아 선택할 수 있습니다. 그런 다음 javascript FileReader 개체를 사용하여 이미지의 base64 인코딩 문자열을 가져옵니다. base64 이미지 문자열은 양식의 텍스트 필드에 저장됩니다. 양식이 제출되면 이 값을 추출하여 XML의 img 요소에 삽입합니다. 그런 다음 이 XML을 xdp와 병합하여 최종 pdf를 생성하는 데 사용합니다.

이 문서에 사용된 사용자 지정 프로필을 이 문서의 자산의 일부로 사용할 수 있도록 했습니다.

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

위의 코드는 파일 요소의 클릭 이벤트를 트리거할 때 실행됩니다. 5줄에서는 업로드된 파일의 컨텐츠를 base64 문자열로 추출하여 텍스트 필드에 저장합니다. 그런 다음 양식이 Adobe 서블릿에 제출되면 이 값이 추출됩니다.

그런 다음 AEM에서 모바일 양식의 다음 속성(고급)을 구성합니다

* 제출 URL - http://localhost:4502/bin/handlemobileformsubmission. 제출된 데이터를 xdp 템플릿과 병합하는 서블릿입니다
* HTML 렌더링 프로필 - &quot;AddImageToMobileForm&quot;을 선택해야 합니다. 그러면 양식에 이미지를 추가하기 위한 코드가 트리거됩니다.

자체 서버에서 이 기능을 테스트하려면 다음 단계를 수행하십시오.

* [AemFormsDocumentServices 번들 배포](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [서비스 사용 번들로 개발 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [이 문서와 연결된 패키지를 다운로드하여 설치합니다.](assets/pdf-from-mobile-form-submission.zip)

* [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)의 속성 페이지를 보고 제출 URL 및 HTML Render 프로필이 올바르게 설정되었는지 확인하십시오

* [XDP를 html로 미리 보기](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 양식에 이미지를 추가하고 제출합니다. 이미지가 포함된 PDF를 다시 가져와야 합니다.

