---
title: AEM Forms을 사용하여 첫 번째 OSGi 서비스 만들기
description: AEM Forms을 사용하여 첫 번째 OSGi 서비스 구축
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
duration: 87
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# OSGi 서비스

OSGi 서비스는 이름/값 쌍으로 여러 서비스 속성과 함께 Java 클래스 또는 서비스 인터페이스입니다. 서비스 속성은 동일한 서비스 인터페이스를 갖는 서비스를 제공하는 상이한 서비스 제공자 사이에서 구별된다.

OSGi 서비스는 서비스 인터페이스에 의해 의미적으로 정의되고 서비스 객체로 구현된다. 서비스의 기능은 구현하는 인터페이스에 의해 정의됩니다. 따라서, 상이한 애플리케이션들이 동일한 서비스를 구현할 수 있다. 서비스 인터페이스를 사용하면 구현이 아니라 바인딩 인터페이스에 의해 번들이 상호 작용할 수 있습니다. 서비스 인터페이스는 가능한 한 적은 구현 세부 사항으로 지정해야 합니다.

## 인터페이스 정의

데이터를 <span class="x x-first x-last">XDP</span> 템플릿과 병합하는 한 가지 메서드를 사용하는 간단한 인터페이스입니다.

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## 인터페이스 구현

인터페이스 구현을 보유할 새 패키지 `com.mysite.samples.impl`을(를) 만듭니다.

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

10행의 `@Component(...)` 주석은 이 Java 클래스를 OSGi 구성 요소로 표시하고 OSGi 서비스로 등록합니다.

`@Reference` 주석은 OSGi 선언 서비스의 일부이며 [Outputservice](https://helpx.adobe.com/kr/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)의 참조를 `outputService` 변수에 삽입하는 데 사용됩니다.


## 번들 빌드 및 배포

* **명령 프롬프트 창** 열기
* `c:\aemformsbundles\mysite\core`(으)로 이동
* `mvn clean install -PautoInstallBundle` 명령 실행
* 위의 명령은 자동으로 번들을 빌드하고 localhost:4502에서 실행되는 AEM 인스턴스에 배포합니다.

다음 위치 `C:\AEMFormsBundles\mysite\core\target`에서도 번들을 사용할 수 있습니다. [Felix 웹 콘솔을 사용하여 AEM에 번들을 배포할 수도 있습니다.](http://localhost:4502/system/console/bundles)

## 서비스 사용

이제 JSP 페이지에서 서비스를 사용할 수 있습니다. 다음 코드 스니펫은 서비스에 대한 액세스 권한을 얻고 서비스에서 구현한 메서드를 사용하는 방법을 보여 줍니다

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

JSP 페이지가 포함된 샘플 패키지는 [여기에서 다운로드](assets/learning_aem_forms.zip)할 수 있습니다.

[전체 번들을 다운로드할 수 있습니다.](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## 패키지 테스트

[패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 AEM으로 패키지를 가져와 설치합니다.

Postman을 사용하여 POST 호출을 수행하고 아래 스크린샷에 표시된 대로 입력 매개 변수를 제공합니다
![postman](assets/test-service-postman.JPG)

## 다음 단계

[Sling 서블릿 만들기](./create-servlet.md)

