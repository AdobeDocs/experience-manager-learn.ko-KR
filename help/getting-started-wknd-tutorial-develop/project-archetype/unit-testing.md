---
title: 단위 테스트
description: 사용자 정의 구성 요소 튜토리얼에서 만든 Byline 구성 요소의 Sling 모델 동작을 검증하는 단위 테스트를 구현합니다.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4089
mini-toc-levels: 1
thumbnail: 30207.jpg
doc-type: Tutorial
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
duration: 706
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '2923'
ht-degree: 100%

---

# 단위 테스트 {#unit-testing}

이 튜토리얼은 [사용자 정의 구성 요소](./custom-component.md) 튜토리얼에서 생성된 Byline 구성 요소의 Sling 모델 동작을 검증하는 단위 테스트 구현을 다룹니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구와 지침을 검토합니다.

_Java™ 8과 Java™ 11이 모두 시스템에 설치된 경우, VS Code 테스트 실행기는 테스트를 실행할 때 더 낮은 Java™ 런타임을 선택할 수 있으며, 그 결과 테스트가 실패할 수 있습니다. 이런 현상이 발생한다면 Java™ 8을 제거하십시오._

### 스타터 프로젝트

>[!NOTE]
>
> 이전 챕터를 성공적으로 완료했다면 프로젝트를 재사용하고 스타터 프로젝트를 체크아웃하는 단계를 건너뛸 수 있습니다.

튜토리얼에서 기반으로 삼는 기본 코드를 확인합니다.

1. `tutorial/unit-testing-start`[GitHub](https://github.com/adobe/aem-guides-wknd)의 분기를 확인합니다.

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 모든 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

완성된 코드는 언제든지 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start)에서 볼 수 있으며 `tutorial/unit-testing-start` 분기로 전환하여 로컬에서 코드를 확인할 수도 있습니다.

## 목표

1. 단위 테스트의 기본 사항을 이해합니다.
1. AEM 코드를 테스트하는 데 일반적으로 사용되는 프레임워크와 도구를 알아봅니다.
1. 단위 테스트를 작성할 때 AEM 리소스를 모의하거나 시뮬레이션하는 옵션을 이해합니다.

## 배경 {#unit-testing-background}

이 튜토리얼에서는 Byline 구성 요소의 [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)&#x200B;([사용자 정의 AEM 구성 요소 만들기](custom-component.md)에서 생성)에 대한 [단위 테스트](https://ko.wikipedia.org/wiki/Unit_testing)를 작성하는 방법을 살펴봅니다. 단위 테스트는 Java™로 작성된 빌드 시간 테스트로, Java™ 코드의 예상 동작을 검증합니다. 각 단위 테스트는 일반적으로 규모가 작으며, 예상 결과와 비교하여 메서드(또는 작업 단위)의 출력을 검증합니다.

당사는 AEM 모범 사례를 활용하고 다음을 채택합니다.

* [JUnit 5](https://junit.org/junit5/)
* [Mockito 테스트 프레임워크](https://site.mockito.org/)
* [wcm.io 테스트 프레임워크](https://wcm.io/testing/) ([Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html) 기반)

## 단위 테스트 및 Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=ko)는 단위 테스트 실행과 [코드 커버리지 보고](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html?lang=ko)를 CI/CD 파이프라인에 통합하여 AEM 코드의 단위 테스트 모범 사례를 장려 및 촉진하는 데 기여합니다.

단위 테스트 코드는 모든 코드 베이스에 적용하는 것이 좋지만 Cloud Manager를 사용할 때는 Cloud Manager에서 실행할 단위 테스트를 제공하여 코드 품질 테스트 및 보고 기능을 활용하는 것이 중요합니다.

## 테스트 Maven 종속성 업데이트 {#inspect-the-test-maven-dependencies}

첫 번째 단계는 테스트 작성 및 실행을 지원하기 위해 Maven 종속성을 검사하는 것입니다. 필요한 종속성은 4개입니다.

1. JUnit5
1. Mockito 테스트 프레임워크
1. Apache Sling Mocks
1. AEM Mocks 테스트 프레임워크 (io.wcm 제공)

**JUnit5**, **Mockito 및 **AEM Mocks** 테스트 종속성은 설정 중에 [AEM Maven Archetype](project-setup.md)을 사용하여 프로젝트에 자동 추가됩니다.

1. 이러한 종속성을 보려면 **aem-guides-wknd/pom.xml**&#x200B;에서 Parent Reactor POM을 열고 `<dependencies>..</dependencies>`로 이동한 다음 `<!-- Testing -->`에서 io.wcm에서 제공하는 JUnit, Mockito, Apache Sling Mocks, and AEM Mock Tests의 종속성을 확인합니다.
1. `io.wcm.testing.aem-mock.junit5`4.1.0 **으로 설정되어 있는지 확인합니다.**

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
   > Archetype **35**&#x200B;는 `io.wcm.testing.aem-mock.junit5` 버전 **4.1.8**&#x200B;로 프로젝트를 생성합니다. 이 챕터의 나머지 내용을 진행하려면 **4.1.0**&#x200B;으로 다운그레이드하십시오.

1. **aem-guides-wknd/core/pom.xml**&#x200B;을 열고 해당 테스트 종속성이 사용 가능한지 확인합니다.

   **코어** 프로젝트의 병렬 소스 폴더에는 단위 테스트와 지원 테스트 파일이 포함됩니다. 이 **테스트** 폴더는 테스트 클래스를 소스 코드에서 분리하지만 테스트가 소스 코드와 동일한 패키지에 있는 것처럼 작동할 수 있도록 합니다.

## JUnit 테스트 만들기 {#creating-the-junit-test}

단위 테스트는 일반적으로 Java™ 클래스와 1:1로 매핑됩니다. 이 챕터에서는 Byline 구성 요소를 지원하는 Sling 모델인 **BylineImpl.Java**&#x200B;에 대한 JUnit 테스트를 작성합니다.

![단위 테스트 src 폴더](assets/unit-testing/core-src-test-folder.png)

*단위 테스트가 저장된 위치입니다.*

1. 테스트할 Java™ 클래스의 위치를 반영하는 Java™ 패키지 폴더 구조에서 `src/test/java` 아래에 새 Java™ 클래스를 만들어서 `BylineImpl.java`에 대한 단위 테스트를 만듭니다.

   ![새로운 BylineImplTest.Java 파일을 만듭니다.](assets/unit-testing/new-bylineimpltest.png)

   현재 테스트 중이기 때문에

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   해당 단위 테스트 Java™ 클래스를 다음에서 생성합니다.

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   단위 테스트 파일 `BylineImplTest.java`의 `Test` 접미사는 다음을 허용하는 규칙입니다.

   1. 이는 `BylineImpl.java`에 _대한_ 테스트 파일로 쉽게 식별할 수 있습니다.
   1. 또한 동시에 테스트 파일을 테스트 대상인 `BylineImpl.java` 클래스&#x200B;_와_ 구별할 수 있습니다.

## BylineImplTest.Java를 검토 중 {#reviewing-bylineimpltest-java}

이 시점에서 JUnit 테스트 파일은 빈 Java™ 클래스입니다.

1. 다음 코드로 파일을 업데이트합니다.

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

1. 첫 번째 메서드 `public void setUp() { .. }`은 JUnit의 `@BeforeEach` 주석이 달려 있습니다. 이 주석은 JUnit 테스트 실행기가 해당 클래스의 각 테스트 메서드를 실행하기 전에 이 메서드를 실행하도록 지시합니다. 이는 모든 테스트에 필요한 공통 테스트 상태를 초기화하는 데 편리한 장소를 제공합니다.

1. 후속 메서드는 테스트 메서드로, 규칙에 따라 이름 앞에 `test`가 붙고 `@Test` 주석이 표시됩니다. 아직 테스트가 구현되지 않았기 때문에 기본적으로 모든 테스트는 실패하도록 설정되어 있습니다.

   먼저 테스트하려는 클래스의 각 공개 메서드에 대해 단일 테스트 메서드부터 시작하며, 여기에는 다음이 해당됩니다.

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 테스트 수행자 | testGetName() |
   | getOccupations() | 테스트 수행자 | testGetOccupations() |
   | isEmpty() | 테스트 수행자 | testIsEmpty() |

   이러한 메서드는 필요에 따라 확장될 수 있습니다. 관련 내용은 챕터 뒷부분에서 살펴보겠습니다.

   이 JUnit 테스트 클래스(JUnit 테스트 케이스라고도 함)가 실행되면 `@Test`로 표시된 각 메서드는 통과하거나 실패할 수 있는 테스트로 실행됩니다.

![생성된 BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. `BylineImplTest.java` 파일을 마우스 오른쪽 버튼으로 클릭한 다음 **실행**을 탭하여 JUnit 테스트를 실행합니다.
테스트가 아직 구현되지 않았기 때문에 예상대로 모든 테스트가 실패합니다.

   ![junit 테스트로 실행](assets/unit-testing/run-junit-tests.png)

   *마우스 오른쪽 버튼으로 BylineImplTests.Java 클릭 > 실행*

## BylineImpl.Java 검토 {#reviewing-bylineimpl-java}

단위 테스트를 작성할 때 두 가지 주요 접근 방식이 있습니다.

* [TDD 또는 테스트 중심 개발](https://ko.wikipedia.org/wiki/Test-driven_development)은 구현이 개발되기 직전에 단위 테스트를 증분적으로 작성하는 것을 포함합니다. 이러한 개발의 경우 테스트를 작성하고, 테스트를 통과할 수 있도록 구현을 작성합니다.
* 구현 우선 개발은 먼저 작동하는 코드를 개발한 다음 해당 코드의 유효성을 검증하는 테스트를 작성하는 것을 포함합니다.

이 튜토리얼에서는 작동 중인 **BylineImpl.java**&#x200B;를 이미 만들었기 때문에 후자의 접근 방식이 사용됩니다. 따라서 공개 메서드의 동작뿐만 아니라 일부 구현 세부 사항도 검토하고 이해해야 합니다. 좋은 테스트는 입력과 출력에만 집중해야 하기 때문에 이러한 접근 방식은 일반적이지 않은 것처럼 보일 수 있습니다. 하지만 AEM에서 작업할 때 작동하는 테스트를 구성하기 위해서는 이해해야 할 다양한 구현 고려 사항이 있습니다.

AEM이라는 컨텍스트에서 TDD는 일정 수준의 전문성을 요구하며 AEM 개발과 AEM 코드의 단위 테스트에 능숙한 AEM 개발자가 가장 잘 채택하는 방식입니다.

## AEM 테스트 컨텍스트 설정  {#setting-up-aem-test-context}

AEM용으로 작성된 대부분의 코드는 JCR, Sling 또는 AEM API에 의존하는데, 이를 제대로 실행하려면 실행 중인 AEM이라는 컨텍스트가 필요합니다.

단위 테스트는 실행 중인 AEM 인스턴스의 컨텍스트 외부에서 빌드할 때 실행되므로 이러한 컨텍스트가 존재하지 않습니다. 컨텍스트 없이 테스트를 실행하기 위해 [wcm.io&#39;s AEM Mocks](https://wcm.io/testing/aem-mock/usage.html)는 API가 _일반적으로_ AEM에서 실행되는 것처럼 작동하게 하는 모의 컨텍스트를 생성합니다.

1. `@ExtendWith`로 표시된 JUnit 확장 기능을 **BylineImplTest.java** 파일에 AMD 컨텍스트로 추가함으로써 **BylineImplTest.java**&#x200B;에서 **wcm.io의** `AemContext`를 사용하여 AEM 컨텍스트를 만듭니다. 확장 기능은 필요한 모든 초기화 및 정리 작업을 처리합니다. 모든 테스트 메서드에 사용할 수 있는 `AemContext`에 대한 클래스 변수를 만듭니다.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   이 `ctx` 변수는 일부 AEM 및 Sling 추상화를 제공하는 모의 AEM 컨텍스트를 노출합니다.

   * BylineImpl Sling 모델은 이 컨텍스트에 등록됩니다.
   * 이 컨텍스트에서 모의 JCR 콘텐츠 구조가 생성됩니다.
   * 이 컨텍스트에서 사용자 정의 OSGi 서비스를 등록할 수 있습니다.
   * SlingHttpServletRequest 오브젝트, ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag 등과 같은 다양한 모의 Sling 및 AEM OSGi 서비스 등 일반적으로 요구되는 다양한 모의 오브젝트와 Helper를 제공합니다.
      * *이러한 오브젝트에 대한 모든 메서드가 구현되어 있는 것은 아닙니다!*
   * 그 외 [여러 기능](https://wcm.io/testing/aem-mock/usage.html)이 있습니다!

   **`ctx`** 오브젝트는 대부분의 모의 컨텍스트에 대한 진입점 역할을 합니다.

1. 각 `@Test` 메서드 이전에 실행되는 `setUp(..)` 메서드에서는 공통 모의 테스트 상태를 정의합니다.

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`**&#x200B;는 테스트할 Sling 모델을 모의 AEM 컨텍스트에 등록하므로 `@Test` 메서드에서 인스턴스화할 수 있습니다.
   * **`load().json`**&#x200B;은 리소스 구조를 모의 컨텍스트에 로드하여 코드가 실제 저장소에서 제공된 것처럼 이러한 리소스와 상호 작용할 수 있도록 합니다. **`BylineImplTest.json`** 파일 내 리소스 정의는 **/content**&#x200B;에서 모의 JCR 컨텍스트에 로드됩니다.
   * **`BylineImplTest.json`**&#x200B;은 아직 존재하지 않으므로, 이를 생성하고 테스트에 필요한 JCR 리소스 구조를 정의하겠습니다.

1. 모의 리소스 구조를 나타내는 JSON 파일은 JUnit Java™ 테스트 파일과 동일한 패키지 경로 지정을 따라 **core/src/test/resources** 아래에 저장됩니다.

   `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl`에서 다음 내용이 포함된 **BylineImplTest.json**&#x200B;이라는 JSON 파일을 만듭니다.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   이 JSON은 Byline 구성 요소 단위 테스트를 위한 모의 리소스(JCR 노드)를 정의합니다. 이 시점에서 JSON은 Byline 구성 요소 콘텐츠 리소스를 표현하는 데 필요한 최소한의 속성 세트인 `jcr:primaryType` 및 `sling:resourceType`을 갖습니다.

   단위 테스트를 수행할 때의 일반적인 규칙은 각 테스트를 충족하는 데 필요한 최소한의 모의 콘텐츠, 컨텍스트 및 코드 세트를 만드는 것입니다. 테스트를 작성하기 전에 완전한 모의 컨텍스트를 구성해야 한다는 생각을 피하는 것이 좋습니다. 불필요한 아티팩트가 생기는 경우가 많기 때문입니다.

   이제 **BylineImplTest.json**&#x200B;이 존재하므로, `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` 실행 시 모의 리소스 정의가 **/content 경로에서 컨텍스트에 로드됩니다.**

## getName() 테스트 {#testing-get-name}

이제 기본적인 모의 컨텍스트 설정이 완료되었으므로 **BylineImpl의 getName()**&#x200B;에 대한 첫 번째 테스트를 작성해 보겠습니다. 이 테스트에서는 **getName()** 메서드가 리소스의 &#39;**name&#39;** 속성에 저장되어 있으며 작성된 올바른 이름을 반환하는지 확인해야 합니다.

1. 다음과 같이 **BylineImplTest.Java**&#x200B;에서 **testGetName**() 메서드를 업데이트합니다.

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

   * **`String expected`**&#x200B;는 예상 값을 설정합니다. 당사는 이 값을 &#39;**Jane Done**&#39;으로 설정합니다.
   * **`ctx.currentResource`**&#x200B;는 코드를 평가할 모의 리소스의 컨텍스트를 설정하므로, 모의 byline 콘텐츠 리소스가 로드되는 위치인 **/content/byline**&#x200B;으로 설정됩니다.
   * **`Byline byline`**&#x200B;은 모의 Request 오브젝트에서 Byline Sling 모델을 적용하여 인스턴스화합니다.
   * **`String actual`**&#x200B;은 Byline Sling 모델 오브젝트에서 테스트하는 메서드인 `getName()`을 호출합니다.
   * **`assertEquals`**&#x200B;는 예상 값이 Sling Model 오브젝트에서 반환된 값과 일치하는지 확인합니다. 값이 같지 않으면 테스트는 실패합니다.

1. 테스트를 실행하는 경우 `NullPointerException` 오류로 실패합니다.

   모의 JSON에서 `name` 속성을 정의하지 않았기 때문에 이 테스트는 실패하지 않습니다. 속성을 정의하면 테스트가 실패하지만 테스트 실행이 아직 그 지점에 도달하지 않았습니다. 이 테스트는 byline 오브젝트 자체의 `NullPointerException` 오류로 인해 실패합니다.

1. `BylineImpl.java`의 경우 `@PostConstruct init()`에서 오류가 발생하면 Sling 모델이 인스턴스화되지 않고 해당 Sling 모델 오브젝트가 null이 됩니다.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   ModelFactory OSGi 서비스는 `AemContext`를 통해(Apache Sling Context를 통해) 제공되지만 BylineImpl의 `init()` 메서드에서 호출되는 `getModelFromWrappedRequest(...)`를 비롯한 모든 메서드가 구현된 것은 아닙니다. 이로 인해 [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)가 발생하고 그 결과 `init()`가 실패하게 되며 결과적으로 `ctx.request().adaptTo(Byline.class)`의 적응은 null 오브젝트가 됩니다.

   제공된 모의 오브젝트는 당사의 코드를 수용할 수 없으므로, 당사가 직접 모의 컨텍스트를 구현해야 합니다. 이를 위해 Mockito를 사용하여 모의 ModelFactory 오브젝트를 생성할 수 있으며 이 오브젝트는`getModelFromWrappedRequest(...)`가 호출되면 모의 Image 오브젝트를 반환합니다.

   Byline Sling 모델을 인스턴스화하려면 이 모의 컨텍스트가 있어야 하므로 이 컨텍스트를 `@Before setUp()` 메서드에 추가할 수 있습니다. 또한 **BylineImplTest** 클래스 위에서 `MockitoExtension.class`를 `@ExtendWith` 주석에 추가해야 합니다.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`**&#x200B;는 [Mockito JUnit Jupiter Extension](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html)으로 실행할 테스트 케이스 클래스를 표시합니다. 이를 통해 @Mock 주석을 사용하여 클래스 수준에서 모의 오브젝트를 정의할 수 있습니다.
   * **`@Mock private Image`**&#x200B;는 `com.adobe.cq.wcm.core.components.models.Image` 유형의 모의 오브젝트를 생성합니다. 이는 클래스 수준에서 정의되므로 필요에 따라 `@Test` 메서드가 동작을 변경할 수 있습니다.
   * **`@Mock private ModelFactory`**&#x200B;는 ModelFactory 유형의 모의 오브젝트를 생성합니다. 이는 순수한 Mockito 모의이며 구현된 메서드가 없습니다. 이는 클래스 수준에서 정의되므로 필요에 따라 `@Test` 메서드가 동작을 변경할 수 있습니다.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`**&#x200B;는 `getModelFromWrappedRequest(..)`가 모의 ModelFactory 오브젝트에서 호출될 때 모의 동작을 등록합니다. `thenReturn (..)`에 정의된 결과는 모의 Image 오브젝트를 반환하는 것입니다. 이 동작은 첫 번째 매개변수가 `ctx`의 요청 오브젝트와 같고, 두 번째 매개변수가 Resource 오브젝트이고, 세 번째 매개변수가 Core Components Image 클래스인 경우에만 호출됩니다. 테스트 전반에 걸쳐 `ctx.currentResource(...)`를&#x200B;**BylineImplTest.json**&#x200B;에 정의된 다양한 모의 리소스에 설정하기 때문에 모든 리소스를 허용하게 됩니다. 나중에 ModelFactory의 이러한 동작을 재정의해야 하기 때문에 **lenient()** 엄격성을 추가한다는 점을 참고하시기 바랍니다.
   * **`ctx.registerService(..)`**&#x200B;는 가장 높은 서비스 순위를 가진 AemContext에 모의 ModelFactory 오브젝트를 등록합니다. 이 작업은 BylineImpl의 `init()`에서 사용되는 ModelFactory가 `@OSGiService ModelFactory model` 필드를 통해 주입되기 때문에 필요합니다. AemContext가 `getModelFromWrappedRequest(..)`에 대한 호출을 처리하는 **당사의** 모의 오브젝트를 주입하도록 하려면 해당 유형(ModelFactory)에서 가장 높은 순위의 서비스로 등록해야 합니다.

1. 테스트를 다시 실행하면 또 실패하나, 이번에는 실패 이유가 명확하게 표시됩니다.

   ![테스트 이름 실패 어설션](assets/unit-testing/testgetname-failure-assertion.png)

   *어설션으로 인한 testGetName() 실패*

   테스트의 삽입 조건이 실패했음을 의미하는 **AssertionError**&#x200B;가 표시되며 이 오류는 **예상 값이 &#39;Jane Doe&#39;**&#x200B;이나 **실제 값이 null**&#x200B;임을 알려 줍니다. 이는 &#39;**name&#39;** 속성이 **BylineImplTest.json**&#x200B;의 **/content/byline** 리소스 정의를 모의하는 데 추가되지 않았기 때문입니다. 따라서 이 속성을 추가해 보겠습니다.

1. **BylineImplTest.json**&#x200B;을 업데이트하여 `"name": "Jane Doe".`를 정의합니다.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 테스트를 다시 실행하면 이제 **`testGetName()`**&#x200B;이 통과됩니다.

   ![테스트 이름 통과](assets/unit-testing/testgetname-pass.png)


## getOccupations() 테스트 {#testing-get-occupations}

좋습니다! 첫 번째 테스트가 통과되었습니다! 이제 `getOccupations()`를 테스트해 보겠습니다. 모의 컨텍스트의 초기화가 `@Before setUp()` 메서드에서 수행되었기 때문에 이 테스트 케이스에서 `getOccupations()`를 비롯한 모든 `@Test` 메서드를 사용할 수 있습니다.

이 메서드는 occupations 속성에 저장된 직업의 알파벳순 정렬(내림차순) 목록을 반환해야 한다는 점을 기억하시기 바랍니다.

1. **`testGetOccupations()`**&#x200B;를 다음과 같이 업데이트합니다.

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

   * **`List<String> expected`**&#x200B;는 예상 결과를 정의합니다.
   * **`ctx.currentResource`**&#x200B;는 /content/byline에 있는 모의 리소스 정의에 대해 컨텍스트를 평가할 현재 리소스를 설정합니다. 이렇게 하면 **BylineImpl.Java**&#x200B;가 모의 리소스의 컨텍스트에서 실행됩니다.
   * **`ctx.request().adaptTo(Byline.class)`**&#x200B;는 모의 Request 오브젝트에서 Byline Sling 모델을 적용하여 인스턴스화합니다.
   * **`byline.getOccupations()`**&#x200B;는 Byline Sling 모델 오브젝트에서 테스트하는 메서드인 `getOccupations()`을 호출합니다.
   * **`assertEquals(expected, actual)`**&#x200B;는 예상 목록이 실제 목록과 동일함을 확인합니다.

1. 위의 **`getName()`**&#x200B;과 마찬가지로, **BylineImplTest.json**&#x200B;은 직업을 정의하지 않으므로 이 테스트를 실행하면 실패합니다. 그 이유는 `byline.getOccupations()`가 빈 목록을 반환하기 때문입니다.

   **BylineImplTest.json**&#x200B;을 업데이트하여 직업 목록을 포함하도록 하고 직업이 알파벳이 아닌 순서로 설정되어 테스트에서 직업이 **`getOccupations()`**&#x200B;에 따라 알파벳순으로 정렬되도록 합니다.

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

1. 테스트를 실행해 보니 역시 통과했습니다! 직업을 분류한 게 맞는 것 같네요!

   ![Get Occupations 통과](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() 통과*

## isEmpty() 테스트 {#testing-is-empty}

테스트할 마지막 메서드는 **`isEmpty()`**&#x200B;입니다.

`isEmpty()` 테스트는 다양한 조건에 대한 테스트가 필요하기 때문에 흥미롭습니다. **BylineImpl.Java**&#x200B;의 `isEmpty()` 메서드를 검토할 때는 다음 조건을 충족해야 합니다.

* 이름이 비어 있으면 true를 반환
* 직업이 null이거나 비어 있는 경우 true를 반환
* 이미지가 null이거나 src URL이 없는 경우 true를 반환
* 이름, 직업 및 Image(src URL 포함)가 있는 경우 false 반환

따라서 당사는 이러한 테스트를 실행하기 위해 `BylineImplTest.json`의 각 특정 조건과 새로운 모의 리소스 구조를 테스트하는 테스트 방법을 만들어야 합니다.

이 검사를 통해 `getName()`, `getOccupations()` 및 `getImage()`가 비어 있을 때 테스트를 건너뛸 수 있었습니다. 해당 상태의 예상 동작이 `isEmpty()`를 통해 테스트되기 때문입니다.

1. 첫 번째 테스트는 속성이 설정되지 않은 새 구성 요소의 상태를 테스트합니다.

   `BylineImplTest.json`에 새 리소스 정의를 추가하고 의미적론적인 이름을 &#39;**빈**&#39;으로 지정합니다.

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

   **`"empty": {...}`**&#x200B;는 `jcr:primaryType` 및 `sling:resourceType`만 있으며 이름이 &#39;empty&#39;인 새로운 리소스 정의를 정의합니다.

   각 테스트 메서드를 실행하기 전에 `@setUp`에서 `BylineImplTest.json`을 `ctx`로 로드한다는 점을 기억하십시오. 이렇게 해야 테스트 시 **/content/empty**&#x200B;에서 새로운 리소스 정의를 즉각 사용할 수 있습니다.

1. `testIsEmpty()`를 다음과 같이 업데이트하고 현재 리소스를 새로운 &#39;**빈**&#39; 모의 리소스 정의로 설정합니다.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   테스트를 실행하여 통과하는지 확인합니다.

1. 다음으로 필수 데이터 포인트(이름, 직업 또는 이미지)가 비어 있는 경우 `isEmpty()`가 true를 반환하는 메서드 세트를 만듭니다.

   각 테스트에서는 별도의 모의 리소스 정의가 사용됩니다. **BylineImplTest.json**&#x200B;은 **이름-없음** 및 **직업-없음**&#x200B;에 대한 추가 리소스 정의로 업데이트합니다.

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

   다음 테스트 메서드를 만들어 각 상태를 테스트합니다.

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

   **`testIsEmpty()`**&#x200B;는 빈 모의 리소스 정의에 대해 테스트를 수행하고 `isEmpty()`가 참인지 확인합니다.

   **`testIsEmpty_WithoutName()`**&#x200B;는 직업은 있지만 이름이 없는 모의 리소스 정의에 대해 테스트를 수행합니다.

   **`testIsEmpty_WithoutOccupations()`**&#x200B;는 이름은 있지만 직업이 없는 모의 리소스 정의에 대해 테스트를 수행합니다.

   **`testIsEmpty_WithoutImage()`**&#x200B;는 이름과 직업이 있는 모의 리소스 정의에 대해 테스트를 수행하지만 null을 반환하도록 모의 Image를 설정합니다. 이 호출에서 반환된 Image 오브젝트가 null이 되도록 하려면 `setUp()`에 정의된 `modelFactory.getModelFromWrappedRequest(..)` 동작을 재정의해야 합니다. Mockito 스텁 기능은 엄격하며 중복 코드를 원하지 않습니다. 따라서 당사는 `setUp()` 메서드에서 동작을 재정의한다는 것을 명시적으로 나타내는 **`lenient`** 설정을 사용하여 모의를 설정합니다.

   **`testIsEmpty_WithoutImageSrc()`**&#x200B;는 이름과 직업을 포함한 모의 리소스 정의에 대한 테스트를 수행하지만 `getSrc()`가 호출되면 모의 Image가 빈 문자열을 반환하도록 설정합니다.

1. 마지막으로, 구성 요소가 올바르게 구성된 경우 **isEmpty()**&#x200B;가 false를 반환하는지 확인하는 테스트를 작성합니다. 이러한 조건에서는 완전히 구성된 Byline 구성 요소를 나타내는 **/content/byline**&#x200B;을 재사용할 수 있습니다.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 이제 BylineImplTest.Java 파일에서 모든 단위 테스트를 실행하고 Java™ 테스트 보고서 출력을 검토합니다.

![모든 테스트 통과](./assets/unit-testing/all-tests-pass.png)

## 빌드의 일부로 단위 테스트 실행 {#running-unit-tests-as-part-of-the-build}

단위 테스트가 실행되며 Maven 빌드의 일부로 통과해야 합니다. 이렇게 하면 애플리케이션이 배포되기 전에 모든 테스트가 성공적으로 통과되는지 확인할 수 있습니다. 패키지나 설치와 같은 Maven 목표를 실행하면 프로젝트의 모든 단위 테스트가 자동으로 호출되고 통과되어야 합니다.

```shell
$ mvn package
```

![mvn 패키지 성공](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

마찬가지로, 테스트 메서드를 실패로 변경하면 빌드가 실패하고 어떤 테스트가 실패했는지와 그 이유가 보고됩니다.

![mvn 패키지 실패](assets/unit-testing/mvn-package-fail.png)

## 코드 검토 {#review-the-code}

완성된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd)에서 확인하거나 로컬로 `tutorial/unit-testing-solution` Git 분기에서 코드를 검토 및 배포할 수 있습니다.
