---
title: 단위 테스트
description: 사용자 지정 구성 요소 자습서에서 만든 라인 구성 요소의 Sling 모델의 동작을 확인하는 단위 테스트를 구현합니다.
version: 6.5, Cloud Service
type: Tutorial
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '3014'
ht-degree: 0%

---

# 단위 테스트 {#unit-testing}

이 자습서에서는 다음 위치에서 만든 라인 구성 요소의 Sling 모델의 동작을 확인하는 단위 테스트 구현을 다룹니다 [사용자 지정 구성 요소](./custom-component.md) 자습서입니다.

## 사전 요구 사항 {#prerequisites}

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment).

_시스템에 Java 8과 Java 11이 모두 설치되어 있는 경우 VS 코드 테스트 실행자는 테스트를 실행할 때 낮은 Java 런타임을 선택하여 테스트 실패를 초래할 수 있습니다. 이 경우 Java 8을 제거합니다._

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 다시 사용하고 시작 프로젝트를 체크 아웃하는 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 라인 코드를 확인합니다.

1. 다음을 확인하십시오 `tutorial/unit-testing-start` 분기 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로파일을 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `tutorial/unit-testing-start`.

## 목표

1. 단위 테스트의 기본 사항을 이해합니다.
1. AEM 코드를 테스트하는 데 일반적으로 사용되는 프레임워크 및 도구에 대해 알아봅니다.
1. 단위 테스트를 작성할 때 AEM 리소스를 비웃거나 시뮬레이션하는 옵션을 이해합니다.

## 배경 {#unit-testing-background}

이 튜토리얼에서는 작성 방법을 살펴보겠습니다 [단위 테스트](https://en.wikipedia.org/wiki/Unit_testing) 을 참조하십시오. [Sling 모델](https://sling.apache.org/documentation/bundles/models.html) (에서 생성) [사용자 지정 AEM 구성 요소 만들기](custom-component.md)). 단위 테스트는 Java 코드의 예상 동작을 확인하는 Java로 작성된 빌드 시간 테스트입니다. 각 단위 테스트는 일반적으로 작으며, 예상 결과와 비교하여 메서드(또는 작업 단위)의 출력을 검증합니다.

Adobe에서는 AEM 우수 사례를 사용하며, 다음과 같은 작업을 수행합니다.

* [주닛트 5](https://junit.org/junit5/)
* [Mockito 테스트 프레임워크](https://site.mockito.org/)
* [wcm.io 테스트 프레임워크](https://wcm.io/testing/) (다음 빌드 [Apache Sling이 Android](https://sling.apache.org/documentation/development/sling-mock.html))

## Cloud Manager 단위 테스트 및 Adobe {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) 단위 테스트 실행 통합 [코드 검사 보고](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) 를 CI/CD 파이프라인으로 전환하여 AEM 코드의 단위 테스트 우수 사례를 권장하고 프로모션합니다.

장치 테스트 코드는 모든 코드 베이스에 권장되지만 Cloud Manager를 사용할 때는 Cloud Manager를 실행할 단위 테스트를 제공하여 코드 품질 테스트 및 보고 기능을 활용하는 것이 중요합니다.

## 테스트 Maven 종속성 업데이트 {#inspect-the-test-maven-dependencies}

첫 번째 단계는 테스트 작성 및 실행을 지원하기 위해 Maven 종속성을 검사하는 것입니다. 필요한 종속성은 4개입니다.

1. JUnit5
1. Mockito 테스트 프레임워크
1. Apache Sling이 Android
1. AEM Android Test Framework (io.wcm)

다음 **JUnit5**, **모키토** 및 **AEM Android** 설치 중에 테스트 종속성이 프로젝트에 자동으로 추가됩니다. [AEM Maven 원형](project-setup.md).

1. 이러한 종속성을 보려면 다음 위치에서 상위 Reactor POM을 엽니다. **aem-guides-wknd/pom.xml**&#x200B;로 이동합니다. `<dependencies>..</dependencies>` 및에서 JUnit, Mockito, Apache Sling Android 및 AEM Mock Tests에 대한 종속성을 i/o.wcm별로 확인합니다. `<!-- Testing -->`.
1. 확인 `io.wcm.testing.aem-mock.junit5` 가 로 설정되어 있습니다. **4.1.0**:

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > 원형 **35** 를 사용하여 프로젝트 생성 `io.wcm.testing.aem-mock.junit5` 버전 **4.1.8**. 로 다운그레이드하십시오. **4.1.0** 이 장의 나머지 부분을 따르다.

1. 열기 **aem-guides-wknd/core/pom.xml** 그리고 해당 테스트 종속성을 사용할 수 있는지 확인합니다.

   의 병렬 소스 폴더 **코어** 프로젝트에는 단위 테스트 및 지원 테스트 파일이 포함됩니다. 이 **테스트** 폴더는 소스 코드로부터 테스트 클래스를 분리하지만, 이 폴더를 사용하면 테스트가 소스 코드와 동일한 패키지에 있는 것처럼 작동할 수 있습니다.

## 주 테스트 만들기 {#creating-the-junit-test}

단위 테스트는 일반적으로 Java 클래스에 1에서 1로 매핑됩니다. 이 장에서는 **BylineImpl.java**- Byline 구성 요소를 지원하는 Sling Model입니다.

![단위 테스트 src 폴더](assets/unit-testing/core-src-test-folder.png)

*단위 테스트가 저장되는 위치입니다.*

1. 에 대한 단위 테스트를 만듭니다. `BylineImpl.java` 새 Java 클래스를 `src/test/java` 를 클릭합니다.

   ![새 BylineImplTest.java 파일 만들기](assets/unit-testing/new-bylineimpltest.png)

   테스트 중이라

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   에서 해당 단위 테스트 Java 클래스를 생성합니다.

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   다음 `Test` 단위 테스트 파일의 접미사, `BylineImplTest.java` 그것은 우리가 할 수 있는 하나의 컨벤션이다

   1. 테스트 파일로 손쉽게 식별 _대상_ `BylineImpl.java`
   1. 테스트 파일도 구분할 수 있습니다 _변환 전:_ 시험 중인 수업인데 `BylineImpl.java`



## BylineImplTest.java 검토 {#reviewing-bylineimpltest-java}

이때 JUnit 테스트 파일은 빈 Java 클래스입니다.

1. 파일을 다음 코드로 업데이트합니다.

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

1. 첫 번째 메서드 `public void setUp() { .. }` 주 주석으로 주석 달기 `@BeforeEach`- 이 클래스에서 각 테스트 메서드를 실행하기 전에 JUnit 테스트 실행자에게 이 메서드를 수행하도록 지시합니다. 이렇게 하면 모든 테스트에 필요한 일반적인 테스트 상태를 초기화하기 편리한 위치를 제공합니다.

1. 그 다음 메서드는 이름이 접두사로 붙은 테스트 메서드입니다 `test` 규칙에 의해 그리고 `@Test` 주석. 기본적으로 모든 테스트는 아직 구현되지 않았으므로 실패로 설정됩니다.

   먼저 테스트하고 있는 클래스의 각 공용 메서드에 대해 하나의 테스트 방법으로 시작합니다.

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 에 의해 테스트됨 | testGetName() |
   | getTowals() | 에 의해 테스트됨 | testGetTowals() |
   | isEmpty() | 에 의해 테스트됨 | testIsEmpty() |

   이 장의 뒷부분에서 확인할 수 있듯이 필요에 따라 이러한 메서드를 확장할 수 있습니다.

   이 JUnit 테스트 클래스(JUnit 테스트 케이스라고도 함)를 실행하면 각 메서드마다 `@Test` 는 전달 또는 실패할 수 있는 테스트로 실행됩니다.

![생성된 BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 마우스 오른쪽 단추를 클릭하여 JUnit 테스트 사례 실행 `BylineImplTest.java` 파일 및 탭 **실행**.
예상대로, 모든 테스트가 아직 구현되지 않았기 때문에 실패합니다.

   ![junit 테스트로 실행](assets/unit-testing/run-junit-tests.png)

   *BylineImplTests.java > Run을 마우스 오른쪽 단추로 클릭합니다.*

## BylineImpl.java 검토 {#reviewing-bylineimpl-java}

단위 테스트를 작성할 때 두 가지 주요 접근 방식이 있습니다.

* [TDD 또는 테스트 기반 개발](https://en.wikipedia.org/wiki/Test-driven_development)는 구현이 개발되기 바로 전에 점진적으로 단위 테스트를 작성하는 것을 포함합니다. 테스트를 작성하고 구현을 작성하여 테스트를 통과합니다.
* 먼저 작업 코드를 개발하고 해당 코드의 유효성을 검사하는 테스트를 작성하는 작업이 포함된 구현 첫 번째 개발.

이 자습서에서는 작업을 이미 만들었으므로 후자의 접근 방식이 사용됩니다 **BylineImpl.java** 이전 장) 따라서 공개 메서드의 동작을 검토하고 이해해야 하며, 구현 세부 사항 중 일부를 고려해야 합니다. 이는 반대의 소리일 수 있습니다. 좋은 테스트는 입력 및 출력만 신경써야 하므로 하지만 AEM에서 작업하는 경우 작업 테스트를 생성하기 위해 알고 있어야 하는 다양한 구현 고려 사항이 있습니다.

AEM의 컨텍스트에서 TDD는 전문 지식이 필요하며 AEM 코드의 AEM 개발 및 장치 테스트에 능숙한 AEM 개발자가 가장 잘 채택합니다.

## AEM 테스트 컨텍스트 설정  {#setting-up-aem-test-context}

AEM에 대해 작성된 대부분의 코드는 JCR, Sling 또는 AEM API를 사용하며, 이 경우 실행 중인 AEM의 컨텍스트가 제대로 실행되어야 합니다.

단위 테스트는 실행 중인 AEM 인스턴스의 컨텍스트 외부에 있는 빌드에서 실행되므로 그러한 컨텍스트가 없습니다. 이걸 쉽게 하기 위해서 [wcm.io의 AEM Android](https://wcm.io/testing/aem-mock/usage.html) 이러한 API를 허용하는 mock 컨텍스트를 만듭니다. _대부분_ AEM에서 실행 중인 것처럼 행동합니다.

1. 을 사용하여 AEM 컨텍스트 만들기 **wcm.io** `AemContext` in **BylineImplTest.java** 를 추가하여 `@ExtendWith` 변환 후 **BylineImplTest.java** 파일. 확장은 필요한 모든 초기화 및 정리 작업을 처리합니다. 에 대한 클래스 변수 만들기 `AemContext` 모든 테스트 메서드에 사용할 수 있습니다.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   이 변수, `ctx`는 많은 AEM 및 Sling 추상을 제공하는 mock AEM 컨텍스트를 표시합니다.

   * BylineImpl Sling Model이 이 컨텍스트에 등록됩니다
   * Mock JCR 컨텐츠 구조가 이 컨텍스트에서 생성됩니다
   * 사용자 지정 OSGi 서비스는 이 컨텍스트에서 등록할 수 있습니다
   * SlingHttpServletRequest 개체, ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag 등과 같은 다양한 Mock Sling 및 AEM OSGi 서비스 등 일반적인 필수 Mock 객체 및 Help를 제공합니다.
      * *이러한 개체의 모든 메서드가 구현되지는 않습니다!*
   * 및 [훨씬 더](https://wcm.io/testing/aem-mock/usage.html)!

   다음 **`ctx`** 개체는 대부분의 mock 컨텍스트에 대한 시작 지점으로 사용됩니다.

1. 에서 `setUp(..)` 메서드 `@Test` 메서드를 사용하여 일반적인 모의 테스트 상태를 정의합니다.

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 테스트할 Sling 모델을 mock AEM 컨텍스트에 등록하여 `@Test` 메서드를 사용합니다.
   * **`load().json`** 리소스 구조를 샘플 컨텍스트에 로드하여 코드가 실제 저장소에서 제공한 것처럼 리소스와 상호 작용할 수 있습니다. 파일의 리소스 정의 **`BylineImplTest.json`** 는 아래의 샘플 JCR 컨텍스트에 로드됩니다 **/content**.
   * **`BylineImplTest.json`** 아직 존재하지 않으므로, 만들고 테스트에 필요한 JCR 리소스 구조를 정의하겠습니다.

1. 샘플 리소스 구조를 나타내는 JSON 파일은 아래에 저장됩니다 **core/src/test/resources** 참고 Java 테스트 파일과 같은 패키지 경로 지정

   에서 새 JSON 파일을 만듭니다. `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` 명명된 이름 **BylineImplTest.json** 다음 컨텐츠와 함께 사용할 수 있습니다.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   이 JSON은 라인 구성 요소 단위 테스트에 대한 샘플 리소스(JCR 노드)를 정의합니다. 이때 JSON에는 개별 구성 요소 컨텐츠 리소스, `jcr:primaryType` 및 `sling:resourceType`.

   단위 테스트를 사용할 때 일반적인 규칙은 각 테스트를 충족하는 데 필요한 최소한의 샘플 컨텐츠, 컨텍스트 및 코드 세트를 만드는 것입니다. 종종 불필요한 가공물이 발생하므로 테스트를 작성하기 전에 전체 샘플 컨텍스트를 작성하려는 유혹을 피하십시오.

   이제 **BylineImplTest.json**, when `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` 가 실행되면 모의 리소스 정의가 경로의 컨텍스트에 로드됩니다 **/content.**

## getName() 테스트 {#testing-get-name}

이제 기본 모의 컨텍스트 설정이 있으므로 첫 번째 테스트를 작성하겠습니다 **BylineImpl의 getName()**. 이 테스트는 메서드가 **getName()** 리소스의 &quot;**name&quot;** 속성을 사용합니다.

1. 업데이트 **testGetName**()의 메서드 **BylineImplTest.java** 아래와 같이 변경하는 것을 의미합니다.

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
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

   * **`String expected`** 예상 값을 설정합니다. 이를 &quot;(으)로 설정합니다.**제인 완료**&quot;.
   * **`ctx.currentResource`** mock 리소스의 컨텍스트를 설정하여 코드를 평가하도록 설정하므로, **/content/byline** 바로 이 경우 mock byline content resource 가 로드됩니다.
   * **`Byline byline`** mock Request 개체에서 Sling 모델을 적용하여 인스턴스화합니다.
   * **`String actual`** 테스트 중인 방법을 호출합니다 `getName()`를 클릭합니다.
   * **`assertEquals`** 필요한 값을 어설션하면 byline Sling Model 개체가 반환한 값과 일치합니다. 이러한 값이 같으면 테스트가 실패합니다.

1. 테스트 실행... 을 실행하면 `NullPointerException`.

   테스트를 정의하지 않았으므로 이 테스트가 실패하지는 않습니다 `name` mock JSON의 속성에 있는 경우, 테스트가 실패하지만 테스트 실행이 해당 시점에 도달하지 않았습니다. 다음 이유로 인해 이 테스트가 실패합니다. `NullPointerException` 를 클릭합니다.

1. 에서 `BylineImpl.java`이면 `@PostConstruct init()` 예외가 발생하여 Sling Model이 인스턴스화할 수 없으며 Sling Model 개체가 null이 됩니다.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   ModelFactory OSGi 서비스는 `AemContext` (Apache Sling 컨텍스트에 따라) 다음을 포함한 모든 메서드가 구현되지 않습니다 `getModelFromWrappedRequest(...)` BylineImpl에서 호출됩니다. `init()` 메서드를 사용합니다. 이 경우 [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)- 용어에 의해 `init()` 실패 및 `ctx.request().adaptTo(Byline.class)` 는 null 개체입니다.

   제공된 개체는 코드를 수용할 수 없으므로 직접 샘플 컨텍스트를 구현해야 합니다. 이를 위해 Mockie를 사용하여 mockFactory 개체를 만들고, 이 개체는 mock Image 개체를 반환하는 경우 `getModelFromWrappedRequest(...)` 이 호출됩니다.

   Byline Sling 모델을 인스턴스화하려면 이 샘플 컨텍스트가 있어야 하므로 Adobe에서는 이 모델을 `@Before setUp()` 메서드를 사용합니다. 또한 `MockitoExtension.class` 변환 후 `@ExtendWith` 위의 주석 **BylineImplTest** 클래스 이름을 지정합니다.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 는 [Mockito Jupiter Extension](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) @Mock 주석을 사용하여 클래스 수준에서 샘플 객체를 정의할 수 있습니다.
   * **`@Mock private Image`** 유형의 mock 객체를 만듭니다. `com.adobe.cq.wcm.core.components.models.Image`. 필요한 경우 클래스 레벨에서 정의됩니다. `@Test` 메서드는 필요에 따라 동작을 변경할 수 있습니다.
   * **`@Mock private ModelFactory`** ModelFactory 유형의 mock 개체를 만듭니다. 이것은 순수한 Mockito 조롱이며 여기에 구현된 방법이 없습니다. 필요한 경우 클래스 레벨에서 정의됩니다. `@Test`메서드는 필요에 따라 동작을 변경할 수 있습니다.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 다음 경우에 mock 동작을 등록합니다. `getModelFromWrappedRequest(..)` mock ModelFactory 개체에서 호출됩니다. 에 정의된 결과 `thenReturn (..)` 은 mock Image 객체를 반환하는 것입니다. 이 동작은 다음 경우에만 호출됩니다. 첫 번째 매개 변수는 `ctx`의 요청 개체인 두 번째 매개 변수는 모든 리소스 개체이고 세 번째 매개 변수는 핵심 구성 요소 이미지 클래스여야 합니다. Adobe에서는 테스트 전체에서 `ctx.currentResource(...)` 에 정의된 다양한 샘플 리소스로 **BylineImplTest.json**. 을(를) 추가합니다 **관대한()** 엄격해야 합니다. 나중에 ModelFactory의 이 동작을 재정의하려고 하기 때문입니다.
   * **`ctx.registerService(..)`.** mock ModelFactory 개체를 Aem Context에 등록하고 가장 높은 서비스 등급을 사용합니다. BylineImpl의 ModelFactory에서 사용되므로 이 작업은 필수입니다 `init()` 는 `@OSGiService ModelFactory model` 필드. AemContext가 주입되는 순서 **adobe** 호출을 처리하는 모의 개체 `getModelFromWrappedRequest(..)`를 지정하는 경우 해당 유형의 가장 높은 등급 서비스(ModelFactory)로 등록해야 합니다.

1. 테스트를 다시 실행하면 다시 실패하지만, 이번에는 메시지가 실패한 이유를 확인합니다.

   ![테스트 이름 실패 검증](assets/unit-testing/testgetname-failure-assertion.png)

   *어설션으로 인해 testGetName() 오류가 발생했습니다.*

   Adobe는 **AssertionError** 즉, 테스트에서 어설픈 상태가 실패하여 우리에게 이 사실을 알려줍니다 **예상 값은 &quot;Jane Doe&quot;입니다.** 하지만 **실제 값이 null입니다.**. &quot;**name&quot;** mock에 속성이 추가되지 않았습니다. **/content/byline** 의 리소스 정의 **BylineImplTest.json**&#x200B;을 추가하여 다음을 수행합니다.

1. 업데이트 **BylineImplTest.json** 다음을 정의합니다. `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 테스트를 다시 실행하고 **`testGetName()`** 이제 가!

   ![테스트 이름 전달](assets/unit-testing/testgetname-pass.png)


## getTowals() 테스트 {#testing-get-occupations}

좋아! 우리의 첫 번째 시험이 합격했어! 계속 진행하고 테스트해 봅시다 `getOccupations()`. 모의 컨텍스트의 초기화가 `@Before setUp()`메서드, 모든 사용자가 사용할 수 있습니다. `@Test` 다음을 포함한 이 테스트 케이스의 메서드 `getOccupations()`.

이 방법은 직업 등록 정보에 저장된 알파벳순으로 정렬된 직업 목록(내림차순)을 반환해야 합니다.

1. 업데이트 **`testGetOccupations()`** 아래와 같이 변경하는 것을 의미합니다.

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
   * **`ctx.currentResource`** 컨텍스트를 /content/byline의 샘플 리소스 정의에 대해 평가하도록 현재 리소스를 설정합니다. 이를 통해 **BylineImpl.java** 은 mock 리소스의 컨텍스트에서 실행됩니다.
   * **`ctx.request().adaptTo(Byline.class)`** mock Request 개체에서 Sling 모델을 적용하여 인스턴스화합니다.
   * **`byline.getOccupations()`** 테스트 중인 방법을 호출합니다 `getOccupations()`를 클릭합니다.
   * **`assertEquals(expected, actual)`** 예측된 목록에 대해 어설션 목록이 실제 목록과 동일합니다.

1. 기억하십시오, **`getName()`** 위에, **BylineImplTest.json** 직업을 정의하지 않기 때문에 테스트를 실행하면 이 테스트가 실패합니다. `byline.getOccupations()` 은 빈 목록을 반환합니다.

   업데이트 **BylineImplTest.json** 직업 목록을 포함하기 위해, 이러한 분류는 기존의 항목순으로 설정되어 있지 않게 설정되며, 이러한 분류는 해당 분야가 사전순으로 정렬되어 있는지 확인할 수 있도록 합니다 **`getOccupations()`**.

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

   *testGetTowals() 가공 패스*

## 테스트 isEmpty() {#testing-is-empty}

마지막으로 테스트할 방법입니다 **`isEmpty()`**.

테스트 `isEmpty()` 다양한 조건에 대한 테스트가 필요하므로 흥미롭습니다. 검토 **BylineImpl.java** s `isEmpty()` 다음 조건을 테스트해야 합니다.

* 이름이 비어 있으면 true를 반환합니다
* 직업이 null이거나 비어 있으면 true를 반환합니다.
* 이미지가 null이거나 src URL이 없으면 true를 반환합니다
* 이름, 직업 및 이미지(src URL 사용)가 있으면 false를 반환합니다

이를 위해서는 새로운 테스트 방법을 만들어야 하며, 각 테스트 방법에서는 특정 조건과 의 새 샘플 리소스 구조를 테스트해야 합니다 `BylineImplTest.json` 테스트 수행

이 확인에서는 다음에 대한 테스트를 건너뛸 수 있었습니다 `getName()`, `getOccupations()` 및 `getImage()` 는 해당 상태의 예상 동작이 `isEmpty()`.

1. 첫 번째 테스트에서는 설정된 속성이 없는 새 구성 요소의 조건을 테스트합니다.

   에 새 리소스 정의 추가 `BylineImplTest.json`시맨틱 이름 &quot;**비어 있음**&quot;

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

   **`"empty": {...}`** &quot;empty&quot;라는 새 리소스 정의를 정의하며, `jcr:primaryType` 및 `sling:resourceType`.

   로드 `BylineImplTest.json` 변환 `ctx` 의 각 테스트 메서드를 실행하기 전에 `@setUp`따라서 의 테스트에서 이 새 리소스 정의를 즉시 사용할 수 있습니다 **/content/empty 입니다.**

1. 업데이트 `testIsEmpty()` 다음과 같이 현재 리소스를 새 &quot;**비어 있음**&quot; 샘플 리소스 정의.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   테스트를 실행하고 통과하는지 확인합니다.

1. 그런 다음 필요한 데이터 포인트(이름, 직업 또는 이미지) 중 하나가 비어 있는 경우, `isEmpty()` 는 true를 반환합니다.

   각 테스트에 대해 개별 샘플 리소스 정의를 사용하여 업데이트합니다. **BylineImplTest.json** 에 대한 추가 리소스 정의 사용 **without-name** 및 **무직업**.

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

   **`testIsEmpty()`** 빈 mock 리소스 정의에 대해 테스트하고 `isEmpty()` 는 true입니다.

   **`testIsEmpty_WithoutName()`** 직업이 있지만 이름이 없는 모의 리소스 정의에 대해 테스트합니다.

   **`testIsEmpty_WithoutOccupations()`** 이름은 있지만 직업이 없는 모의 리소스 정의에 대해 테스트합니다.

   **`testIsEmpty_WithoutImage()`** 이름과 직업을 사용하여 샘플 리소스 정의에 대해 테스트하지만 mock Image를 null로 반환하도록 설정합니다. 을(를) 재정의하려고 합니다 `modelFactory.getModelFromWrappedRequest(..)`에 정의된 동작 `setUp()` 이 호출에서 반환된 이미지 개체가 null인지 확인합니다. Mockito에 관한 Stub는 엄격하고 중복된 코드를 원하지 않습니다. 따라서 Mock을 **`lenient`** 명시적으로 표시할 설정 `setUp()` 메서드를 사용합니다.

   **`testIsEmpty_WithoutImageSrc()`** 이름과 직업을 사용하여 샘플 리소스 정의에 대해 테스트하지만 mock Image를 설정하면 빈 문자열이 반환됩니다 `getSrc()` 이 호출됩니다.

1. 마지막으로, 테스트를 작성하여 **isEmpty()** 구성 요소가 올바르게 구성되면 false를 반환합니다. 이 조건의 경우 다시 사용할 수 있습니다 **/content/byline** 전체 구성된 부산물 구성 요소를 나타냅니다.

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

에서 완성된 코드 보기 [GitHub](https://github.com/adobe/aem-guides-wknd) 코드를 Git Brach에서 로컬로 검토하고 배포합니다 `tutorial/unit-testing-solution`.
