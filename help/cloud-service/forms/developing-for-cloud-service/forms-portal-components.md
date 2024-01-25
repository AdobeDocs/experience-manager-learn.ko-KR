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
duration: 92
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Forms 포털 구성 요소

AEM Forms은 다음과 같은 포털 구성 요소를 즉시 제공합니다.

**Search &amp; Lister**: 이 구성 요소를 사용하면 양식 저장소에서 포털 페이지로 양식을 나열할 수 있고 지정된 기준에 따라 양식을 나열하는 구성 옵션을 제공할 수 있습니다.

**초안 및 제출**: 검색 및 목록 구성 요소에는 Forms 작성자가 공개한 양식이 표시되지만 초안 및 제출 구성 요소에는 이후 및 제출된 양식을 완료하기 위해 초안으로 저장된 양식이 표시됩니다. 이 구성 요소는 로그인한 사용자에게 개인화된 경험을 제공합니다.

**링크**: 이 구성 요소를 사용하여 페이지의 어디에서든 양식에 대한 링크를 만들 수 있습니다.

## Forms 포털 구성 요소 활성화

IntelliJ를 시작하고 [이전 단계입니다.](./getting-started.md) ui.apps->src->main->content->jcr_root->apps.bankingapplication->components를 확장합니다.

Adobe Experience Manager(AEM) 사이트에서 핵심 구성 요소(기본 포털 구성 요소 포함)를 사용하려면 프록시 구성 요소를 만들어 사이트에 맞게 활성화해야 합니다.
새로 만든 프록시 구성 요소는 기본 양식 구성 요소를 가리켜야 합니다. 이렇게 하면 구성 요소로부터 모든 것을 상속받을 수 있습니다. 이 작업은 프록시 구성 요소의 content.xml에서 resourceSuperType을 변경하여 수행합니다. content.xml에서 제목과 구성 요소 그룹도 지정합니다.
>[!NOTE]
>
> 각각에 대해 리소스 슈퍼 유형을 구성할 수 있습니다. [여기의 구성 요소](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### 초안 및 제출

기존 구성 요소의 복사본 만들기(예: `button`) 이름을 로 지정합니다. _draftsandsubmissions_.
![draftsandsubmissions](assets/forms-portal-components2.png)
의 콘텐츠 바꾸기 `.content.xml` 다음 XML을 사용하여

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 검색 및 리스터

버튼 구성 요소의 복사본을 만들고 이름을 로 변경합니다. _searchandlister_.
의 콘텐츠 바꾸기 `.content.xml` 다음 XML을 사용하여


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### 링크 구성 요소

버튼 구성 요소의 복사본을 만들고 이름을 로 변경합니다. _링크_.
의 콘텐츠 바꾸기 `.content.xml` 다음 XML을 사용하여


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
