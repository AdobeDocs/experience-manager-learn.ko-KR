---
title: 단위 테스트
description: 이 자습서에서는 사용자 지정 구성 요소 자습서에서 만든 타임라인 구성 요소의 Sling 모델의 동작을 확인하는 단위 테스트 구현에 대해 설명합니다.
sub-product: 사이트
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: API, AEM 프로젝트 원형
topic: 컨텐츠 관리, 개발
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3013'
ht-degree: 0%

---


# 단위 테스트 {#unit-testing}

이 자습서에서는 [사용자 지정 구성 요소](./custom-component.md) 자습서에서 만든 라인 구성 요소의 Sling 모델의 동작을 확인하는 단위 테스트 구현을 다룹니다.

## 전제 조건 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

_시스템에 Java 8과 Java 11이 모두 설치되어 있는 경우 VS 코드 테스트 실행자는 테스트를 실행할 때 낮은 Java 런타임을 선택하여 테스트 실패를 초래할 수 있습니다. 이 경우 Java 8._ 제거

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 다시 사용하고 시작 프로젝트를 체크 아웃하는 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 라인 코드를 확인합니다.

1. [GitHub](https://github.com/adobe/aem-guides-wknd)에서 `tutorial/unit-testing-start` 분기를 확인하십시오

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Maven 기술을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start)에서 완료된 코드를 보거나 분기 `tutorial/unit-testing-start`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## 목표

1. 단위 테스트의 기본 사항을 이해합니다.
1. AEM 코드를 테스트하는 데 일반적으로 사용되는 프레임워크 및 도구에 대해 알아봅니다.
1. 단위 테스트를 작성할 때 AEM 리소스를 비웃거나 시뮬레이션하는 옵션을 이해합니다.

## 배경 {#unit-testing-background}

이 자습서에서는 타임라인 구성 요소의 [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)([사용자 지정 AEM 구성 요소 만들기](custom-component.md)에서 작성)에 대해 [단위 테스트](https://en.wikipedia.org/wiki/Unit_testing)를 작성하는 방법을 알아봅니다. 단위 테스트는 Java 코드의 예상 동작을 확인하는 Java로 작성된 빌드 시간 테스트입니다. 각 단위 테스트는 일반적으로 작으며, 예상 결과와 비교하여 메서드(또는 작업 단위)의 출력을 검증합니다.

AEM 우수 사례를 사용하며, 다음과 같이 사용합니다.

* [주닛트 5](https://junit.org/junit5/)
* [Mockito 테스트 프레임워크](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/) ( [Apache Sling Android를 기반으로 구축](https://sling.apache.org/documentation/development/sling-mock.html))

## Cloud Manager 단위 테스트 및 Adobe {#unit-testing-and-adobe-cloud-manager}

[Adobe 클라우드 ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=ko-KR) 관리자단위 테스트 실행 및  [코드 검사 보고를 ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) 해당 CI/CD 파이프라인에 통합하여 AEM 코드의 모범 사례를 권장하고 프로모션합니다.

장치 테스트 코드는 모든 코드 베이스에 권장되지만 Cloud Manager를 사용할 때는 Cloud Manager를 실행할 단위 테스트를 제공하여 코드 품질 테스트 및 보고 기능을 활용하는 것이 중요합니다.

## Inspect 테스트 Maven 종속성 {#inspect-the-test-maven-dependencies}

첫 번째 단계는 테스트 작성 및 실행을 지원하기 위해 Maven 종속성을 검사하는 것입니다. 필요한 종속성은 4개입니다.

1. JUnit5
1. Mockito 테스트 프레임워크
1. Apache Sling이 Android
1. AEM Android Test Framework (io.wcm)

**JUnit5**, **Mockito** 및 **AEM Android** 테스트 종속성은 설정 중에 [AEM Maven Archetype](project-setup.md)을 사용하여 프로젝트에 자동으로 추가됩니다.

1. 이러한 종속성을 보려면 **aem-guides-wknd/pom.xml**&#x200B;에서 부모 반응기 POM을 열고 `<dependencies>..</dependencies>`(으)로 이동하여 다음 종속성이 정의되어 있는지 확인하십시오.

   ```xml
   <dependencies>
       ...       
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.6.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>3.0.2</version>
           <scope>test</scope>
       </dependency>        
       ...
   </dependencies>
   ```

1. **aem-guides-wknd/core/pom.xml**&#x200B;을 열고 해당 테스트 종속성을 사용할 수 있는지 확인합니다.

   ```xml
   ...
   <!-- Testing -->
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <exclusions>
           <exclusion>
               <groupId>org.apache.sling</groupId>
               <artifactId>org.apache.sling.models.impl</artifactId>
           </exclusion>
           <exclusion>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-simple</artifactId>
           </exclusion>
       </exclusions>
       <scope>test</scope>
   </dependency>
   <!-- Required to be able to support injection with @Self and @Via -->
   <dependency>
       <groupId>org.apache.sling</groupId>
       <artifactId>org.apache.sling.models.impl</artifactId>
       <version>1.4.4</version>
       <scope>test</scope>
   </dependency>
   ...
   ```

   **코어** 프로젝트의 병렬 소스 폴더에는 단위 테스트와 지원 테스트 파일이 포함됩니다. 이 **test** 폴더는 소스 코드로부터 테스트 클래스를 분리하지만, 테스트가 소스 코드와 동일한 패키지에 있는 것처럼 작동할 수 있도록 합니다.

## 주 테스트 만들기 {#creating-the-junit-test}

단위 테스트는 일반적으로 Java 클래스에 1에서 1로 매핑됩니다. 이 장에서는 Byline 구성 요소를 지원하는 Sling 모델인 **BylineImpl.java**&#x200B;에 대한 JUnit 테스트를 작성합니다.

![단위 테스트 src 폴더](assets/unit-testing/core-src-test-folder.png)

*단위 테스트가 저장되는 위치입니다.*

1. 테스트할 Java 클래스의 위치를 미러링하는 Java 패키지 폴더 구조에서 `src/test/java` 아래에 새 Java 클래스를 만들어 `BylineImpl.java`에 대한 단위 테스트를 만듭니다.

   ![새 BylineImplTest.java 파일 만들기](assets/unit-testing/new-bylineimpltest.png)

   테스트 중이라

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   에서 해당 단위 테스트 Java 클래스를 생성합니다.

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   단위 테스트 파일 `BylineImplTest.java`의 `Test` 접미사는 우리가 사용할 수 있는 규칙입니다

   1. 테스트 파일 _for_ `BylineImpl.java`로 쉽게 식별합니다
   1. 그러나 테스트 파일 _을 테스트 중인 클래스_&#x200B;와 구별하십시오. `BylineImpl.java`



## BylineImplTest.java 검토 {#reviewing-bylineimpltest-java}

이때 JUnit 테스트 파일은 빈 Java 클래스입니다. 파일을 다음 코드로 업데이트합니다.

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class BylineImplTest {

    @BeforeEach
    void setUp() throws Exception {

    }

    @Test 
    void testGetName() { 
        fail("Not yet implemented");
    }
    
    @Test 
    void testGetOccupations() { 
        fail("Not yet implemented");
    }

    @Test 
    void testIsEmpty() { 
        fail("Not yet implemented");
    }
}
```

1. 첫 번째 메서드 `public void setUp() { .. }`은 JUnit의 `@BeforeEach`로 주석을 달며, 이 메서드는 JUnit 테스트 러너에게 이 메서드를 실행하도록 지시하고 이 클래스에서 각 테스트 메서드를 실행합니다. 이렇게 하면 모든 테스트에 필요한 일반적인 테스트 상태를 초기화하기 편리한 위치를 제공합니다.

2. 그 다음 메서드는 이름 앞에 `test`이 붙고 `@Test` 주석이 표시되어 있는 테스트 메서드입니다. 기본적으로 모든 테스트는 아직 구현되지 않았으므로 실패로 설정됩니다.

   먼저 테스트하고 있는 클래스의 각 공용 메서드에 대해 하나의 테스트 방법으로 시작합니다.

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 에 의해 테스트됨 | testGetName() |
   | getTowals() | 에 의해 테스트됨 | testGetTowals() |
   | isEmpty() | 에 의해 테스트됨 | testIsEmpty() |

   이 장의 뒷부분에서 확인할 수 있듯이 필요에 따라 이러한 메서드를 확장할 수 있습니다.

   이 JUnit 테스트 클래스(JUnit 테스트 케이스라고도 함)를 실행하면 `@Test`으로 표시된 각 메서드가 통과 또는 실패할 수 있는 테스트로 실행됩니다.

![생성된 BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. `BylineImplTest.java` 파일을 마우스 오른쪽 단추로 클릭하고 **실행**을 탭하여 JUnit 테스트 케이스를 실행합니다.
예상대로, 모든 테스트가 아직 구현되지 않았기 때문에 실패합니다.

   ![junit 테스트로 실행](assets/unit-testing/run-junit-tests.png)

   *BylineImplTests.java > Run을 마우스 오른쪽 단추로 클릭합니다.*

## BylineImpl.java 검토 {#reviewing-bylineimpl-java}

단위 테스트를 작성할 때 두 가지 주요 접근 방식이 있습니다.

* [구현이 개발되기](https://en.wikipedia.org/wiki/Test-driven_development) 바로 전에 장치 테스트를 점진적으로 기록하는 TDD 또는 테스트 기반 개발 테스트를 작성하고 구현을 작성하여 테스트를 통과합니다.
* 먼저 작업 코드를 개발하고 해당 코드의 유효성을 검사하는 테스트를 작성하는 작업이 포함된 구현 첫 번째 개발.

이 자습서에서는 작업 **BylineImpl.java**&#x200B;을 이미 이전 장에서 만들었으므로 후자의 접근 방식이 사용됩니다. 따라서 공개 메서드의 동작을 검토하고 이해해야 하며, 구현 세부 사항 중 일부를 고려해야 합니다. 이는 반대의 소리일 수 있습니다. 좋은 테스트는 입력 및 출력만 신경써야 하므로 하지만 AEM에서 작업하는 경우 작업 테스트를 생성하기 위해 알고 있어야 하는 다양한 구현 고려 사항이 있습니다.

AEM의 컨텍스트에서 TDD는 전문 지식이 필요하며 AEM 코드의 AEM 개발 및 장치 테스트에 능숙한 AEM 개발자가 가장 잘 채택합니다.

## AEM 테스트 컨텍스트 설정  {#setting-up-aem-test-context}

AEM에 대해 작성된 대부분의 코드는 JCR, Sling 또는 AEM API를 사용하며, 이 경우 실행 중인 AEM의 컨텍스트가 제대로 실행되어야 합니다.

단위 테스트는 실행 중인 AEM 인스턴스의 컨텍스트 외부에 있는 빌드에서 실행되므로 그러한 컨텍스트가 없습니다. 이 작업을 용이하게 하기 위해 [wcm.io의 AEM Android](https://wcm.io/testing/aem-mock/usage.html)에서는 이러한 API가 _대부분_&#x200B;에서 실행 중인 것처럼 작동할 수 있는 모의 컨텍스트를 만듭니다.

1. **BylineImplTest.java**&#x200B;에서 **wcm.io&#39;s** `AemContext`를 사용하여 AEM 컨텍스트를 **BylineImplTest.java** 파일에 `@ExtendWith`로 데코레이팅된 JUnit 확장자로 추가하여 만듭니다. 확장은 필요한 모든 초기화 및 정리 작업을 처리합니다. 모든 테스트 메서드에 사용할 수 있는 `AemContext`에 대한 클래스 변수를 만듭니다.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   이 변수 `ctx`은 많은 AEM 및 Sling 추상을 제공하는 mock AEM 컨텍스트를 표시합니다.

   * BylineImpl Sling 모델이 이 컨텍스트에 등록됩니다
   * Mock JCR 컨텐츠 구조가 이 컨텍스트에서 생성됩니다
   * 사용자 지정 OSGi 서비스는 이 컨텍스트에서 등록할 수 있습니다
   * SlingHttpServletRequest 개체, ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag 등과 같은 다양한 Mock Sling 및 AEM OSGi 서비스 등 일반적인 필수 Mock 객체 및 Help를 제공합니다.
      * *이러한 개체의 모든 메서드가 구현되지는 않습니다!*
   * 그리고 [훨씬 더 많은](https://wcm.io/testing/aem-mock/usage.html)!

   **`ctx`** 개체는 대부분의 mock 컨텍스트에 대한 시작 지점으로 사용됩니다.

1. 각 `@Test` 메서드 전에 실행되는 `setUp(..)` 메서드에서 일반적인 모의 테스트 상태를 정의합니다.

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 테스트할 Sling 모델을 mock AEM 컨텍스트에 등록하여 메서드에서 인스턴스화할 수  `@Test` 있습니다.
   * **`load().json`** 리소스 구조를 샘플 컨텍스트에 로드하여 코드가 실제 저장소에서 제공한 것처럼 리소스와 상호 작용할 수 있습니다. **`BylineImplTest.json`** 파일의 리소스 정의는 **/content** 아래의 mock JCR 컨텍스트에 로드됩니다.
   * **`BylineImplTest.json`** 아직 존재하지 않으므로, 만들고 테스트에 필요한 JCR 리소스 구조를 정의하겠습니다.

1. 샘플 리소스 구조를 나타내는 JSON 파일은 JUnit Java 테스트 파일과 같은 패키지 경로 지정 다음에 **core/src/test/resources** 아래에 저장됩니다.

   다음 컨텐츠가 있는 **core/test/resources/com/adobe/aem/guides/wknd/core/models/impl**&#x200B;에 BylineImplTest.json **라는 새 JSON 파일을 만듭니다.**

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   이 JSON은 라인 구성 요소 단위 테스트에 대한 샘플 리소스(JCR 노드)를 정의합니다. 이 시점에서 JSON에는 타임라인 구성 요소 컨텐츠 리소스, `jcr:primaryType` 및 `sling:resourceType`을 나타내는 데 필요한 최소 속성 세트가 있습니다.

   단위 테스트를 사용할 때 일반적인 규칙은 각 테스트를 충족하는 데 필요한 최소한의 샘플 컨텐츠, 컨텍스트 및 코드 세트를 만드는 것입니다. 종종 불필요한 가공물이 발생하므로 테스트를 작성하기 전에 전체 샘플 컨텍스트를 작성하려는 유혹을 피하십시오.

   이제 **BylineImplTest.json**&#x200B;이 있으므로 `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`가 실행되면 샘플 리소스 정의가 **/content.NET** 경로의 컨텍스트에 로드됩니다.

## getName() 테스트 {#testing-get-name}

이제 기본 mock 컨텍스트 설정이 있으므로 **BylineImpl의 getName()**&#x200B;에 대한 첫 번째 테스트를 작성하겠습니다. 이 테스트에서는 **getName()** 메서드가 리소스의 &quot;**name&quot;** 속성에 저장된 올바른 작성 이름을 반환하는지 확인해야 합니다.

1. **BylineImplTest.java**&#x200B;에서 **testGetName**() 메서드를 다음과 같이 업데이트합니다.

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** 예상 값을 설정합니다. 이를 &quot;**Jane Done**&quot;로 설정합니다.
   * **`ctx.currentResource`** mock 리소스의 컨텍스트를 설정하여 코드를 평가합니다. 그러면  **/content/bylineas로 설정되며, 이** 는 mock byline content 리소스가 로드되는 위치입니다.
   * **`Byline byline`** mock Request 개체에서 Sling 모델을 적용하여 인스턴스화합니다.
   * **`String actual`** 는 Byline Sling Model 개체 `getName()`에서 테스트하고 있는 메서드를 호출합니다.
   * **`assertEquals`** 필요한 값을 어설션하면 byline Sling Model 개체가 반환한 값과 일치합니다. 이러한 값이 같으면 테스트가 실패합니다.

1. 테스트를 실행하면 `NullPointerException`으로 실패합니다.

   샘플 JSON에서 `name` 속성을 정의하지 않았기 때문에 이 테스트가 실패하지 않으며, 이로 인해 테스트가 실패하지만 테스트 실행이 해당 시점에 도달하지 않았습니다! byline 개체 자체의 `NullPointerException` 때문에 이 테스트가 실패합니다.

1. `BylineImpl.java`에서 `@PostConstruct init()`에 예외가 발생하면 Sling Model이 인스턴스화할 수 없으며 Sling Model 개체가 null이 됩니다.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   ModelFactory OSGi 서비스가 `AemContext`(Apache Sling Context의 방식)을 통해 제공되지만 BylineImpl의 `init()` 메서드에서 호출되는 `getModelFromWrappedRequest(...)` 를 포함하여 모든 메서드가 구현되지 않는 것으로 나타났습니다. 이로 인해 [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)이(가) 발생합니다. 이 경우 `init()`이(가) 실패하고 `ctx.request().adaptTo(Byline.class)`이(가) null 개체가 됩니다.

   제공된 개체는 코드를 수용할 수 없으므로 직접 샘플 컨텍스트를 구현해야 합니다. 이를 위해 Mockie를 사용하여 `getModelFromWrappedRequest(...)`이 호출될 때 mock Image 개체를 반환하는 mockFactory 개체를 만들 수 있습니다.

   Byline Sling Model을 인스턴스화하려면 이 샘플 컨텍스트가 있어야 하므로 `@Before setUp()` 메서드에 추가할 수 있습니다. 또한 **BylineImplTest** 클래스 위에 `@ExtendWith` 주석을 추가해야 합니다.`MockitoExtension.class`

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 클래스 수준에서 샘플 객체를 정의하기 위해 @Mock 주석을  [사용할 수 ](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) 있는 Mockito JUnit Jupiter Extensioning을 사용하여 실행할 테스트 사례 클래스를 표시합니다.
   * **`@Mock private Image`** 유형의 mock 개체를 만듭니다  `com.adobe.cq.wcm.core.components.models.Image`. 이는 필요한 경우 `@Test` 메서드가 필요에 따라 동작을 변경할 수 있도록 클래스 수준에서 정의됩니다.
   * **`@Mock private ModelFactory`** ModelFactory 유형의 mock 개체를 만듭니다. 이것은 순수한 Mockito 조롱이며 여기에 구현된 방법이 없습니다. 이는 클래스 수준에서 정의되므로 필요한 경우 `@Test`메서드가 필요에 따라 동작을 변경할 수 있습니다.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** mock ModelFactory 개체 `getModelFromWrappedRequest(..)` 에서 이 호출될 때 mock 동작을 등록합니다. `thenReturn (..)`에 정의된 결과는 샘플 이미지 개체를 반환하는 것입니다. 이 동작은 다음 경우에만 호출됩니다. 첫 번째 매개 변수는 `ctx` 의 요청 개체와 동일하고, 두 번째 매개 변수는 모든 리소스 개체이며, 세 번째 매개 변수는 코어 구성 요소 이미지 클래스여야 합니다. Adobe에서는 테스트 기간 동안 `ctx.currentResource(...)`을 **BylineImplTest.json**&#x200B;에 정의된 다양한 샘플 리소스로 설정할 예정이므로 모든 리소스를 허용합니다. 나중에 ModelFactory의 이 동작을 재정의하려고 하므로 **enliant()** 엄격함을 추가합니다.
   * **`ctx.registerService(..)`.** mock ModelFactory 개체를 Aem Context에 등록하고 가장 높은 서비스 등급을 사용합니다. BylineImpl의 `init()`에 사용된 ModelFactory가 `@OSGiService ModelFactory model` 필드를 통해 삽입되므로 이 작업이 필요합니다. AemContext에서 **adobe** 샘플 개체를 주입하여 `getModelFromWrappedRequest(..)` 호출을 처리하려면 해당 유형의 가장 높은 등급 서비스(ModelFactory)로 등록해야 합니다.

1. 테스트를 다시 실행하면 다시 실패하지만, 이번에는 메시지가 실패한 이유를 확인합니다.

   ![테스트 이름 실패 검증](assets/unit-testing/testgetname-failure-assertion.png)

   *어설션으로 인해 testGetName() 오류가 발생했습니다.*

   테스트의 어설션 조건을 의미하는 **AssertionError**&#x200B;이(가) 수신되고, **예상 값은 &quot;Jane Doe&quot;**&#x200B;이지만 **실제 값은 null**&#x200B;임을 알려줍니다. 이는 &quot;**name&quot;** 속성이 **BylineImplTest.json**&#x200B;의 mock **/content/byline** 리소스 정의에 추가되지 않았으므로 다음과 같이 추가하겠습니다.

1. **BylineImplTest.json**&#x200B;을 업데이트하여 `"name": "Jane Doe".` 정의

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 테스트를 다시 실행하면 **`testGetName()`**&#x200B;이(가) 전달됩니다!

   ![테스트 이름 전달](assets/unit-testing/testgetname-pass.png)


## getTowals() 테스트 {#testing-get-occupations}

좋아! 우리의 첫 번째 시험이 합격했어! 계속 진행하여 `getOccupations()`을 테스트해 보겠습니다. Mock 컨텍스트의 초기화가 `@Before setUp()`메서드에서 수행되었으므로 `getOccupations()`를 포함하여 이 테스트 케이스의 모든 `@Test` 메서드에 이 메서드를 사용할 수 있습니다.

이 방법은 직업 등록 정보에 저장된 알파벳순으로 정렬된 직업 목록(내림차순)을 반환해야 합니다.

1. 다음과 같이 **`testGetOccupations()`** 업데이트:

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** 예상 결과를 정의합니다.
   * **`ctx.currentResource`** 컨텍스트를 /content/byline의 샘플 리소스 정의에 대해 평가하도록 현재 리소스를 설정합니다. 이렇게 하면 **BylineImpl.java**&#x200B;가 mock 리소스의 컨텍스트에서 실행됩니다.
   * **`ctx.request().adaptTo(Byline.class)`** mock Request 개체에서 Sling 모델을 적용하여 인스턴스화합니다.
   * **`byline.getOccupations()`** 는 Byline Sling Model 개체 `getOccupations()`에서 테스트하고 있는 메서드를 호출합니다.
   * **`assertEquals(expected, actual)`** 예측된 목록에 대해 어설션 목록이 실제 목록과 동일합니다.

1. 위의 **`getName()`**&#x200B;과 마찬가지로 **BylineImplTest.json**&#x200B;도 직업을 정의하지 않으므로 `byline.getOccupations()`이 빈 목록을 반환하므로 이 테스트를 실행하면 실패합니다.

   **BylineImplTest.json**&#x200B;을 업데이트하여 직업 목록을 포함하십시오. 이 목록은 순서가 사전순으로 설정되어 있고, 테스트 시 해당 직업이 **`getOccupations()`**&#x200B;별로 알파벳순으로 정렬되어 있는지 확인합니다.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. 테스트를 실행하면 다시 통과합니다! 정렬된 직업들이 잘 되는 것 같아!

   ![직업 통과 받기](assets/unit-testing/testgetoccupations-pass.png)

   *testGetPoties() 가공 패스*

## 테스트 isEmpty() {#testing-is-empty}

**`isEmpty()`**&#x200B;을(를) 테스트하는 마지막 방법입니다.

`isEmpty()` 테스트는 다양한 조건에 대한 테스트가 필요하므로 흥미롭습니다. **BylineImpl.java**&#39;s `isEmpty()` 메서드를 검토하여 다음 조건을 테스트해야 합니다.

* 이름이 비어 있으면 true를 반환합니다
* 직업이 null이거나 비어 있으면 true를 반환합니다.
* 이미지가 null이거나 src URL이 없으면 true를 반환합니다
* 이름, 직업 및 이미지(src URL 사용)가 있으면 false를 반환합니다

이를 위해 새 테스트 방법을 만들어야 하며, 각 테스트 방법은 특정 조건과 `BylineImplTest.json`의 새 샘플 리소스 구조를 테스트하여 이러한 테스트를 수행할 수 있습니다.

이 검사는 `getName()`, `getOccupations()` 및 `getImage()`가 비어 있는 경우에 대한 테스트를 건너뛸 수 있도록 했습니다. 왜냐하면 해당 상태의 예상 동작이 `isEmpty()`를 통해 테스트되기 때문입니다.

1. 첫 번째 테스트에서는 설정된 속성이 없는 새 구성 요소의 조건을 테스트합니다.

   새 리소스 정의를 `BylineImplTest.json`에 추가하여 의미 이름 &quot;**empty**&quot;를 지정합니다.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** 및 만 포함하는 &quot;empty&quot;라는 새 리소스 정의 `jcr:primaryType` 를  `sling:resourceType`정의합니다.

   `@setUp`에서 각 테스트 메서드를 실행하기 전에 `BylineImplTest.json`을 `ctx`에 로드해야 한다는 점을 기억하십시오. 따라서 이 새 리소스 정의는 **/content/empty.**&#x200B;의 테스트에서 즉시 사용할 수 있습니다.

1. `testIsEmpty()`을 다음과 같이 업데이트하여 현재 리소스를 새 &quot;**empty**&quot; mock 리소스 정의로 설정합니다.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   테스트를 실행하고 통과하는지 확인합니다.

1. 그런 다음 필요한 데이터 포인트(이름, 직업 또는 이미지) 중 하나가 비어 있는 경우 `isEmpty()`이 true를 반환하는 메서드 집합을 만듭니다.

   각 테스트에 대해 개별 mock 리소스 정의를 사용하려면 **BylineImplTest.json**&#x200B;을 **name 없이** 및 **without-profiles**&#x200B;에 대한 추가 리소스 정의로 업데이트하십시오.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   다음 테스트 메서드를 만들어 이러한 각 상태를 테스트합니다.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** 빈 mock 리소스 정의에 대해 테스트하고 이 `isEmpty()` 가 true라고 합니다.

   **`testIsEmpty_WithoutName()`** 직업이 있지만 이름이 없는 모의 리소스 정의에 대해 테스트합니다.

   **`testIsEmpty_WithoutOccupations()`** 이름은 있지만 직업이 없는 모의 리소스 정의에 대해 테스트합니다.

   **`testIsEmpty_WithoutImage()`** 이름과 직업을 사용하여 샘플 리소스 정의에 대해 테스트하지만 mock Image를 null로 반환하도록 설정합니다. 이 호출에서 반환되는 이미지 개체가 null인지 확인하기 위해 `setUp()`에 정의된 `modelFactory.getModelFromWrappedRequest(..)` 동작을 무시하려고 합니다. Mockito에 관한 Stub는 엄격하고 중복된 코드를 원하지 않습니다. 따라서 `setUp()` 메서드에서 이 동작을 재정의하고 있다는 것을 명시적으로 알리기 위해 **`lenient`** 설정으로 mock을 설정합니다.

   **`testIsEmpty_WithoutImageSrc()`** 이름과 직업으로 모의 리소스 정의에 대해 테스트하지만 이 호출되면 mock Image를 설정하여 빈 문자열 `getSrc()` 을 반환합니다.

1. 끝으로, 구성 요소가 올바르게 구성되면 **isEmpty()**&#x200B;에서 false를 반환하는지 확인하는 테스트를 작성합니다. 이 조건에 대해, 완전히 구성된 Byline 구성 요소를 나타내는 **/content/byline** 을 다시 사용할 수 있습니다.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 이제 BylineImplTest.java 파일에서 모든 단위 테스트를 실행하고 Java 테스트 보고서 출력을 검토합니다.

![모든 테스트 통과](./assets/unit-testing/all-tests-pass.png)

## 빌드의 일부로 단위 테스트 실행 {#running-unit-tests-as-part-of-the-build}

maven 빌드의 일부로 전달하려면 단위 테스트가 실행됩니다. 이렇게 하면 응용 프로그램을 배포하기 전에 모든 테스트가 성공적으로 전달됩니다. 패키지 또는 설치와 같은 Maven 목표를 실행하면 자동으로 호출되고 프로젝트의 모든 단위 테스트를 통과해야 합니다.

```shell
$ mvn package
```

![mvn 패키지 성공](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

마찬가지로, 테스트 방법을 실패 로 변경하면 빌드가 실패하고 테스트가 실패한 및 이유를 보고합니다.

![mvn 패키지 실패](assets/unit-testing/mvn-package-fail.png)

## 코드 검토 {#review-the-code}

완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd)에서 확인하거나 Git brach `tutorial/unit-testing-solution`에서 로컬로 코드를 검토하고 배포합니다.
