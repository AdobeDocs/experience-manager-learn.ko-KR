---
title: HTM5 양식 제출에서 PDF 생성
description: 모바일 양식 제출에서 PDF 생성
feature: Mobile Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
duration: 132
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# HTM5 양식 제출에서 PDF 생성 {#generate-pdf-from-htm-form-submission}

이 문서에서는 HTML5(즉, 모바일 Forms) 양식 제출에서 pdf를 생성하는 단계를 설명합니다. 이 데모에서는 HTML5 양식에 이미지를 추가하고 이미지를 최종 pdf로 병합하는 데 필요한 단계도 설명합니다.


제출된 데이터를 xdp 템플릿에 병합하려면 다음을 수행합니다

HTML5 양식 제출을 처리하는 서블릿 작성

* 이 서블릿 내에서 제출된 데이터를 가져옵니다.
* 이 데이터를 xdp 템플릿과 병합하여 pdf 생성
* PDF를 호출 응용 프로그램으로 다시 스트리밍

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

모바일 양식에 이미지를 추가하고 pdf에 해당 이미지를 표시하기 위해 다음을 사용했습니다

XDP 템플릿 - xdp 템플릿에 btnAddImage라는 이미지 필드와 단추를 추가했습니다. 다음 코드는 사용자 지정 프로필에서 btnAddImage의 클릭 이벤트를 처리합니다. 보시다시피 file1 클릭 이벤트를 트리거합니다. 이 사용 사례를 달성하기 위해 xdp에서는 코딩이 필요하지 않습니다

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[사용자 지정 프로필](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). 사용자 지정 프로필을 사용하면 모바일 양식의 HTML DOM 개체를 보다 쉽게 조작할 수 있습니다. 숨겨진 파일 요소가 HTML.jsp에 추가됩니다. 사용자가 &quot;사진 추가&quot;를 클릭하면 파일 요소의 클릭 이벤트가 트리거됩니다. 이를 통해 사용자는 첨부할 사진을 찾아보고 선택할 수 있다. 그런 다음 javascript FileReader 개체를 사용하여 이미지의 base64 인코딩 문자열을 가져옵니다. base64 이미지 문자열은 양식의 텍스트 필드에 저장됩니다. 양식이 제출되면 이 값을 추출하여 XML의 img 요소에 삽입합니다. 그런 다음 이 XML을 사용하여 xdp와 병합하여 최종 PDF를 생성합니다.

이 문서에 사용된 사용자 지정 프로필은 이 문서의 자산의 일부로 사용할 수 있습니다.

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

위의 코드는 파일 요소의 클릭 이벤트를 트리거할 때 실행됩니다. 5행에서는 업로드한 파일의 내용을 base64 문자열로 추출하여 텍스트 필드에 저장합니다. 그런 다음 양식이 서블릿에 제출되면 이 값이 추출됩니다.

그런 다음 AEM에서 모바일 양식의 다음 속성(고급)을 구성합니다

* 제출 URL - http://localhost:4502/bin/handlemobileformsubmission. 제출된 데이터를 xdp 템플릿과 병합하는 서블릿입니다
* HTML 렌더링 프로필 - &quot;AddImageToMobileForm&quot;을 선택해야 합니다. 이렇게 하면 양식에 이미지를 추가하는 코드가 트리거됩니다.

자체 서버에서 이 기능을 테스트하려면 다음 단계를 따르십시오.

* [AemFormsDocumentServices 번들 배포](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Service User 번들을 사용한 개발 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [이 문서와 연결된 패키지를 다운로드하여 설치하십시오.](assets/pdf-from-mobile-form-submission.zip)

* [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)의 속성 페이지를 보고 제출 URL 및 HTML 렌더링 프로필이 올바르게 설정되었는지 확인하십시오

* [XDP를 html로 미리 보기](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 양식에 이미지를 추가하고 제출합니다. 이미지가 포함된 PDF을 다시 가져와야 합니다.
