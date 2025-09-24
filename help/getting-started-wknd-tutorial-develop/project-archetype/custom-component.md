---
title: 사용자 정의 구성 요소
description: 작성된 콘텐츠를 표시하는 사용자 정의 Byline 구성 요소의 엔드 투 엔드 생성을 다룹니다. 비즈니스 로직을 캡슐화하여 Byline 구성 요소를 채우고 해당 구성 요소를 렌더링하는 해당 HTL을 개발하는 Sling 모델을 포함합니다.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4072
mini-toc-levels: 1
thumbnail: 30181.jpg
doc-type: Tutorial
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
duration: 1039
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '3883'
ht-degree: 100%

---

# 사용자 정의 구성 요소 {#custom-component}

이 튜토리얼은 대화 상자에서 작성된 콘텐츠를 표시하는 사용자 정의 `Byline` AEM 구성 요소의 엔드 투 엔드 생성을 다루며 구성 요소의 HTL을 채우는 비즈니스 로직을 캡슐화하는 Sling 모델의 개발을 살펴봅니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구와 지침을 검토합니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 챕터를 성공적으로 완료했다면 프로젝트를 재사용하고 스타터 프로젝트를 체크아웃하는 단계를 건너뛸 수 있습니다.

튜토리얼에서 기반으로 삼는 기본 코드를 확인합니다.

1. `tutorial/custom-component-start`[GitHub](https://github.com/adobe/aem-guides-wknd)의 분기를 확인합니다.

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 모든 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

완성된 코드는 언제든지 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution)에서 볼 수 있으며 `tutorial/custom-component-solution` 분기로 전환하여 로컬에서 코드를 확인할 수도 있습니다.

## 목표

1. 사용자 정의 AEM 구성 요소를 빌드하는 방법 이해
1. Sling 모델을 사용하여 비즈니스 로직을 캡슐화하는 방법 학습
1. HTL 스크립트 내에서 Sling 모델을 사용하는 방법 이해

## 빌드 결과물 {#what-build}

WKND 튜토리얼의 이 부분에서는 문서의 참여자에 관하여 작성된 정보를 표시하는 데 사용되는 Byline 구성 요소를 만듭니다.

![Byline 구성 요소 예](assets/custom-component/byline-design.png)

*Byline 구성 요소*

Byline 구성 요소의 구현에는 Byline 콘텐츠를 수집하는 대화 상자와 다음과 같은 세부 정보를 검색하는 사용자 정의 Sling 모델이 포함됩니다.

* 이름
* 이미지
* 직업

## Byline 구성 요소 만들기 {#create-byline-component}

먼저, Byline C구성 요소 노드 구조를 만들고 대화 상자를 정의합니다. 이는 AEM의 구성 요소를 나타내며 JCR 내의 위치에 따라 구성 요소의 리소스 유형을 암묵적으로 정의합니다.

대화 상자는 콘텐츠 작성자가 제공할 수 있는 인터페이스를 표시합니다. 이 구현의 경우, AEM WCM 핵심 구성 요소의 **이미지** 구성 요소가 Byline의 이미지 작성 및 렌더링을 처리하는 데 사용되므로 이 구성 요소의 `sling:resourceSuperType`으로 설정해야 합니다.

### 구성 요소 정의 만들기 {#create-component-definition}

1. **ui.apps** 모듈에서 `/apps/wknd/components`로 이동한 다음 `byline`이라는 폴더를 생성합니다.
1. `byline` 폴더 안에 `.content.xml`이라는 이름의 파일을 추가합니다.

   ![노드를 생성하기 위한 대화 상자](assets/custom-component/byline-node-creation.png)

1. 다음 내용으로 `.content.xml` 파일을 채웁니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   위의 XML 파일은 제목, 설명, 그룹을 포함한 구성 요소에 대한 정의를 제공합니다. `sling:resourceSuperType`은 [핵심 이미지 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html?lang=ko)인 `core/wcm/components/image/v2/image`를 가리킵니다.

### HTL 스크립트 만들기 {#create-the-htl-script}

1. `byline` 폴더 안에 구성 요소의 HTML 표현을 담당하는 `byline.html` 파일을 추가합니다. 파일 이름을 폴더와 동일하게 지정해야 합니다. 파일 이름이 이 리소스 유형을 렌더링하는 데 사용하는 기본 스크립트가 되기 때문입니다.

1. `byline.html`에 다음 코드를 추가합니다.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html`은 Sling 모델이 생성되면 [나중에 다시 방문](#byline-htl)합니다. HTL 파일의 현재 상태로 인해 구성 요소를 페이지에 드래그 앤 드롭하면 AEM Sites의 페이지 편집기에서 구성 요소가 빈 상태로 표시됩니다.

### 대화 상자 정의 만들기 {#create-the-dialog-definition}

다음으로, 다음 필드를 사용하여 Byline 구성 요소에 대한 대화 상자를 정의합니다.

* **이름**: 참여자의 이름을 입력하는 텍스트 필드입니다.
* **이미지**: 참여자 프로필 사진에 대한 참조입니다.
* **직업**: 참여자에게 귀속된 직업 목록입니다. 직업은 알파벳순으로 오름차순(a~z) 정렬해야 합니다.

1. `byline` 폴더 안에 `_cq_dialog`이라는 이름의 폴더를 생성합니다.
1. `byline/_cq_dialog` 안에 `.content.xml`이라는 이름의 파일을 추가합니다. 이 파일은 대화 상자에 대한 XML 정의입니다. 다음 XML을 추가합니다.

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

   이러한 대화 상자 노드 정의는 [Sling 리소스 병합](https://sling.apache.org/documentation/bundles/resource-merger.html)을 사용하여 `sling:resourceSuperType` 구성 요소에서 어떠한 대화 상자 탭이 상속되는지 제어합니다. 이 경우 **핵심 구성 요소의 이미지 구성 요소**&#x200B;가 여기에 해당합니다.

   ![byline에 대해 완료된 대화 상자](assets/custom-component/byline-dialog-created.png)

### 정책 대화 상자 만들기 {#create-the-policy-dialog}

대화 상자 생성과 동일한 접근 방식에 따라 핵심 구성 요소의 이미지 구성 요소로부터 상속된 정책 구성에서 원치 않는 필드를 숨기기 위해 정책 대화 상자(이전에는 디자인 대화 상자라고 함)를 만듭니다.

1. `byline` 폴더 안에 `_cq_design_dialog`이라는 이름의 폴더를 생성합니다.
1. `byline/_cq_design_dialog` 폴더 안에 `.content.xml`이라는 이름의 파일을 생성합니다. 다음 XML로 파일을 업데이트합니다. 이 방법은 `.content.xml`을 열어 아래의 XML을 복사/붙여넣기하기에 가장 쉬운 방법입니다.

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

   앞의 **정책 대화 상자** XML의 기반은 [Core Components Image 구성 요소](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml)에서 가져온 것입니다.

   대화 상자 구성과 마찬가지로 [Sling 리소스 병합](https://sling.apache.org/documentation/bundles/resource-merger.html)은 기타 일반적인 경우 `sling:hideResource="{Boolean}true"` 속성을 포함하는 노드 정의와 동일하게 `sling:resourceSuperType`로부터 상속되는 연관 없는 필드를 숨기는 데 사용됩니다.

### 코드 배포 {#deploy-the-code}

1. IDE를 통해 또는 Maven 기술을 사용하여 `ui.apps` 내 변경 사항을 동기화합니다.

   ![AEM 서버 Byline 구성 요소로 내보내기](assets/custom-component/export-byline-component-aem.png)

## 페이지에 구성 요소 추가 {#add-the-component-to-a-page}

단순성을 유지하고 AEM 구성 요소 개발에 집중하기 위해 현재 상태의 Byline 구성 요소를 문서 페이지에 추가하여 `cq:Component` 노드 정의가 올바른지 확인해 보겠습니다. 또한 AEM이 새로운 구성 요소 정의를 인식하고 구성 요소의 대화 상자가 작성에 적합한지 확인합니다.

### AEM Assets에 이미지 추가

먼저 AEM Assets에 샘플 헤드샷을 업로드하여 Byline 구성 요소의 이미지를 채우는 데 사용합니다.

1. AEM Assets 내 LA 스케이트보드장 폴더로 이동합니다([http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks)).

1. **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)**&#x200B;에 대한 헤드샷을 폴더에 업로드합니다.

   ![AEM Assets에 업로드된 헤드샷](assets/custom-component/stacey-roswell-headshot-assets.png)

### 구성 요소 작성 {#author-the-component}

다음으로 AEM의 페이지에 Byline 구성 요소를 추가합니다. Byline 구성 요소는 `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 정의를 통해 **WKND Sites 프로젝트 - 콘텐츠** 구성 요소 그룹에 추가되므로 **정책**&#x200B;을 통해 **WKND Sites 프로젝트 - 콘텐츠** 구성 요소 그룹을 허용하는 모든 **컨테이너**&#x200B;에서 자동으로 제공됩니다. 따라서 이 구성 요소는 문서 페이지의 레이아웃 컨테이너에서 사용할 수 있습니다.

1. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)에 위치한 LA 스케이트보드장 문서로 이동합니다.

1. 왼쪽 사이드바에서 **Byline 구성 요소**&#x200B;를 열린 문서 페이지의 레이아웃 컨테이너의 **하단**&#x200B;에 드래그 앤 드롭합니다.

   ![페이지에 Byline 구성 요소 추가](assets/custom-component/add-to-page.png)

1. 왼쪽 사이드바가 열린 상태로 **표시되며**&#x200B;자산 파인더**가 선택되어 있는지 확인합니다.

1. **Byline 구성 요소 플레이스홀더**&#x200B;를 선택합니다. 이렇게 하면 플레이스홀더는 액션 바가 표시됩니다. 그다음 **렌치** 아이콘을 탭하여 대화 상자를 엽니다.

1. 대화 상자가 열려 있고 첫 번째 탭(자산)이 활성화된 상태에서 왼쪽 사이드바를 열고 자산 파인더에서 이미지를 이미지 드롭 영역으로 드래그합니다. &#39;stacey&#39;를 검색하여 WKND ui.content 패키지에 제공되는 Stacey Roswells의 프로필 사진을 찾습니다.

   ![대화 상자에 이미지 추가](assets/custom-component/add-image.png)

1. 이미지를 추가한 후 **속성** 탭을 클릭하여 **이름** 및 **직업**&#x200B;을 입력합니다.

   직업을 입력할 때는 **알파벳 역순**&#x200B;으로 입력합니다. 이렇게 하면 Sling 모델에 구현된 알파벳순 비즈니스 로직이 검증됩니다.

   변경 사항을 저장하려면 오른쪽 하단의 **완료** 버튼을 탭합니다.

   ![Byline 구성 요소의 속성 채우기](assets/custom-component/add-properties.png)

   AEM 작성자는 대화 상자를 통해 구성 요소를 구성하고 작성합니다. 이 시점에서 Byline 구성 요소를 개발할 때 데이터를 수집하기 위한 대화 상자가 포함되었지만 작성된 콘텐츠를 렌더링하는 로직은 아직 추가되지 않았습니다. 그러므로 플레이스홀더만 표시됩니다.

1. 대화 상자를 저장한 후 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline)로 이동하여 AEM 페이지 아래의 Byline 구성 요소 콘텐츠 노드에 구성 요소의 콘텐츠가 어떻게 저장되어 있는지 검토합니다.

   `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`에서 LA 스케이트보드장 페이지 아래의 Byline 구성 요소 콘텐츠 노드를 찾습니다.

   `name`, `occupations`, `fileReference` 등의 속성 이름이 **Byline 노드**&#x200B;에 저장되어 있다는 점에 주목하십시오.

   또한 노드의 `sling:resourceType`이 이 콘텐츠 노드를 Byline 구성 요소 구현에 바인딩하는 `wknd/components/content/byline`에 설정되어 있다는 점도 확인해 보십시오.

   ![CRXDE 내 Byline 속성](assets/custom-component/byline-properties-crxde.png)

## Byline Sling 모델 만들기 {#create-sling-model}

다음으로, Byline 구성 요소에 대한 비즈니스 로직을 수용하고 데이터 모델 역할을 하는 Sling 모델을 만들어 보겠습니다.

Sling 모델은 JCR에서 Java™ 변수로 데이터를 매핑하는 것을 용이하게 하고 AEM 컨텍스트에서 개발할 때 효율성을 제공하는 주석 기반 Java™ POJO(Plain Old Java™ 오브젝트)입니다.

### Maven 종속성 검토 {#maven-dependency}

Byline Sling 모델은 AEM이 제공하는 여러 가지 Java™ API를 활용합니다. 이러한 API는 `core` 모듈의 POM 파일에 나열된 `dependencies`를 통해 제공됩니다. 이 튜토리얼에 사용된 프로젝트는 AEM as a Cloud Service용으로 작성되었습니다. 하지만 AEM 6.5/6.4와 하위 호환된다는 특성이 있으며 이에 따라 클라우드 서비스와 AEM 6.x에 대한 종속성을 모두 포함합니다.

1. `<src>/aem-guides-wknd/core/pom.xml` 아래의 `pom.xml` 파일을 엽니다.
1. `aem-sdk-api` - **AEM as a Cloud Service 전용**&#x200B;에 대한 종속성 찾기

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=ko)에는 AEM에서 공개된 모든 Java™ API가 포함되어 있습니다. 이 프로젝트를 빌드할 때 기본적으로 `aem-sdk-api`가 사용됩니다. 해당 버전은 `aem-guides-wknd/pom.xml`에 위치한 프로젝트 루트의 Parent Reactor POM에서 유지 관리됩니다.

1. `uber-jar` - **AEM 6.5/6.4 전용**&#x200B;에 대한 종속성 찾기

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   `uber-jar`는 `classic` 프로필, 즉 `mvn clean install -PautoInstallSinglePackage -Pclassic`이 호출될 때만 포함됩니다. 다시 말씀드리지만 이 프로젝트만의 독특한 특징입니다. AEM Project Archetype에서 생성된 실제 프로젝트의 경우 지정된 AEM 버전이 6.5 또는 6.4이면 `uber-jar`가 기본값입니다.

   [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=ko#experience-manager-api-dependencies)는 AEM 6.x에서 공개된 모든 공개 Java™ API를 포함합니다. 버전은 프로젝트 루트의 Parent Reactor POM인 `aem-guides-wknd/pom.xml`에서 유지 관리됩니다.

1. `core.wcm.components.core`에 대한 종속성을 찾습니다.

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   이는 AEM 핵심 구성 요소에서 공개된 전체 공개 Java™ API입니다. AEM 핵심 구성 요소는 AEM 외부에서 유지 관리되는 프로젝트이므로 별도의 릴리스 주기를 갖습니다. 이러한 이유로, 별도로 포함되어야 하는 종속성은 `uber-jar` 또는 `aem-sdk-api`에 포함되지 **않습니다**.

   uber-jar와 마찬가지로 이 종속성에 대한 버전은 `aem-guides-wknd/pom.xml`의 Parent Reactor POM에서 유지 관리됩니다.

   이 튜토리얼의 뒷부분에서는 Core Components Image 클래스를 사용하여 Byline 구성 요소에 이미지를 표시합니다. Sling 모델을 빌드하고 컴파일하려면 핵심 구성 요소 종속성이 필요합니다.

### Byline 인터페이스 {#byline-interface}

Byline에 대한 공개 Java™ 인터페이스를 만듭니다. `Byline.java`는 `byline.html` HTL 스크립트를 지원하는 데 필요한 공개 메서드를 정의합니다.

1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 폴더 내 `core` 모듈에서 `Byline.java`라는 파일을 생성합니다.

   ![Byline 인터페이스 만들기](assets/custom-component/create-byline-interface.png)

1. 다음 메서드를 사용하여 `Byline.java`를 업데이트합니다.

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

   처음 두 가지 메서드는 Byline 구성 요소에 대한 **이름** 및 **직업**&#x200B;의 값을 노출합니다.

   `isEmpty()` 메서드는 구성 요소에 렌더링할 콘텐츠가 있는지 또는 구성 요소가 구성을 대기하고 있는지 확인하는 데 사용됩니다.

   한편 이미지에 대한 메서드는 존재하지 않습니다. [이 내용은 나중에 검토](#tackling-the-image-problem)하도록 하겠습니다.

1. 이 경우 Sling 모델에 해당하는 공개 Java™ 클래스를 포함하는 Java™ 패키지는 패키지의 `package-info.java` 파일을 사용하여 버전을 지정해야 합니다.

   WKND 소스의 Java™ 패키지인 `com.adobe.aem.guides.wknd.core.models`는 `1.0.0` 버전을 선언하며 영향을 미치지 않는 공개 인터페이스 및 메서드가 추가되므로 버전을 `1.1.0`으로 올려야 합니다. `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java`에서 파일을 열고 `@Version("1.0.0")`을 `@Version("2.1.0")`으로 업데이트합니다.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

이 패키지의 파일이 변경될 때마다 [패키지 버전은 의미상으로 조정](https://semver.org/)되어야 합니다. 그렇지 않은 경우 Maven 프로젝트의 [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd)이 잘못된 패키지 버전을 감지하여 빌드를 중단합니다. 다행히도 Maven 플러그인은 실패 시 잘못된 Java™ 패키지 버전과 올바른 버전을 보고합니다. 위반 상태인 Java™ 패키지에서 `package-info.java`의 `@Version("...")` 선언을 플러그인에서 권장하는 버전으로 업데이트하여 잘못된 버전을 수정합니다.

### Byline 구현 {#byline-implementation}

`BylineImpl.java`는 이전에 정의된 `Byline.java` 인터페이스를 구현하는 Sling 모델의 구현입니다. `BylineImpl.java`에 대한 전체 코드는 이 섹션의 하단에서 확인할 수 있습니다.

1. `core/src/main/java/com/adobe/aem/guides/core/models` 아래에서 `impl`라는 폴더를 만듭니다.
1. `impl` 폴더에 `BylineImpl.java` 파일을 만듭니다.

   ![Byline Impl 파일](assets/custom-component/byline-impl-file.png)

1. `BylineImpl.java`를 엽니다. 이를 통해 `Byline` 인터페이스가 구현됨을 명시합니다. IDE의 자동 완성 기능을 사용하거나 `Byline` 인터페이스 구현에 필요한 메서드를 포함하도록 파일을 수동으로 업데이트합니다.

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

1. 다음과 같은 클래스 수준 주석으로 `BylineImpl.java`를 업데이트하여 Sling 모델 주석을 추가합니다. 이 `@Model(..)` 주석은 클래스를 Sling 모델로 변환합니다.

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

   이 주석과 매개변수를 검토해 보겠습니다.

   * `@Model` 주석은 AEM에 배포될 때 BylineImpl을 Sling 모델로 등록합니다.
   * `adaptables` 매개변수는 이 모델이 요청에 따라 조정될 수 있음을 명시합니다.
   * `adapters` 매개변수를 사용하면 구현 클래스를 Byline 인터페이스에 등록할 수 있습니다. 이를 통해 HTL 스크립트는 구현을 직접 거치지 않고도 인터페이스를 통해 Sling 모델을 호출할 수 있습니다. [어댑터에 대한 자세한 내용은 여기에서 확인](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110)할 수 있습니다.
   * `resourceType`은 이전에 생성된 Byline 구성 요소 리소스 유형을 가리키며 여러 구현이 있는 경우 올바른 모델을 확인하는 데 도움이 됩니다. [모델 클래스를 리소스 유형과 연결하는 방법에 대한 자세한 내용은 여기](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)에서 확인할 수 있습니다.

### Sling 모델 메서드 구현 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

구현된 첫 번째 메서드는 `getName()`입니다. 이 메서드는 `name` 속성에서 Byline의 JCR 콘텐츠 노드에 저장된 값을 반환합니다.

이를 위해 `@ValueMapValue` Sling 모델 주석을 사용하여 요청의 리소스 ValueMap을 통해 Java™ 필드에 값을 주입합니다.


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

JCR 속성은 Java™ 필드로 이름을 공유하므로(둘 다 &#39;name&#39;) `@ValueMapValue`는 이 연결을 자동으로 해결하고 속성 값을 Java™ 필드에 주입합니다.

#### getOccupations() {#implementing-get-occupations}

다음으로 구현할 메서드는 `getOccupations()`입니다. 이 메서드는 JCR 속성 `occupations`에 저장된 직업을 로드하고 이를 알파벳순으로 정렬한 컬렉션을 반환합니다.

`getName()`에서 살펴본 것과 동일한 기술을 사용하여 속성 값을 Sling 모델의 필드에 주입할 수 있습니다.

Sling 모델에서 주입된 Java™ 필드 `occupations`를 통해 JCR 속성 값을 사용할 수 있게 되면 분류 비즈니스 로직을 `getOccupations()` 메서드에 적용할 수 있습니다.


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

마지막 공개 메서드는 `isEmpty()`로, 구성 요소를 렌더링할 만큼 &#39;충분히 작성된 상태&#39;로 간주해야 하는 시점을 결정합니다.

이 구성 요소의 경우, 비즈니스 요구 사항은 세 가지 필드가 모두 채워져야 하며 구성 요소를 렌더링하기 *전*&#x200B;에 `name, image and occupations`가 모두 채워져야 합니다.


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


#### &#39;이미지 문제&#39; 해결 {#tackling-the-image-problem}

이름과 직업 조건을 확인하는 것은 사소한 작업이며 Apache Commons Lang3는 편리한 [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) 클래스를 제공합니다. 그러나 Core Components Image 구성 요소가 이미지를 표시하는 데 사용되기 때문에 **Image의 존재**&#x200B;를 어떻게 검증할 수 있는지는 불분명합니다.

이를 해결하는 두 가지 방법이 있습니다.

`fileReference` JCR 속성이 자산으로 확인되는지 확인합니다 *또는* 이 리소스를 Core Component Image Sling Model로 변환하고 `getSrc()` 메서드가 비어 있지 않은 상태인지 확인합니다.

이제 **두 번째** 접근 방식을 사용해 보겠습니다. 첫 번째 접근 방식으로도 충분할 가능성이 높지만 이 튜토리얼에서는 후자를 사용하여 Sling 모델의 다른 기능을 탐색해 보겠습니다.

1. Image를 가져오는 비공개 메서드를 만듭니다. 이 메서드는 HTL 자체에서 Image 오브젝트를 노출할 필요가 없으므로 비공개로 유지되며 `isEmpty().` 지원에만 사용됩니다.

   `getImage()`에 대한 다음 비공개 메서드를 추가합니다.

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   위에서 언급했듯이 **Image Sling 모델**&#x200B;을 얻는 데는 두 가지 접근 방식이 더 있습니다.

   첫 번째 방식은 `@Self` 주석을 사용하여 현재 요청을 Core Component의 `Image.class`를 자동 조정합니다.

   두 번째 방식은 편리한 서비스인 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi 서비스를 사용하는 것으로, 이는 Java™ 코드에서 다른 유형의 Sling 모델을 생성하는 데 도움이 됩니다.

   이제 두 번째 접근 방식을 사용해 보겠습니다.

   >[!NOTE]
   >
   >실제 구현에서는 첫 번째 방식을 사용하는 것이 더 간단하고 덜 번거로운 솔루션이므로 `@Self`가 선호됩니다. 두 번째 접근 방식의 경우 더 복잡한 구성 요소에 유용한 Sling 모델의 더 많은 측면을 탐색해야 하므로 이 튜토리얼에서는 두 번째 접근 방식을 사용합니다.

   Sling 모델은 Java™ POJO이고 OSGi 서비스가 아니므로 일반적인 `@Reference` OSGi 주입 주석을 사용할 수 **없습니다**. 그 대신 Sling 모델은 유사한 기능을 제공하는 특별한 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 주석을 제공합니다.

1. `OSGiService` 주석을 포함하여 `ModelFactory`를 주입하도록 `BylineImpl.java`를 업데이트합니다.

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

   `ModelFactory`를 사용할 수 있기 때문에 다음을 사용하여 Core Component Image Sling 모델을 생성할 수 있습니다.

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   이 방법은 요청과 리소스가 모두 필요한데, Sling 모델에서는 아직 둘 다 제공되지 않는다는 점에 유의하시기 바랍니다. 두 가지를 확보하려면 더 많은 Sling Model 주석이 사용됩니다.

   현재 요청을 받으려면 **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 주석을 사용하여 `adaptable`(`@Model(..)`에서 `SlingHttpServletRequest.class`로 정의됨)을 Java™ 클래스 필드에 주입할 수 있습니다.

1. **@Self** 주석을 추가하여 **SlingHttpServletRequest 요청**&#x200B;을 받습니다.

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   기억하시겠지만 위에서 `@Self Image image`를 사용하여 Core Component Image Sling 모델을 주입하는 것이 옵션 중 하나였습니다. `@Self` 주석은 조정 가능한 오브젝트(이 경우 SlingHttpServletRequest)를 주입하고 주석 필드 유형을 조정하고자 합니다. Core Component Image Sling 모델은 SlingHttpServletRequest 오브젝트에서 조정 가능하므로 이 방법이 효과적일 수 있으며, 탐색이 더 필요한 `modelFactory` 접근 방식보다 코드도 적습니다.

   이제 ModelFactory API를 통해 Image 모델을 인스턴스화하는 데 필요한 변수가 주입되었습니다. Sling 모델의 **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 주석을 사용하여 Sling 모델이 인스턴스화된 후 이 오브젝트를 확보해 보겠습니다.

   `@PostConstruct`는 매우 유용하고 생성자와 비슷한 역할을 하지만 클래스가 인스턴스화되고 주석이 달린 모든 Java™ 필드가 주입된 후에 호출됩니다. 다른 Sling 모델 주석은 Java™ 클래스 필드(변수)에 주석을 달지만 `@PostConstruct`는 보통 `init()`로 이름이 지정되나 어떤 이름이든 지정 가능하며 매개변수가 없는 빈 메서드에 주석을 달 수 있습니다.

1. **@PostConstruct** 메서드를 추가합니다.

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

   Sling 모델은 OSGi 서비스가 **아니므로** 클래스 상태를 유지하는 것이 안전합니다. `@PostConstruct`는 종종 일반 생성자와 유사하게 나중에 사용하기 위해 Sling Model 클래스 상태를 파생시키고 설정합니다.

   `@PostConstruct` 메서드가 예외를 발생시키는 경우, Sling 모델은 인스턴스화되지 않은 상태이며 null입니다.

1. 이제 이미지 오브젝트를 반환하도록 **getImage()**&#x200B;를 업데이트할 수 있습니다.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. `isEmpty()`로 돌아가서 구현을 마무리하겠습니다.

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

   `getImage()`는 여러 번 호출하더라도 문제가 되지 않습니다. 이러한 호출은 초기화된 `image` 클래스 변수를 반환하며 비용이 지나치게 소요되지는 않으나 불필요한 호출을 방지할 필요가 있는 `modelFactory.getModelFromWrappedRequest(...)`를 호출하지 않기 때문입니다.

1. 최종 `BylineImpl.java`는 다음과 같아야 합니다.


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


## Byline HTL {#byline-htl}

`ui.apps` 모듈에서 AME 구성 요소의 이전 설정으로 생성된 `/apps/wknd/components/byline/byline.html`을 엽니다.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

지금까지 이 HTL 스크립트가 한 일을 살펴보겠습니다.

* `placeholderTemplate`은 구성 요소가 완전히 구성되지 않았을 때 표시되는 핵심 구성 요소의 플레이스홀더를 가리킵니다. 또한 AEM Sites 페이지 편집기에서 위 `cq:Component`의 `jcr:title` 속성에 정의된 구성 요소 제목이 있는 상자로 렌더링됩니다.

* `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}`는 위에서 정의된 `placeholderTemplate`를 로드하며 현재 `false`로 하드 코딩된 부울 값을 플레이스홀더 템플릿에 전달합니다. `isEmpty`가 true인 경우 플레이스홀더 템플릿은 회색 상자를 렌더링하고, true가 아닌 경우 아무것도 렌더링하지 않습니다.

### Byline HTL 업데이트

1. 다음의 기본 HTML 구조로 **byline.html**&#x200B;을 업데이트합니다.

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

   CSS 클래스는 [BEM 명명 규칙](https://getbem.com/naming/)을 따른다는 점에 유의하시기 바랍니다. BEM 규칙 사용이 필수는 아니지만 Core Component CSS 클래스에서 사용되며 일반적으로 깔끔하고 읽기 쉬운 CSS 규칙을 생성하므로 사용이 권장됩니다.

### HTL에서 Sling 모델 오브젝트 인스턴스화 {#instantiating-sling-model-objects-in-htl}

[블록 문 사용](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use)은 HTL 스크립트에서 Sling 모델 오브젝트를 인스턴스화하고 HTL 변수에 할당하는 데 사용됩니다.

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"`은 BylineImpl에 의해 구현된 Byline 인터페이스(com.adobe.aem.guides.wknd.models.Byline)를 사용하고, 현재 SlingHttpServletRequest를 이에 맞게 조정하며, 결과는 HTL 변수 이름 Byline(`data-sly-use.<variable-name>`)에 저장됩니다.

1. 외부 `div`를 업데이트하여 공개 인터페이스에서 **Byline** Sling 모델을 참조합니다.

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Sling 모델 메서드 액세스 {#accessing-sling-model-methods}

HTL은 JSTL에서 차용하여 Java™ 게터 메서드 이름을 동일하게 줄인 표현을 사용합니다.

예를 들어 Byline Sling 모델의 `getName()` 메서드 호출은 `byline.name`로 축약할 수 있습니다. 이와 유사하게 `byline.isEmpty`는 `byline.empty`로 축약할 수 있습니다. `byline.getName` 또는 `byline.isEmpty` 등 전체 메서드 이름을 사용해도 좋습니다. `()`는 HTL에서 메서드를 호출하는 데 사용되지 않습니다(JSTL과 유사).

매개변수가 필요한 Java™ 메서드는 HTL에서 사용할 수 **없습니다**. 이는 HTL의 논리를 단순하게 유지하기 위한 의도입니다.

1. Byline 이름은 Byline Sling 모델 또는 HTL `${byline.name}`에서 `getName()`을 호출하여 구성 요소에 추가할 수 있습니다.

   `h2` 태그를 업데이트합니다.

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### HTL 표현식 옵션 사용 {#using-htl-expression-options}

[HTL 표현식 옵션](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options)은 HTL의 콘텐츠에 대한 수정자 역할을 하며 날짜 형식 지정부터 i18n 번역까지 다양합니다. 표현식은 값의 목록이나 배열을 결합하는 데에도 사용할 수 있는데, 이는 직업을 쉼표로 구분된 형식으로 표시하는 데 필요합니다.

표현식은 HTL 표현식의 `@` 연산자를 통해 추가됩니다.

1. 직업 목록을 “, ”로 묶으려면 다음 코드를 사용합니다.

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 플레이스홀더를 조건부로 표시 {#conditionally-displaying-the-placeholder}

AEM 구성 요소에 대한 대부분의 HTL 스크립트는 **플레이스홀더 패러다임**&#x200B;을 사용하여 작성자에게 **구성 요소가 잘못 작성되어 AEM 게시에 표시되지 않음을 나타내는** 시각적 단서를 제공합니다. 이러한 결정을 유도하는 규칙은 지원 역할을 담당하는 구성 요소의 Sling 모델에 메서드를 구현하는 것입니다. 이 경우 `Byline.isEmpty()`가 여기에 해당합니다.

`isEmpty()` 메서드는 Byline Sling 모델에서 호출되고 결과(또는 `!` 연산자를 통한 부정적인 결과)는 `hasContent`라는 이름의 HTL 변수에 저장됩니다.

1. 외부 `div`를 업데이트하여 `hasContent`라는 이름의 HTL 변수를 저장합니다.

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   `data-sly-test`의 사용에 유의하시기 바랍니다. HTL `test` 블록이 핵심으로, HTL 변수를 설정하고 위치해 있는 HTML 요소를 렌더링하거나 렌더링하지 않습니다. 렌더링 여부는 HTL 표현식 평가 결과를 기준으로 결정됩니다. &#39;true&#39;이면 HTML 요소가 렌더링되고, 그렇지 않으면 렌더링되지 않습니다.

   이 HTL 변수 `hasContent`는 이제 플레이스홀더를 조건부로 표시하거나 숨기는 데 재사용될 수 있습니다.

1. `placeholderTemplate`에 대한 파일 하단의 조건부 호출을 다음과 같이 업데이트합니다.

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 핵심 구성 요소를 사용하여 이미지 표시 {#using-the-core-components-image}

`byline.html`에 대한 HTL 스크립트가 이제 거의 완성되었으며, 이미지만 없는 상태입니다.

`sling:resourceSuperType`은 이미지 작성을 위해 Core Component의 Image 구성 요소를 가리키기 때문에 Core Component의 Image 구성 요소는 이미지를 렌더링하는 데 사용될 수 있습니다.

이를 위해 현재의 Byline 리소스를 포함하되,`core/wcm/components/image/v2/image` 리소스 유형을 사용하여 Core Component의 Image 구성 요소 리소스 유형을 강제로 지정하겠습니다. 이는 구성 요소 재사용을 위한 강력한 패턴입니다. 이를 위해 HTL의 `data-sly-resource` 블록이 사용됩니다.

1. 다음을 통해 `div`를 `cmp-byline__image` 클래스로 바꿉니다.

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   `data-sly-resource`는 상대 경로 `'.'`를 통해 현재 리소스를 포함하고 리소스 유형이 `core/wcm/components/image/v2/image`인 현재 리소스(또는 Byline 콘텐츠 리소스)를 강제로 포함합니다.

   핵심 구성 요소 리소스 유형은 스크립트 내에서 사용되므로 프록시를 통해 사용되지 않고 직접 사용되며 콘텐츠에 영구적으로 저장되지 않습니다.

2. 아래와 같이 `byline.html`이 완료됩니다.

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

3. 코드 베이스를 로컬 AEM 인스턴스에 배포합니다. 변경 사항이 `core` 및 `ui.apps`에 적용되므로 두 모듈을 배포해야 합니다.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   AEM 6.5/6.4에 배포하려면 `classic` 프로필을 호출합니다.

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > Maven 프로필 `autoInstallSinglePackage`를 사용하여 루트에서 전체 프로젝트를 빌드할 수도 있지만 이렇게 하면 페이지의 콘텐츠 변경 사항을 덮어쓰게 될 수 있습니다. 기존 AEM 콘텐츠를 깔끔하게 덮어쓰기 위해 튜토리얼 스타터 코드에 맞게 `ui.content/src/main/content/META-INF/vault/filter.xml`가 수정되었기 때문입니다. 실제 상황에서 이 부분은 문제가 되지 않습니다.

### 스타일이 지정되지 않은 Byline 구성 요소 검토 {#reviewing-the-unstyled-byline-component}

1. 업데이트를 배포한 후 [LA 스케이트보드장 완벽 가이드](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) 페이지로 이동하거나 이 챕터의 앞부분에서 Byline 구성 요소를 추가한 곳으로 이동합니다.

1. 이제 **이미지**, **이름**, **직업**&#x200B;이 표시되며 스타일이 지정되지 않으나 작동 중인 Byline 구성 요소가 존재합니다.

   ![스타일이 지정되지 않은 Byline 구성 요소](assets/custom-component/unstyled.png)

### Sling 모델 등록 검토 {#reviewing-the-sling-model-registration}

[AEM 웹 콘솔의 Sling 모델 상태 보기](http://localhost:4502/system/console/status-slingmodels)에는 AEM에 등록된 모든 Sling 모델이 표시됩니다. 이 목록을 검토하면 Byline Sling 모델이 설치 및 인식되는지 확인할 수 있습니다.

**BylineImpl**&#x200B;이 목록에 표시되지 않으면 Sling 모델의 주석에 문제가 있거나 모델이 핵심 프로젝트의 올바른 패키지(`com.adobe.aem.guides.wknd.core.models`)에 추가되지 않았을 가능성이 높습니다.

![Byline Sling 모델 등록 완료](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## Byline 스타일 {#byline-styles}

제공된 크리에이티브 디자인에 맞춰 Byline 구성 요소를 정렬하기 위해 스타일을 지정해 보겠습니다. 이를 위해서는 **ui.frontend** 모듈에서 파일을 업데이트해야 합니다.

### 기본 스타일 추가

Byline 구성 요소에 기본 스타일을 추가합니다.

1. `/src/main/webpack/components` 아래에서 IDE 및 **ui.frontend** 프로젝트로 돌아갑니다.
1. 이름이 `_byline.scss`인 파일을 생성합니다.

   ![Byline 프로젝트 탐색기](assets/custom-component/byline-style-project-explorer.png)

1. `_byline.scss`에 SCSS로 작성된 Byline 구현 CSS를 추가합니다.

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

1. 터미널을 열고 `ui.frontend` 모듈로 이동합니다.
1. 다음 npm 명령을 사용하여 `watch` 프로세스를 시작합니다.

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. 브라우저로 돌아가서 [LA 스케이트보드장 문서](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)로 이동합니다. 구성 요소에 업데이트된 스타일이 표시됩니다.

   ![완성된 Byline 구성 요소](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 오래된 CSS가 제공되지 않도록 브라우저 캐시를 지워야 할 수도 있고, 전체 스타일을 적용하기 위해 Byline 구성 요소가 있는 페이지를 새로 고쳐야 할 수도 있습니다.

## 축하합니다! {#congratulations}

축하합니다! Adobe Experience Manager를 사용하여 처음부터 사용자 정의 구성 요소를 만들었습니다.

### 다음 단계 {#next-steps}

Java™ 코드에 대한 JUnit 테스트를 작성하는 방법을 살펴보고 AEM 구성 요소 개발을 계속 학습하여 모든 것이 제대로 개발되었는지, 구현된 비즈니스 로직이 올바르고 완전한지 확인하시기 바랍니다.

* [단위 테스트 또는 AEM 구성 요소 작성](unit-testing.md)

완성된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd)에서 확인하거나 로컬로 `tutorial/custom-component-solution` Git 분기에서 코드를 검토 및 배포할 수 있습니다.

1. [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 저장소를 복제합니다.
1. `tutorial/custom-component-solution` 분기를 확인합니다.
