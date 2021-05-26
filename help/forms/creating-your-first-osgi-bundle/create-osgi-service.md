---
title: AEM Forms을 사용하여 첫 번째 OSGi 서비스 만들기
description: 'AEM Forms을 사용하여 첫 번째 OSGi 서비스 구축 '
feature: 적응형 양식
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: c74c6f5627e69e32bbf0098d6b6bab122cace798
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 2%

---


# OSGi 서비스

OSGi 서비스는 이름/값 쌍으로 많은 서비스 속성과 함께 Java 클래스 또는 서비스 인터페이스입니다. 서비스 속성은 동일한 서비스 인터페이스를 통해 서비스를 제공하는 서로 다른 서비스 공급자를 구분합니다.

OSGi 서비스는 서비스 인터페이스에 의해 의미적으로 정의되며 서비스 객체로 구현됩니다. 서비스 기능은 구현되는 인터페이스에 의해 정의됩니다. 이에 따라, 서로 다른 어플리케이션이 동일한 서비스를 구현할 수 있다. 서비스 인터페이스를 사용하면 구현이 아니라 바인딩 인터페이스를 통해 번들이 상호 작용할 수 있습니다. 서비스 인터페이스는 가능한 한 적은 수의 구현 세부 사항을 사용하여 지정해야 합니다.

## 인터페이스 정의

데이터를 <span class="x x-first x-last">XDP</span> 템플릿과 병합하는 방법이 하나인 간단한 인터페이스입니다.

```java
package com.learningaemforms.adobe.core;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface {
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
} 
```

## 인터페이스 구현

인터페이스의 구현을 보류하기 위해 `com.learningaemforms.adobe.core.impl` 이라는 새 패키지를 만듭니다.

```java
package com.learningaemforms.adobe.core.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.learningaemforms.adobe.core.MyfirstInterface;
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

10줄의 주석 `@Component(...)`은 이 Java 클래스를 OSGi 구성 요소로 표시하고 OSGi 서비스로 등록합니다.

`@Reference` 주석은 OSGi 선언적 서비스의 일부이며, [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)에 대한 참조를 변수 `outputService`에 주입하는 데 사용됩니다.


## 번들 빌드 및 배포

* **명령 프롬프트 창**&#x200B;을 엽니다.
* 다음으로 이동 `c:\aemformsbundles\learningaemforms\core`
* `mvn clean install -PautoInstallBundle` 명령을 실행합니다.
* 위의 명령은 localhost:4502에서 실행되는 AEM 인스턴스에 번들을 자동으로 빌드 및 배포합니다.

이 번들은 `C:\AEMFormsBundles\learningaemforms\core\target` 위치에서도 사용할 수 있습니다. 이 번들은 [Felix 웹 콘솔을 사용하여 AEM에 배포할 수도 있습니다.](http://localhost:4502/system/console/bundles)

## 서비스 사용

이제 JSP 페이지에서 서비스를 사용할 수 있습니다. 다음 코드 조각은 서비스에 대한 액세스 권한을 가져오고 서비스에 의해 구현된 메서드를 사용하는 방법을 보여 줍니다

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.learningaemforms.adobe.core.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

JSP 페이지가 포함된 샘플 패키지는 ![여기에서 다운로드할 수 있습니다.](assets/learning-aem-forms.zip)

## 패키지 테스트

[패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 패키지를 AEM에 가져와 설치합니다

postman을 사용하여 POST 호출을 만들고 아래 스크린샷에 표시된 대로 입력 매개 변수를 제공합니다
![postman](assets/test-service-postman.JPG)
