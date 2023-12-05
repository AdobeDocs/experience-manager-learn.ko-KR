---
title: AEM Forms을 사용하여 첫 번째 OSGi 서비스 만들기
description: AEM Forms을 사용하여 첫 번째 OSGi 서비스 구축
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
duration: 126
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# OSGi 서비스

OSGi 서비스는 이름/값 쌍으로 여러 서비스 속성과 함께 Java 클래스 또는 서비스 인터페이스입니다. 서비스 속성은 동일한 서비스 인터페이스를 갖는 서비스를 제공하는 상이한 서비스 제공자 사이에서 구별된다.

OSGi 서비스는 서비스 인터페이스에 의해 의미적으로 정의되고 서비스 객체로 구현된다. 서비스의 기능은 구현하는 인터페이스에 의해 정의됩니다. 따라서, 상이한 애플리케이션들이 동일한 서비스를 구현할 수 있다. 서비스 인터페이스를 사용하면 구현이 아니라 바인딩 인터페이스에 의해 번들이 상호 작용할 수 있습니다. 서비스 인터페이스는 가능한 한 적은 구현 세부 사항으로 지정해야 합니다.

## 인터페이스 정의

데이터를 와 병합하기 위한 한 가지 메서드를 사용하는 간단한 인터페이스 <span class="x x-first x-last">XDP</span> 템플릿.

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## 인터페이스 구현

이라는 새 패키지 만들기 `com.mysite.samples.impl` 인터페이스 구현을 유지할 수 있습니다.

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

주석 `@Component(...)` 10번 행에서 이 Java 클래스를 OSGi 구성 요소로 표시하고 OSGi 서비스로 등록합니다.

다음 `@Reference` 주석은 OSGi 선언 서비스의 일부이며 의 참조를 삽입하는 데 사용됩니다. [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 변수에 `outputService`.


## 번들 빌드 및 배포

* 열기 **명령 프롬프트 창**
* 다음으로 이동 `c:\aemformsbundles\mysite\core`
* 명령 실행 `mvn clean install -PautoInstallBundle`
* 위의 명령은 자동으로 번들을 빌드하고 localhost:4502에서 실행되는 AEM 인스턴스에 배포합니다.

다음 위치에서도 번들을 사용할 수 있습니다 `C:\AEMFormsBundles\mysite\core\target`. 번들은 다음을 사용하여 AEM에 배포할 수도 있습니다. [Felix 웹 콘솔.](http://localhost:4502/system/console/bundles)

## 서비스 사용

이제 JSP 페이지에서 서비스를 사용할 수 있습니다. 다음 코드 스니펫은 서비스에 대한 액세스 권한을 얻고 서비스에서 구현한 메서드를 사용하는 방법을 보여 줍니다

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

JSP 페이지를 포함하는 샘플 패키지는 다음과 같을 수 있습니다 [여기에서 다운로드됨](assets/learning_aem_forms.zip)

[전체 번들을 다운로드할 수 있습니다.](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## 패키지 테스트

를 사용하여 AEM에 패키지 가져오기 및 설치 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)

Postman을 사용하여 아래 스크린샷과 같이 POST 호출을 수행하고 입력 매개 변수를 제공합니다
![우체부](assets/test-service-postman.JPG)

## 다음 단계

[Sling 서블릿 만들기](./create-servlet.md)

