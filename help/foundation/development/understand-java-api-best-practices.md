---
title: AEM에서 Java API 우수 사례 이해
description: AEM은 개발 중에 사용할 수 있는 많은 Java API를 노출하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축되었습니다. 이 문서에서는 주요 API의 사용 시기 및 이유를 설명합니다.
version: 6.2, 6.3, 6.4, 6.5
sub-product: foundation, assets, sites
feature: null
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
translation-type: tm+mt
source-git-commit: fcb47ee3878f6a789b2151e283431c4806e12564
workflow-type: tm+mt
source-wordcount: '2028'
ht-degree: 1%

---


# Java API 우수 사례 이해

Adobe Experience Manager(AEM)은 개발 중에 사용할 수 있는 많은 Java API를 노출하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축되었습니다. 이 문서에서는 주요 API의 사용 시기 및 이유를 설명합니다.

AEM은 4개의 기본 Java API 세트를 기반으로 구축되었습니다.

* **AEM(Adobe Experience Manager)**

   * 페이지, 자산, 워크플로우 등 제품 개요

* **[!DNL Apache Sling]웹 프레임워크**

   * 리소스, 값 맵 및 HTTP 요청과 같은 REST 및 리소스 기반 추상화

* **JCR([!DNL Apache Jackrabbit Oak])**

   * 노드, 속성 및 세션 등의 데이터 및 컨텐츠 추상적인 요소를 생성할 수 있습니다.

* **[!DNL OSGi (Apache Felix)]**

   * OSGi 애플리케이션 컨테이너는 서비스 및 (OSGi) 구성 요소와 같은 추상적 요소를 생성합니다.

## Java API 환경 설정 &quot;경험의 규칙&quot;

일반 규칙은 다음 순서에 따라 API/추상적인 방법을 선호합니다.

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

AEM에서 API를 제공하는 경우 JCR 및 OSGi보다 API를 [!DNL Sling]선호합니다. AEM에서 API를 제공하지 않는 경우 JCR 및 OSGi보다 [!DNL Sling] 선호합니다.

이 순서는 일반적인 규칙이며, 이것은 예외가 있다는 것을 의미합니다. 이 규칙에서 벗어나는 허용되는 이유는 다음과 같습니다.

* 잘 알려진 예외 사항(아래에 설명되어 있음).
* 더 높은 수준의 API에서는 필요한 기능을 사용할 수 없습니다.
* 선호도가 낮은 API를 사용하는 기존 코드(사용자 지정 또는 AEM 제품 코드)의 컨텍스트에서 작동하고, 새 API로 이동하는 비용은 불합리적입니다.

   * 혼합을 만드는 것보다 낮은 수준의 API를 일관되게 사용하는 것이 좋습니다.

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API는 제품 사용 사례와 관련된 추상적 기능과 기능을 제공합니다.

예를 들어 AEM [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) 및 [페이지](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) API는 AEM의 노드에 대한 추상적인 웹 페이지를 `cq:Page` 제공합니다.

이러한 노드는 [!DNL Sling] API를 통해 리소스로 사용할 수 있고 JCR API를 Nodes로 사용할 수 있지만 AEM API는 일반적인 사용 사례에 대한 추상적인 요소를 제공합니다. AEM API를 사용하면 AEM 제품 간의 일관된 비헤이비어와 사용자 정의 및 AEM 익스텐션이 보장됩니다.

### com.adobe.* vs com.day.* API

AEM API에는 다음과 같은 Java 패키지로 식별되는 패키지 내 환경 설정이 기본 설정 순서로 있습니다.

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 워크플로우 또는 작업(예: 제품 간 사용)과 같은 크로스 제품 플랫폼 사용 사례를 지원하는 반면, `com.adobe.granite` AEM Assets, 사이트 등).

`com.day.cq` 에는 &quot;원본&quot; API가 포함되어 있습니다. 이러한 API는 Adobe의 인수 전 및/또는 그 주위에 존재했던 핵심 추상 및 기능을 해결합니다 [!DNL Day CQ]. 이러한 API는 지원되며 (최신) 대체 요소를 `com.adobe.cq` 제공하거나 제공하지 않는 한, `com.adobe.granite` 피해야 합니다.

아래와 같은 새로운 추상 [!DNL Content Fragments] 이 [!DNL Experience Fragments] 아래 설명되지 않고 `com.adobe.cq` 공간 내에 `com.day.cq` 구축됩니다.

### 쿼리 API

AEM은 여러 쿼리 언어를 지원합니다. 3개의 기본 언어는 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath 및 [AEM Query Builder입니다](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

가장 중요한 문제는 코드 베이스에서 일관된 쿼리 언어를 유지하는 것으로 복잡성과 이해에 필요한 비용을 줄이는 것입니다.

모든 쿼리 언어는 JCR-SQL2로 [!DNL Apache Oak] 이동하여 최종 쿼리 실행을 위한 성능 프로필을 더하고, JCR-SQL2로의 변환 시간은 쿼리 시간 자체에 비해 미미합니다.

기본 API는 [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)로, 최고 수준의 추상화이며 쿼리에 대한 결과를 생성, 실행 및 검색하는 데 강력한 API를 제공하고 다음을 제공합니다.

* 간단하고 매개 변수화된 쿼리 구성(맵으로 모델링된 쿼리 매개 변수)
* 기본 [Java API 및 HTTP API](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTB 쿼리 디버거](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [OOOTB는](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 일반적인 쿼리 요구 사항을 지원하는 것을 예측합니다.

* 확장 가능한 API, 사용자 지정 [쿼리 예측](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 및 XPath는 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-)[및](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)JCR APIs [[!DNL Sling] 를 통해 직접 실행할 수 있으며, 결과는](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 리소스 [또는](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)JCR 노드(각각 JCR Nodes)를 반환합니다.

>[!CAUTION]
>
>AEM QueryBuilder API는 ResourceResolver 개체를 노출합니다. 이 누수를 방지하려면 이 [코드 샘플을 따르십시오](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).


## [!DNL Sling] API

* [**Apache[!DNL Sling]API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) 는 AEM의 기반이 되는 RESTful 웹 프레임워크입니다. [!DNL Sling] HTTP 요청 라우팅, JCR 노드를 리소스로 모델, 보안 컨텍스트 등을 제공합니다.

[!DNL Sling] API는 확장 가능한 JCR API보다 API를 사용하여 구축한 애플리케이션의 동작을 더 쉽고 안전하게 늘릴 수 있도록 익스텐션을 위해 구축되어 [!DNL Sling] 있습니다.

### API의 일반적인 [!DNL Sling] 사용

* JCR 노드에 [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 액세스하고 ValueMaps를 통해 해당 데이터에 [액세스합니다](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* ResourceResolver를 통해 보안 컨텍스트를 [제공합니다](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* ResourceResolver의 [만들기/이동/복사/삭제 메서드를 통해 리소스를 만들고 제거합니다](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* ModifiableValueMap을 통해 속성을 [업데이트합니다](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* 요청 처리 빌드 블록 작성

   * [서비스](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [서블릿 필터](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 비동기 작업 처리 구성 블록

   * [이벤트 및 작업 핸들러](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [일정](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling Models](https://sling.apache.org/documentation/bundles/models.html)

* [서비스 사용자](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

JCR( [Java Content Repository) 2.0 API는](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) JCR 구현을 위한 사양(AEM의 경우 [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))의 일부입니다. 모든 JCR 구현은 이러한 API를 준수하고 구현해야 하며, 따라서 AEM 컨텐츠과 상호 작용하기 위한 최저 수준의 API입니다.

JCR 자체는 컨텐츠 저장소로 사용되는 계층적/트리 기반 NoSQL 데이터 저장소 AEM입니다. JCR에는 컨텐츠 CRUD에서 콘텐츠 쿼리 등 다양한 지원 API가 포함되어 있습니다. 이러한 강력한 API에도 불구하고 더 높은 수준의 AEM 및 추상적 요소보다 선호하는 경우는 [!DNL Sling] 드물다.

항상 Apache Jackrabbit Oak API보다 JCR API를 선호합니다. JCR API는 JCR 저장소 ***와 상호 작용하기*** 위한 반면 Oak API는 JCR 저장소를 ***구현하기 위한*** 것입니다.

### JCR API에 대한 일반적인 오해

JCR은 AEM 컨텐츠 저장소이지만 API는 컨텐츠와 상호 작용하기 위한 기본 방법은 아닙니다. 대신 AEM API(페이지, 자산, 태그 등)를 선호합니다. 또는 리소스 API가 더 나은 추상적인 요소를 제공함에 따라 리소스 API가 전송됩니다.

>[!CAUTION]
>
>AEM 애플리케이션에서 JCR APIs 세션 및 노드 인터페이스를 광범위하게 사용하는 것은 코드 냄새입니다. API를 대신 [!DNL Sling] 사용하지 않아야 합니다.

### JCR API의 일반적인 사용

* [액세스 제어 관리](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [승인 가능한 관리(사용자/그룹)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR 관찰(JCR 이벤트 청취)
* 딥 노드 구조 만들기

   * Sling API는 리소스 생성을 지원하지만 JCR API는 [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) 및 [JcrUtil에 편리한 방법을 제공하므로](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) 심층 구조를 신속하게 만들 수 있습니다.

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi 선언적 서비스 1.2 구성 요소 주석 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi 선언적 서비스 1.2 메타데이터 주석 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API와 더 높은 수준의 API(AEM, [!DNL Sling]및 JCR) 간에 거의 겹치지 않으며 OSGi API를 사용해야 하는 경우는 드물며 높은 수준의 AEM 개발 전문 기술이 필요합니다.

### OSGi와 Apache Felix API 비교

OSGi는 모든 OSGi 컨테이너가 구현하고 준수해야 하는 사양을 정의합니다. AEM OSGi 구현인 Apache Felix는 몇 개의 자체 API도 제공합니다.

* Apache Felix API(`org.osgi`)보다 OSGi API(`org.apache.felix`)를 선호합니다.

### OSGi API의 일반적인 사용

* OSGi 서비스 및 구성 요소를 선언하기 위한 주석입니다.

   * OSGi [서비스 및 구성 요소를 선언하기 위해 Felix SCR 주석을](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) 통한 OSGi 선언적 서비스(DS) 1.2 주석 [](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) 필요

* OSGi API를 사용하여 동적으로 OSGi 서비스/구성 요소를 [실행/등록할 수 있습니다](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * 조건부 OSGi 서비스/구성 요소 관리가 필요하지 않은 경우(대부분의 경우)에는 OSGi DS 1.2 주석 사용을 선호합니다.

## 규칙에 대한 예외

다음은 위에 정의된 규칙에 대한 일반적인 예외입니다.

### AEM Asset API

* 보다 [ 더 `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) 선호합니다 [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * 한편 `com.day.cq` 자산 API는 AEM 에셋 관리 사용 사례에 더 많은 무료 툴을 제공합니다.
   * [MOCK] The Granite Assets API support low-level asset management-case (version, relations).

### 쿼리 API

* AEM QueryBuilder는 [제안](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), 맞춤법 검사 및 색인 힌트와 같은 특정 쿼리 기능을 지원하지 않습니다. 이러한 함수를 사용하여 쿼리하려면 JCR-SQL2를 사용하는 것이 좋습니다.

### [!DNL Sling] 서블릿 등록 {#sling-servlet-registration}

* [!DNL Sling] servlet 등록, @ [SlingServletResourceTypes가 포함된 OSGi DS 1.2 주석을 선호함](https://sling.apache.org/documentation/the-sling-engine/servlets.html) `@SlingServlet`

### [!DNL Sling] 필터 등록 {#sling-filter-registration}

* [!DNL Sling] 필터 등록, @ [SlingServletFilter가 포함된 OSGi DS 1.2 주석을 선호함](https://sling.apache.org/documentation/the-sling-engine/filters.html) `@SlingFilter`

## 유용한 코드 조각

다음은 언급된 API를 사용하는 일반적인 사용 사례에 대한 우수 사례를 보여주는 유용한 Java 코드 조각입니다. 또한 이러한 코드 조각은 낮은 기본 API에서 더 선호하는 API로 이동하는 방법을 보여줍니다.

### ResourceResolver에 대한 JCR [!DNL Sling] 세션

#### Sling 리소스 확인자 자동 닫기

AEM 6.2부터 [!DNL Sling] ResourceResolver `AutoClosable` 는 [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 문 안에 있습니다. 이 구문을 사용하면 명시적 호출이 필요하지 `resourceResolver .close()` 않습니다.

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

#### 수동으로 Sling ResourceResolver 닫힘

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

### JCR 경로 [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR 노드 [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] aem Asset

#### 권장 방법

`DamUtil.resolveToAsset(..)`필요에 따라 트리 위로 이동하여 자산 개체 `dam:Asset` 에 있는 모든 리소스를 확인합니다.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 대체 방법

자산을 적응하려면 리소스 자체가 `dam:Asset` 노드여야 합니다.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] AEM 페이지에 대한 리소스

#### 권장 방법

`pageManager.getContainingPage(..)` 필요에 따라 트리 위로 이동하여 페이지 개체 `cq:Page` 에 있는 모든 리소스를 확인합니다.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 대체 방법 {#alternative-approach-1}

페이지에 리소스를 적용하려면 리소스 자체가 `cq:Page` 노드여야 합니다.

```java
Page page = resource.adaptTo(Page.class);
```

### AEM 페이지 속성 읽기

Page 개체의 getter를 사용하여 잘 알려진 속성(`getTitle()`, `getDescription()`등)을 가져옵니다. 다른 속성 `page.getProperties()` 을 검색하기 위해 ValueMap을 `[cq:Page]/jcr:content` 가져옵니다.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM 자산 메타데이터 속성 읽기

자산 API는 노드에서 속성을 읽기 위한 편리한 방법을 `[dam:Asset]/jcr:content/metadata` 제공합니다. 이 매개 변수는 ValueMap이 아니며, 두 번째 매개 변수(기본값 및 자동 유형 지정)는 지원되지 않습니다.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 속성 [!DNL Sling] [!DNL Resource] 읽기 {#read-sling-resource-properties}

속성이 AEM API(페이지, 자산)가 직접 액세스할 수 없는 위치(속성 또는 상대 리소스)에 저장되어 있는 경우 리소스 및 ValueMaps를 사용하여 데이터를 얻을 수 있습니다. [!DNL Sling]

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

이 경우 AEM 개체를 원하는 속성이나 하위 리소스를 효율적으로 찾기 위해 [!DNL Sling][!DNL Resource] 로 변환해야 할 수 있습니다.

#### AEM 페이지 [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset to [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Slings ModifiableValueMap을 사용하여 속성 쓰기

노드 [!DNL Sling]에 속성 [을](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) 쓰려면 &#39;s ModifiableValueMap&#39;을 사용하십시오. 이 작업은 즉시 노드에만 쓸 수 있습니다(상대 속성 경로는 지원되지 않음).

리소스에 대한 쓰기 권한이 `.adaptTo(ModifiableValueMap.class)` 필요한 호출은 null을 반환합니다.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEM 페이지 만들기

AEM에서 페이지를 올바르게 정의하고 초기화하는 데 필요한 페이지 템플릿을 사용할 때 항상 PageManager를 사용하여 페이지를 만듭니다.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Create a [!DNL Sling] Resource

ResourceResolver는 리소스를 만드는 기본 작업을 지원합니다. 더 높은 수준의 추상적 요소를 만들 때(AEM 페이지, 자산, 태그 등) 해당 관리자가 제공하는 방법을 사용합니다.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 리소스 [!DNL Sling] 삭제

ResourceResolver는 리소스 제거를 지원합니다. 더 높은 수준의 추상적인 요소를 만들 때(AEM 페이지, 자산, 태그 등) 해당 관리자가 제공하는 방법을 사용합니다.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
