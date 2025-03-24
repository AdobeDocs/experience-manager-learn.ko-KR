---
title: 구성 요소에 대한 슬링 모델 만들기
description: 구성 요소에 대한 슬링 모델 만들기
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: f4a18f02-61a2-4fa3-bfbb-41bf696cd2a8
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# 구성 요소에 대한 슬링 모델 만들기

AEM의 슬링 모델 은 구성 요소에 대한 백엔드 논리 개발을 간소화하는 데 사용되는 Java 기반 프레임워크입니다. 개발자가 주석을 사용하여 AEM 리소스(JCR 노드)의 데이터를 Java 오브젝트에 매핑할 수 있으므로 구성 요소에 대한 동적 데이터를 깔끔하고 효율적으로 처리할 수 있습니다.
이 클래스인 CountriesDropDownImpl은 AEM(Adobe Experience Manager) 프로젝트에서 CountriesDropDown 인터페이스를 구현한 것입니다. 사용자가 선택한 대륙을 기반으로 국가를 선택할 수 있는 드롭다운 구성 요소의 전원을 켭니다. 드롭다운 데이터는 AEM DAM(Digital Asset Manager)에 저장된 JSON 파일에서 동적으로 로드됩니다.

클래스의 **필드**

* **multiSelect**: 드롭다운에서 여러 항목을 선택할 수 있는지 여부를 나타냅니다.
기본값이 false인 속성을 @ValueMapValue 구성 요소 속성에서 삽입됩니다.
* **요청**: 현재 HTTP 요청을 나타냅니다. 컨텍스트별 정보에 액세스하는 데 유용합니다.
* **대륙**: 드롭다운에 대해 선택한 대륙(예: &quot;아시아&quot;, &quot;유럽&quot;)을 저장합니다.
아무 것도 제공되지 않는 경우 기본값이 &quot;아시아&quot;인 구성 요소의 속성 대화 상자에서 삽입됩니다.
* **resourceResolver**:AEM 저장소의 리소스에 액세스하고 조작하는 데 사용됩니다.
* **jsonData**: 선택한 대륙에 해당하는 JSON 파일에서 구문 분석된 데이터를 저장하는 JSONObject입니다.

클래스의 **메서드**

* **getContinent()** 대륙 필드의 값을 반환하는 간단한 방법입니다.
디버깅 목적으로 반환되는 값을 기록합니다.
* **init()** Lifecycle 메서드에 @PostConstruct이 추가되고, 클래스가 만들어지고 종속성이 주입된 후 실행됩니다.continent 값을 기반으로 JSON 파일의 경로를 동적으로 구성합니다.
resourceResolver를 사용하여 AEM DAM에서 JSON 파일을 가져옵니다.
파일을 에셋에 조정하고, 콘텐츠를 읽고, JSONObject로 구문 분석합니다.
이 프로세스 중에 오류 또는 경고를 기록합니다.
* **getEnums()** 구문 분석된 JSON 데이터에서 모든 키(국가 코드)를 검색합니다.
키를 알파벳순으로 정렬하고 배열로 반환합니다.
반환되는 국가 코드의 수를 기록합니다.
* **getEnumNames()** 구문 분석된 JSON 데이터에서 모든 국가 이름을 추출합니다.
이름을 알파벳순으로 정렬하고 배열로 반환합니다.
총 국가 수와 검색된 각 국가 이름을 기록합니다.
* **isMultiSelect()** 드롭다운에서 다중 선택을 허용하는지 여부를 나타내는 multiSelect 필드의 값을 반환합니다.



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## 다음 단계

[빌드, 배포 및 테스트](./build.md)
