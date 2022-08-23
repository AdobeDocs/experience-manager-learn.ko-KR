---
title: AEM의 Java API 우수 사례
description: AEM은 개발 중에 사용할 많은 Java API를 표시하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축됩니다. 이 문서에서는 주요 API와 그 사용 시기 및 이유를 설명합니다.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '2071'
ht-degree: 2%

---

# Java API 우수 사례

AEM(Adobe Experience Manager)은 개발 중에 사용할 많은 Java API를 표시하는 풍부한 오픈 소스 소프트웨어 스택에 빌드되어 있습니다. 이 문서에서는 주요 API와 그 사용 시기 및 이유를 설명합니다.

AEM은 4개의 기본 Java API 세트를 기반으로 구축됩니다.

* **AEM(Adobe Experience Manager)**

   * 페이지, 자산, 워크플로우 등과 같은 제품 추상

* **Apache Sling Web Framework**

   * 리소스, 값 맵 및 HTTP 요청과 같은 REST 및 리소스 기반 추상화.

* **JCR(Apache Jackrabbit Oak)**

   * 노드, 속성 및 세션과 같은 데이터 및 컨텐츠 추상.

* **OSGi(Apache Felix)**

   * 서비스 및 (OSGi) 구성 요소와 같은 OSGi 애플리케이션 컨테이너 추상.

## Java API 기본 설정 &quot;경험 규칙&quot;

일반적인 규칙은 API/추상을 다음 순서로 선호합니다.

1. **AEM**
1. **슬링**
1. **JCR**
1. **OSGi**

API가 AEM에서 제공하는 경우 보다 선호합니다 [!DNL Sling], JCR 및 OSGi. AEM에서 API를 제공하지 않는 경우 를 선호합니다 [!DNL Sling] JCR과 OSGi를 통해 지원됩니다.

이 순서는 일반적인 규칙이며, 이것은 예외가 있음을 의미합니다. 이 규칙에서 벗어나는 허용되는 이유는 다음과 같습니다.

* 잘 알려진 예외 사항입니다.
* 더 높은 수준의 API에서는 필수 기능을 사용할 수 없습니다.
* 기존 코드(사용자 지정 또는 AEM 제품 코드)의 컨텍스트에서 작동하며, 이 코드 자체는 덜 선호하는 API를 사용하며, 새 API로 이동하는 비용은 적절하지 않습니다.

   * 혼합을 만드는 것보다 낮은 수준의 API를 일관되게 사용하는 것이 좋습니다.

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API는 생산화된 사용 사례와 관련된 추상화와 기능을 제공합니다.

예: AEM [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 및 [페이지](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API는 `cq:Page` 웹 페이지를 나타내는 AEM의 노드입니다.

이러한 노드는 [!DNL Sling] API는 리소스로, JCR API는 노드로, AEM API는 일반적인 사용 사례에 대한 추상을 제공합니다. AEM API를 사용하면 AEM 제품, 사용자 지정 및 AEM 확장 간에 일관된 동작을 보장합니다.

### com.adobe.&#42; vs com.day.&#42; API

AEM API에는 기본 설정 순서로 다음 Java 패키지로 식별되는 인트라 패키지 기본 설정이 있습니다.

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 는 제품 사용 사례를 지원합니다. `com.adobe.granite` 은 워크플로우 또는 작업(제품 간에 사용)과 같은 제품 간 플랫폼 사용 사례를 지원합니다. AEM Assets, Sites 등).

`com.day.cq` 에는 &quot;원래&quot; API가 포함되어 있습니다. 이러한 API는 Adobe의 고객 확보 전 및/또는 그 주변에 존재했던 핵심 추상 및 기능을 해결합니다 [!DNL Day CQ]. 이러한 API는 지원되지 않으므로 `com.adobe.cq` 또는 `com.adobe.granite` (최신) 대체 요소를 제공합니다.

다음과 같은 새로운 추상 [!DNL Content Fragments] 및 [!DNL Experience Fragments] 기본적으로 `com.adobe.cq` 공간 대신 `com.day.cq` 아래에 설명되어 있습니다.

### 쿼리 API

AEM에서는 여러 쿼리 언어를 지원합니다. 3개의 주요 언어는 다음과 같습니다 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath 및 [AEM Query Builder](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

가장 중요한 문제는 코드 베이스에서 일관된 쿼리 언어를 유지하는 것이며, 이를 통해 복잡성을 줄이고 비용을 절감할 수 있습니다.

모든 쿼리 언어에는 [!DNL Apache Oak] 최종 쿼리 실행을 위해 JCR-SQL2에 트랜스더링하며 JCR-SQL2로의 변환 시간은 쿼리 시간 자체에 비해 거의 무시할 수 있습니다.

기본 API는 다음과 같습니다 [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)- 가장 높은 수준의 추상화이며 쿼리에 대한 결과를 구성, 실행 및 검색하기 위한 강력한 API를 제공하며 다음을 제공합니다.

* 간단한 매개 변수화된 쿼리 구성(맵으로 모델링된 쿼리 매개 변수)
* 기본 [Java API 및 HTTP API](https://helpx.adobe.com/kr/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [AEM Query Debugger](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [AEM 설명](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 일반적인 쿼리 요구 사항 지원

* 사용자 지정 API를 개발할 수 있도록 해줍니다 [쿼리 설명](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 및 XPath는 를 통해 직접 실행할 수 있습니다 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) 및 [JCR API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), 결과를 반환합니다 [[!DNL Sling] 리소스](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 또는 [JCR 노드](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html)각각 입니다.

>[!CAUTION]
>
>AEM QueryBuilder API에서 ResourceResolver 개체가 누출됩니다. 이 누수를 완화하려면 다음 단계를 수행하십시오 [코드 샘플](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).

## [!DNL Sling] API

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) 는 AEM의 기반이 되는 RESTful 웹 프레임워크입니다. [!DNL Sling] 에서는 HTTP 요청 라우팅, JCR 노드를 리소스로 모델, 보안 컨텍스트 등을 제공합니다.

[!DNL Sling] API는 확장을 위해 빌드되므로 이를 사용하여 빌드된 애플리케이션의 동작을 보다 쉽고 안전하게 늘릴 수 있습니다 [!DNL Sling] 확장 가능한 JCR API보다 큰 API.

### 의 일반적인 사용 [!DNL Sling] API

* JCR 노드를으로 액세스 [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 를 통해 데이터에 액세스 [값 맵](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* 를 통해 보안 컨텍스트 제공 [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* ResourceResolver를 통해 리소스 만들기 및 제거 [생성/이동/복사/삭제 방법](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* 를 통해 속성 업데이트 [수정 가능한 값 맵](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* 빌딩 요청 처리 빌딩 블록

   * [서블릿](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [서블릿 필터](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 비동기 작업 처리 빌딩 블록

   * [이벤트 및 작업 처리기](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [스케줄러](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)

* [서비스 사용자](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

다음 [JCR(Java Content Repository) 2.0 API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) 는 JCR 구현에 대한 사양의 일부입니다(AEM의 경우). [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). 모든 JCR 구현은 이러한 API를 준수하고 구현해야 하므로 AEM 콘텐츠과 상호 작용하기 위한 가장 낮은 수준의 API입니다.

JCR 자체는 컨텐츠 저장소로 사용하는 계층적/트리 기반 NoSQL 데이터 저장소 AEM입니다. JCR에는 컨텐츠 CRUD부터 콘텐츠 쿼리 등 다양한 지원되는 API의 범위가 있습니다. 이러한 강력한 API에도 불구하고 높은 수준의 AEM보다 선호되는 경우는 거의 없습니다. [!DNL Sling] 추상.

Apache Jackrabbit Oak API보다 항상 JCR API를 선호합니다. JCR API는 다음 용입니다 ***상호 작용*** JCR 저장소를 사용하는 반면 Oak API는 을 위한 것입니다 ***구현*** JCR 저장소.

### JCR API에 대한 일반적인 오해

JCR은 AEM 컨텐츠 저장소이지만 API는 컨텐츠와 상호 작용하기 위해 선호되는 방법이 아닙니다. 대신 AEM API(페이지, 자산, 태그 등)를 선호합니다. 또는 Sling 리소스 API에서 더 나은 추상을 제공하므로

>[!CAUTION]
>
>AEM 애플리케이션에서 JCR API의 세션 및 노드 인터페이스를 광범위하게 사용하는 것은 코드 냄새입니다. 확인 [!DNL Sling] API를 대신 사용하면 안 됩니다.

### JCR API의 일반적인 사용

* [액세스 제어 관리](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [승인 가능 관리(사용자/그룹)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR 관찰(JCR 이벤트 수신)
* 딥 노드 구조 만들기

   * Sling API에서는 리소스 생성을 지원하지만 JCR API에서는 의 편의 방법이 있습니다. [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) 및 [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) 그것은 심층 구조를 창조하는 것을 촉진합니다.

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2 구성 요소 주석 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Metatype 주석 JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi 프레임워크 JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API와 상위 수준 API(AEM, [!DNL Sling], 및 JCR) 및 OSGi API를 사용해야 하는 경우가 드물며 AEM 개발에 대한 높은 수준의 전문 지식이 필요합니다.

### OSGi와 Apache Felix API

OSGi는 모든 OSGi 컨테이너가 구현하고 준수해야 하는 사양을 정의합니다. AEM OSGi 구현인 Apache Felix는 자체 API 몇 가지를 제공합니다.

* OSGi API 선호(`org.osgi`)을 Apache Felix API에 대해 지원합니다(`org.apache.felix`).

### OSGi API의 일반적인 사용

* OSGi 서비스 및 구성 요소를 선언하기 위한 주석.

   * 기본 설정 [OSGi 선언적 서비스(DS) 1.2 주석](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Felix SCR 주석](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) OSGi 서비스 및 구성 요소 선언

* 동적으로 인코드를 위한 OSGi API [OSGi 서비스/구성 요소 등록 해제/등록](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * 조건부 OSGi 서비스/구성 요소 관리가 필요하지 않은 경우(대부분의 경우 ) OSGi DS 1.2 주석 사용을 선호합니다.

## 규칙에 대한 예외

다음은 위에 정의된 규칙에 대한 일반적인 예외입니다.

### OSGi API

OSGi 구성 요소 속성에서 정의하거나 읽는 등의 저수준 OSGi 추상을 처리할 때 다음과 같이 제공되는 새로운 추상 `org.osgi` 상위 수준 Sling 작업보다 선호됩니다. 경쟁 Sling 추상화가 `@Deprecated` 그리고 추천합니다 `org.osgi` 대체.

또한 OSGi 구성 노드 정의가 더 선호됩니다 `cfg.json` 오버 `sling:OsgiConfig` 형식 지정

### AEM Asset API

* 기본 설정 [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) over [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * 반면에 `com.day.cq` Assets API는 AEM 자산 관리 사용 사례에 더 많은 무료 도구를 제공합니다.
   * Granite Assets API는 낮은 수준의 자산 관리 사용 사례(버전, 관계)를 지원합니다.

### 쿼리 API

* AEM QueryBuilder는 [추천](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), 맞춤법 검사 및 색인 힌트 등의 기타 일반적인 함수 간에 있을 수 있습니다. 이러한 함수를 사용하여 쿼리하려면 JCR-SQL2가 선호됩니다.

### [!DNL Sling] 서블릿 등록 {#sling-servlet-registration}

* [!DNL Sling] 서블릿 등록, 선호 [OSGi DS 1.2 주석(@SlingServletResourceTypes 포함)](https://sling.apache.org/documentation/the-sling-engine/servlets.html) over `@SlingServlet`

### [!DNL Sling] 필터 등록 {#sling-filter-registration}

* [!DNL Sling] 필터 등록, 선호 [OSGi DS 1.2 주석(@SlingServletFilter 포함)](https://sling.apache.org/documentation/the-sling-engine/filters.html) over `@SlingFilter`

## 유용한 코드 조각

다음은 설명된 API를 사용하는 일반적인 사용 사례에 대한 우수 사례를 보여주는 유용한 Java 코드 조각입니다. 이러한 코드 조각은 더 낮은 기본 API에서 더 선호하는 API로 이동하는 방법을 보여 줍니다.

### JCR 세션 [!DNL Sling] ResourceResolver

#### Sling ResourceResolver 자동 닫기

AEM 6.2 이후, [!DNL Sling] ResourceResolver가 `AutoClosable` 에서 [리소스 사용](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 문. 이 구문을 사용하는에 대한 명시적 호출입니다 `resourceResolver .close()` 이 필요하지 않습니다.

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

#### Sling ResourceResolver를 수동으로 닫았습니다.

ResourceResolver는 `finally` 블록 위에 표시된 자동 닫기 기술을 사용할 수 없는 경우

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

### JCR 노드 대상 [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] AEM Asset

#### 권장 방법

`DamUtil.resolveToAsset(..)`에서 모든 리소스를 확인합니다. `dam:Asset` 필요에 따라 트리 위로 이동하여 Asset 개체에 추가합니다.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 대체 방법

자산을 자산에 적응하려면 리소스 자체가 `dam:Asset` 노드 아래에 있어야 합니다.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] AEM에 리소스 페이지

#### 권장 방법

`pageManager.getContainingPage(..)` 에서 모든 리소스를 확인합니다. `cq:Page` 필요에 따라 트리 위로 이동하여 Page 개체에 추가합니다.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 대체 방법 {#alternative-approach-1}

리소스를 페이지에 적용하려면 리소스 자체가 `cq:Page` 노드 아래에 있어야 합니다.

```java
Page page = resource.adaptTo(Page.class);
```

### AEM 페이지 속성 읽기

Page 개체의 getter를 사용하여 잘 알려진 속성(`getTitle()`, `getDescription()`등) 및 `page.getProperties()` 얻다 `[cq:Page]/jcr:content` 다른 속성을 검색하기 위한 ValueMap입니다.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM 자산 메타데이터 속성 읽기

Asset API는 `[dam:Asset]/jcr:content/metadata` 노드 아래에 있어야 합니다. 이 매개 변수는 ValueMap이 아니며, 두 번째 매개 변수(기본값 및 자동 유형 캐스팅이 지원되지 않습니다.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 읽기 [!DNL Sling] [!DNL Resource] 속성 {#read-sling-resource-properties}

속성이 AEM API(페이지, 자산)에서 직접 액세스할 수 없는 위치(속성 또는 상대 리소스)에 저장되는 경우, [!DNL Sling] 리소스 및 ValueMap을 사용하여 데이터를 가져올 수 있습니다.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

이 경우 AEM 개체를 [!DNL Sling] [!DNL Resource] 를 사용하여 원하는 속성 또는 하위 리소스를 효율적으로 찾을 수 있습니다.

#### AEM 페이지 대상 [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset 대상 [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 속성을 사용하여 쓰기 [!DNL Sling]의 수정 가능한 값 맵

사용 [!DNL Sling]s [수정 가능한 값 맵](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) 노드에 속성을 씁니다. 직접 노드에만 쓸 수 있습니다(상대 속성 경로는 지원되지 않음).

에 대한 호출을 확인합니다. `.adaptTo(ModifiableValueMap.class)` 리소스에 대한 쓰기 권한이 필요합니다. 그렇지 않으면 null이 반환됩니다.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEM 페이지 만들기

AEM에서 페이지를 올바로 정의하고 초기화하는 데 필요한 페이지 템플릿을 사용하려면 항상 페이지 관리자를 사용하여 페이지를 만드십시오.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### 만들기 [!DNL Sling] 리소스

ResourceResolver는 리소스를 만드는 기본 작업을 지원합니다. 더 높은 수준의 추상을 만드는 경우(AEM 페이지, 자산, 태그 등) 각 관리자가 제공하는 방법을 사용합니다.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 삭제 [!DNL Sling] 리소스

ResourceResolver에서 리소스 제거를 지원합니다. 더 높은 수준의 추상을 만들 때(AEM 페이지, 자산, 태그 등) 각 관리자가 제공하는 방법을 사용합니다.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
