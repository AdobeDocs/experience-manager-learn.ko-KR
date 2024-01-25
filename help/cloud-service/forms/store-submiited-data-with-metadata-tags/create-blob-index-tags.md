---
title: 사용자 지정 메타데이터 태그를 추가하는 자습서
description: Azure에서 메타데이터 태그가 있는 양식 데이터를 저장하려면 사용자 지정 제출을 만드세요.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 40
exl-id: eb9bcd1b-c86f-4894-bcf8-9c17e74dd6ec
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 1%

---

# Blob 인덱스 태그 추가

데이터 세트가 커지면 데이터 바다에서 특정 개체를 찾는 것이 어려울 수 있습니다. Blob 인덱스 태그는 키-값 인덱스 태그 속성을 사용하여 데이터 관리 및 검색 기능을 제공합니다. 단일 컨테이너 내에서 또는 저장소 계정의 모든 컨테이너에서 개체를 분류하고 찾을 수 있습니다. 예를 들어 blob 인덱스 태그가 있습니다 _**CustomerType=Platinum**_&#x200B;여기서 Platinum은 CustomerType 필드의 값입니다.

![색인 태그](assets/blob-with-index-tags1.png)
다음 코드는 제출된 데이터의 해당 값을 사용하여 Blob 인덱스 데이터 태그 문자열을 만듭니다

```java
@Override
    public String getMetaDataTags(String submittedFormName,String formPath,Session session,String formData) {

        JsonObject jsonObject = JsonParser.parseString(formData).getAsJsonObject();
        List<String>metaDataTags = new ArrayList<String>();
        metaDataTags.add("formName="+submittedFormName);
        Map< String, String > map = new HashMap< String, String >();
        map.put("path", formPath);
        map.put("1_property", "Searchable");
        map.put("1_property.value","true");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map),session);
        query.setStart(0);
        query.setHitsPerPage(20);
        SearchResult result = query.getResult();
        logger.debug("Get result hits " + result.getHits().size());
        for (Hit hit: result.getHits()) {
            try {
                    logger.debug(hit.getPath());
                    String jsonElementName = (String) hit.getProperties().get("name");
                    String fieldName = hit.getProperties().get("name").toString();
                    if(jsonObject.get(jsonElementName).isJsonArray())
                    {
                        
                        JsonArray arrayOfValues = jsonObject.get(jsonElementName).getAsJsonArray();
                        StringBuilder valuesString = new StringBuilder();
                        for(int j=0;j<arrayOfValues.size();j++)
                        {
                            valuesString.append(arrayOfValues.get(j).getAsString());
                            if(j < arrayOfValues.size() -1)
                            {
                                valuesString.append(" and ");
                            }
                        }

                        
                        metaDataTags.add(fieldName + "=" + valuesString.toString());

                    }
                    else
                    {
                        logger.debug("The searchable field name is " + fieldName + "the json element name is " + jsonElementName);
                        metaDataTags.add(fieldName + "=" + jsonObject.get(jsonElementName).getAsString());
                    }

            } catch (RepositoryException e) {
                throw new RuntimeException(e);
            }
        }
        return String.join("&",metaDataTags);
    }
```

## 다음 단계

[사용자 정의 제출 핸들러 만들기](./create-custom-submit.md)
