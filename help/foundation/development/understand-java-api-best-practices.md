---
title: Java&trade; AEM의 API 모범 사례
description: AEM은 개발 중에 사용할 수 있도록 많은 Java&trade, API를 노출하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축됩니다. 이 문서에서는 주요 API와 이러한 API를 사용해야 하는 시기 및 이유를 살펴봅니다.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 498
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 0%

---

# Java™ API 모범 사례

Adobe Experience Manager(AEM)는 개발 중에 사용할 수 있도록 많은 Java™ API를 노출하는 풍부한 오픈 소스 소프트웨어 스택을 기반으로 구축됩니다. 이 문서에서는 주요 API와 이러한 API를 사용해야 하는 시기 및 이유를 살펴봅니다.

AEM은 4개의 기본 Java™ API 세트를 기반으로 구축됩니다.

* **Adobe Experience Manager (AEM)**

   * 페이지, 에셋, 워크플로 등과 같은 제품 추상화

* **Apache Sling 웹 프레임워크**

   * 리소스, 값 맵 및 HTTP 요청과 같은 REST 및 리소스 기반 추상화입니다.

* **JCR (Apache Jackrabbit Oak)**

   * 노드, 속성 및 세션과 같은 데이터 및 콘텐츠 추상화입니다.

* **OSGi(Apache Felix)**

   * 서비스 및 (OSGi) 구성 요소와 같은 OSGi 애플리케이션 컨테이너 추상.

## Java™ API 환경 설정 &quot;경험에 근거한 규칙&quot;

일반적인 규칙은 다음 순서로 API/추상화를 선호하는 것입니다.

1. **AEM**
1. **슬링**
1. **JCR**
1. **OSGi**

AEM에서 API를 제공하는 경우 선호합니다. [!DNL Sling], JCR 및 OSGi. AEM에서 API를 제공하지 않는 경우 [!DNL Sling] jcr 및 OSGi를 통해

이 순서는 일반적인 규칙이며, 예외가 존재함을 의미합니다. 이 규칙을 벗어나는 데 사용할 수 있는 이유는 다음과 같습니다.

* 아래에 설명된 대로 잘 알려진 예외 사항입니다.
* 필요한 기능은 더 높은 수준의 API에서 사용할 수 없습니다.
* 자체 선호도가 낮은 API를 사용하는 기존 코드(사용자 지정 또는 AEM 제품 코드) 컨텍스트에서 작동하고 새 API로 이동하는 비용은 정당하지 않습니다.

   * 혼합을 만드는 것보다 더 낮은 수준의 API를 일관되게 사용하는 것이 더 좋습니다.

## AEM API

* [**AEM API JavaDocs**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM API는 제품화된 사용 사례와 관련된 추상화 및 기능을 제공합니다.

(예: AEM) [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 및 [페이지](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API는 다음에 대한 추상화를 제공합니다. `cq:Page` 웹 페이지를 나타내는 AEM의 노드.

이러한 노드는 다음을 통해 사용할 수 있습니다. [!DNL Sling] AEM API as Resources와 JCR APIs as Nodes에서는 일반적인 사용 사례에 대한 추상화를 제공합니다. AEM API를 사용하면 AEM 제품과 AEM에 대한 사용자 지정 및 확장 간에 일관된 동작이 보장됩니다.

### com.adobe.&#42; vs com.day.&#42; API

AEM API에는 기본 설정 순서로 다음 Java™ 패키지로 식별되는 패키지 내 기본 설정이 있습니다.

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

다음 `com.adobe.cq` 패키지는 제품 사용 사례를 지원하지만 `com.adobe.granite` 은 워크플로 또는 작업(AEM Assets, Sites 등 제품에서 사용됨)과 같은 제품 간 플랫폼 사용 사례를 지원합니다.

다음 `com.day.cq` 패키지에 &quot;원본&quot; API가 포함되어 있습니다. 이러한 API는 Adobe의 획득 전 및/또는 주변에서 존재했던 핵심 추상화 및 기능을 다룹니다 [!DNL Day CQ]. 이러한 API는 지원되며, 다음과 같은 경우가 아니면 피해야 합니다. `com.adobe.cq` 또는 `com.adobe.granite` 패키지는 (최신) 대안을 제공하지 않습니다.

다음과 같은 새 추상화 [!DNL Content Fragments] 및 [!DNL Experience Fragments] 기본 제공: `com.adobe.cq` 대신 공백 `com.day.cq` 아래에 설명되어 있습니다.

### 쿼리 API

AEM은 여러 쿼리 언어를 지원합니다. 세 가지 주요 언어는 다음과 같습니다. [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath 및 [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

가장 중요한 문제는 코드 베이스 전반에 걸쳐 일관된 쿼리 언어를 유지함으로써 복잡성과 이해 비용을 줄이는 것입니다.

모든 쿼리 언어에는 다음과 같이 효과적으로 동일한 성능 프로필이 있습니다. [!DNL Apache Oak] 최종 쿼리 실행을 위해 JCR-SQL2에 트랜스 더빙하고, JCR-SQL2에 대한 전환 시간은 쿼리 시간 자체에 비해 무시할 수 있습니다.

기본 API는 [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)는 최상위 수준의 추상화이며 쿼리에 대한 결과를 구성, 실행 및 검색하는 강력한 API를 제공하며 다음을 제공합니다.

* 간단한 매개 변수가 있는 쿼리 구문(맵으로 모델링된 쿼리 매개 변수)
* 기본 [Java™ API 및 HTTP API](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* [AEM 쿼리 디버거](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [AEM 조건자](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html) 일반 쿼리 요구 사항 지원

* 사용자 정의 개발을 허용하는 확장 가능한 API [쿼리 조건자](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* JCR-SQL2 및 XPath는 다음을 통해 직접 실행할 수 있습니다. [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) 및 [JCR API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), 결과 반환 a [[!DNL Sling] 리소스](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 또는 [JCR 노드](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), 각각

>[!CAUTION]
>
>AEM QueryBuilder API가 ResourceResolver 개체를 누출합니다. 이 누출을 완화하려면 다음을 수행하십시오 [코드 샘플](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## [!DNL Sling] API

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) 는 AEM의 기반이 되는 RESTful 웹 프레임워크입니다. [!DNL Sling] 는 HTTP 요청 라우팅을 제공하고, JCR 노드를 리소스로 모델링하고, 보안 컨텍스트를 제공하는 등의 작업을 수행합니다.

[!DNL Sling] API는 확장을 위해 빌드된다는 추가적인 이점을 제공합니다. 즉, 을 사용하여 빌드된 애플리케이션의 동작을 늘리는 것이 더 쉽고 안전합니다 [!DNL Sling] 확장 불가능한 JCR API보다 작은 API.

### 의 일반적인 사용 [!DNL Sling] API

* JCR 노드에 다음으로 액세스 [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 및 를 통해 데이터 액세스 [값 맵](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* 를 통해 보안 컨텍스트 제공 [ResourceResolution](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* ResourceResolver의 [create/move/copy/delete 메서드](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* 를 통해 속성 업데이트 [수정 가능한 값 맵](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* 빌딩 요청 처리 빌딩 블록

   * [서블릿](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [서블릿 필터](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 비동기 작업 처리 빌딩 블록

   * [이벤트 및 작업 핸들러](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [스케줄러](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 모델](https://sling.apache.org/documentation/bundles/models.html)

* [서비스 사용자](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

다음 [JCR(Java™ Content Repository) 2.0 API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) 는 JCR 구현을 위한 사양의 일부입니다(AEM의 경우). [아파치 잭래빗 오크](https://jackrabbit.apache.org/oak/docs/)). 모든 JCR 구현은 이러한 API를 준수하고 구현해야 하며, 따라서 AEM 콘텐츠와 상호 작용하기 위한 가장 낮은 수준의 API입니다.

JCR 자체는 AEM이 컨텐츠 저장소로 사용하는 계층/트리 기반의 NoSQL 데이터 저장소입니다. JCR에는 콘텐츠 CRUD에서 콘텐츠 쿼리에 이르기까지 지원되는 다양한 API가 있습니다. 이러한 강력한 API에도 불구하고 높은 수준의 AEM 및 보다 선호되는 경우는 드뭅니다 [!DNL Sling] 추상화.

항상 Apache Jackrabbit Oak API보다 JCR API를 선호합니다. JCR API는 ***상호 작용*** JCR 저장소를 사용하는 경우, Oak API는 ***구현*** JCR 저장소.

### JCR API에 대한 일반적인 오해

JCR은 AEM 콘텐츠 저장소이지만 해당 API는 콘텐츠와 상호 작용하기 위한 기본 방법이 아닙니다. 대신 더 나은 추상화를 제공할 수 있으므로 AEM API(페이지, 에셋, 태그 등) 또는 Sling 리소스 API를 선호합니다.

>[!CAUTION]
>
>AEM 애플리케이션에서 JCR API의 세션 및 노드 인터페이스를 광범위하게 사용하는 것은 코드 효과입니다. 확인 [!DNL Sling] API를 대신 사용해야 합니다.

### JCR API의 일반적인 사용

* [액세스 제어 관리](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [승인 가능 관리(사용자/그룹)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR 관찰(JCR 이벤트 수신)
* 딥 노드 구조 만들기

   * Sling API는 리소스 생성을 지원하지만 JCR API에는 편리한 메서드가 있습니다. [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) 및 [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) 그것은 깊은 구조를 만드는 것을 촉진합니다.

## OSGi API

* [**OSGi R6 JavaDocs**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi 선언 서비스 1.2 구성 요소 주석 JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi 선언 서비스 1.2 메타 유형 주석 JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi 프레임워크 JavaDocs**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API와 상위 수준 API(AEM, [!DNL Sling], 및 JCR)을 사용하고 OSGi API를 사용해야 하는 경우는 드물며 높은 수준의 AEM 개발 전문 지식이 필요합니다.

### OSGi와 Apache Felix API 비교

OSGi는 모든 OSGi 컨테이너가 구현하고 준수해야 하는 사양을 정의합니다. AEM OSGi 구현인 Apache Felix는 몇 가지 자체 API도 제공합니다.

* OSGi API 선호(`org.osgi`) Apache Felix API( )를 통해`org.apache.felix`).

### OSGi API의 일반적인 사용

* OSGi 서비스 및 구성 요소를 선언하기 위한 OSGi 주석입니다.

   * 기본 설정 [OSGi 선언 서비스(DS) 1.2 주석](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) 초과 [Felix SCR 주석](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) OSGi 서비스 및 구성 요소 선언

* 동적으로 인코드에 사용되는 OSGi API [osgi 서비스/구성 요소 등록 취소/등록](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * 조건부 OSGi 서비스/구성 요소 관리가 필요하지 않은 경우(대부분의 경우) OSGi DS 1.2 주석의 사용을 선호합니다.

## 규칙에 대한 예외

다음은 위에서 정의한 규칙에 대한 일반적인 예외입니다.

### OSGi API

OSGi 구성 요소 속성에서 정의 또는 읽기와 같은 낮은 수준의 OSGi 추상을 처리할 때에서 제공하는 최신 추상 `org.osgi` 높은 수준의 Sling 추상화보다 선호됩니다. 경쟁 관계에 있는 Sling 추상화는 로 표시되지 않았습니다. `@Deprecated` 및 제안 `org.osgi` 대체.

또한 OSGi 구성 노드 정의가 선호합니다. `cfg.json` 다음 기간 초과 `sling:OsgiConfig` 포맷.

### AEM Asset API

* 기본 설정 [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) 초과 [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * 동안 `com.day.cq` Assets API는 AEM 자산 관리 사용 사례에 더 많은 무료 도구를 제공합니다.
   * Granite Assets API는 낮은 수준의 자산 관리 사용 사례(버전, 관계)를 지원합니다.

### 쿼리 API

* AEM QueryBuilder는 와 같은 특정 쿼리 함수를 지원하지 않습니다. [제안 사항](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), 맞춤법 검사 및 색인 힌트 는 다른 덜 일반적인 함수와 유사합니다. 이러한 함수로 쿼리하려면 JCR-SQL2가 좋습니다.

### [!DNL Sling] 서블릿 등록 {#sling-servlet-registration}

* [!DNL Sling] 서블릿 등록, 환경 설정 [OSGi DS 1.2 주석(@SlingServletResourceTypes 포함)](https://sling.apache.org/documentation/the-sling-engine/servlets.html) 초과 `@SlingServlet`

### [!DNL Sling] 등록 필터링 {#sling-filter-registration}

* [!DNL Sling] 필터 등록, 환경 설정 [OSGi DS 1.2 주석(@SlingServletFilter 포함)](https://sling.apache.org/documentation/the-sling-engine/filters.html) 초과 `@SlingFilter`

## 유용한 코드 조각

다음은 논의된 API를 사용하는 일반적인 사용 사례에 대한 모범 사례를 보여 주는 유용한 Java™ 코드 조각입니다. 이러한 스니펫은 선호도가 낮은 API에서 선호도가 높은 API로 이동하는 방법을 보여 줍니다.

### JCR 세션 대상 [!DNL Sling] ResourceResolution

#### Sling ResourceResolver 자동 닫기

AEM 6.2 이후 [!DNL Sling] ResourceResolver는 `AutoClosable` 다음 기간: [리소스 사용](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 명령문입니다. 이 구문을 사용하여에 대한 명시적 호출 `resourceResolver .close()` 이 필요하지 않습니다.

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

ResourceResolver를 수동으로 닫아야 함 `finally` 블록: 위에 표시된 자동 닫기 기술을 사용할 수 없는 경우

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

### JCR 경로: [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR 노드 대상 [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] AEM Asset에

#### 권장 접근 방식

다음 `DamUtil.resolveToAsset(..)` 함수는 아래의 모든 리소스를 확인합니다. `dam:Asset` 필요한 경우 트리 위로 걸어 올라가 Asset 개체에 액세스합니다.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 대안적 접근

리소스를 에셋에 적용하려면 리소스 자체가 `dam:Asset` 노드.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] 리소스-AEM 페이지

#### 권장 접근 방식

`pageManager.getContainingPage(..)` 은(는) 아래의 모든 리소스를 확인합니다. `cq:Page` 필요에 따라 트리 위로 이동하여 Page 객체를 만듭니다.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 대안적 접근 {#alternative-approach-1}

리소스를 페이지에 적용하려면 리소스 자체가 `cq:Page` 노드.

```java
Page page = resource.adaptTo(Page.class);
```

### AEM 페이지 속성 읽기

Page 개체의 getter를 사용하여 잘 알려진 속성(`getTitle()`, `getDescription()`등) 및 `page.getProperties()` 을(를) 가져오려면 `[cq:Page]/jcr:content` 다른 속성을 검색하기 위한 ValueMap

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM Asset 메타데이터 속성 읽기

Asset API는에서 속성을 읽는 편리한 방법을 제공합니다. `[dam:Asset]/jcr:content/metadata` 노드. 이 매개 변수는 ValueMap이 아니며 두 번째 매개 변수(기본값 및 자동 유형 캐스팅)는 지원되지 않습니다.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 읽기 [!DNL Sling] [!DNL Resource] 속성 {#read-sling-resource-properties}

속성이 AEM API(페이지, 에셋)에서 직접 액세스할 수 없는 위치(속성 또는 상대 리소스)에 저장되는 경우 [!DNL Sling] 리소스 및 ValueMaps를 사용하여 데이터를 가져올 수 있습니다.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

이 경우 AEM 개체를 로 변환해야 할 수 있습니다. [!DNL Sling] [!DNL Resource] 원하는 속성이나 하위 리소스를 효율적으로 찾을 수 있습니다.

#### 페이지 대상 AEM [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### 에셋 대상 AEM [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 다음을 사용하여 속성 쓰기 [!DNL Sling]의 ModifiableValueMap

사용 [!DNL Sling]의 [수정 가능한 값 맵](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) 를 입력하여 노드에 속성을 기록합니다. 직접 실행 노드에만 쓸 수 있습니다(상대 속성 경로는 지원되지 않음).

다음에 대한 호출을 기록합니다. `.adaptTo(ModifiableValueMap.class)` 리소스에 대한 쓰기 권한이 필요합니다. 그렇지 않으면 null을 반환합니다.

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

### 만들기 [!DNL Sling] 리소스

ResourceResolver는 리소스를 만드는 기본 작업을 지원합니다. 상위 수준 추상화(AEM Pages, Assets, Tags 등)를 만들 때는 해당 관리자가 제공하는 메서드를 사용합니다.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 삭제 [!DNL Sling] 리소스

ResourceResolver가 리소스 제거를 지원합니다. 상위 수준 추상화(AEM Pages, Assets, Tags 등)를 만들 때는 해당 관리자가 제공하는 메서드를 사용합니다.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
