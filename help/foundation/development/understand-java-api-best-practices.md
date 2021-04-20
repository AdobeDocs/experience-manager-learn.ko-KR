---
title: AEM에서 Java API 우수 사례 이해
description: AEM은 개발 중에 사용할 수 있는 많은 Java API를 표시하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축되었습니다. 이 문서에서는 주요 API와 API를 사용해야 하는 시기와 이유를 설명합니다.
version: 6.2, 6.3, 6.4, 6.5
sub-product: foundation, assets, sites
feature: APIs
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 2%

---


# Java API 우수 사례 이해

AEM(Adobe Experience Manager)은 개발 중에 사용할 수 있는 많은 Java API를 표시하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축되었습니다. 이 문서에서는 주요 API와 API를 사용해야 하는 시기와 이유를 설명합니다.

AEM은 4개의 기본 Java API 세트를 기반으로 구축되었습니다.

* **AEM(Adobe Experience Manager)**

   * 페이지, 자산, 워크플로우 등 제품 개요

* **[!DNL Apache Sling]웹 프레임워크**

   * 리소스, 가치 맵 및 HTTP 요청과 같은 REST 및 리소스 기반 추상적 요소를 참조하십시오.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * 노드, 속성 및 세션과 같은 데이터 및 컨텐츠 추상.

* **[!DNL OSGi (Apache Felix)]**

   * 서비스 및 (OSGi) 구성 요소와 같은 OSGi 애플리케이션 컨테이너 추상.

## Java API 환경 설정 &quot;경험규칙&quot;

일반적인 규칙은 다음 순서에 따라 API/추상적인 방법을 사용하는 것입니다.

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

AEM에서 API를 제공하는 경우 [!DNL Sling], JCR 및 OSGi 대신 사용하십시오. AEM에서 API를 제공하지 않는 경우 JCR 및 OSGi보다 [!DNL Sling]을 선호합니다.

이 순서는 일반적인 규칙이며, 예외가 있음을 의미합니다. 이 규칙에서 벗어나는 허용되는 이유는 다음과 같습니다.

* 아래에 설명된 대로 잘 알려진 예외 사항입니다.
* 더 높은 수준의 API에서는 필수 기능을 사용할 수 없습니다.
* 선호도가 낮은 API를 사용하는 기존 코드(사용자 지정 또는 AEM 제품 코드)의 컨텍스트에서 작동하고, 새 API로 이동하는 비용은 정확하지 않습니다.

   * 혼합을 만드는 것보다 낮은 수준의 API를 일관되게 사용하는 것이 좋습니다.

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API는 제품 사용 사례에 따라 추상적인 기능과 기능을 제공합니다.

예를 들어 AEM [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) 및 [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) API는 웹 페이지를 나타내는 AEM의 `cq:Page` 노드에 대한 추상적인 정보를 제공합니다.

이러한 노드는 [!DNL Sling] API를 리소스로 사용할 수 있고 JCR API를 노드로 사용할 수 있지만 AEM API는 일반적인 사용 사례에 대한 추상적인 요소를 제공합니다. AEM API를 사용하면 AEM 제품과 AEM 간의 사용자 정의 및 확장 프로그램 간의 일관된 비헤이비어를 보장할 수 있습니다.

### com.adobe.* vs com.day.* API

AEM API에는 다음 Java 패키지로 식별되는 패키지 내 환경 설정이 환경 설정 순서로 있습니다.

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 워크플로우 또는 작업과 같은 제품 간 플랫폼 사용 사례를 지원하는 반면, 제품 간에 사용되는 경우는 다음과 같습니다.  `com.adobe.granite` AEM Assets, 사이트 등).

`com.day.cq` 에 &quot;원본&quot; API가 포함되어 있습니다. 이러한 API는 Adobe의 [!DNL Day CQ] 인수 이전 및/또는 그 주위에 존재했던 핵심 추상 및 기능을 해결합니다. `com.adobe.cq` 또는 `com.adobe.granite`이(가) 새로운 대체 항목을 제공하지 않는 한 이러한 API는 지원되며 이러한 API를 막을 수 없습니다.

[!DNL Content Fragments] 및 [!DNL Experience Fragments]과 같은 새로운 추상들은 아래 설명된 `com.day.cq` 대신 `com.adobe.cq` 공간에 내장되어 있습니다.

### 쿼리 API

AEM은 여러 쿼리 언어를 지원합니다. 3개의 기본 언어는 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath 및 [AEM Query Builder](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/querybuilder-api.html)입니다.

가장 중요한 문제는 코드 베이스에서 일관된 쿼리 언어를 유지 관리하여 복잡성과 비용 부담을 줄이는 것입니다.

최종 쿼리 실행을 위해 [!DNL Apache Oak] JCR-SQL2에 파일을 더하고 JCR-SQL2로의 변환 시간은 쿼리 시간 자체와 거의 비교할 수 없을 만큼 매우 큽니다.

기본 API는 [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)이며, 가장 높은 수준의 추상화이며 쿼리에 대한 결과를 생성, 실행 및 검색하는 데 강력한 API를 제공하며 다음을 제공합니다.

* 간단하고 매개 변수화된 쿼리 구성(맵으로 모델링된 쿼리 매개 변수)
* 기본 [Java API 및 HTTP API](https://helpx.adobe.com/kr/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTB 쿼리 디버거](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [일반적인 ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 쿼리 요구 사항을 지원하는 OOTB 예측

* 확장 가능한 API - 사용자 지정 [쿼리 개발을 허용하는 ](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html) 예측
* JCR-SQL2 및 XPath는 각각 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) 및 [JCR APIs](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)를 통해 직접 실행할 수 있으며, [[!DNL Sling] 리소스](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 또는 [JCR 노드](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)을 반환합니다.

>[!CAUTION]
>
>AEM QueryBuilder API에서 ResourceResolver 개체를 유출합니다. 이 누수를 방지하려면 이 [코드 샘플](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)을(를) 따르십시오.


## [!DNL Sling] API

* [**Apache  [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apachee [!DNL Sling]](https://sling.apache.org/) 는 AEM의 기반이 되는 RESTful 웹 프레임워크입니다. [!DNL Sling] HTTP 요청 라우팅을 제공하고 JCR 노드를 리소스로 모델링하며 보안 컨텍스트를 제공하는 등 다양한 기능을 제공합니다.

[!DNL Sling] API는 확장 가능한 JCR API보다 API를 사용하여 빌드한 애플리케이션의 동작을  [!DNL Sling] 보다 쉽고 안전하게 늘릴 수 있는 확장 기능을 제공합니다.

### [!DNL Sling] API의 일반적인 사용

* JCR 노드를 [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)으로 액세스하고 [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)를 통해 해당 데이터에 액세스합니다.

* [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)을(를) 통해 보안 컨텍스트를 제공합니다.
* ResourceResolver의 [만들기/이동/복사/삭제 메서드](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)를 통해 리소스를 만들고 제거합니다.
* [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)을(를) 통해 속성을 업데이트하는 중입니다.
* 요청 처리 구성 블록 작성 중

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [서블릿 필터](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 비동기 작업 처리 구성 블록

   * [이벤트 및 작업 핸들러](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [스케줄러](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)

* [서비스 사용자](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR(Java Content Repository) 2.0 APIs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)는 JCR 구현을 위한 사양(AEM의 경우 [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))의 일부입니다. 모든 JCR 구현은 이러한 API를 준수하고 구현해야 하며, 따라서 AEM 컨텐츠과 상호 작용하기 위한 최저 수준의 API입니다.

JCR 자체는 컨텐츠 저장소로 사용하는 계층/트리 기반 NoSQL 데이터 저장소 AEM입니다. JCR에는 컨텐츠 CRUD부터 콘텐츠 쿼리 등 다양한 지원 API가 포함되어 있습니다. 이러한 강력한 API에도 불구하고 더 높은 수준의 AEM 및 [!DNL Sling] 추상적인 API보다 선호되는 경우는 거의 없습니다.

항상 Apache Jackrabbit Oak API보다 JCR API를 선호합니다. JCR API는 ***JCR 리포지토리와 상호 작용***&#x200B;하는 데 사용하는 반면 Oak API는 ***JCR 리포지토리 구현***&#x200B;에 사용됩니다.

### JCR API에 대한 일반적인 오해

JCR은 AEM 컨텐츠 저장소이지만, API는 컨텐츠와 상호 작용하기 위한 기본 방법은 아닙니다. 대신 AEM API(페이지, 자산, 태그 등)를 선호합니다. 또는 리소스 API가 더 나은 추상적인 요소를 제공하므로

>[!CAUTION]
>
>AEM 애플리케이션에서 JCR APIs 세션 및 노드 인터페이스를 광범위하게 사용하는 것은 코드 냄새입니다. [!DNL Sling] API를 대신 사용하지 않아야 합니다.

### JCR API의 일반적인 사용

* [액세스 제어 관리](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [승인 가능한 관리(사용자/그룹)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR 관찰(JCR 이벤트 수신 대기)
* 딥 노드 구조 만들기

   * Sling API는 리소스 생성을 지원하지만 JCR API는 하위 구조를 신속하게 만드는 [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) 및 [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html)에 편리한 방법을 제공합니다.

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi 선언적 서비스 1.2 구성 요소 주석 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi 선언적 서비스 1.2 메타데이터 주석 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API와 상위 수준 API(AEM, [!DNL Sling] 및 JCR) 간에 거의 겹치지 않으며, OSGi API를 사용해야 하는 경우는 드물며 수준 높은 AEM 개발 전문 기술이 필요합니다.

### OSGi와 Apache Felix API 비교

OSGi는 모든 OSGi 컨테이너가 구현하고 준수해야 하는 사양을 정의합니다. AEM OSGi 구현인 Apache Felix는 몇 개의 자체 API도 제공합니다.

* Apache Felix API(`org.apache.felix`)보다 OSGi API(`org.osgi`)를 선호합니다.

### OSGi API의 일반적인 사용

* OSGi 서비스 및 구성 요소를 선언하기 위한 주석.

   * OSGi 서비스 및 구성 요소를 선언하려면 [Felix SCR 주석](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html)보다 [OSGi 선언적 서비스(DS) 1.2 주석](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)을 선호함

* 동적으로 in-code [OSGi 서비스/components](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)에 대한 OSGi API.

   * 조건부 OSGi 서비스/구성 요소 관리가 필요하지 않은 경우(가장 자주 사용하는 경우) OSGi DS 1.2 주석 사용을 선호합니다.

## 규칙에 대한 예외

다음은 위에 정의된 규칙에 대한 일반적인 예외입니다.

### AEM Asset API

* [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html)보다 [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html)를 선호합니다.

   * `com.day.cq` 자산 API는 AEM 자산 관리 사용 사례에 더 많은 무료 도구를 제공합니다.
   * Granite Assets API는 낮은 수준의 에셋 관리 사용 사례(버전, 관계)를 지원합니다.

### 쿼리 API

* AEM QueryBuilder는 [제안](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), 맞춤법 검사 및 색인 힌트와 같은 특정 쿼리 함수를 지원하지 않습니다. 이러한 함수를 사용하여 쿼리하려면 JCR-SQL2를 사용하는 것이 좋습니다.

### [!DNL Sling] Servlet 등록  {#sling-servlet-registration}

* [!DNL Sling] servlet 등록, @SlingServletResourceTypesover가 포함된  [OSGi DS 1.2 ](https://sling.apache.org/documentation/the-sling-engine/servlets.html) 주석을 선호함  `@SlingServlet`

### [!DNL Sling] 등록 필터링  {#sling-filter-registration}

* [!DNL Sling] 필터 등록, @ [SlingServletFilterover가 포함된 OSGi DS 1.2 주석 ](https://sling.apache.org/documentation/the-sling-engine/filters.html) 선호  `@SlingFilter`

## 유용한 코드 조각

다음은 언급된 API를 사용하는 일반적인 사용 사례에 대한 우수 사례를 보여주는 유용한 Java 코드 조각입니다. 또한 이러한 코드 조각은 낮은 기본 API에서 보다 선호하는 API로 이동하는 방법을 보여줍니다.

### [!DNL Sling] ResourceResolver에 대한 JCR 세션

#### Sling ResourceResolver 자동 닫기

AEM 6.2부터 [!DNL Sling] ResourceResolver는 [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 문의 `AutoClosable`입니다. 이 구문을 사용하면 `resourceResolver .close()`에 대한 명시적 호출이 필요하지 않습니다.

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

### [!DNL Sling] [!DNL Resource] AEM Asset

#### 권장 방법

`DamUtil.resolveToAsset(..)`필요에 따라 트리 위로  `dam:Asset` 이동하여 에셋 개체 아래의 모든 리소스를 확인합니다.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 대체 방법

자산을 자산에 적용하려면 리소스 자체가 `dam:Asset` 노드여야 합니다.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] AEM 페이지에 대한 리소스

#### 권장 방법

`pageManager.getContainingPage(..)` 필요에 따라 트리 위로  `cq:Page` 이동하여 Page 개체 아래의 모든 리소스를 확인합니다.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 대체 방법 {#alternative-approach-1}

리소스를 페이지에 적용하려면 리소스 자체가 `cq:Page` 노드여야 합니다.

```java
Page page = resource.adaptTo(Page.class);
```

### AEM 페이지 속성 읽기

Page 개체의 getter를 사용하여 잘 알려진 속성(`getTitle()`, `getDescription()` 등)을 가져옵니다. 및 `page.getProperties()`을(를) 사용하여 다른 속성을 검색할 수 있는 `[cq:Page]/jcr:content` ValueMap을 가져옵니다.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM 자산 메타데이터 속성 읽기

자산 API는 `[dam:Asset]/jcr:content/metadata` 노드에서 속성을 읽기 위한 편리한 방법을 제공합니다. 이 매개 변수는 ValueMap이 아니며, 두 번째 매개 변수(기본값 및 자동 유형 변환)는 지원되지 않습니다.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### [!DNL Sling] [!DNL Resource] 속성 {#read-sling-resource-properties} 읽기

속성을 AEM API(페이지, 자산)가 직접 액세스할 수 없는 위치(속성 또는 상대 리소스)에 저장하면 [!DNL Sling] 리소스 및 ValueMaps를 사용하여 데이터를 얻을 수 있습니다.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

이 경우 원하는 속성 또는 하위 리소스를 효율적으로 찾으려면 AEM 객체를 [!DNL Sling] [!DNL Resource]으로 변환해야 할 수 있습니다.

#### AEM Page to [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset을 [!DNL Sling] [!DNL Resource](으)로

```java
Resource resource = asset.adaptTo(Resource.class);
```

### [!DNL Sling]의 수정 가능 값 맵을 사용하여 속성 쓰기

노드에 속성을 쓰려면 [!DNL Sling]의 [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)을 사용합니다. 이 작업은 즉시 노드에만 쓸 수 있습니다(상대 속성 경로는 지원되지 않음).

`.adaptTo(ModifiableValueMap.class)`에 대한 호출에는 리소스에 대한 쓰기 권한이 필요합니다. 그렇지 않으면 null이 반환됩니다.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEM 페이지 만들기

AEM에서 페이지를 올바르게 정의하고 초기화하는 데 필요한 페이지 템플릿을 사용하여 페이지를 만드는 데에는 항상 PageManager를 사용합니다.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### [!DNL Sling] 리소스 만들기

ResourceResolver는 리소스를 만드는 기본 작업을 지원합니다. 더 높은 수준의 추상적인 요소를 만들 때(AEM 페이지, 자산, 태그 등) 해당 관리자가 제공하는 방법을 사용합니다.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### [!DNL Sling] 리소스 삭제

ResourceResolver는 리소스 제거를 지원합니다. 더 높은 수준의 추상적인 요소를 만들 때(AEM 페이지, 자산, 태그 등) 해당 관리자가 제공하는 방법을 사용합니다.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
