---
title: AEM Forms Portal 구성 요소 활성화
description: 핵심 구성 요소를 사용하여 AEM Forms 포털 구축
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Forms 포털 구성 요소

AEM Forms은 즉시 다음과 같은 포털 구성 요소를 제공합니다.

**검색 및 목록**: 이 구성 요소로 양식 리포지토리의 양식을 포털 페이지로 나열할 수 있으며, 지정된 기준에 따라 양식을 나열하는 구성 옵션을 제공합니다.

**초안 및 제출**: Search &amp; Lister 구성 요소는 Forms 작성자가 공개한 양식을 표시하는 동안 초안 및 제출 구성 요소는 나중 및 제출된 양식을 완료하기 위해 초안으로 저장된 양식을 표시합니다. 이 구성 요소는 로그인한 모든 사용자에게 개인화된 경험을 제공합니다.

**링크**: 이 구성 요소로 페이지의 어디에서나 양식에 대한 링크를 만들 수 있습니다.

## Forms Portal 구성 요소 활성화

IntelliJ를 시작하고 [이전 단계.](./getting-started.md) ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

Adobe Experience Manager(AEM) 사이트에서 핵심 구성 요소(기본 제공 포털 구성 요소 포함)를 사용하려면 프록시 구성 요소를 만들어 사이트에 활성화해야 합니다.
새로 만든 프록시 구성 요소는 기본 양식 구성 요소를 가리켜야 하므로 이 구성 요소로부터 모든 내용을 상속할 수 있습니다. 이 작업은 프록시 구성 요소의 content.xml에서 resourceSuperType을 변경하여 수행됩니다. content.xml에서 제목과 구성 요소 그룹도 지정합니다.
>[!NOTE]
>
> 각 리소스 슈퍼 유형을 구성할 수 있습니다 [이러한 구성 요소는 여기에서 확인할 수 있습니다](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### 초안 및 제출

기존 구성 요소의 사본 만들기(예 `button`) 및으로 이름을 지정합니다. _초안 제출_.
![초안 제출](assets/forms-portal-components2.png)
에서 컨텐츠를 바꿉니다 `.content.xml` 다음 XML을 사용하여 다음을 수행합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 검색 및 목록

단추 구성 요소의 사본을 만들고 이름을 로 변경합니다. _searchchandliter_.
에서 컨텐츠를 바꿉니다 `.content.xml` 다음 XML을 사용하여 다음을 수행합니다.


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### 링크 구성 요소

단추 구성 요소의 사본을 만들고 이름을 로 변경합니다. _링크_.
에서 컨텐츠를 바꿉니다 `.content.xml` 다음 XML을 사용하여 다음을 수행합니다.


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

프로젝트가 배포되면 AEM 페이지에서 이러한 구성 요소를 사용하여 Forms 포털을 만들 수 있습니다.