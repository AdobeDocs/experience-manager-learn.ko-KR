---
title: HTL에서 Sling 모델 매개 변수화
description: HTL의 매개 변수를 AEM의 Sling 모델로 전달하는 방법을 알아봅니다.
version: Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
source-git-commit: f45d04b489771b9d3613cb4ce5e7b94d531ac783
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---


# HTL에서 Sling 모델 매개 변수화

Adobe Experience Manager(AEM)는 동적 및 적응식 웹 애플리케이션을 구축하기 위한 강력한 프레임워크를 제공합니다. Sling 모델을 매개 변수화하여 유연성과 재사용성을 향상시키는 강력한 기능 중 하나입니다. 이 튜토리얼에서는 매개 변수가 있는 Sling 모델을 만들고 HTL(HTML 템플릿 언어)에서 사용하여 다이내믹 콘텐츠를 렌더링하는 방법을 안내합니다.

## HTL 스크립트

이 예에서 HTL 스크립트는 `ParameterizedModel` Sling 모델에 두 개의 매개 변수를 보냅니다. 이 모델은 `getValue()` 메서드에서 이러한 매개 변수를 조작하고 표시할 결과를 반환합니다.

이 예제에서는 두 개의 String 매개 변수를 전달하지만, `@RequestAttribute`](#sling-model-implementation)(으)로 주석을 단 [Sling 모델 필드 형식이 HTL에서 전달된 개체 또는 값의 형식과 일치하는 경우 어떤 형식이든 Sling 모델에 전달할 수 있습니다.

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **매개 변수화:** `data-sly-use` 문은 `myParameterOne` 및 `myParameterTwo`을(를) 사용하여 `ParameterizedModel`의 인스턴스를 만듭니다.
- **조건부 렌더링:** `data-sly-test`은(는) 콘텐츠를 표시하기 전에 모델이 준비되었는지 확인합니다.
- **자리 표시자 호출:** `placeholderTemplate`은(는) 모델이 준비되지 않은 경우를 처리합니다.

## Sling 모델 구현

다음은 Sling 모델을 구현하는 방법입니다.

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **모델 주석:** `@Model` 주석은 이 클래스를 `SlingHttpServletRequest`에서 조정할 수 있고 `ParameterizedModel` 인터페이스를 구현하는 슬링 모델로 지정합니다.
- **요청 특성:** `@RequestAttribute` 주석이 모델에 HTL 매개 변수를 삽입합니다.
- **메서드:** `getValue()`은(는) 매개 변수를 연결하고 `isReady()`은(는) 매개 변수가 비어 있지 않은지 확인합니다.

`ParameterizedModel` 인터페이스는 다음과 같이 정의됩니다.

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## 출력 예

`"Hello"` 및 `"World"` 매개 변수를 사용하여 HTL 스크립트는 다음 출력을 생성합니다.

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

이는 AEM의 매개 변수가 있는 Sling 모델이 HTL을 통해 제공되는 입력 매개 변수를 기반으로 영향을 받는 방법을 보여 줍니다.

