---
title: 단위 테스트
description: 사용자 지정 구성 요소 자습서에서 만든 Byline 구성 요소의 Sling 모델 동작을 확인하는 단위 테스트를 구현합니다.
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
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# 단위 테스트 {#unit-testing}

이 자습서에서는 [사용자 지정 구성 요소](./custom-component.md) 자습서에서 만든 Byline 구성 요소의 Sling 모델 동작의 유효성을 검사하는 단위 테스트의 구현을 다룹니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

_시스템에 Java™ 8과 Java™ 11이 모두 설치되어 있으면 VS 코드 테스트 실행기가 테스트를 실행할 때 더 낮은 Java™ 런타임을 선택하여 테스트 실패를 초래할 수 있습니다. 이 경우 Java™ 8._&#x200B;을 제거합니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 재사용하고 스타터 프로젝트 체크 아웃 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 코드 체크 아웃:

1. [GitHub](https://github.com/adobe/aem-guides-wknd)에서 `tutorial/unit-testing-start` 분기 확인

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

[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start)에서 완성된 코드를 항상 보거나 `tutorial/unit-testing-start` 분기로 전환하여 코드를 로컬로 확인할 수 있습니다.

## 목표

1. 단위 테스트의 기본 사항을 이해합니다.
1. AEM 코드를 테스트하는 데 일반적으로 사용되는 프레임워크 및 도구에 대해 알아봅니다.
1. 단위 테스트를 작성할 때 AEM 리소스를 비웃거나 시뮬레이션하는 옵션을 이해합니다.

## 배경 {#unit-testing-background}

이 자습서에서는 Byline 구성 요소의 [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)&#x200B;([사용자 지정 AEM 구성 요소 만들기](custom-component.md)에서 생성됨)에 대해 [단위 테스트](https://en.wikipedia.org/wiki/Unit_testing)를 작성하는 방법을 알아봅니다. 단위 테스트는 Java™으로 작성된 빌드 시간 테스트로서 Java™ 코드의 예상 동작을 확인합니다. 각 단위 테스트는 일반적으로 작으며 예상 결과에 대해 방법(또는 작업 단위)의 출력을 확인합니다.

AEM 우수 사례를 사용하고 다음을 사용합니다.

* [JUnit 5](https://junit.org/junit5/)
* [모키토 테스트 프레임워크](https://site.mockito.org/)
* [wcm.io 테스트 프레임워크](https://wcm.io/testing/)&#x200B;([Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html)에서 빌드)

## 장치 테스트 및 Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html)은(는) 단위 테스트 실행 및 [코드 검사 보고](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html)를 CI/CD 파이프라인에 통합하여 단위 테스트 AEM 코드의 모범 사례를 장려하고 홍보하는 데 도움이 됩니다.

단위 테스트 코드는 모든 코드 기반에 좋은 방법이지만, Cloud Manager을 사용할 때는 Cloud Manager이 실행할 수 있는 단위 테스트를 제공하여 코드 품질 테스트 및 보고 기능을 활용하는 것이 중요합니다.

## 테스트 Maven 종속성 업데이트 {#inspect-the-test-maven-dependencies}

첫 번째 단계는 테스트 작성 및 실행을 지원하기 위해 Maven 종속성을 검사하는 것입니다. 다음 네 가지 종속성이 필요합니다.

1. JUnit5
1. 모키토 테스트 프레임워크
1. Apache Sling 모크스
1. AEM Mocks 테스트 프레임워크(io.wcm 기준)

**JUnit5**, **Mockito 및 &#x200B;** AEM Mocks** 테스트 종속성은 [AEM Maven Archetype](project-setup.md)을(를) 사용하여 설정하는 동안 프로젝트에 자동으로 추가됩니다.

1. 이러한 종속성을 보려면 **aem-guides-wknd/pom.xml**&#x200B;에서 상위 Reactor POM을 열고 `<dependencies>..</dependencies>`(으)로 이동한 다음 `<!-- Testing -->`에서 io.wcm을 통해 JUnit, Mockito, Apache Sling Mocks 및 AEM Mock Tests에 대한 종속성을 확인하십시오.
1. `io.wcm.testing.aem-mock.junit5`이(가) **4.1.0**(으)로 설정되어 있는지 확인하십시오.

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
   > Archetype **35**&#x200B;은(는) `io.wcm.testing.aem-mock.junit5` 버전 **4.1.8**&#x200B;의 프로젝트를 생성합니다. 이 장의 나머지 부분을 따르려면 **4.1.0**(으)로 다운그레이드하십시오.

1. **aem-guides-wknd/core/pom.xml**&#x200B;을(를) 열고 해당 테스트 종속성을 사용할 수 있는지 확인합니다.

   **core** 프로젝트의 병렬 소스 폴더에는 단위 테스트와 모든 지원 테스트 파일이 포함됩니다. 이 **test** 폴더에서는 테스트 클래스를 소스 코드로부터 분리하지만 테스트가 소스 코드와 동일한 패키지에 있는 것처럼 작동할 수 있습니다.

## JUnit 테스트 만들기 {#creating-the-junit-test}

단위 테스트는 일반적으로 Java™ 클래스를 사용하여 1대 1을 매핑합니다. 이 장에서는 Byline 구성 요소를 지원하는 Sling 모델인 **BylineImpl.java**&#x200B;에 대한 JUnit 테스트를 작성합니다.

![테스트 src 폴더 단위](assets/unit-testing/core-src-test-folder.png)

*단위 테스트가 저장되는 위치입니다.*

1. 테스트할 Java™ 클래스의 위치를 미러링하는 Java™ 패키지 폴더 구조의 `src/test/java` 아래에 새 Java™ 클래스를 만들어 `BylineImpl.java`에 대한 단위 테스트를 만드십시오.

   ![새 BylineImplTest.java 파일 만들기](assets/unit-testing/new-bylineimpltest.png)

   테스트 중이므로

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   에서 해당 단위 테스트 Java™ 클래스를 만듭니다.

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   단위 테스트 파일 `BylineImplTest.java`의 `Test` 접미사는 규칙이므로 다음을 수행할 수 있습니다.

   1. 테스트 파일 _for_ `BylineImpl.java`(으)로 쉽게 식별
   1. 테스트 파일 _과(와) 테스트 중인 클래스_&#x200B;을(를) 구별합니다. `BylineImpl.java`

## BylineImplTest.java 검토 {#reviewing-bylineimpltest-java}

이 시점에서 JUnit 테스트 파일은 빈 Java™ 클래스입니다.

1. 다음 코드를 사용하여 파일을 업데이트합니다.

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

1. 첫 번째 메서드 `public void setUp() { .. }`에는 이 클래스의 각 테스트 메서드를 실행하기 전에 JUnit 테스트 실행자에게 이 메서드를 실행하도록 지시하는 JUnit의 `@BeforeEach` 주석이 추가됩니다. 이렇게 하면 모든 테스트에 필요한 일반적인 테스트 상태를 초기화할 수 있는 편리한 위치를 제공합니다.

1. 후속 메서드는 규칙에 따라 이름이 `test` 접두사로 사용되고 `@Test` 주석으로 표시되는 테스트 메서드입니다. 아직 구현하지 않았기 때문에 기본적으로 모든 테스트가 실패하도록 설정됩니다.

   먼저 테스트 중인 클래스의 각 공용 메서드에 대한 단일 테스트 메서드로 시작하므로

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 은(는) 다음을 통해 테스트함: | testGetName() |
   | getOccuptions() | 은(는) 다음을 통해 테스트함: | testGetOccuptions() |
   | isEmpty() | 은(는) 다음을 통해 테스트함: | testIsEmpty() |

   이 방법은 이 장의 뒷부분에서 살펴보겠지만 필요에 따라 확장될 수 있습니다.

   이 JUnit 테스트 클래스(JUnit 테스트 사례라고도 함)가 실행되면 `@Test`(으)로 표시된 각 메서드가 통과 또는 실패할 수 있는 테스트로 실행됩니다.

![생성된 BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. `BylineImplTest.java` 파일을 마우스 오른쪽 단추로 클릭하고 **실행**&#x200B;을 탭하여 JUnit 테스트 사례를 실행합니다.
예상대로 모든 테스트가 아직 구현되지 않아 실패합니다.

   ![junit 테스트로 실행](assets/unit-testing/run-junit-tests.png)

   *BylineImplTests.java를 마우스 오른쪽 단추로 클릭 > 실행*

## BylineImpl.java 검토 {#reviewing-bylineimpl-java}

단위 테스트를 작성할 때 두 가지 기본 접근 방식이 있습니다.

* [TDD 또는 테스트 기반 개발](https://en.wikipedia.org/wiki/Test-driven_development): 단위 테스트를 점진적으로 작성하는 작업이 포함된 것으로, 구현이 개발되기 직전에 테스트를 작성하고 구현을 작성하여 테스트를 통과해야 합니다.
* 구현 우선 개발(먼저 작업 코드를 개발한 다음 해당 코드의 유효성을 검사하는 테스트를 작성하는 작업 포함).

이 자습서에서는 후자의 접근 방식이 사용됩니다(이전 장에서 작업 **BylineImpl.java**&#x200B;을(를) 이미 만들었으므로). 이러한 점 때문에, 우리는 그것의 공적인 방법 뿐만 아니라 그것의 구현 세부사항 중 일부를 검토하고 이해해야 한다. 좋은 테스트는 입력과 출력에만 신경 써야 하므로 이는 반대로 들릴 수 있지만 AEM에서 작업할 때 작업 테스트를 구성하기 위해 이해해야 하는 다양한 구현 고려 사항이 있습니다.

AEM의 맥락에서 TDD는 일정 수준의 전문성을 필요로 하며, AEM 개발 및 AEM 코드의 단위 테스트에 능숙한 AEM 개발자가 가장 잘 채택합니다.

## AEM 테스트 컨텍스트 설정  {#setting-up-aem-test-context}

AEM용으로 작성된 대부분의 코드는 JCR, Sling 또는 AEM API에 의존하며, 이를 위해서는 실행 중인 AEM의 컨텍스트가 제대로 실행되어야 합니다.

단위 테스트는 실행 중인 AEM 인스턴스의 컨텍스트 외부에 있는 빌드 시 실행되므로 이러한 컨텍스트는 없습니다. 이를 용이하게 하기 위해 [wcm.io의 AEM AEM Mocks](https://wcm.io/testing/aem-mock/usage.html)는 이러한 API가 _대부분_&#x200B;에서 실행되는 것처럼 작동하도록 하는 모의 컨텍스트를 만듭니다.

1. **BylineImplTest.java** 파일에 `@ExtendWith`(으)로 데코레이트된 JUnit 확장명으로 추가하여 **BylineImplTest.java**&#x200B;의 **wcm.io** `AemContext`을(를) 사용하여 AEM 컨텍스트를 만듭니다. 확장은 필요한 모든 초기화 및 정리 작업을 처리합니다. 모든 테스트 메서드에 사용할 수 있는 `AemContext`의 클래스 변수를 만듭니다.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   이 변수 `ctx`은(는) 일부 AEM 및 Sling 추상을 제공하는 모의 AEM 컨텍스트를 노출합니다.

   * BylineImpl Sling 모델이 이 컨텍스트에 등록됩니다.
   * 모의 JCR 콘텐츠 구조는 이 컨텍스트에서 작성됩니다
   * 이 컨텍스트에서는 사용자 정의 OSGi 서비스를 등록할 수 있습니다
   * SlingHttpServletRequest 개체, ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag 등과 같은 다양한 모의 Sling 및 AEM OSGi 서비스와 같이 필요한 다양한 모의 개체와 도우미를 제공합니다.
      * *이 개체에 대해 일부 메서드가 구현되지 않았습니다!*
   * [훨씬 더](https://wcm.io/testing/aem-mock/usage.html)!

   **`ctx`** 개체는 대부분의 모의 컨텍스트에 대한 진입점 역할을 합니다.

1. 각 `@Test` 메서드보다 먼저 실행되는 `setUp(..)` 메서드에서 공통 모의 테스트 상태를 정의합니다.

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`**&#x200B;은(는) 테스트할 Sling 모델을 모의 AEM 컨텍스트에 등록하므로 `@Test` 메서드에서 인스턴스화할 수 있습니다.
   * **`load().json`**&#x200B;은(는) 리소스 구조를 모의 컨텍스트에 로드하므로 실제 리포지토리에서 제공한 것처럼 코드가 이러한 리소스와 상호 작용할 수 있습니다. **`BylineImplTest.json`** 파일의 리소스 정의가 **/content** 아래의 모의 JCR 컨텍스트에 로드되었습니다.
   * **`BylineImplTest.json`**&#x200B;이(가) 아직 없습니다. 있으므로 만들고 테스트에 필요한 JCR 리소스 구조를 정의하겠습니다.

1. 모의 리소스 구조를 나타내는 JSON 파일은 JUnit Java™ 테스트 파일과 동일한 패키지 경로 지정 후 **core/src/test/resources** 아래에 저장됩니다.

   `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl`에 다음 콘텐츠로 **BylineImplTest.json**&#x200B;이라는 JSON 파일을 만듭니다.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   이 JSON은 Byline 구성 요소 단위 테스트를 위한 모의 리소스(JCR 노드)를 정의합니다. 이때 JSON에는 Byline 구성 요소 콘텐츠 리소스 `jcr:primaryType` 및 `sling:resourceType`을(를) 나타내는 데 필요한 최소 속성 집합이 있습니다.

   단위 테스트를 사용하여 작업할 때 일반적인 규칙은 각 테스트를 충족하는 데 필요한 최소 모의 콘텐츠, 컨텍스트 및 코드 세트를 만드는 것입니다. 불필요한 아티팩트를 만드는 경우가 많으므로, 테스트를 작성하기 전에 완전한 모의 문맥을 구축하려는 유혹을 피하십시오.

   **BylineImplTest.json**&#x200B;이(가) 있는 상태에서 `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`을(를) 실행하면 모의 리소스 정의가 경로 **/콘텐츠의 컨텍스트에 로드됩니다.**

## getName() 테스트 {#testing-get-name}

기본 모의 컨텍스트 설정이 있으므로 **BylineImpl의 getName()**&#x200B;에 대한 첫 번째 테스트를 작성해 보겠습니다. 이 테스트에서는 **getName()** 메서드가 리소스의 &quot;**name&quot;** 속성에 저장된 올바른 작성 이름을 반환하는지 확인해야 합니다.

1. **BylineImplTest.java**&#x200B;에서 **testGetName**() 메서드를 다음과 같이 업데이트합니다.

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

   * **`String expected`**&#x200B;이(가) 예상 값을 설정합니다. 이 값은 &quot;**Jane Done**&quot;(으)로 설정됩니다.
   * **`ctx.currentResource`**&#x200B;은(는) 코드를 평가할 모의 리소스의 컨텍스트를 설정하므로 이 값은 **/content/byline**(으)로 설정되며 여기서 모의 byline 콘텐츠 리소스가 로드됩니다.
   * **`Byline byline`**&#x200B;은(는) 모의 요청 개체에서 Byline Sling 모델을 조정하여 인스턴스화합니다.
   * **`String actual`**&#x200B;은(는) Byline Sling Model 개체에서 테스트 중인 메서드 `getName()`을(를) 호출합니다.
   * **`assertEquals`**&#x200B;은(는) 예상 값이 byline Sling Model 개체에서 반환된 값과 일치함을 어설션합니다. 이 값이 같지 않으면 테스트가 실패합니다.

1. 테스트를 실행하면 `NullPointerException`(으)로 실패합니다.

   모의 JSON에 `name` 속성을 정의하지 않았으므로 이 테스트가 실패하지 않습니다. 이렇게 하면 테스트 실행이 해당 시점에 도달하지 않았더라도 테스트가 실패합니다. byline 개체 자체의 `NullPointerException`(으)로 인해 이 테스트가 실패합니다.

1. `BylineImpl.java`에서 `@PostConstruct init()`에서 예외를 throw하면 Sling 모델이 인스턴스화되지 못하고 해당 Sling 모델 개체가 null이 됩니다.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   ModelFactory OSGi 서비스가 Apache Sling 컨텍스트를 통해 `AemContext`을(를) 통해 제공되는 동안 BylineImpl의 `init()` 메서드에서 호출되는 `getModelFromWrappedRequest(...)`을(를) 포함하여 모든 메서드가 구현되지는 않습니다. 이렇게 하면 [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)이(가) 발생하고, 이로 인해 `init()`이(가) 실패하며, 결과 `ctx.request().adaptTo(Byline.class)`의 조정은 null 개체입니다.

   제공된 Mock이 코드를 수용할 수 없으므로 직접 Mock 컨텍스트를 구현해야 합니다. 이를 위해 Mockito를 사용하여 `getModelFromWrappedRequest(...)`이(가) 호출될 때 Mock Image 개체를 반환하는 Mock ModelFactory 개체를 만들 수 있습니다.

   Byline Sling 모델을 인스턴스화하려면 이 모의 컨텍스트가 있어야 하므로 `@Before setUp()` 메서드에 추가할 수 있습니다. **BylineImplTest** 클래스 위의 `@ExtendWith` 주석에도 `MockitoExtension.class`을(를) 추가해야 합니다.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`**&#x200B;은(는) @Mock 주석을 사용하여 클래스 수준에서 모의 개체를 정의할 수 있도록 하는 [Mockito JUnit Jupiter Extension](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html)으로 실행할 테스트 사례 클래스를 표시합니다.
   * **`@Mock private Image`**&#x200B;이(가) `com.adobe.cq.wcm.core.components.models.Image` 형식의 모의 개체를 만듭니다. 필요에 따라 `@Test` 메서드가 필요에 따라 동작을 변경할 수 있도록 클래스 수준에서 정의됩니다.
   * **`@Mock private ModelFactory`**&#x200B;이(가) ModelFactory 형식의 모의 개체를 만듭니다. 이것은 순수한 모키토 모의이며, 그 위에 구현된 방법이 없습니다. 필요에 따라 `@Test`메서드가 필요에 따라 동작을 변경할 수 있도록 클래스 수준에서 정의됩니다.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`**&#x200B;은(는) `getModelFromWrappedRequest(..)`이(가) mock ModelFactory 개체에서 호출될 때 모의 동작을 등록합니다. `thenReturn (..)`에 정의된 결과는 모의 이미지 개체를 반환하는 것입니다. 이 동작은 첫 번째 매개 변수가 `ctx`의 요청 개체와 같고, 두 번째 매개 변수가 임의의 Resource 개체이며, 세 번째 매개 변수가 Core Components Image 클래스여야 하는 경우에만 호출됩니다. 테스트 전체에서 `ctx.currentResource(...)`을(를) **BylineImplTest.json**&#x200B;에 정의된 다양한 모의 리소스로 설정하고 있으므로 모든 리소스를 허용합니다. 나중에 ModelFactory의 이 동작을 재정의하려고 하므로 **lenient()** 제한을 추가합니다.
   * **`ctx.registerService(..)`**&#x200B;이(가) 서비스 순위가 가장 높은 AemContext에 mock ModelFactory 개체를 등록합니다. BylineImpl의 `init()`에 사용된 ModelFactory가 `@OSGiService ModelFactory model` 필드를 통해 삽입되므로 이 작업이 필요합니다. AemContext에서 `getModelFromWrappedRequest(..)`에 대한 호출을 처리하는 **our** 모의 개체를 삽입하려면 해당 형식의 최상위 서비스(ModelFactory)로 등록해야 합니다.

1. 테스트를 다시 실행하고 실패해도 이번에는 메시지가 실패한 이유를 명확하게 알 수 있습니다.

   ![테스트 이름 오류 어설션](assets/unit-testing/testgetname-failure-assertion.png)

   어설션으로 인해 *testGetName() 실패*

   테스트에서 어설션 조건이 실패했음을 의미하는 **AssertionError**&#x200B;이(가) 수신되며 이는 **예상 값이 &quot;Jane Doe&quot;**&#x200B;이지만 **실제 값이 null임**&#x200B;을 알려줍니다. **BylineImplTest.json**&#x200B;의 mock **/content/byline** 리소스 정의에 &quot;**name&quot;** 속성이 추가되지 않았으므로 적절하므로 추가하겠습니다.

1. **BylineImplTest.json**&#x200B;을(를) 업데이트하여 `"name": "Jane Doe".` 정의

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 테스트를 다시 실행하면 **`testGetName()`**&#x200B;이(가) 통과됩니다!

   ![테스트 이름 통과](assets/unit-testing/testgetname-pass.png)


## getOccuptions() 테스트 {#testing-get-occupations}

좋습니다! 첫 번째 테스트가 통과되었습니다! `getOccupations()`을(를) 계속 테스트해 보겠습니다. 모의 컨텍스트의 초기화는 `@Before setUp()` 메서드에서 수행되었으므로 이 테스트 사례에서는 `getOccupations()`을(를) 포함하여 모든 `@Test` 메서드에서 사용할 수 있습니다.

이 메서드는 Occuptions 속성에 저장된 알파벳순으로 정렬된 작업 목록(내림차순)을 반환해야 합니다.

1. 다음과 같이 **`testGetOccupations()`**&#x200B;을(를) 업데이트합니다.

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

   * **`List<String> expected`**&#x200B;이(가) 예상 결과를 정의합니다.
   * **`ctx.currentResource`**&#x200B;은(는) /content/byline의 모의 리소스 정의에 대해 컨텍스트를 평가하는 현재 리소스를 설정합니다. 이렇게 하면 **BylineImpl.java**&#x200B;이(가) 모의 리소스의 컨텍스트에서 실행됩니다.
   * **`ctx.request().adaptTo(Byline.class)`**&#x200B;은(는) 모의 요청 개체에서 Byline Sling 모델을 조정하여 인스턴스화합니다.
   * **`byline.getOccupations()`**&#x200B;은(는) Byline Sling Model 개체에서 테스트 중인 메서드 `getOccupations()`을(를) 호출합니다.
   * **`assertEquals(expected, actual)`**&#x200B;이(가) 예상 목록이 실제 목록과 같다고 어설션합니다.

1. 위의 **`getName()`**&#x200B;과(와) 마찬가지로 **BylineImplTest.json**&#x200B;은(는) 작업을 정의하지 않으므로 `byline.getOccupations()`이(가) 빈 목록을 반환하므로 이 테스트를 실행하면 실패합니다.

   작업 목록을 포함하도록 **BylineImplTest.json**&#x200B;을(를) 업데이트하십시오. 이 작업은 비알파벳 순서로 설정되어 테스트에서 작업이 **`getOccupations()`**&#x200B;에 따라 알파벳순으로 정렬되어 있는지 확인합니다.

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

1. 테스트를 실행하면 다시 통과합니다! 정렬된 직업을 만드는 것이 효과가 있는 것 같습니다!

   ![작업 가져오기](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccuptions() 통과*

## isEmpty() 테스트 {#testing-is-empty}

**`isEmpty()`**&#x200B;을(를) 테스트할 마지막 메서드입니다.

`isEmpty()`을(를) 테스트하려면 다양한 조건에 대한 테스트가 필요하므로 흥미롭습니다. **BylineImpl.java**&#x200B;의 `isEmpty()` 메서드를 검토하고 다음 조건을 테스트해야 합니다.

* 이름이 비어 있으면 true 반환
* 작업이 null이거나 비어 있으면 true 반환
* 이미지가 null이거나 src URL이 없으면 true 반환
* 이름, 직업 및 이미지(src URL 포함)가 있으면 false를 반환합니다

이를 위해 각 테스트에서 특정 조건 및 새 모의 리소스 구조를 `BylineImplTest.json`에서 테스트하는 테스트 메서드를 만들어야 이러한 테스트를 수행할 수 있습니다.

이 검사를 통해 해당 상태의 예상 동작이 `isEmpty()`을(를) 통해 테스트되므로 `getName()`, `getOccupations()` 및 `getImage()`이(가) 비어 있는 경우 테스트를 건너뛸 수 있습니다.

1. 첫 번째 테스트는 속성이 설정되지 않은 완전히 새로운 구성 요소의 상태를 테스트합니다.

   의미 체계 이름 &quot;**empty**&quot;을(를) 지정하여 `BylineImplTest.json`에 새 리소스 정의를 추가하십시오.

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

   **`"empty": {...}`**&#x200B;은(는) `jcr:primaryType` 및 `sling:resourceType`만 포함하는 &quot;empty&quot;라는 새 리소스 정의를 정의합니다.

   `@setUp`에서 각 테스트 메서드를 실행하기 전에 `BylineImplTest.json`을(를) `ctx`에 로드하므로 **/content/empty의 테스트에서 이 새 리소스 정의를 즉시 사용할 수 있습니다.**

1. 현재 리소스를 새로운 &quot;**empty**&quot; 모의 리소스 정의로 설정하여 `testIsEmpty()`을(를) 다음과 같이 업데이트합니다.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   테스트를 실행하고 통과했는지 확인합니다.

1. 그런 다음 필요한 데이터 요소(이름, 직업 또는 이미지)가 비어 있으면 `isEmpty()`에서 true를 반환하도록 메서드 집합을 만듭니다.

   각 테스트의 경우 개별 모의 리소스 정의가 사용되며 **BylineImplTest.json**&#x200B;을(를) **이름 없음** 및 **작업 없음**&#x200B;에 대한 추가 리소스 정의로 업데이트합니다.

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

   다음 테스트 방법을 만들어 이러한 각 상태를 테스트합니다.

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

   **`testIsEmpty()`**&#x200B;이(가) 빈 모의 리소스 정의를 테스트하고 `isEmpty()`이(가) true임을 확인합니다.

   작업이 있지만 이름이 없는 모의 리소스 정의에 대해 **`testIsEmpty_WithoutName()`**&#x200B;을(를) 테스트합니다.

   **`testIsEmpty_WithoutOccupations()`**&#x200B;은(는) 이름이 있지만 직종이 없는 모의 리소스 정의에 대해 테스트합니다.

   **`testIsEmpty_WithoutImage()`**&#x200B;은(는) 이름과 직업을 가진 모의 리소스 정의에 대해 테스트하지만 모의 이미지가 null로 반환되도록 설정합니다. 이 호출에서 반환된 Image 개체가 null인지 확인하기 위해 `setUp()`에 정의된 `modelFactory.getModelFromWrappedRequest(..)` 동작을 재정의하려고 합니다. Mockito 스텁 기능은 엄격하며 중복 코드를 원하지 않습니다. 따라서 **`lenient`** 설정으로 모의를 설정하여 `setUp()` 메서드의 동작을 재정의하고 있음을 명시적으로 확인합니다.

   **`testIsEmpty_WithoutImageSrc()`**&#x200B;은(는) 이름 및 직업을 가진 모의 리소스 정의에 대해 테스트하지만 `getSrc()`을(를) 호출할 때 빈 문자열을 반환하도록 모의 이미지를 설정합니다.

1. 마지막으로, 구성 요소가 올바르게 구성되면 **isEmpty()**&#x200B;이(가) false를 반환하는지 확인하기 위한 테스트를 작성하십시오. 이 조건의 경우 완전히 구성된 Byline 구성 요소를 나타내는 **/content/byline**&#x200B;을(를) 다시 사용할 수 있습니다.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 이제 BylineImplTest.java 파일에서 모든 단위 테스트를 실행하고 Java™ 테스트 보고서 출력을 검토합니다.

![모든 테스트 통과](./assets/unit-testing/all-tests-pass.png)

## 빌드의 일부로 단위 테스트 실행 {#running-unit-tests-as-part-of-the-build}

단위 테스트가 실행되며 Maven 빌드의 일부로 통과해야 합니다. 이렇게 하면 응용 프로그램을 배포하기 전에 모든 테스트를 성공적으로 통과할 수 있습니다. 패키지 또는 설치와 같은 Maven 목표를 실행하면 자동으로 호출되며 프로젝트의 모든 단위 테스트를 통과해야 합니다.

```shell
$ mvn package
```

![mvn 패키지 성공](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

마찬가지로 테스트 방법을 실패로 변경하면 빌드가 실패하고 실패한 테스트와 이유를 보고합니다.

![mvn 패키지 실패](assets/unit-testing/mvn-package-fail.png)

## 코드 검토 {#review-the-code}

[GitHub](https://github.com/adobe/aem-guides-wknd)에서 완료된 코드를 보거나 Git 분기 `tutorial/unit-testing-solution`에서 로컬로 코드를 검토하고 배포합니다.
