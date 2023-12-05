---
title: 웹에 최적화된 이미지 제공 Java&trade; API
description: AEM as a Cloud Service의 웹 최적화 이미지 제공 Java&trade; API를 사용하여 고성능의 웹 경험을 개발하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
exl-id: c6bb9d6d-aef0-42d5-a189-f904bbbd7694
duration: 527
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 0%

---

# 웹에 최적화된 이미지 제공 Java™ API

AEM as a Cloud Service의 웹 최적화 이미지 제공 Java™ API를 사용하여 고성능의 웹 경험을 개발하는 방법에 대해 알아봅니다.

AEM as a Cloud Service 지원 [웹에 최적화된 이미지 제공](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html) 는 자산의 최적화된 이미지 웹 렌디션을 자동으로 생성합니다. 웹에 최적화된 이미지 제공은 다음과 같은 세 가지 주요 방법을 사용할 수 있습니다.

1. [AEM 코어 WCM 구성 요소 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
2. 다음을 수행하는 사용자 지정 구성 요소 만들기 [AEM 코어 WCM 구성 요소 이미지 구성 요소를 확장합니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html#tackling-the-image-problem)
3. AssetDelivery Java™ API를 사용하여 웹에 최적화된 이미지 URL을 생성하는 사용자 지정 구성 요소를 만듭니다.

이 문서에서는 AEM as a Cloud Service과 AEM SDK 모두에서 코드 기반으로 작동할 수 있도록 사용자 지정 구성 요소에서 웹에 최적화된 이미지 Java™ API를 사용하는 방법에 대해 알아봅니다.

## Java™ API

다음 [AssetDelivery API](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) 는 이미지 에셋에 대해 웹에 최적화된 게재 URL을 생성하는 OSGi 서비스입니다. `AssetDelivery.getDeliveryURL(...)` 허용되는 옵션은 다음과 같습니다. [여기에 문서화됨](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

다음 `AssetDelivery` AEM OSGi 서비스는 as a Cloud Service으로 실행할 때만 충족됩니다. AEM SDK에서 `AssetDelivery` OSGi 서비스 반환 `null`. AEM에서 as a Cloud Service으로 실행할 때는 웹 최적화 URL을 조건부로 사용하고 AEM SDK에서는 대체 이미지 URL을 사용하는 것이 가장 좋습니다. 일반적으로 에셋의 웹 렌디션은 충분한 대체 요소입니다.


### OSGi 서비스의 API 사용

다음을 표시합니다.`AssetDelivery` 사용자 지정 OSGi 서비스에서 선택 사항으로 참조되므로 사용자 지정 OSGi 서비스를 AEM SDK에서 계속 사용할 수 있습니다.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### Sling 모델에서 API 사용

다음을 표시합니다.`AssetDelivery` 사용자 지정 Sling 모델을 AEM SDK에서 계속 사용할 수 있도록 사용자 지정 Sling 모델에서 선택 사항으로 를 참조하십시오.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### API의 조건부 사용

조건부로 웹에 최적화된 이미지 URL 또는 을 기반으로 한 대체 URL 반환 `AssetDelivery` OSGi 서비스 가용성. 조건부 사용을 사용하면 AEM SDK에서 코드를 실행할 때 코드가 작동할 수 있습니다.

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## 예제 코드

다음 코드는 웹에 최적화된 이미지 URL을 사용하여 이미지 에셋 목록을 표시하는 예제 구성 요소를 만듭니다.

코드가 AEM에서 as a Cloud Service으로 실행되면 웹에 최적화된 웹 이미지 렌디션이 사용자 지정 구성 요소에서 사용됩니다.

![AEM에서 웹에 최적화된 이미지 as a Cloud Service](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEM as a Cloud Service에서는 AssetDelivery API를 지원하므로 웹에 최적화된 웹 렌디션이 사용됩니다_

AEM SDK에서 코드가 실행되면, 정적 웹 표현물이 덜 사용되므로 로컬 개발 중에 구성 요소가 작동할 수 있습니다.

![AEM SDK의 웹에 최적화된 대체 이미지](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_AEM SDK는 AssetDelivery API를 지원하지 않으므로 대체 정적 웹 렌디션(PNG 또는 JPEG)이 사용됩니다_

구현은 세 가지 논리적 조각으로 나뉩니다.

1. 다음 `WebOptimizedImage` OSGi 서비스는 AEM에서 제공하는 서비스에 대해 &quot;스마트 프록시&quot; 역할을 합니다 `AssetDelivery` AEM as a Cloud Service 및 AEM SDK에서 모두 실행을 처리할 수 있는 OSGi 서비스입니다.
2. 다음 `ExampleWebOptimizedImages` Sling 모델은 표시할 이미지 자산 목록과 웹에 최적화된 URL을 수집하는 비즈니스 논리를 제공합니다.
3. 다음 `example-web-optimized-images` AEM 구성 요소에서는 웹에 최적화된 이미지 목록을 표시하도록 HTL을 구현합니다.

아래 예제 코드는 코드베이스에서 복사하고 필요에 따라 업데이트할 수 있습니다.

### OSGI 서비스

다음 `WebOptimizedImage` OSGi 서비스는 주소 지정 가능한 공용 인터페이스(`WebOptimizedImage`) 및 내부 구현(`WebOptimizedImageImpl`). 다음 `WebOptimizedImageImpl` AEM에서 실행할 때 웹에 최적화된 이미지 URL과 AEM SDK에서 정적 웹 렌디션 URL을 반환하여 구성 요소가 AEM as a Cloud Service SDK에서 계속 작동하도록 합니다.

#### 인터페이스

인터페이스는 Sling 모델과 같은 다른 코드가 상호 작용할 수 있는 OSGi 서비스 계약을 정의합니다.

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### 구현

OSGi 서비스 구현에는 AEM에 대한 선택적 참조가 포함됩니다 `AssetDelivery` OSGi 서비스 및 다음의 경우에 적합한 이미지 URL을 선택하기 위한 폴백 논리 `AssetDelivery` 은(는) `null` AEM SDK에서. 대체 로직은 요구 사항에 따라 업데이트할 수 있습니다.

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### Sling 모델

다음 `ExampleWebOptimizedImages` Sling 모델은 지정 가능한 공용 인터페이스로 분할됩니다(`ExampleWebOptimizedImages`) 및 내부 구현(`ExampleWebOptimizedImagesImpl`);

다음 `ExampleWebOptimizedImagesImpl` Sling 모델은 표시할 이미지 에셋 목록을 수집하고 사용자 지정 항목을 호출합니다 `WebOptimizedImage` 웹에 최적화된 이미지 URL을 가져오는 OSGi 서비스입니다. 이 슬링 모델은 AEM 구성 요소를 나타내므로 다음과 같은 일반적인 방법을 사용합니다. `isEmpty()`, `getId()`, 및 `getData()` 그러나 이러한 방법은 웹에 최적화된 이미지를 사용하는 것과 직접적인 관련이 없습니다.

#### 인터페이스

인터페이스는 HTL과 같은 다른 코드가 상호 작용할 수 있는 Sling 모델 계약을 정의합니다.

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### 구현

Sling 모델은 사용자 정의 `WebOptimizeImage` OSGi 서비스를 통해 해당 구성 요소가 표시하는 이미지 에셋에 대해 웹에 최적화된 이미지 URL을 수집합니다.

이 예제에서는 간단한 쿼리를 사용하여 이미지 에셋을 수집합니다.

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### AEM 구성 요소

AEM 구성 요소는 의 Sling 리소스 유형으로 `WebOptimizedImagesImpl` Sling 모델 구현이며, 이미지 목록 표시를 담당합니다.



구성 요소가 다음 목록을 수신함: `Img` 를 통한 오브젝트 `getImages()` AEM as a Cloud Service에서 실행할 때 웹에 최적화된 WEBP 이미지가 여기에 포함됩니다. 구성 요소가 다음 목록을 수신함: `Img` 를 통한 오브젝트 `getImages()` AEM SDK에서 실행할 때 정적 PNG/JPEG 웹 이미지가 포함됩니다.

#### HTL

HTL은 `WebOptimizedImages` Sling 모델 및 렌더링  `Img` 개체 반환 기준 `getImages()`.

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```
