---
title: AEM SPA 편집기를 사용한 개발 - Hello World 자습서
description: AEM SPA Editor 에서는 단일 페이지 애플리케이션 또는 SPA의 컨텍스트 내 편집을 지원합니다. 이 자습서는 AEM SPA Editor JS SDK와 함께 사용할 SPA 개발을 소개합니다. 이 자습서는 사용자 지정 Hello World 구성 요소를 추가하여 We.Retail 저널 앱을 확장합니다. 사용자는 React 또는 Angular 프레임워크을 사용하여 자습서를 완료할 수 있습니다.
version: 6.3, 6.4, 6.5
topic: SPA
feature: SPA Editor
role: Developer
level: Beginner
exl-id: e900301d-411c-4c02-8443-2a0fa56b65b5
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '3144'
ht-degree: 1%

---

# AEM SPA 편집기를 사용한 개발 - Hello World 자습서 {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> 이 자습서는 **사용되지 않음**. 다음 중 하나를 수행하는 것이 좋습니다. [AEM SPA 편집기 및 Angular 시작하기](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/angular/overview.html) 또는 [AEM SPA 편집기 및 반응 시작하기](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/react/overview.html)

AEM SPA Editor 에서는 단일 페이지 애플리케이션 또는 SPA의 컨텍스트 내 편집을 지원합니다. 이 자습서는 AEM SPA Editor JS SDK와 함께 사용할 SPA 개발을 소개합니다. 이 자습서는 사용자 지정 Hello World 구성 요소를 추가하여 We.Retail 저널 앱을 확장합니다. 사용자는 React 또는 Angular 프레임워크을 사용하여 자습서를 완료할 수 있습니다.

>[!NOTE]
>
> 단일 페이지 애플리케이션(SPA) 편집기 기능을 사용하려면 AEM 6.4 서비스 팩 2 이상이 필요합니다.
>
> SPA 편집기는 SPA 프레임워크 기반 클라이언트측 렌더링(예: React 또는 Angular)이 필요한 프로젝트에 권장되는 솔루션입니다.

## 필수 조건 읽기 {#prereq}

이 자습서는 SPA 구성 요소를 AEM 구성 요소에 매핑하여 컨텍스트 내 편집을 활성화하는 데 필요한 단계를 강조 표시하기 위한 것입니다. 이 자습서를 시작하는 사용자는 Adobe Experience Manager, AEM을 사용한 개발에 대한 기본 개념뿐만 아니라 React of Angular 프레임워크을 사용한 개발에 대해서도 잘 알고 있어야 합니다. 이 자습서에서는 백엔드 및 프런트 엔드 개발 작업을 모두 다룹니다.

이 자습서를 시작하기 전에 다음 리소스를 검토하는 것이 좋습니다.

* [SPA 편집기 기능 비디오](spa-editor-framework-feature-video-use.md) - SPA Editor 및 We.Retail Journal 앱에 대한 비디오 개요입니다.
* [React.js 자습서](https://reactjs.org/tutorial/tutorial.html) - React 프레임워크를 사용한 개발에 대한 소개입니다.
* [Angular 자습서](https://angular.io/tutorial) - Angular을 사용한 개발 소개

## 로컬 개발 환경 {#local-dev}

이 튜토리얼은 다음 용도로 디자인되었습니다.

[Adobe Experience Manager 6.5](https://helpx.adobe.com/kr/experience-manager/6-5/release-notes.html) 또는 [Adobe Experience Manager 6.4](https://helpx.adobe.com/kr/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [서비스 팩 5](https://helpx.adobe.com/kr/experience-manager/6-4/release-notes/sp-release-notes.html)

이 자습서에서는 다음 기술 및 도구를 설치해야 합니다.

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) 및 npm 5.6.0+(npm이 node.js와 함께 설치됨)

새 터미널을 열고 다음을 실행하여 위의 도구 설치를 다시 확인하십시오.

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## 개요 {#overview}

기본 개념은 SPA 구성 요소를 AEM 구성 요소에 매핑하는 것입니다. 서버측을 실행하는 AEM 구성 요소는 컨텐츠를 JSON 형식으로 내보냅니다. JSON 콘텐츠는 브라우저에서 클라이언트측을 실행하는 SPA에 의해 사용됩니다. SPA 구성 요소와 AEM 구성 요소 간의 1:1 매핑이 만들어집니다.

![SPA 구성 요소 매핑](assets/spa-editor-helloworld-tutorial-use/mapto.png)

인기 있는 프레임워크 [React JS](https://reactjs.org/) 및 [Angular](https://angular.io/) 기본적으로 지원됩니다. 사용자는 가장 편한 프레임워크에서 Angular 또는 React로 이 자습서를 완료할 수 있습니다.

## 프로젝트 설정 {#project-setup}

SPA 개발은 AEM 개발에 한 발, 다른 한 발도 있다. 목표는 SPA 개발이 독립적으로, (대부분) AEM에 대해 불가지도록 허용하는 것입니다.

* SPA 프로젝트는 프런트 엔드 개발 시 AEM 프로젝트와 독립적으로 작동할 수 있습니다.
* Webpack, NPM, [!DNL Grunt] 및 [!DNL Gulp]계속 사용합니다.
* AEM용으로 빌드하려면 SPA 프로젝트가 컴파일되고 AEM 프로젝트에 자동으로 포함됩니다.
* SPA을 AEM에 배포하는 데 사용되는 표준 AEM 패키지.

![객체 및 배포 개요](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA 개발에는 AEM 개발에 한 발씩, 다른 한 발에는 AEM 개발을 독립적으로 수행할 수 있도록 하며, (대부분) SPA과 알 수 없습니다.*

이 자습서의 목표는 We.Retail 저널 앱을 새 구성 요소로 확장하는 것입니다. 먼저 We.Retail 저널 앱에 대한 소스 코드를 다운로드하고 로컬 AEM에 배포합니다.

1. **다운로드** 최신 항목 [GitHub의 We.Retail 저널 코드](https://github.com/adobe/aem-sample-we-retail-journal).

   또는 명령줄에서 리포지토리 복제:

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >이 자습서는 **마스터** 분기 **1.2.1-스냅샷** 프로젝트의 버전입니다.

1. 다음 구조가 표시됩니다.

   ![프로젝트 폴더 구조](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   이 프로젝트에는 다음과 같은 전문 모듈이 포함되어 있습니다.

   * `all`: 전체 프로젝트를 하나의 패키지에 포함 및 설치합니다.
   * `bundles`: 다음 두 개의 OSGi 번들을 포함합니다. 를 포함하는 commons 및 core [!DNL Sling Models] 및 기타 Java 코드를 참조하십시오.
   * `ui.apps`: 는 프로젝트의 /apps 부분, 즉 JS 및 CSS clientlibs, 구성 요소, 런타임 모드별 구성을 포함합니다.
   * `ui.content`: 구조적 컨텐츠 및 구성 포함(`/content`, `/conf`)
   * `react-app`: We.Retail Journal React 응용 프로그램입니다. 이는 Maven 모듈과 웹 팩 프로젝트입니다.
   * `angular-app`: We.Retail 분개 Angular 애플리케이션. 둘 다 [!DNL Maven] 모듈 및 웹 팩 프로젝트

1. 새 터미널 창을 열고 다음 명령을 실행하여 전체 앱을 빌드하고 에서 실행되는 로컬 AEM 인스턴스에 배포합니다. [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > 이 프로젝트에서 전체 프로젝트를 빌드하고 패키징할 Maven 프로필은 `autoInstallSinglePackage`

1. 다음으로 이동합니다.

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   We.Retail 저널 앱은 AEM Sites 편집기 내에 표시되어야 합니다.

1. in [!UICONTROL 편집] 모드에서 편집할 구성 요소를 선택하고 컨텐츠를 업데이트합니다.

   ![구성 요소 편집](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. 을(를) 선택합니다 [!UICONTROL 페이지 속성] 아이콘을 클릭하여 열기 [!UICONTROL 페이지 속성]. 선택 [!UICONTROL 템플릿 편집] 을 눌러 페이지의 템플릿을 엽니다.

   ![페이지 속성 메뉴](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. 최신 버전의 SPA Editor에서는 [편집 가능한 템플릿](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 기존 사이트 구현과 동일한 방식으로 사용할 수 있습니다. 이 작업은 나중에 사용자 지정 구성 요소로 다시 방문하게 됩니다.

   >[!NOTE]
   >
   > AEM 6.5 및 AEM 6.4 + 만 해당 **서비스 팩 5** 편집 가능한 템플릿을 지원합니다.

## 개발 개요 {#development-overview}

![개요 개발](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA 개발 이터레이션은 AEM과 독립적으로 수행됩니다. SPA을 AEM에 배포할 준비가 되면 위에 설명된 대로 다음과 같은 고급 단계가 수행됩니다.

1. AEM 프로젝트 빌드가 호출되고, 이 빌드가 차례로 트리거됩니다. We.Retail 저널은 [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. SPA 프로젝트 [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) 컴파일된 SPA을 AEM 프로젝트에 AEM 클라이언트 라이브러리로 포함합니다.
1. AEM 프로젝트는 컴파일된 SPA과 기타 지원 AEM 코드를 포함한 AEM 패키지를 생성합니다.

## AEM 구성 요소 만들기 {#aem-component}

**모습: AEM 개발자**

AEM 구성 요소가 먼저 생성됩니다. AEM 구성 요소는 React 구성 요소에서 읽는 JSON 속성을 렌더링합니다. AEM 구성 요소는 구성 요소의 편집 가능한 속성에 대한 대화 상자를 제공할 책임이 있습니다.

사용 [!DNL Eclipse], 또는 기타 [!DNL IDE], We.Retail Journal Maven 프로젝트를 가져옵니다.

1. 반응기 업데이트 **pom.xml** 를 제거하려면 [!DNL Apache Rat] 플러그인. 이 플러그인은 각 파일을 확인하여 라이센스 헤더가 있는지 확인합니다. 따라서 이 기능에 신경 쓸 필요가 없습니다.

   in **aem-sample-we-retail-journal/pom.xml** 제거 **apache-rate-plugin**:

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. 에서 **we-retail-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) 모듈 아래의 새로운 노드를 만듭니다 `ui.apps/jcr_root/apps/we-retail-journal/components` 명명된 이름 **헬로월드** 유형 **cq:구성 요소**.
1. 다음 속성을 **헬로월드** 구성 요소, XML(`/helloworld/.content.xml`) 아래의 제품에서 사용할 수 있습니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Hello World 구성 요소](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > 편집 가능한 템플릿 기능을 보여주기 위해 `componentGroup="Custom Components"`. 실제 프로젝트에서 구성 요소 그룹의 수를 최소화하는 것이 가장 좋습니다. 따라서 더 나은 그룹이 &quot;[!DNL We.Retail Journal]&quot;을 눌러 다른 컨텐츠 구성 요소와 일치시킵니다.
   >
   > AEM 6.5 및 AEM 6.4 + 만 해당 **서비스 팩 5** 편집 가능한 템플릿을 지원합니다.

1. 다음에 대해 사용자 지정 메시지를 구성할 수 있는 대화 상자가 만들어집니다 **Hello World** 구성 요소. 아래 `/apps/we-retail-journal/components/helloworld` 노드 이름 추가 **cq:dialog** 의 **nt:구조화되지 않음**.
1. 다음 **cq:dialog** 에는 이름이 지정된 속성에 텍스트를 유지하는 단일 텍스트 필드가 표시됩니다 **[!DNL message]**. 새로 만든 다음 **cq:dialog** 아래 XML로 표시되는 다음 노드 및 속성을 추가합니다(`helloworld/_cq_dialog/.content.xml`) :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
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
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
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

   ![파일 구조](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   위의 XML 노드 정의에서는 사용자가 &quot;메시지&quot;를 입력할 수 있는 단일 텍스트 필드를 사용하는 대화 상자가 만들어집니다. 속성을 확인합니다 `name="./message"` 내 `<message />` 노드 아래에 있어야 합니다. 이 이름은 AEM 내의 JCR에 저장될 속성의 이름입니다.

1. 그런 다음 빈 정책 대화 상자가 만들어집니다(`cq:design_dialog`). 템플릿 편집기에서 구성 요소를 보려면 정책 대화 상자가 필요합니다. 이 간단한 사용 사례의 경우 빈 대화 상자가 됩니다.

   아래 `/apps/we-retail-journal/components/helloworld` 노드 이름 추가 `cq:design_dialog` 의 `nt:unstructured`.

   구성은 아래 XML로 표시됩니다(`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. 명령줄에서 AEM에 코드 베이스를 배포합니다.

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   in [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) 구성 요소가 배포되었는지 확인합니다. `/apps/we-retail-journal/components:`

   ![CRXDE Lite에 배포된 구성 요소 구조](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Sling 모델 만들기 {#create-sling-model}

**모습: AEM 개발자**

다음 a [!DNL Sling Model] 이 생성되어 [!DNL Hello World] 구성 요소. 기존 WCM 사용 사례에서는 [!DNL Sling Model] 는 비즈니스 로직을 구현하고 HTL(서버측 렌더링 스크립트)은 [!DNL Sling Model]. 이렇게 하면 렌더링 스크립트가 비교적 간단해집니다.

[!DNL Sling Models] 서버측 비즈니스 로직을 구현하는 데에도 SPA 사용 사례에서 사용됩니다. 차이점은 [!DNL SPA] 사용 사례, [!DNL Sling Models] 이 기능에는 메서드를 직렬화된 JSON으로 노출합니다.

>[!NOTE]
>
>개발자는 가장 좋은 방법으로서 [AEM 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko) 가능한 경우 핵심 구성 요소가 제공하는 기타 기능 [!DNL Sling Models] 는 &quot;SPA-ready&quot;인 JSON 출력을 사용하여 개발자가 프런트 엔드 프레젠테이션에 더 집중할 수 있도록 합니다.

1. 원하는 편집기에서 를 엽니다. **we-retail-journal-commons** 프로젝트 ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. 패키지에서 `com.adobe.cq.sample.spa.commons.impl.models`:
   * 이름이 인 새 클래스 만들기 `HelloWorld`.
   * 용 구현 인터페이스 추가 `com.adobe.cq.export.json.ComponentExporter.`

   ![새 Java 클래스 마법사](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   다음 `ComponentExporter` 인터페이스는 [!DNL Sling Model] AEM Content Services와 호환됩니다.

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. 이름이 인 정적 변수 추가 `RESOURCE_TYPE` 식별하려면 [!DNL HelloWorld] 구성 요소의 리소스 유형:

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. 에 대한 OSGi 주석 추가 `@Model` 및 `@Exporter`. 다음 `@Model` 주석을 달면 [!DNL Sling Model]. 다음 `@Exporter` 주석을 달면 메서드를 [!DNL Jackson Exporter] 프레임워크.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. 메서드 구현 `getDisplayMessage()` jcr 속성을 반환하려면 `message`. 를 사용하십시오 [!DNL Sling Model] 주석 `@ValueMapValue` 속성을 쉽게 검색할 수 있도록 만들기 `message` 구성 요소 아래에 저장됩니다. 다음 `@Optional` 구성 요소를 페이지에 처음 추가할 때는  `message`  을 채우지 않습니다.

   비즈니스 로직의 일부로, &quot;**Hello**&quot;, 이 메시지에 추가됩니다.

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > 메서드 이름 `getDisplayMessage` 는 중요합니다. 이 [!DNL Sling Model] 는 [!DNL Jackson Exporter] JSON 속성으로 노출됩니다. `displayMessage`. 다음 [!DNL Jackson Exporter] 는 모든 것을 정리 및 노출합니다. `getter` 매개 변수를 사용하지 않는 메서드(ignore로 명시적으로 표시되지 않는 경우) 나중에 React/Angular 앱에서 이 속성 값을 읽고 응용 프로그램의 일부로 표시합니다.

   메서드 `getExportedType` 도 중요합니다. 구성 요소의 값입니다 `resourceType` 는 JSON 데이터를 프런트 엔드 구성 요소에 &quot;매핑&quot;하는 데 사용됩니다(Angular / React). 다음 섹션에서 이 방법을 살펴보겠습니다.

1. 메서드 구현 `getExportedType()` 의 리소스 유형을 반환하려면 `HelloWorld` 구성 요소.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   에 대한 전체 코드 [**HelloWorld.java** 여기에서 찾을 수 있습니다.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Apache Maven을 사용하여 코드를 AEM에 배포합니다.

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   의 배포 및 등록 확인 [!DNL Sling Model] 으로 이동 [[!UICONTROL 상태] > [!UICONTROL Sling 모델]](http://localhost:4502/system/console/status-slingmodels) 를 클릭합니다.

   다음을 볼 수 있습니다. `HelloWorld` Sling 모델은 `we-retail-journal/components/helloworld` Sling 리소스 유형 및 리소스 유형이 [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## React 구성 요소 만들기 {#react-component}

**모습: 프런트 엔드 개발자**

다음으로, React 구성 요소가 만들어집니다. 를 엽니다. **react-app** 모듈 ( `<src>/aem-sample-we-retail-journal/react-app`) 원하는 편집기를 사용할 수 있습니다.

>[!NOTE]
>
> 관심 있는 경우에만 이 섹션을 건너뛸 수 있습니다 [Angular 개발](#angular-component).

1. 내부 `react-app` 폴더가 src 폴더로 이동합니다. 구성 요소 폴더를 확장하여 기존 React 구성 요소 파일을 봅니다.

   ![react 구성 요소 파일 구조](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. 구성 요소 폴더 아래에 새 파일을 추가합니다. `HelloWorld.js`.
1. 열기 `HelloWorld.js`. 가져오기 구문을 추가하여 React 구성 요소 라이브러리를 가져옵니다. 두 번째 가져오기 구문을 추가하여 `MapTo` Adobe이 제공한 도우미. 다음 `MapTo` helper에서는 AEM 구성 요소의 JSON에 React 구성 요소의 매핑을 제공합니다.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. 가져오기 아래에 이름이 지정된 새 클래스가 만들어집니다. `HelloWorld` React 를 확장합니다. `Component` 인터페이스. 필요한 를 추가합니다 `render()` 메서드를 `HelloWorld` 클래스 이름을 지정합니다.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. 다음 `MapTo` helper는 이름이 인 개체를 자동으로 포함합니다 `cqModel` React 구성 요소의 prop의 일부로, 다음 `cqModel` 에 의해 노출된 모든 속성 포함 [!DNL Sling Model].

   다음 사항을 기억하십시오 [!DNL Sling Model] 앞에서 만든 메서드에 메서드가 포함되어 있습니다. `getDisplayMessage()`. `getDisplayMessage()` 라는 이름의 JSON 키로 변환됩니다 `displayMessage` 출력 시.

   구현 `render()` 출력 방법 `h1` 의 값을 포함하는 태그입니다. `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html)JavaScript에 대한 구문 확장인 가 구성 요소의 최종 마크업을 반환하는 데 사용됩니다.

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. 구성 편집 방법을 구현합니다. 이 메서드는 를 통해 전달됩니다 `MapTo` 도우미 및 은 구성 요소가 비어 있는 경우 자리 표시자를 표시하는 정보를 AEM 편집기에 제공합니다. 이 문제는 구성 요소가 SPA에 추가되지만 아직 작성되지 않은 경우 발생합니다. 아래에 다음을 추가하십시오. `HelloWorld` 클래스:

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. 파일의 끝 부분에서 `MapTo` 도우미, 전달 `HelloWorld` 클래스 및 `HelloWorldEditConfig`. 이렇게 하면 AEM 구성 요소의 리소스 유형을 기반으로 하여 React 구성 요소가 AEM 구성 요소에 매핑됩니다. `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   에 대해 완료된 코드 [**HelloWorld.js** 여기에서 찾을 수 있습니다.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. 파일을 엽니다. `ImportComponents.js`. 다음에서 찾을 수 있습니다. `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   를 필요로 하는 라인 추가 `HelloWorld.js` 컴파일된 JavaScript 번들에 있는 다른 구성 요소와 함께 사용할 수 있습니다.

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. 에서  `components`  폴더 이름이 인 새 파일 만들기 `HelloWorld.css` 형제로서 `HelloWorld.js.` 파일에 다음 정보를 추가하여 기본적인 스타일을 만듭니다. `HelloWorld` 구성 요소:

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 다시 열기 `HelloWorld.js` 필요한 가져오기 구문 아래에서 를 업데이트합니다. `HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. Apache Maven을 사용하여 코드를 AEM에 배포합니다.

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) open `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. app.js에서 HelloWorld를 빠르게 검색하여 React 구성 요소가 컴파일된 앱에 포함되어 있는지 확인합니다.

   >[!NOTE]
   >
   > **app.js** 번들 React 앱입니다. 이 코드는 더 이상 사람이 읽을 수 없습니다. 다음 `npm run build` 명령은 최신 브라우저에서 해석할 수 있는 컴파일된 JavaScript를 출력하는 최적화된 빌드를 트리거했습니다.


## angular 구성 요소 만들기 {#angular-component}

**모습: 프런트 엔드 개발자**

>[!NOTE]
>
> React 개발에만 관심이 있는 경우에는 언제든지 이 섹션을 건너뛸 수 있습니다.

그런 다음 Angular 구성 요소가 만들어집니다. 를 엽니다. **angular-앱** 모듈 (`<src>/aem-sample-we-retail-journal/angular-app`) 원하는 편집기를 사용할 수 있습니다.

1. 내부 `angular-app` 폴더: `src` 폴더를 입력합니다. 구성 요소 폴더를 확장하여 기존 Angular 구성 요소 파일을 봅니다.

   ![Angular 파일 구조](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. 이름이 인 구성 요소 폴더 아래에 새 폴더를 추가합니다. `helloworld`. 아래 `helloworld` 폴더 추가 이름 `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. 열기 `helloworld.component.ts`. 가져오기 구문을 추가하여 Angular 가져오기 `Component` 및 `Input` 클래스를 참조하십시오. 새 구성 요소를 만들고 `styleUrls` 및 `templateUrl` to `helloworld.component.css` 및 `helloworld.component.html`. 마지막으로 클래스 내보내기 `HelloWorldComponent` 에 대해 예상된 `displayMessage`.

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > 만약 [!DNL Sling Model] 이전에 만들어진 방법이 **getDisplayMessage()**. 이 메서드의 serialized JSON은 **displayMessage**: 이제 Angular 앱에서 읽고 있습니다.

1. 열기 `helloworld.component.html` 를 포함하려면 다음을 수행합니다 `h1` 인쇄할 태그 `displayMessage` 속성:

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. 업데이트 `helloworld.component.css` 구성 요소에 대한 몇 가지 기본 스타일을 포함하도록 업데이트되고 수정되었습니다.

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 업데이트 `helloworld.component.spec.ts` 다음 테스트 베드 사용:

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. 다음 업데이트 `src/components/mapping.ts` 를 `HelloWorldComponent`. 추가 `HelloWorldEditConfig` 구성 요소가 구성되기 전에 AEM 편집기에서 자리 표시자가 표시됩니다. 끝으로 AEM 구성 요소를 을 사용하여 Angular 구성 요소에 매핑할 줄을 추가합니다 `MapTo` 도우미.

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   에 대한 전체 코드 [**mapping.ts** 여기에서 찾을 수 있습니다.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. 업데이트 `src/app.module.ts` 를 업데이트하려면 **NgModule**. 추가 **`HelloWorldComponent`** 로서의 **선언** 에 속해 있습니다. **AppModule**. 또한 `HelloWorldComponent` 로서의 **entryComponent** 이렇게 하면 JSON 모델이 처리될 때 컴파일되고 앱에 동적으로 포함됩니다.

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   에 대해 완료된 코드 [**app.module.ts** 여기에서 찾을 수 있습니다.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Maven을 사용하여 AEM에 코드를 배포합니다.

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) open `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. 에 대한 빠른 검색 수행 **HelloWorld** in `main.js` angular 구성 요소가 포함되어 있는지 확인합니다.

   >[!NOTE]
   >
   > **main.js** 는 번들 Angular 앱입니다. 이 코드는 더 이상 사람이 읽을 수 없습니다. npm run build 명령은 최신 브라우저에서 해석할 수 있는 컴파일된 JavaScript를 출력하는 최적화된 빌드를 트리거했습니다.

## 템플릿 업데이트 {#template-update}

1. React 및/또는 Angular 버전에 대한 편집 가능한 템플릿으로 이동합니다.

   * (Angular) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. 기본 을 선택합니다 [!UICONTROL 레이아웃 컨테이너] 을(를) 선택하고 을(를) 선택합니다. [!UICONTROL 정책] 아이콘을 클릭하여 정책을 엽니다.

   ![레이아웃 정책 선택](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   아래 **[!UICONTROL 속성]** > **[!UICONTROL 허용된 구성 요소]**&#x200B;에 대해 검색을 수행합니다. **[!DNL Custom Components]**. 다음을 볼 수 있습니다. **[!DNL Hello World]** 구성 요소를 선택합니다. 오른쪽 상단에 있는 확인란을 클릭하여 변경 사항을 저장합니다.

   ![레이아웃 컨테이너 정책 구성](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. 저장한 후에는 **[!DNL HelloWorld]** 구성 요소를 [!UICONTROL 레이아웃 컨테이너].

   ![허용된 구성 요소가 업데이트됨](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > AEM 6.5 및 AEM 6.4.5만 SPA Editor의 편집 가능한 템플릿 기능을 지원합니다. AEM 6.4를 사용하는 경우 CRXDE Lite을 통해 허용된 구성 요소에 대한 정책을 수동으로 구성해야 합니다. `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` 또는 `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   에 대한 업데이트된 정책 구성을 보여주는 CRXDE Lite [!UICONTROL 허용된 구성 요소] 에서 [!UICONTROL 레이아웃 컨테이너]:

   ![레이아웃 컨테이너에서 허용된 구성 요소에 대한 업데이트된 정책 구성을 표시하는 CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## 결합하기 {#putting-together}

1. angular 또는 React 페이지로 이동합니다.

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. 를 찾습니다. **[!DNL Hello World]** 구성 요소를 드래그하여 놓습니다 **[!DNL Hello World]** 구성 요소를 생성하지 않고 페이지에 속해 있어야 합니다.

   ![hello world 드래그 + 드롭](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   자리 표시자가 나타납니다.

   ![헬로 월드 플레이스 홀더](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. 구성 요소를 선택하고 대화 상자에 메시지를 추가합니다(예: &quot;World&quot; 또는 &quot;Your Name&quot;). 변경 사항을 저장합니다.

   ![렌더링된 구성 요소](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   문자열 &quot;Hello&quot;는 항상 메시지 앞에 추가됩니다. 이것은 `HelloWorld.java` [!DNL Sling Model].

## 다음 단계 {#next-steps}

[HelloWorld 구성 요소에 대한 솔루션 완료](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* 에 대한 전체 소스 코드 [[!DNL We.Retail Journal] GitHub에서](https://github.com/adobe/aem-sample-we-retail-journal)
* React와 함께 개발에 대한 자세한 자습서를 확인하십시오 [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/kr/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## 문제 해결 {#troubleshooting}

### Eclipse에서 프로젝트를 작성할 수 없습니다. {#unable-to-build-project-in-eclipse}

**오류:** 을 가져오는 동안 오류가 발생했습니다. [!DNL We.Retail Journal] 인식할 수 없는 목표 실행을 위해 Eclipse로 프로젝트:

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![일식 오류 마법사](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**해결 방법**: 마침을 눌러 나중에 이 문제를 해결합니다. 이렇게 한다고 자습서가 완료되지 않습니다.

**오류**: React 모듈, `react-app`Maven 빌드 중에 가 성공적으로 빌드되지 않았습니다.

**해결 방법:** 삭제 `node_modules` 폴더 아래 **react-app**. Apache Maven 명령 다시 실행 `mvn  clean install -PautoInstallSinglePackage` 를 클릭합니다.

### AEM의 충족되지 않은 종속성 {#unsatisfied-dependencies-in-aem}

![패키지 관리자 종속성 오류](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

AEM 종속성이 충족되지 않으면, **[!UICONTROL AEM 패키지 관리자]** 또는 **[!UICONTROL AEM 웹 콘솔]** (Felix 콘솔) SPA 편집기 기능을 사용할 수 없음을 나타냅니다.

### 구성 요소가 표시되지 않음

**오류**: 성공적으로 배포하고 컴파일된 버전의 React/Angular 앱에 업데이트가 있는지 확인한 후에도 `helloworld` 구성 요소를 페이지로 끌면 구성 요소가 표시되지 않습니다. AEM UI에서 구성 요소를 볼 수 있습니다.

**해결 방법**: 브라우저의 기록/캐시를 지우거나 새 브라우저를 열거나 시크릿 모드를 사용합니다. 작동하지 않는 경우 로컬 AEM 인스턴스에서 클라이언트 라이브러리 캐시를 무효화합니다. AEM은 효율성을 위해 큰 clientlibraries를 캐시하려고 합니다. 오래된 코드가 캐시된 문제를 수정하기 위해 경우에 따라 캐시를 수동으로 무효화해야 합니다.

다음 위치로 이동합니다. [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) 캐시 무효화를 클릭합니다. React/Angular 페이지로 돌아가서 페이지를 새로 고칩니다.

![클라이언트 라이브러리 다시 작성](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
