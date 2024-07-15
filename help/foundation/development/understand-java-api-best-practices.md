---
title: Java&trade; AEM의 API 모범 사례
description: AEM은 개발 중에 사용할 수 있도록 많은 Java&trade; API를 노출하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축되었습니다. 이 문서에서는 주요 API와 이러한 API를 사용해야 하는 시기 및 이유를 살펴봅니다.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 416
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 0%

---

# Java™ API 모범 사례

Adobe Experience Manager(AEM)는 개발 중에 사용할 수 있도록 많은 Java™ API를 노출하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축됩니다. 이 문서에서는 주요 API와 이러한 API를 사용해야 하는 시기 및 이유를 살펴봅니다.

AEM은 4개의 기본 Java™ API 세트를 기반으로 구축됩니다.

* **Adobe Experience Manager(AEM)**

   * 페이지, 에셋, 워크플로 등과 같은 제품 추상화

* **Apache Sling 웹 프레임워크**

   * 리소스, 값 맵 및 HTTP 요청과 같은 REST 및 리소스 기반 추상화입니다.

* **JCR(Apache Jackrabbit Oak)**

   * 노드, 속성 및 세션과 같은 데이터 및 콘텐츠 추상화입니다.

* **OSGi(Apache Felix)**

   * 서비스 및 (OSGi) 구성 요소와 같은 OSGi 애플리케이션 컨테이너 추상.

## Java™ API 환경 설정 &quot;경험에 근거한 규칙&quot;

일반적인 규칙은 다음 순서로 API/추상화를 선호하는 것입니다.

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

AEM에서 API를 제공하는 경우 [!DNL Sling], JCR 및 OSGi보다 선호합니다. AEM에서 API를 제공하지 않는 경우 JCR 및 OSGi보다 [!DNL Sling]을(를) 선호합니다.

이 순서는 일반적인 규칙이며, 예외가 존재함을 의미합니다. 이 규칙을 벗어나는 데 사용할 수 있는 이유는 다음과 같습니다.

* 아래에 설명된 대로 잘 알려진 예외 사항입니다.
* 필요한 기능은 더 높은 수준의 API에서 사용할 수 없습니다.
* 자체 선호도가 낮은 API를 사용하는 기존 코드(사용자 지정 또는 AEM 제품 코드) 컨텍스트에서 작동하고 새 API로 이동하는 비용은 정당하지 않습니다.

   * 혼합을 만드는 것보다 더 낮은 수준의 API를 일관되게 사용하는 것이 더 좋습니다.

## AEM API

* [**AEM API JavaDocs**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM API는 제품화된 사용 사례와 관련된 추상화 및 기능을 제공합니다.

예를 들어 AEM의 [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 및 [Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API는 웹 페이지를 나타내는 AEM의 `cq:Page` 노드에 대한 추상을 제공합니다.

[!DNL Sling] API를 리소스로 사용하고 JCR API를 노드로 사용하여 이러한 노드를 사용할 수 있지만, AEM의 API는 일반적인 사용 사례에 대한 추상을 제공합니다. AEM API를 사용하면 AEM 제품과 AEM에 대한 사용자 지정 및 확장 간에 일관된 동작이 보장됩니다.

### com.adobe.&#42;과(와) com.day 비교.&#42;개 API

AEM API에는 기본 설정 순서로 다음 Java™ 패키지로 식별되는 패키지 내 기본 설정이 있습니다.

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 패키지는 제품 사용 사례를 지원하는 반면 `com.adobe.granite`은(는) 워크플로 또는 작업(AEM Assets, Sites 등 제품에서 사용됨)과 같은 제품 간 플랫폼 사용 사례를 지원합니다.

`com.day.cq` 패키지에 &quot;원본&quot; API가 포함되어 있습니다. 이러한 API는 Adobe이 [!DNL Day CQ]을(를) 획득하기 전 및/또는 주변에서 존재했던 핵심 추상 및 기능을 해결합니다. `com.adobe.cq` 또는 `com.adobe.granite` 패키지가 (최신) 대체 요소를 제공하지 않는 한 이러한 API는 지원되며 사용하지 않아야 합니다.

[!DNL Content Fragments] 및 [!DNL Experience Fragments]과(와) 같은 새 추상이 아래 설명된 `com.day.cq`이(가) 아닌 `com.adobe.cq` 공간에 작성되었습니다.

### 쿼리 API

AEM은 여러 쿼리 언어를 지원합니다. 세 가지 주요 언어는 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath 및 [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)입니다.

가장 중요한 문제는 코드 베이스 전반에 걸쳐 일관된 쿼리 언어를 유지함으로써 복잡성과 이해 비용을 줄이는 것입니다.

[!DNL Apache Oak]이(가) 최종 쿼리 실행을 위해 JCR-SQL2에 트랜스-더미하므로 모든 쿼리 언어에는 효과적으로 동일한 성능 프로필이 있으며 JCR-SQL2로의 전환 시간은 쿼리 시간 자체에 비해 무시할 수 있습니다.

기본 API는 최상위 수준의 추상화이며 쿼리에 대한 결과를 구성, 실행 및 검색하는 강력한 API를 제공하는 [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)이며, 다음을 제공합니다.

* 간단한 매개 변수가 있는 쿼리 구문(맵으로 모델링된 쿼리 매개 변수)
* 기본 [Java™ API 및 HTTP API](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* [AEM 쿼리 디버거](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* 일반적인 쿼리 요구 사항을 지원하는 [AEM 조건자](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html)

* 사용자 지정 [쿼리 조건자](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)의 개발을 허용하는 확장 가능한 API
* JCR-SQL2 및 XPath는 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) 및 [JCR API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)를 통해 직접 실행할 수 있으며 각각 [[!DNL Sling] 리소스](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 또는 [JCR 노드](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html)를 반환합니다.

>[!CAUTION]
>
>AEM QueryBuilder API가 ResourceResolver 개체를 누출합니다. 이 누출을 완화하려면 이 [코드 샘플](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)을 따르십시오.
>

## [!DNL Sling]개 API

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/)은(는) AEM의 기반이 되는 RESTful 웹 프레임워크입니다. [!DNL Sling]은(는) HTTP 요청 라우팅을 제공하고, JCR 노드를 리소스로 모델링하고, 보안 컨텍스트를 제공하는 등의 다양한 기능을 제공합니다.

[!DNL Sling] API에는 확장용으로 빌드되는 추가적인 이점이 있습니다. 즉, 확장 가능성이 낮은 JCR API보다 [!DNL Sling] API를 사용하여 빌드된 응용 프로그램의 동작을 더 쉽고 안전하게 늘릴 수 있습니다.

### [!DNL Sling] API의 일반적인 사용

* JCR 노드에 [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)(으)로 액세스하고 [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)를 통해 해당 데이터에 액세스합니다.

* [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)을(를) 통해 보안 컨텍스트를 제공하고 있습니다.
* ResourceResolver의 [만들기/이동/복사/삭제 메서드](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)를 통해 리소스를 만들고 제거하는 중입니다.
* [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)을(를) 통해 속성을 업데이트하는 중입니다.
* 빌딩 요청 처리 빌딩 블록

   * [서블릿](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [서블릿 필터](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 비동기 작업 처리 빌딩 블록

   * [이벤트 및 작업 처리기](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [스케줄러](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)

* [서비스 사용자](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR(Java™ Content Repository) 2.0 API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)는 JCR 구현 사양의 일부입니다(AEM의 경우 [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). 모든 JCR 구현은 이러한 API를 준수하고 구현해야 하며, 따라서 AEM 콘텐츠와 상호 작용하기 위한 가장 낮은 수준의 API입니다.

JCR 자체는 AEM이 컨텐츠 저장소로 사용하는 계층/트리 기반의 NoSQL 데이터 저장소입니다. JCR에는 콘텐츠 CRUD에서 콘텐츠 쿼리에 이르기까지 지원되는 다양한 API가 있습니다. 이러한 강력한 API에도 불구하고 높은 수준의 AEM 및 [!DNL Sling] 추상화보다 선호되는 경우는 거의 없습니다.

항상 Apache Jackrabbit Oak API보다 JCR API를 선호합니다. JCR API는 JCR 리포지토리와 ***상호 작용***&#x200B;하는 데 사용되는 반면, Oak API는 JCR 리포지토리를 ***구현***&#x200B;하는 데 사용됩니다.

### JCR API에 대한 일반적인 오해

JCR은 AEM의 콘텐츠 저장소이지만 해당 API는 콘텐츠와 상호 작용하기 위한 기본 방법이 아닙니다. 대신 더 나은 추상화를 제공할 수 있으므로 AEM API(페이지, Assets, 태그 등) 또는 Sling 리소스 API를 선호합니다.

>[!CAUTION]
>
>AEM 애플리케이션에서 JCR API의 세션 및 노드 인터페이스를 광범위하게 사용하는 것은 코드 효과입니다. 대신 [!DNL Sling] API를 사용해야 합니다.

### JCR API의 일반적인 사용

* [액세스 제어 관리](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [승인 가능한 관리(사용자/그룹)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR 관찰(JCR 이벤트 수신)
* 딥 노드 구조 만들기

   * Sling API는 리소스 생성을 지원하지만 [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) 및 [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html)에서 딥 구조 생성을 빠르게 하는 편리한 메서드가 있습니다.

## OSGi API

* [**OSGi R6 JavaDocs**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi 선언 서비스 1.2 구성 요소 주석 JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi 선언 서비스 1.2 메타타입 주석 JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi 프레임워크 JavaDocs**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API와 상위 수준 API(AEM, [!DNL Sling] 및 JCR) 간에 겹치는 부분이 거의 없으며, OSGi API를 사용해야 하는 경우는 드물어 높은 수준의 AEM 개발 전문 지식이 필요합니다.

### OSGi와 Apache Felix API 비교

OSGi는 모든 OSGi 컨테이너가 구현하고 준수해야 하는 사양을 정의합니다. AEM의 OSGi 구현인 Apache Felix는 몇 가지 자체 API도 제공합니다.

* Apache Felix API(`org.apache.felix`)보다 OSGi API(`org.osgi`)를 선호합니다.

### OSGi API의 일반적인 사용

* OSGi 서비스 및 구성 요소를 선언하기 위한 OSGi 주석입니다.

   * OSGi 서비스 및 구성 요소를 선언하기 위해 [OSGi DS(선언 서비스) 1.2 주석 선호](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) [Felix SCR 주석](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html)보다 큼

* 동적으로 코드 내 [실행/OSGi 서비스/구성 요소 등록](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)을 위한 OSGi API입니다.

   * 조건부 OSGi 서비스/구성 요소 관리가 필요하지 않은 경우(대부분의 경우) OSGi DS 1.2 주석의 사용을 선호합니다.

## 규칙에 대한 예외

다음은 위에서 정의한 규칙에 대한 일반적인 예외입니다.

### OSGi API

OSGi 구성 요소 속성에서 정의하거나 읽는 것과 같은 낮은 수준의 OSGi 추상을 처리할 때, `org.osgi`에서 제공하는 최신 추상이 높은 수준의 Sling 추정보다 선호됩니다. 경쟁하는 Sling 추상이 `@Deprecated`(으)로 표시되지 않았으며 `org.osgi` 대체 요소를 제안합니다.

또한 OSGi 구성 노드 정의는 `sling:OsgiConfig` 형식보다 `cfg.json`을(를) 선호합니다.

### AEM Asset API

* [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html)보다 [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html)을(를) 선호합니다.

   * `com.day.cq` Assets API는 AEM의 에셋 관리 사용 사례에 더 많은 무료 도구를 제공합니다.
   * Granite Assets API는 낮은 수준의 에셋 관리 사용 사례(버전, 관계)를 지원합니다.

### 쿼리 API

* AEM QueryBuilder는 [제안](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), 맞춤법 검사 및 인덱스 힌트와 같은 덜 일반적인 함수 중 특정 쿼리 함수를 지원하지 않습니다. 이러한 함수로 쿼리하려면 JCR-SQL2가 좋습니다.

### [!DNL Sling] 서블릿 등록 {#sling-servlet-registration}

* [!DNL Sling] 서블릿 등록, `@SlingServlet`보다 [@SlingServletResourceTypesOSGi DS 1.2 주석을 선호함](https://sling.apache.org/documentation/the-sling-engine/servlets.html)

### [!DNL Sling] 필터 등록 {#sling-filter-registration}

* [!DNL Sling] 필터 등록, `@SlingFilter`보다 [OSGi DS 1.2 주석을 @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html)하세요.

## 유용한 코드 조각

다음은 논의된 API를 사용하는 일반적인 사용 사례에 대한 모범 사례를 보여 주는 유용한 Java™ 코드 조각입니다. 이러한 스니펫은 선호도가 낮은 API에서 선호도가 높은 API로 이동하는 방법을 보여 줍니다.

### [!DNL Sling] ResourceResolver에 대한 JCR 세션

#### Sling ResourceResolver 자동 닫기

AEM 6.2 이후 [!DNL Sling] ResourceResolver는 [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 문에서 `AutoClosable`입니다. 이 구문을 사용하면 `resourceResolver .close()`에 대한 명시적 호출이 필요하지 않습니다.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### Sling ResourceResolver를 수동으로 닫음

위에 표시된 자동 닫기 기술을 사용할 수 없는 경우 `finally` 블록에서 ResourceResolver를 수동으로 닫아야 합니다.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### [!DNL Sling] [!DNL Resource]에 대한 JCR 경로

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### [!DNL Sling] [!DNL Resource]에 대한 JCR 노드

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### AEM 자산에 대한 [!DNL Sling] [!DNL Resource]

#### 권장 접근 방식

`DamUtil.resolveToAsset(..)` 함수는 필요에 따라 트리 위로 이동하여 `dam:Asset` 아래의 모든 리소스를 에셋 개체로 확인합니다.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 대안적 접근

리소스를 자산에 적용하려면 리소스 자체가 `dam:Asset` 노드여야 합니다.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### AEM 페이지에 대한 [!DNL Sling] 리소스

#### 권장 접근 방식

`pageManager.getContainingPage(..)`은(는) 필요에 따라 트리 위로 이동하여 `cq:Page` 아래의 모든 리소스를 페이지 개체로 확인합니다.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 대안적 접근 {#alternative-approach-1}

리소스를 페이지에 적용하려면 리소스 자체가 `cq:Page` 노드여야 합니다.

```java
Page page = resource.adaptTo(Page.class);
```

### AEM 페이지 속성 읽기

Page 개체의 getter를 사용하여 잘 알려진 속성(`getTitle()`, `getDescription()` 등)을 가져오고 `page.getProperties()`을(를) 사용하여 다른 속성을 검색하기 위한 `[cq:Page]/jcr:content` ValueMap을 가져옵니다.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM Asset 메타데이터 속성 읽기

Asset API는 `[dam:Asset]/jcr:content/metadata` 노드에서 속성을 읽는 데 편리한 방법을 제공합니다. 이 매개 변수는 ValueMap이 아니며 두 번째 매개 변수(기본값 및 자동 유형 캐스팅)는 지원되지 않습니다.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### [!DNL Sling] [!DNL Resource] 속성 읽기 {#read-sling-resource-properties}

AEM API(Page, Asset)가 직접 액세스할 수 없는 위치(속성 또는 상대 리소스)에 속성이 저장되면 [!DNL Sling] 리소스 및 ValueMaps를 사용하여 데이터를 가져올 수 있습니다.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

이 경우 원하는 속성이나 하위 리소스를 효율적으로 찾으려면 AEM 개체를 [!DNL Sling] [!DNL Resource](으)로 변환해야 할 수 있습니다.

#### [!DNL Sling] [!DNL Resource](으)로 페이지 AEM

```java
Resource resource = page.adaptTo(Resource.class);
```

#### 자산을 [!DNL Sling] [!DNL Resource](으)로 AEM

```java
Resource resource = asset.adaptTo(Resource.class);
```

### [!DNL Sling]의 ModifiableValueMap을 사용하여 속성을 씁니다.

[!DNL Sling]의 [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)을(를) 사용하여 노드에 속성을 씁니다. 직접 실행 노드에만 쓸 수 있습니다(상대 속성 경로는 지원되지 않음).

`.adaptTo(ModifiableValueMap.class)`을(를) 호출하려면 리소스에 대한 쓰기 권한이 필요하지만 그렇지 않으면 null을 반환합니다.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEM 페이지 만들기

AEM에서 페이지를 제대로 정의하고 초기화하는 데 필요한 페이지 템플릿 을 사용할 때 항상 페이지 관리자를 사용하여 페이지를 만듭니다.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### [!DNL Sling] 리소스 만들기

ResourceResolver는 리소스를 만드는 기본 작업을 지원합니다. 상위 수준 추상화(AEM Pages, Assets, Tags 등)를 만들 때는 해당 관리자가 제공하는 메서드를 사용합니다.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### [!DNL Sling] 리소스 삭제

ResourceResolver가 리소스 제거를 지원합니다. 상위 수준 추상화(AEM Pages, Assets, Tags 등)를 만들 때는 해당 관리자가 제공하는 메서드를 사용합니다.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
