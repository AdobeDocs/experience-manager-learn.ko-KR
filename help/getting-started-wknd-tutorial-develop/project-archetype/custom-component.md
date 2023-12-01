---
title: 사용자 지정 구성 요소
description: 작성된 콘텐츠를 표시하는 사용자 지정 인라인 구성 요소의 전체 작성을 다룹니다. 비즈니스 논리를 캡슐화하는 슬링 모델 개발을 포함하여 바이라인 구성 요소를 채우고 해당 HTL을 구성 요소 렌더링에 추가합니다.
version: 6.5, Cloud Service
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4072
mini-toc-levels: 1
thumbnail: 30181.jpg
doc-type: Tutorial
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '4057'
ht-degree: 0%

---

# 사용자 지정 구성 요소 {#custom-component}

이 튜토리얼에서는 사용자 지정의 전체적인 만들기에 대해 설명합니다 `Byline` 대화 상자에서 작성된 콘텐츠를 표시하고 슬링 모델 개발을 탐색하여 구성 요소의 HTL을 채우는 비즈니스 논리를 캡슐화하는 AEM 구성 요소입니다.

## 사전 요구 사항 {#prerequisites}

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment).

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 재사용하고 스타터 프로젝트 체크 아웃 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 코드 체크 아웃:

1. 다음을 확인하십시오. `tutorial/custom-component-start` 에서 분기 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Maven 기술을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 모든 Maven 명령에 대한 프로필

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

에서 완성된 코드를 항상 볼 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) 또는 분기로 전환하여 코드를 로컬에서 확인합니다. `tutorial/custom-component-solution`.

## 목표

1. 사용자 지정 AEM 구성 요소를 빌드하는 방법 이해
1. 비즈니스 논리를 Sling 모델로 캡슐화하는 방법에 대해 알아보기
1. HTL 스크립트 내에서 Sling 모델을 사용하는 방법을 이해합니다

## 빌드할 항목 {#what-build}

WKND 튜토리얼의 이 부분에서는 문서 기여자에 대한 작성된 정보를 표시하는 데 사용되는 인라인 구성 요소가 만들어집니다.

![byline 구성 요소 예](assets/custom-component/byline-design.png)

*Byline 구성 요소*

Byline 구성 요소의 구현에는 byline 콘텐츠를 수집하는 대화 상자와 다음과 같은 세부 정보를 검색하는 사용자 지정 Sling 모델이 포함되어 있습니다.

* 이름
* 이미지
* 직업

## Byline 구성 요소 만들기 {#create-byline-component}

먼저 Byline Component 노드 구조를 만들고 대화 상자를 정의합니다. AEM의 구성 요소를 나타내고 JCR에서의 해당 위치로 구성 요소의 리소스 유형을 암시적으로 정의합니다.

대화 상자는 콘텐츠 작성자가 제공할 수 있는 인터페이스를 표시합니다. 이 구현의 경우 AEM WCM 핵심 구성 요소 **이미지** 구성 요소는 Byline의 이미지 작성 및 렌더링을 처리하는 데 사용되므로 이 구성 요소를 다음과 같이 설정해야 합니다 `sling:resourceSuperType`.

### 구성 요소 정의 만들기 {#create-component-definition}

1. 다음에서 **ui.apps** 모듈, 다음으로 이동 `/apps/wknd/components` 폴더 만들기 `byline`.
1. 내부 `byline` 폴더, 다음 이름의 파일 추가 `.content.xml`

   ![노드를 만드는 대화 상자](assets/custom-component/byline-node-creation.png)

1. 채우기 `.content.xml` 파일이 포함된 파일:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   위의 XML 파일은 제목, 설명 및 그룹을 포함하여 구성 요소에 대한 정의를 제공합니다. 다음 `sling:resourceSuperType` 포인트 대상 `core/wcm/components/image/v2/image`, - [핵심 이미지 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html).

### HTL 스크립트 만들기 {#create-the-htl-script}

1. 내부 `byline` 폴더, 파일 추가 `byline.html`: 구성 요소의 HTML 프레젠테이션을 담당합니다. Sling이 이 리소스 유형을 렌더링하는 데 사용하는 기본 스크립트가 되므로 파일과 동일한 파일의 이름을 지정하는 것이 중요합니다.

1. 에 다음 코드를 추가합니다 `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

다음 `byline.html` 은(는) [나중에 다시 방문함](#byline-htl)Sling 모델이 만들어지면 HTL 파일의 현재 상태를 사용하면 구성 요소를 드래그하여 페이지에 놓을 때 AEM Sites의 페이지 편집기에서 빈 상태로 표시할 수 있습니다.

### 대화 상자 정의 만들기 {#create-the-dialog-definition}

다음으로, 다음 필드가 있는 Byline 구성 요소의 대화 상자를 정의합니다.

* **이름**: 기여자의 이름을 나타내는 텍스트 필드입니다.
* **이미지**: 기여자의 생체 인식 사진에 대한 참조.
* **직업**: 기여자로 인한 작업 목록입니다. 직업은 알파벳순으로 오름차순(a에서 z까지)으로 정렬해야 합니다.

1. 내부 `byline` 폴더, 다음 이름의 폴더 만들기 `_cq_dialog`.
1. 내부 `byline/_cq_dialog`, 다음 이름의 파일 추가 `.content.xml`. 대화 상자에 대한 XML 정의입니다. 다음 XML을 추가합니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
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
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
                       <properties
                               jcr:primaryType="nt:unstructured"
                               jcr:title="Properties"
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
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
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

   이러한 대화 상자 노드 정의는 [Sling 리소스 병합](https://sling.apache.org/documentation/bundles/resource-merger.html) 에서 상속되는 대화 상자 탭을 제어하려면 `sling:resourceSuperType` 구성 요소, 이 경우 **핵심 구성 요소의 이미지 구성 요소**.

   ![byline에 대한 대화 상자 완료됨](assets/custom-component/byline-dialog-created.png)

### 정책 대화 상자 만들기 {#create-the-policy-dialog}

대화 상자 만들기와 동일한 방법에 따라 핵심 구성 요소의 이미지 구성 요소에서 상속된 정책 구성에서 원하지 않는 필드를 숨기기 위한 정책 대화 상자(이전의 디자인 대화 상자)를 만듭니다.

1. 내부 `byline` 폴더, 다음 이름의 폴더 만들기 `_cq_design_dialog`.
1. 내부 `byline/_cq_design_dialog`, (이)라는 이름의 파일 만들기 `.content.xml`. 다음 XML을 사용하여 파일을 다음과 같이 업데이트합니다. 가장 쉽게 다음을 열 수 있습니다 `.content.xml` 아래의 XML을 복사하여 붙여넣을 수 있습니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   이전 의 기초 **정책 대화 상자** XML은 [코어 구성 요소 이미지 구성 요소](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   대화 상자 구성과 마찬가지로 [Sling 리소스 병합](https://sling.apache.org/documentation/bundles/resource-merger.html) 에서 상속된 관련 없는 필드를 숨기는 데 사용됩니다. `sling:resourceSuperType`를 사용하여 노드 정의에서 볼 수 있듯이 `sling:hideResource="{Boolean}true"` 속성.

### 코드 배포 {#deploy-the-code}

1. 의 변경 내용 동기화 `ui.apps` IDE를 사용하거나 Maven 기술을 사용합니다.

   ![AEM server byline 구성 요소로 내보내기](assets/custom-component/export-byline-component-aem.png)

## 페이지에 구성 요소 추가 {#add-the-component-to-a-page}

AEM 구성 요소 개발에 초점을 맞추고 단순하게 유지하기 위해 현재 상태의 Byline 구성 요소를 문서 페이지에 추가하여 을 확인하겠습니다. `cq:Component` 노드 정의가 올바릅니다. 또한 AEM에서 새 구성 요소 정의를 인식하고 구성 요소의 대화 상자가 작성에 작동하는지 확인합니다.

### AEM Assets에 이미지 추가

먼저 샘플 헤드 샷을 AEM Assets에 업로드하여 를 사용하여 Byline 구성 요소에서 이미지를 채웁니다.

1. AEM Assets의 LA Skateparks 폴더로 이동합니다. [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. 에 대한 헤드 샷 업로드  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** 폴더로 이동합니다.

   ![AEM Assets에 업로드된 헤드샷](assets/custom-component/stacey-roswell-headshot-assets.png)

### 구성 요소 작성 {#author-the-component}

그런 다음 AEM의 페이지에 Byline 구성 요소를 추가합니다. Byline 구성 요소가 **WKND Sites 프로젝트 - 콘텐츠** 구성 요소 그룹, `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 정의, 모든 사용자가 자동으로 사용할 수 있음 **컨테이너** 누구 **정책** 허용: **WKND Sites 프로젝트 - 콘텐츠** 구성 요소 그룹. 따라서 문서 페이지의 레이아웃 컨테이너에서 사용할 수 있습니다.

1. LA Skatepark 문서( )로 이동합니다. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 왼쪽 사이드바에서 을(를) 끌어서 놓습니다. **Byline 구성 요소** 에 **bottom** 연 문서 페이지의 레이아웃 컨테이너

   ![페이지에 byline 구성 요소 추가](assets/custom-component/add-to-page.png)

1. 왼쪽 사이드바가 열려 있는지 확인합니다.**표시 및**&#x200B;자산 파인더**가 선택되어 있습니다.

1. 다음 항목 선택 **인라인 구성 요소 플레이스홀더**&#x200B;를 누르고 작업 표시줄을 표시한 다음 **렌치** 대화 상자를 여는 아이콘

1. 대화 상자를 열고 첫 번째 탭(에셋)이 활성화된 상태에서 왼쪽 사이드바를 열고 에셋 파인더에서 이미지를 이미지 드롭 영역으로 드래그합니다. WKND ui.content 패키지에서 제공되는 Stacey Roflowers 바이오 사진을 찾기 위해 &quot;스테이시&quot;를 검색하세요.

   ![대화 상자에 이미지 추가](assets/custom-component/add-image.png)

1. 이미지를 추가한 후 **속성** 탭으로 이동하여 **이름** 및 **직업**.

   직업을 입력할 때 다음을 입력합니다. **알파벳 역순** Sling 모델에서 구현된 알파벳화 비즈니스 논리를 검증하도록 순서를 지정합니다.

   탭 **완료** 버튼을 클릭하여 변경 사항을 저장합니다.

   ![byline 구성 요소의 속성 채우기](assets/custom-component/add-properties.png)

   AEM 작성자는 대화 상자를 통해 구성 요소를 구성하고 작성합니다. 이 시점에서 Byline 구성 요소 개발 시 데이터 수집을 위한 대화 상자가 포함되지만 작성된 콘텐츠를 렌더링하는 논리는 아직 추가되지 않았습니다. 따라서 자리 표시자만 표시됩니다.

1. 대화 상자를 저장한 후 다음으로 이동 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) AEM 페이지 아래의 byline 구성 요소 콘텐츠 노드에 구성 요소의 콘텐츠가 저장되는 방식을 검토합니다.

   LA Skate Parks 페이지 아래에서 Byline 구성 요소 콘텐츠 노드를 찾습니다. 즉, `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   속성 이름을 확인합니다. `name`, `occupations`, 및 `fileReference` 다음에 저장됩니다. **바이라인 노드**.

   또한 `sling:resourceType` / 노드가 로 설정됨 `wknd/components/content/byline` - 는 이 콘텐츠 노드를 Byline 구성 요소 구현에 바인딩합니다.

   ![CRXDE의 인라인 속성](assets/custom-component/byline-properties-crxde.png)

## Byline Sling 모델 만들기 {#create-sling-model}

다음으로 데이터 모델로 작동하는 슬링 모델을 만들어 Byline 구성 요소에 대한 비즈니스 논리를 포함하겠습니다.

Sling 모델은 JCR에서 Java™ 변수로의 데이터 매핑을 촉진하고 AEM 컨텍스트에서 개발 시 효율성을 제공하는 주석 기반 Java™ POJO(일반 이전 Java™ 개체)입니다.

### Maven 종속성 검토 {#maven-dependency}

Byline Sling 모델은 AEM에서 제공하는 여러 Java™ API를 사용합니다. 이러한 API는 `dependencies` 다음에 나열됨: `core` 모듈의 POM 파일입니다. 이 자습서에 사용된 프로젝트는 AEM용으로 as a Cloud Service으로 빌드되었습니다. 그러나 AEM 6.5/6.4와 이전 버전과 호환되므로 독특합니다. 따라서 Cloud Service과 AEM 6.x에 대한 종속성이 모두 포함됩니다.

1. 를 엽니다. `pom.xml` 아래에 파일 `<src>/aem-guides-wknd/core/pom.xml`.
1. 다음에 대한 종속성 찾기 `aem-sdk-api` - **AEM as a Cloud Service 전용**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   다음 [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) AEM에 의해 노출된 모든 공개 Java™ API를 포함합니다. 다음 `aem-sdk-api` 이 프로젝트를 빌드할 때 기본적으로 사용됩니다. 버전은 의 프로젝트 루트에서 상위 반응기 pom에 유지됩니다. `aem-guides-wknd/pom.xml`.

1. 에 대한 종속성 찾기 `uber-jar` - **AEM 6.5/6.4만**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   다음 `uber-jar` 다음 경우에만 포함됩니다 `classic` 프로필이 호출됩니다. 즉, `mvn clean install -PautoInstallSinglePackage -Pclassic`. 이는 이 프로젝트에 고유합니다. AEM Project Archetype에서 생성된 실제 프로젝트에서는 `uber-jar` 지정된 AEM 버전이 6.5 또는 6.4인 경우 기본값입니다.

   다음 [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) AEM 6.x에 의해 노출된 모든 공개 Java™ API를 포함합니다. 버전은 프로젝트 루트의 상위 반응기 pom에서 유지됩니다. `aem-guides-wknd/pom.xml`.

1. 다음에 대한 종속성 찾기 `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   AEM 핵심 구성 요소에 의해 노출된 전체 공개 Java™ API입니다. AEM 핵심 구성 요소는 AEM 외부에서 유지 관리되는 프로젝트이므로 릴리스 주기가 별도로 있습니다. 이러한 이유로 별도로 포함되어야 하는 종속성이며 다음과 같습니다 **아님** 에 포함됨 `uber-jar` 또는 `aem-sdk-api`.

   uber-jar와 마찬가지로 이 종속성에 대한 버전은 의 상위 리액터 pom 파일에서 유지됩니다. `aem-guides-wknd/pom.xml`.

   이 자습서의 뒷부분에서 핵심 구성 요소의 이미지 클래스를 사용하여 바이라인 구성 요소에 이미지를 표시할 수 있습니다. 슬링 모델을 빌드하고 컴파일하려면 핵심 구성 요소 종속성이 있어야 합니다.

### Byline 인터페이스 {#byline-interface}

바이라인에 대한 공개 Java™ 인터페이스를 만듭니다. 다음 `Byline.java` 은(는) 다음을 추진하는 데 필요한 공용 메서드를 정의합니다. `byline.html` HTL 스크립트.

1. 내부, `core` 다음 내의 모듈 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 폴더 (이)라는 이름의 파일 만들기 `Byline.java`

   ![byline 인터페이스 만들기](assets/custom-component/create-byline-interface.png)

1. 업데이트 `Byline.java` 다음 메서드를 사용하여

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   처음 두 메서드에서는 **이름** 및 **직업** (Byline 구성 요소)

   다음 `isEmpty()` 메서드는 구성 요소에 렌더링할 콘텐츠가 있는지 또는 구성 대기 중인지 확인하는 데 사용됩니다.

   이미지에 대한 메서드는 없습니다. [이 내용은 나중에 검토됩니다](#tackling-the-image-problem).

1. 공개 Java™ 클래스가 포함된 Java™ 패키지(이 경우 Sling 모델)는 패키지의 버전을 사용하여 지정해야 합니다.  `package-info.java` 파일.

   WKND 소스의 Java™ 패키지 이후 `com.adobe.aem.guides.wknd.core.models` 버전 선언 `1.0.0`, 그리고 중단되지 않는 공용 인터페이스와 메서드를 추가하고 버전을 다음으로 늘려야 합니다. `1.1.0`. 다음 위치에서 파일을 엽니다. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` 및 업데이트 `@Version("1.0.0")` 끝 `@Version("2.1.0")`.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

이 패키지의 파일이 변경될 때마다 [패키지 버전은 의미론적으로 조정해야 합니다](https://semver.org/). 그렇지 않은 경우 Maven 프로젝트의 [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd) 은(는) 잘못된 패키지 버전을 감지하고 빌드된 내용을 중단합니다. 다행히도 실패 시 Maven 플러그인은 잘못된 Java™ 패키지 버전 및 그 버전을 보고합니다. 업데이트 `@Version("...")` 를 위반하는 Java™ 패키지의 선언 `package-info.java` 플러그인이 권장하는 버전으로 수정할 수 있습니다.

### Byline 구현 {#byline-implementation}

다음 `BylineImpl.java` 는 을 구현하는 슬링 모델 구현입니다 `Byline.java` 이전에 정의된 인터페이스입니다. 의 전체 코드 `BylineImpl.java` 은 이 섹션의 맨 아래에서 찾을 수 있습니다.

1. 다음 이름의 폴더 만들기 `impl` 아래에 `core/src/main/java/com/adobe/aem/guides/core/models`.
1. 다음에서 `impl` 폴더, 파일 만들기 `BylineImpl.java`.

   ![인라인 Impl 파일](assets/custom-component/byline-impl-file.png)

1. `BylineImpl.java`를 엽니다. 다음을 구현하도록 지정 `Byline` 인터페이스. IDE의 자동 완성 기능을 사용하거나 구현에 필요한 메서드를 포함하도록 파일을 수동으로 업데이트하십시오. `Byline` 인터페이스:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. 를 업데이트하여 슬링 모델 주석 추가 `BylineImpl.java` 다음 클래스 수준 주석을 사용하는 경우입니다. 이 `@Model(..)`주석은 클래스를 슬링 모델로 변환하는 것입니다.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   이 주석과 해당 매개 변수를 검토해 보겠습니다.

   * 다음 `@Model` 주석은 BylineImpl을 AEM에 배포할 때 Sling 모델로 등록합니다.
   * 다음 `adaptables` 매개 변수는 이 모델을 요청으로 조정할 수 있도록 지정합니다.
   * 다음 `adapters` 매개 변수를 사용하면 구현 클래스를 Byline 인터페이스에 등록할 수 있습니다. 이를 통해 HTL 스크립트는 인터페이스를 통해(직접 구현하지 않고) 슬링 모델을 호출할 수 있습니다. [어댑터에 대한 자세한 내용은 여기에서 확인하십시오](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * 다음 `resourceType` 는 Byline 구성 요소 리소스 유형(이전에 만든 항목)을 가리키며, 구현이 여러 개인 경우 올바른 모델을 해결하는 데 도움이 됩니다. [모델 클래스를 리소스 유형과 연관시키는 방법에 대한 자세한 내용은 여기에서 확인하십시오](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Sling 모델 메서드 구현 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

구현된 첫 번째 메서드는 다음과 같습니다. `getName()`를 반환하면 속성 아래에 있는 byline의 JCR 콘텐츠 노드에 저장된 값이 반환됩니다 `name`.

이 경우 `@ValueMapValue` Sling 모델 주석은 요청 리소스의 ValueMap을 사용하여 Java™ 필드에 값을 삽입하는 데 사용됩니다.


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

JCR 속성은 이름을 Java™ 필드로 공유하므로(둘 다 &quot;name&quot;임), `@ValueMapValue` 자동으로 이 연관을 해결하고 속성 값을 Java™ 필드에 삽입합니다.

#### getOccuptions() {#implementing-get-occupations}

구현할 다음 메서드는 입니다. `getOccupations()`. 이 메서드는 JCR 속성에 저장된 작업을 로드합니다 `occupations` 정렬된(알파벳순) 컬렉션을 반환합니다.

에서 탐색한 것과 동일한 기법 사용 `getName()` 속성 값을 슬링 모델의 필드에 삽입할 수 있습니다.

JCR 속성 값을 삽입된 Java™ 필드를 통해 Sling 모델에서 사용할 수 있게 되면 `occupations`, 정렬 비즈니스 논리는 `getOccupations()` 메서드를 사용합니다.


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

마지막 공개 메서드는 `isEmpty()` 구성 요소가 렌더링할 만큼 &quot;충분히 작성&quot;된 것으로 간주해야 하는 시기를 결정합니다.

이 구성 요소의 경우 비즈니스 요구 사항은 모두 세 개의 필드입니다. `name, image and occupations` 작성해야 합니다. *다음 이전* 구성 요소를 렌더링할 수 있습니다.


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### &quot;이미지 문제&quot; 해결 {#tackling-the-image-problem}

이름과 직업 조건을 확인하는 것은 사소한 일이며 Apache Commons Lang3은 유용한 기능을 제공합니다 [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) 클래스. 하지만, 어떻게 **이미지 유무** 핵심 구성 요소의 이미지 구성 요소를 사용하여 이미지를 표시하기 때문에 의 유효성을 검사할 수 있습니다.

이 문제를 해결하는 방법에는 두 가지가 있습니다.

다음을 확인함: `fileReference` JCR 속성이 자산으로 확인됩니다. *또는* 이 리소스를 핵심 구성 요소 이미지 Sling 모델로 변환하고 `getSrc()` 메서드가 비어 있지 않습니다.

을(를) 사용합니다. **초** 접근. 첫 번째 접근 방법으로도 충분하지만, 이 자습서에서는 후자를 사용하여 슬링 모델의 다른 기능을 탐색할 수 있습니다.

1. 이미지를 가져오는 개인 메서드를 만듭니다. 이 메서드는 HTL 자체의 이미지 개체를 노출할 필요가 없고 드라이브에만 사용되므로 비공개로 둡니다 `isEmpty().`

   에 대해 다음 개인 방법 추가 `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   위에서 설명한 대로 를 가져오는 두 가지 접근 방식이 더 있습니다. **이미지 Sling 모델**:

   첫 번째 사용자는 `@Self` 주석: 현재 요청을 자동으로 핵심 구성 요소에 조정 `Image.class`

   두 번째는 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi 서비스는 편리한 서비스이며 Java™ 코드에서 다른 유형의 Sling 모델을 만드는 데 도움이 됩니다.

   두 번째 방법을 사용하겠습니다.

   >[!NOTE]
   >
   >실제 구현에서 다음을 사용하여 &quot;One&quot;에 접근합니다. `@Self` 보다 간단하고 우아한 솔루션이므로 가 선호됩니다. 이 자습서에서는 두 번째 접근 방식을 사용합니다. 두 번째 접근 방식은 보다 복잡한 구성 요소로서 유용한 Sling 모델의 더 많은 측면을 탐색해야 하기 때문입니다.

   Sling 모델은 OSGi 서비스가 아닌 Java™ POJO이므로 일반적인 OSGi 주입 주석은 `@Reference` **할 수 없음** 를 사용합니다. 대신 Sling 모델이 특별한 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 유사한 기능을 제공하는 주석입니다.

1. 업데이트 `BylineImpl.java` 다음을 포함 `OSGiService` 주석을 삽입할 `ModelFactory`:

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   포함 `ModelFactory` 사용 가능한 핵심 구성 요소의 이미지 슬링 모델은 다음을 사용하여 생성할 수 있습니다.

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   그러나 이 메서드는 Sling 모델에서 아직 사용할 수 없는 요청과 리소스를 모두 필요로 합니다. 이를 위해 더 많은 Sling 모델 주석이 사용됩니다!

   현재 요청을 가져오려면 **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 주석을 사용하여 `adaptable` (다음에서 정의됨) `@Model(..)` 다음으로: `SlingHttpServletRequest.class`를 Java™ 클래스 필드로 복사합니다.

1. 추가 **@Self** 주석을 사용하여 **SlingHttpServletRequest 요청**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   기억하기, 사용 `@Self Image image` 핵심 구성 요소의 이미지 슬링 모델을 주입하는 것은 위의 옵션이었습니다. - `@Self` 주석은 조정 가능한 개체(이 경우 SlingHttpServletRequest)를 삽입하고 주석 필드 유형에 맞게 조정하려고 합니다. 핵심 구성 요소의 이미지 Sling 모델은 SlingHttpServletRequest 오브젝트에서 조정할 수 있으므로 제대로 작동했을 것이며 더 탐색적인 것보다 코드가 적습니다 `modelFactory` 접근.

   이제 ModelFactory API를 통해 이미지 모델을 인스턴스화하는 데 필요한 변수가 삽입되었습니다. Sling 모델 을 사용해 보겠습니다. **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 슬링 모델이 인스턴스화된 후 이 개체를 가져오는 주석입니다.

   `@PostConstruct` 는 매우 유용하며 생성자와 비슷한 용량으로 작동하지만 클래스가 인스턴스화되고 주석이 달린 모든 Java™ 필드가 주입된 후 호출됩니다. 다른 Sling 모델 주석은 Java™ 클래스 필드(변수)에 주석을 달지만, `@PostConstruct` 일반적으로 로 이름이 지정된 void, zero 매개 변수 방법에 주석을 추가합니다. `init()` (하지만 이름은 무엇이든 지정할 수 있습니다.)

1. 추가 **@PostConstruct** 방법:

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   Sling 모델은 **아님** OSGi 서비스를 통해 클래스 상태를 유지하는 것이 안전합니다. 자주 `@PostConstruct` 는 일반 생성자가 수행하는 작업과 유사한, 나중에 사용할 수 있도록 Sling Model 클래스 상태를 파생하고 설정합니다.

   다음과 같은 경우 `@PostConstruct` 메서드가 예외를 throw합니다. Sling 모델이 인스턴스화되지 않고 null입니다.

1. **getImage()** 이제 를 업데이트하여 이미지 개체를 반환할 수 있습니다.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 다음으로 돌아가기 `isEmpty()` 구현을 완료합니다.

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   에 대한 여러 호출 참고 `getImage()` 은(는) 초기화된 을(를) 반환하므로 문제가 없습니다. `image` 클래스 변수 및 를 호출하지 않음 `modelFactory.getModelFromWrappedRequest(...)` 비용이 지나치게 많이 드는 것은 아니지만 불필요하게 전화를 하는 것을 피할 가치가 있다.

1. 최종 `BylineImpl.java` 은(는) 다음과 같아야 합니다.


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## 바이라인 HTL {#byline-htl}

다음에서 `ui.apps` 모듈, 열기 `/apps/wknd/components/byline/byline.html` AEM 구성 요소의 이전 설정에서 만들어졌습니다.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

지금까지 이 HTL 스크립트가 수행하는 작업을 검토해 보겠습니다.

* 다음 `placeholderTemplate` 는 구성 요소가 완전히 구성되지 않았을 때 표시되는 핵심 구성 요소의 자리 표시자를 가리킵니다. AEM Sites 페이지 편집기에서 위에 정의된 대로 구성 요소 제목이 있는 상자로 렌더링됩니다. `cq:Component`의  `jcr:title` 속성.

* 다음 `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` 를 로드합니다. `placeholderTemplate` 위에서 정의하고 부울 값으로 전달합니다(현재 하드 코딩됨). `false`)을 클릭하여 자리 표시자 템플릿에서 사용할 수 있습니다. 날짜 `isEmpty` 은 true이고, 자리 표시자 템플릿은 회색 상자를 렌더링하고, 그렇지 않으면 아무 것도 렌더링하지 않습니다.

### 인라인 HTL 업데이트

1. 업데이트 **byline.html** 다음 골격 HTML 구조:

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   CSS 클래스는 [BEM 명명 규칙](https://getbem.com/naming/). BEM 규칙은 필수가 아니지만, 핵심 구성 요소 CSS 클래스에서 사용되며 일반적으로 읽을 수 있는 깔끔한 CSS 규칙을 만들기 때문에 BEM이 권장됩니다.

### HTL에서 Sling 모델 개체 인스턴스화 {#instantiating-sling-model-objects-in-htl}

다음 [블록 문 사용](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) 는 HTL 스크립트에서 Sling 모델 개체를 인스턴스화하고 HTL 변수에 할당하는 데 사용됩니다.

다음 `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` bylineImpl에 의해 구현된 Byline 인터페이스(com.adobe.aem.guides.wknd.models.Byline)를 사용하고 현재 SlingHttpServletRequest를 해당 인터페이스에 조정하며, 그 결과는 HTL 변수 이름 byline( `data-sly-use.<variable-name>`).

1. 외부 업데이트 `div` 을(를) 참조하려면 **필자** 공용 인터페이스별 Sling 모델:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Sling 모델 메서드 액세스 {#accessing-sling-model-methods}

HTL은 JSTL에서 가져오며 동일한 단축의 Java™ getter 메서드 이름을 사용합니다.

예를 들어 Byline Sling 모델 `getName()` 메서드는 다음으로 줄일 수 있습니다. `byline.name`, 대신 비슷한 방식으로 `byline.isEmpty`, 다음과 단락될 수 있습니다. `byline.empty`. 전체 메서드 이름을 사용하는 경우 `byline.getName` 또는 `byline.isEmpty`도 작동합니다. 다음을 참고하십시오. `()` 는 HTL에서 메서드를 호출하는 데 사용되지 않습니다(JSTL과 유사).

매개 변수가 필요한 Java™ 메서드 **할 수 없음** htl에서 사용됩니다. 이는 HTL의 논리를 간단하게 유지하기 위한 것입니다.

1. Byline 이름을 구성 요소에 추가하려면 `getName()` Byline Sling 모델 또는 HTL의 메서드: `${byline.name}`.

   업데이트 `h2` 태그:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### HTL 표현식 옵션 사용 {#using-htl-expression-options}

[HTL 표현식 옵션](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) htl의 콘텐츠에 대한 수정자로 사용되며 날짜 형식부터 i18n 번역까지 다양합니다. 표현식은 쉼표로 구분된 형식으로 작업을 표시하는 데 필요한 목록이나 값 배열을 결합하는 데 사용할 수도 있습니다.

표현식은 를 통해 추가됩니다 `@` HTL 표현식의 연산자입니다.

1. &quot;, &quot;로 작업 목록을 조인하려면 다음 코드를 사용합니다.

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 조건부로 자리 표시자 표시 {#conditionally-displaying-the-placeholder}

AEM 구성 요소에 대한 대부분의 HTL 스크립트는 **자리 표시자 패러다임** 작성자에게 시각적 신호를 제공하려면 **구성 요소가 잘못 작성되고 AEM Publish에 표시되지 않음을 나타냄**. 이 결정을 유도하는 규칙은 이 경우 구성 요소의 지원 Sling 모델에 대해 메서드를 구현하는 것입니다. `Byline.isEmpty()`.

다음 `isEmpty()` 메서드는 Byline Sling 모델 및 결과(또는 음수)를 통해 호출됩니다. `!` 연산자가 라는 HTL 변수에 저장됩니다. `hasContent`:

1. 외부 업데이트 `div` 이름이 인 HTL 변수를 저장하려면 `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   의 사용을 주목하십시오 `data-sly-test`, HTL `test` 블록은 중요합니다. 블록은 HTL 변수를 설정하고 해당 변수를 기반으로 하는 HTML 요소를 렌더링/렌더링하지 않습니다. HTL 표현식 평가 결과를 기반으로 합니다. true이면 HTML 요소가 렌더링되고 그렇지 않으면 렌더링되지 않습니다.

   이 HTL 변수 `hasContent` 이제 자리 표시자를 조건부로 표시하거나 숨기는 데 재사용할 수 있습니다.

1. 조건부 호출을에 업데이트 `placeholderTemplate` 파일 하단에 있고 다음과 같이 표시됩니다.

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 핵심 구성 요소를 사용하여 이미지 표시 {#using-the-core-components-image}

에 대한 HTL 스크립트 `byline.html` 은(는) 이제 거의 완료되었으며 이미지만 누락되었습니다.

다음으로: `sling:resourceSuperType` 핵심 구성 요소의 이미지 구성 요소를 가리키게 하여 이미지를 작성합니다. 핵심 구성 요소의 이미지 구성 요소를 사용하여 이미지를 렌더링할 수 있습니다.

이 경우 현재 인라인 리소스를 포함하지만 리소스 유형을 사용하여 핵심 구성 요소의 이미지 구성 요소에 리소스 유형을 강제 적용해 보겠습니다 `core/wcm/components/image/v2/image`. 이는 구성 요소 재사용을 위한 강력한 패턴입니다. 이 경우 HTL의 `data-sly-resource` 블록이 사용됩니다.

1. 바꾸기 `div` 의 클래스로 `cmp-byline__image` 다음을 사용하여:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   이 `data-sly-resource`: 상대 경로를 통해 현재 리소스를 포함합니다. `'.'`, 그리고 의 리소스 유형으로 현재 리소스(또는 byline 콘텐츠 리소스)를 강제로 포함합니다. `core/wcm/components/image/v2/image`.

   핵심 구성 요소 리소스 유형은 스크립트 내 사용이며 콘텐츠로 지속되지 않으므로 프록시를 통하지 않고 직접 사용됩니다.

2. 완료됨 `byline.html` 아래:

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. 로컬 AEM 인스턴스에 코드 베이스를 배포합니다. 다음에 대한 변경 이후 `core` 및 `ui.apps` 두 모듈을 모두 배포해야 합니다.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   AEM 6.5/6.4에 배포하려면 다음을 호출하십시오. `classic` 프로필:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > Maven 프로필을 사용하여 루트에서 전체 프로젝트를 빌드할 수도 있습니다 `autoInstallSinglePackage` 하지만 이렇게 하면 페이지의 콘텐츠 변경 사항을 덮어쓸 수 있습니다. 이유는 다음과 같습니다. `ui.content/src/main/content/META-INF/vault/filter.xml` 튜토리얼 시작 코드가 기존 AEM 콘텐츠를 완전히 덮어쓰도록 수정되었습니다. 실제 시나리오에서는 이것이 문제가 아닙니다.

### 스타일이 지정되지 않은 Byline 구성 요소 검토 {#reviewing-the-unstyled-byline-component}

1. 업데이트를 배포한 후 다음으로 이동합니다. [LA 스케이트보드장 최종 가이드](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) 페이지 또는 이 장의 앞부분에서 바이라인 구성 요소를 추가한 위치입니다.

1. 다음 **이미지**, **이름**, 및 **직업** 이제 가 나타나고 스타일이 지정되지 않았지만 작업 중인 Byline 구성 요소가 있습니다.

   ![스타일이 지정되지 않은 byline 구성 요소](assets/custom-component/unstyled.png)

### Sling 모델 등록 검토 {#reviewing-the-sling-model-registration}

다음 [AEM Web Console의 Sling 모델 상태 보기](http://localhost:4502/system/console/status-slingmodels) AEM에 등록된 모든 Sling 모델을 표시합니다. 이 목록을 검토하여 Byline Sling 모델이 설치되어 있는지 확인하고 인식되었는지 확인할 수 있습니다.

다음과 같은 경우 **BylineImpl** 이 목록에 표시되지 않습니다. 슬링 모델의 주석에 문제가 있거나 모델이 올바른 패키지에 추가되지 않았을 수 있습니다. (`com.adobe.aem.guides.wknd.core.models`)을 클릭하여 핵심 프로젝트에 추가합니다.

![Byline Sling 모델이 등록됨](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## 줄 스타일 {#byline-styles}

Byline 구성 요소를 제공된 크리에이티브 디자인에 맞추기 위해 스타일링해 보겠습니다. 이 작업은 SCSS 파일을 사용하고 **ui.frontend** 모듈.

### 기본 스타일 추가

Byline 구성 요소의 기본 스타일을 추가합니다.

1. IDE로 돌아가서 **ui.frontend** 프로젝트 `/src/main/webpack/components`:
1. 이름이 인 파일 만들기 `_byline.scss`.

   ![byline 프로젝트 탐색기](assets/custom-component/byline-style-project-explorer.png)

1. SCSS로 작성된 Byline 구현 CSS를 `_byline.scss`:

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. 터미널을 열고 로 이동합니다. `ui.frontend` 모듈.
1. 시작 `watch` 다음 npm 명령을 사용하여 을 처리합니다.

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. 브라우저로 돌아가서 [LA SkateParks 문서](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 구성 요소에 업데이트된 스타일이 표시됩니다.

   ![완료된 필자 구성 요소](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 오래된 CSS가 제공되지 않도록 브라우저 캐시를 지우고 전체 스타일을 가져오려면 Byline 구성 요소로 페이지를 새로 고쳐야 할 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. Adobe Experience Manager을 사용하여 처음부터 사용자 지정 구성 요소를 만들었습니다!

### 다음 단계 {#next-steps}

모든 것이 제대로 개발되고 구현된 비즈니스 논리가 올바르고 완전한지 확인하기 위해 Byline Java™ 코드에 대한 JUnit 테스트를 작성하는 방법을 탐색하여 AEM 구성 요소 개발에 대해 계속 알아보십시오.

* [단위 테스트 또는 AEM 구성 요소 작성](unit-testing.md)

에서 완료된 코드 보기 [GitHub](https://github.com/adobe/aem-guides-wknd) 또는 Git 분기의 로컬에서 코드를 검토하고 배포합니다 `tutorial/custom-component-solution`.

1. 복제 [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 리포지토리.
1. 다음을 확인하십시오. `tutorial/custom-component-solution` 분기
