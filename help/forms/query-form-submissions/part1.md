---
title: 특정 필드를 검색할 수 있도록 설정
description: Azure 포털에 저장된 양식 제출을 쿼리하는 단계를 안내하는 다중 파트 튜토리얼입니다.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 1fb7ca83-0ba6-48a3-b3d3-079d0ef89245
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 1%

---

# 특정 필드를 검색할 수 있도록 설정

양식의 검색 가능한 필드는 일반적으로 제출된 데이터를 검색하거나 필터링하기 위한 기준으로 사용할 수 있는 양식 내 필드를 나타냅니다.
이 사용 사례에서는 다음 필드 유형을 확장하여 검색할 수 있도록 했습니다

* checkbox 그룹
* 드롭다운
* 라디오 단추

양식 작성자는 이러한 필드 유형을 아래와 같이 검색 가능한 것으로 표시할 수 있습니다
![검색 가능 필드](assets/searchable-fields.png)

다음 구조를 만들어 필드를 확장했습니다

![확장된 필드](assets/extend-component.png)

다음은 .content.xml 파일의 컨텐츠입니다

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
          jcr:primaryType="nt:unstructured"
          jcr:title="Check box group"
          sling:resourceType="cq/gui/components/authoring/dialog">
    <content
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container">
        <items jcr:primaryType="nt:unstructured">
            <tabs
                    jcr:primaryType="nt:unstructured"
                    sling:resourceType="granite/ui/components/coral/foundation/tabs"
                    maximized="{Boolean}false">
                <items jcr:primaryType="nt:unstructured">

                    <properties
                            jcr:primaryType="nt:unstructured"
                            jcr:title="Additional Properties"
                            sling:resourceType="granite/ui/components/coral/foundation/container"
                            margin="{Boolean}true">
                        <items jcr:primaryType="nt:unstructured">
                            <columns
                                    jcr:primaryType="nt:unstructured"
                                    sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                    margin="{Boolean}true">
                                <items jcr:primaryType="nt:unstructured">
                                    <column
                                            jcr:primaryType="nt:unstructured"
                                            sling:resourceType="granite/ui/components/coral/foundation/container">
                                        <items jcr:primaryType="nt:unstructured">
                                            <Searchable
                                                    jcr:primaryType="nt:unstructured"
                                                    sling:resourceType="granite/ui/components/coral/foundation/form/checkbox"
                                                    emptyText="Want to include in search?"
                                                    fieldDescription="Indicate if you want to use in search"
                                                    text="Want to use this field in query"
                                                    value="{Boolean}true"
                                                    uncheckedValue="{Boolean}false"

                                                    name="./Searchable"
                                                    checked="{Boolean}false"
                                                    required="{Boolean}false"/>


                                        </items>
                                    </column>
                                </items>
                            </columns>
                        </items>
                    </properties>
                </items>
            </tabs>
        </items>
    </content>
</jcr:root>
```

## 다음 단계

[사용자 정의 제출 핸들러 만들기](./part2.md)
