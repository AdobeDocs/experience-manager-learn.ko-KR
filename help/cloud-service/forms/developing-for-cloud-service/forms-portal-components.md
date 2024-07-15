---
title: AEM Forms 포털 구성 요소 활성화
description: 핵심 구성 요소를 사용하여 AEM Forms 포털 구축
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
duration: 78
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Forms 포털 구성 요소

AEM Forms은 다음과 같은 포털 구성 요소를 즉시 제공합니다.

**검색 및 목록 작성기**: 이 구성 요소를 사용하면 양식 리포지토리의 양식을 포털 페이지에 나열할 수 있으며 지정된 조건을 기반으로 양식을 나열하는 구성 옵션을 제공합니다.

**초안 및 제출**: 검색 및 목록 구성 요소에는 Forms 작성자가 공개한 양식이 표시되지만 초안 및 제출 구성 요소에는 나중에 제출한 양식을 완성하기 위해 초안으로 저장된 양식이 표시됩니다. 이 구성 요소는 로그인한 사용자에게 개인화된 경험을 제공합니다.

**링크**: 이 구성 요소를 사용하면 페이지의 어디에서든 양식에 대한 링크를 만들 수 있습니다.

## Forms 포털 구성 요소 활성화

IntelliJ를 시작하고 [ 이전 단계에서 만든 BankingApplication 프로젝트를 엽니다.](./getting-started.md) ui.apps->src->main->content->jcr_root->apps.bankingapplication->구성 요소를 확장합니다.

Adobe Experience Manager(AEM) 사이트에서 핵심 구성 요소(기본 포털 구성 요소 포함)를 사용하려면 프록시 구성 요소를 만들어 사이트에 맞게 활성화해야 합니다.
새로 만든 프록시 구성 요소는 기본 양식 구성 요소를 가리켜야 합니다. 이렇게 하면 구성 요소로부터 모든 것을 상속받을 수 있습니다. 이 작업은 프록시 구성 요소의 content.xml에서 resourceSuperType을 변경하여 수행합니다. content.xml에서 제목과 구성 요소 그룹도 지정합니다.
>[!NOTE]
>
> 각 [다음 구성 요소에 대해 리소스 슈퍼 유형을 구성할 수 있습니다.](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### 초안 및 제출

기존 구성 요소(예: `button`)의 복사본을 만들고 이름을 _draftsandsubmissions_(으)로 지정합니다.
![draftsandsubmissions](assets/forms-portal-components2.png)
`.content.xml`의 내용을 다음 XML로 바꿉니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 검색 및 리스터

단추 구성 요소를 복사하고 이름을 _searchandlistist_(으)로 바꾸십시오.
`.content.xml`의 내용을 다음 XML로 바꿉니다.


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### 링크 구성 요소

단추 구성 요소를 복사하고 이름을 _link_(으)로 바꾸십시오.
`.content.xml`의 내용을 다음 XML로 바꿉니다.


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

프로젝트가 배포되면 AEM 페이지에서 이러한 구성 요소를 사용하여 Forms 포털을 만들 수 있습니다.

## 다음 단계

[클라우드 서비스 구성 포함](./azure-storage-fdm.md)
