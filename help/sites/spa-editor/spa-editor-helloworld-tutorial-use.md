---
title: AEM SPA 편집기를 사용한 개발 - Hello World 자습서
description: AEM SPA Editor는 단일 페이지 애플리케이션 또는 SPA의 상황에 맞는 편집을 지원합니다. 이 자습서는 AEM SPA Editor JS SDK와 함께 사용할 SPA 개발을 소개합니다. 이 자습서는 사용자 지정 Hello World 구성 요소를 추가하여 We.Retail 저널 앱을 확장합니다. 사용자는 반응 또는 각도 프레임워크를 사용하여 자습서를 완료할 수 있습니다.
sub-product: 사이트, 컨텐츠 서비스
feature: spa-editor
topics: development, single-page-applications
audience: developer
doc-type: tutorial
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '3181'
ht-degree: 0%

---


# AEM SPA 편집기를 사용한 개발 - Hello World 자습서 {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> 이 자습서는 **더 이상 사용되지 않습니다**. 다음 중 하나를 수행하는 것이 좋습니다. [AEM SPA 편집기 및 Angular](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-angular-tutorial/overview.html) 또는 [AEM SPA Editor로 시작하기 및 반응](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-react-tutorial/overview.html)

AEM SPA Editor는 단일 페이지 애플리케이션 또는 SPA의 상황에 맞는 편집을 지원합니다. 이 자습서는 AEM SPA Editor JS SDK와 함께 사용할 SPA 개발을 소개합니다. 이 자습서는 사용자 지정 Hello World 구성 요소를 추가하여 We.Retail 저널 앱을 확장합니다. 사용자는 반응 또는 각도 프레임워크를 사용하여 자습서를 완료할 수 있습니다.

>[!NOTE]
>
> 단일 페이지 애플리케이션(SPA) 편집기 기능을 사용하려면 AEM 6.4 서비스 팩 2 이상이 필요합니다.
>
> SPA 편집기는 SPA 프레임워크 기반의 클라이언트측 렌더링(예: 반응 또는 각도)이 필요한 프로젝트에 권장되는 솔루션입니다.

## 사전 요구 사항 읽기 {#prereq}

이 자습서는 컨텍스트 내 편집을 활성화하기 위해 SPA 구성 요소를 AEM 구성 요소에 매핑하는 데 필요한 단계를 강조 표시하기 위한 것입니다. 이 자습서를 시작하는 사용자는 Impact of Angular 프레임워크를 사용하여 개발하는 것뿐만 아니라, AEM, Adobe Experience Manager의 개발 기본 개념을 잘 알고 있어야 합니다. 이 자습서에서는 백엔드 개발 및 프런트 엔드 개발 작업을 모두 다룹니다.

이 자습서를 시작하기 전에 다음 리소스를 검토하는 것이 좋습니다.

* [SPA Editor 기능 비디오](spa-editor-framework-feature-video-use.md) - SPA Editor 및 We.Retail Journal 앱의 비디오 개요입니다.
* [Responsive.js 자습서](https://reactjs.org/tutorial/tutorial.html) - Responsive 프레임워크를 사용한 개발 소개
* [각 자습서](https://angular.io/tutorial) - 각진 학습 방식의 개발

## 로컬 개발 환경 {#local-dev}

이 튜토리얼은 다음과 같은 용도로 고안되었습니다.

[Adobe Experience Manager 6.5](https://helpx.adobe.com/kr/experience-manager/6-5/release-notes.html) 또는 [Adobe Experience Manager 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [서비스 팩 5](https://helpx.adobe.com/kr/experience-manager/6-4/release-notes/sp-release-notes.html)

이 자습서에서는 다음 기술과 도구를 설치해야 합니다.

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

기본 개념은 SPA 구성 요소를 AEM 구성 요소에 매핑하는 것입니다. AEM 구성 요소, 서버측 실행, JSON 형식으로 컨텐츠 내보내기 JSON 콘텐츠는 브라우저에서 클라이언트 쪽을 실행하는 SPA에 의해 사용됩니다. SPA 구성 요소와 AEM 구성 요소 간의 1:1 매핑이 만들어집니다.

![SPA 구성 요소 매핑](assets/spa-editor-helloworld-tutorial-use/mapto.png)

널리 사용되는 프레임워크 [인 React JS](https://reactjs.org/) 및 [Angular](https://angular.io/) 가 기본적으로 지원됩니다. 사용자는 가장 익숙한 프레임워크에서 이 자습서를 완료할 수 있습니다.

## 프로젝트 설정 {#project-setup}

스파 개발은 AEM개발과 다른 한 발자국만 발전시켰다. SPA 개발은 AEM에 관계없이 독립적으로 이루어집니다.

* SPA 프로젝트는 프런트 엔드 개발 과정에서 AEM 프로젝트와 독립적으로 수행할 수 있습니다.
* Webpack, NPM과 같은 프런트 엔드 빌드 도구 및 기술 [!DNL Grunt] 은 [!DNL Gulp]계속 사용됩니다.
* AEM용 SPA 프로젝트는 컴파일되어 AEM 프로젝트에 자동으로 포함됩니다.
* AEM에 SPA를 배포하는 데 사용되는 표준 AEM 패키지.

![객체 및 배포 개요](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA 개발은 AEM 개발에 한피트 정도이고 다른 한 발은 SPA 개발을 독립적으로 수행할 수 있으며, (대부분) AEM에 관계없이 모든 것이 가능합니다.*

이 자습서의 목표는 새 구성 요소로 We.Retail 저널 앱을 확장하는 것입니다. 먼저 We.Retail Journal 앱의 소스 코드를 다운로드하여 로컬 AEM에 배포합니다.

1. **GitHub에서 최신** We.Retail 저널 코드를 다운로드합니다 [](https://github.com/adobe/aem-sample-we-retail-journal).

   또는 명령줄에서 리포지토리 복제:

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >이 자습서는 프로젝트의 **1.2.1-SNAPSHOT** 버전이 있는 **마스터** 분기에 대해작동합니다.

2. 다음 구조가 표시됩니다.

   ![프로젝트 폴더 구조](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   이 프로젝트에는 다음과 같은 마비되는 모듈이 포함되어 있습니다.

   * `all`:단일 패키지로 전체 프로젝트를 임베드하여 설치합니다.
   * `bundles`:다음 두 개의 OSGi 번들을 포함합니다.commons 및 core [!DNL Sling Models] 와 기타 Java 코드를 포함합니다.
   * `ui.apps`:프로젝트의 /apps 부분, 즉 JS 및 CSS clientlibs, components, runmode 특정 구성을 포함합니다.
   * `ui.content`:구조적 컨텐츠 및 구성 포함 (`/content`, `/conf`
   * `react-app`:We.Retail Journal 반응형 응용 프로그램. 이는 Maven 모듈과 웹 팩 프로젝트입니다.
   * `angular-app`:We.Retail 분개 각도 응용 프로그램. 이는 [!DNL Maven] 모듈과 웹 팩 프로젝트입니다.

3. 새 터미널 창을 열고 다음 명령을 실행하여 http://localhost:4502에서 실행되는 로컬 AEM 인스턴스에 전체 앱을 빌드하고 배포합니다 [](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > 이 프로젝트에서는 전체 프로젝트를 빌드하고 패키지할 Maven 프로필이 `autoInstallSinglePackage`

   >[!CAUTION]
   >
   > 빌드 중에 오류가 발생하는 경우 Maven settings.xml 파일에 Adobe의 Maven 아티팩트 저장소가 [포함되어 있는지 확인합니다](https://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html).

4. 다음으로 이동:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   We.Retail 저널 앱이 AEM Sites 편집기 내에 표시되어야 합니다.

5. 편집  모드에서 편집할 구성 요소를 선택하고 컨텐츠를 업데이트합니다.

   ![구성 요소 편집](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

6. 페이지 [!UICONTROL 속성] 아이콘을 선택하여 [!UICONTROL 페이지 속성을 엽니다]. 템플릿 [!UICONTROL 편집을] 선택하여 페이지의 템플릿을 엽니다.

   ![페이지 속성 메뉴](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

7. 최신 버전의 SPA 편집기에서 편집 가능한 템플릿 [을 기존 사이트 구현과](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 같은 방식으로 사용할 수 있습니다. 사용자 지정 구성 요소와 함께 나중에 다시 방문됩니다.

   >[!NOTE]
   >
   > AEM 6.5 및 AEM 6.4 + **서비스 팩 5만** 편집 가능한 템플릿을 지원합니다.

## 개발 개요 {#development-overview}

![개요 개발](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA 개발 반복은 AEM과 독립적으로 이루어집니다. SPA가 AEM에 배치될 준비가 되면 다음과 같은 고급 단계가 수행됩니다(위 그림 참조).

1. AEM 프로젝트 빌드가 호출되어 SPA 프로젝트 빌드가 트리거됩니다. We.Retail Journal은 the [**frontend-maven-plugin을 사용합니다**](https://github.com/eirslett/frontend-maven-plugin).
2. SPA 프로젝트의 [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) 는 AEM 프로젝트에 컴파일된 SPA를 AEM 클라이언트 라이브러리로 포함합니다.
3. AEM 프로젝트는 컴파일된 SPA와 기타 지원 AEM 코드를 포함한 AEM 패키지를 생성합니다.

## AEM 구성 요소 만들기 {#aem-component}

**페르소나:AEM 개발자**

AEM 구성 요소가 먼저 만들어집니다. AEM 구성 요소는 반응 구성 요소에서 읽는 JSON 속성을 렌더링합니다. AEM 구성 요소는 구성 요소의 편집 가능한 속성에 대한 대화 상자도 제공합니다.

We. [!DNL Eclipse]Retail Journal [!DNL IDE]Maven 프로젝트를 사용하거나 다른 방법으로 가져옵니다.

1. 원자로 **pom.xml** 을 업데이트하여 플러그인을 [!DNL Apache Rat] 제거합니다. 이 플러그인은 라이센스 헤더가 있는지 확인하기 위해 각 파일을 확인합니다. Adobe는 이러한 기능을 신경 쓸 필요가 없습니다.

   aem-sample-we-retail-journal/pom.xml **에서** **apache-rate-plugin**&#x200B;제거:

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

2. **we-retail-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) 모듈에서 cq:ComponentBelow `ui.apps/jcr_root/apps/we-retail-journal/components` 형식 **의 helworld라는 이름의 새 노드를** ****&#x200B;만듭니다.
3. 아래 XML( **Help World** )로 표시된 도움말 월드 구성 요소에 다음 속성을`/helloworld/.content.xml`추가합니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Hello World Component](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > 편집 가능한 템플릿 기능을 설명하기 위해 특별히 설정을 `componentGroup="Custom Components"`설정했습니다. 실제 프로젝트에는 구성 요소 그룹 수를 최소화하는 것이 가장 좋으므로 다른 컨텐츠 구성 요소와 일치하는 더 나은 그룹이 &quot;[!DNL We.Retail Journal]&quot;이 될 것입니다.
   >
   > AEM 6.5 및 AEM 6.4 + **서비스 팩 5만** 편집 가능한 템플릿을 지원합니다.

4. 다음 대화 상자를 만들어 **Hello World 구성 요소에 대해 사용자 지정 메시지를 구성할 수** 있습니다. 아래 `/apps/we-retail-journal/components/helloworld` 에 **nt:unstructured의** cq:dialog **노드 이름을**&#x200B;추가합니다.
5. cq:dialog **에** 이름이 지정된 속성에 텍스트를 유지하는 단일 텍스트 필드가 표시됩니다 **[!DNL message]**. 새로 만든 **cq:dialog** 아래에서 아래 XML로 표시된 다음 노드 및 속성을 추가합니다. (`helloworld/_cq_dialog/.content.xml`)

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

   위의 XML 노드 정의는 사용자가 &quot;메시지&quot;를 입력할 수 있는 단일 텍스트 필드가 포함된 대화 상자를 만듭니다. 노드 `name="./message"` 내의 속성을 `<message />` 확인합니다. AEM 내의 JCR에 저장되는 속성의 이름입니다.

6. 다음으로 빈 정책 대화 상자가 만들어집니다(`cq:design_dialog`). 템플릿 편집기에서 구성 요소를 보려면 정책 대화 상자가 필요합니다. 이 간단한 사용 사례의 경우 빈 대화 상자가 됩니다.

   아래의 노드 이름 `/apps/we-retail-journal/components/helloworld` 을 `cq:design_dialog` `nt:unstructured`추가합니다.

   구성은 아래의 XML로 표시됩니다(`helloworld/_cq_design_dialog/.content.xml`).

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

7. 명령줄에서 AEM에 코드 베이스를 배포합니다.

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   CRXDE Lite에서 [](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) 구성 요소가 `/apps/we-retail-journal/components:`

   ![CRXDE Lite에 배포된 구성 요소 구조](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## 슬링 모델 만들기 {#create-sling-model}

**페르소나:AEM 개발자**

다음 a [!DNL Sling Model] 가 구성 요소를 [!DNL Hello World] 뒤로 되돌립니다. 일반적인 WCM 사용 사례에서는 비즈니스 로직을 구현하고 HTL(서버측 렌더링 스크립트)이 해당 서버에 대한 호출을 만듭니다 [!DNL Sling Model] [!DNL Sling Model]. 따라서 렌더링 스크립트가 비교적 간단하게 유지됩니다.

[!DNL Sling Models] 또한 SPA 사용 사례에서 서버측 비즈니스 로직을 구현하는 데 사용됩니다. 차이점은 사용 사례에서 [!DNL SPA] 메서드 [!DNL Sling Models] 를 직렬화된 JSON으로 노출한다는 점입니다.

>[!NOTE]
>
>개발자는 가능하면 [AEM 코어 구성 요소를 사용하는 것이](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html) 좋습니다. 핵심 구성 요소는 개발자가 프런트 엔드 프레젠테이션에 더 집중할 수 있도록 &quot;SPA-ready&quot; JSON 출력 [!DNL Sling Models] 을 제공합니다.

1. 선택한 편집기의 **we-retail-journal-commons** 프로젝트()를 엽니다 `<src>/aem-sample-we-retail-journal/bundles/commons`.
2. 패키지에서 `com.adobe.cq.sample.spa.commons.impl.models`:
   * 이름이 지정된 새 클래스를 만듭니다 `HelloWorld`.
   * 구현 인터페이스 추가 `com.adobe.cq.export.json.ComponentExporter.`

   ![새 Java 클래스 마법사](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   이 `ComponentExporter` 인터페이스는 AEM Content Services와 호환되도록 [!DNL Sling Model] 구현되어야 합니다.

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

3. 구성 요소의 리소스 유형 `RESOURCE_TYPE` 을 식별하기 위해 이름이 [!DNL HelloWorld] 지정된 정적 변수를 추가합니다.

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

4. 및 에 대한 OSGi 주석 `@Model` 을 추가합니다 `@Exporter`. 주석을 달면 `@Model` 해당 클래스가 하나의 그룹으로 등록됩니다 [!DNL Sling Model]. 이 `@Exporter` 주석에는 프레임워크를 사용하여 메서드를 직렬화된 JSON으로 [!DNL Jackson Exporter] 표시합니다.

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

5. JCR 속성 `getDisplayMessage()` 을 반환하는 메서드를 구현합니다 `message`. 의 주석 [!DNL Sling Model] 을 사용하여 구성 요소 아래에 `@ValueMapValue` `message` 저장된 속성을 쉽게 검색할 수 있습니다. 주석을 `@Optional` 중요한 것은 구성 요소를 페이지에 처음 추가할 때 채워지지 `message` 않기 때문입니다.

   비즈니스 논리의 일부로서 메시지에 &quot;**Hello**&quot;라는 문자열이 추가됩니다.

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
   > 메서드 이름이 `getDisplayMessage` 중요합니다. 이 일련 번호 [!DNL Sling Model] 를 사용하여 [!DNL Jackson Exporter] 직렬화하면 JSON 속성으로 노출됩니다. `displayMessage`. 매개 변수 [!DNL Jackson Exporter] 를 사용하지 않는 모든 `getter` 메서드를 일련화하고 노출합니다(명시적으로 ignore로 표시되지 않음). 나중에 반응형/각도 앱에서 이 속성 값을 읽고 애플리케이션의 일부로 표시합니다.

   방법 `getExportedType` 도 중요하다. 구성 요소의 값은 JSON 데이터를 프런트 엔드 구성 요소에 &quot;매핑&quot;하는 데 `resourceType` 사용됩니다(각도 / 반응). 다음 코너에서 살펴보도록 하겠습니다

6. 구성 요소 `getExportedType()` 의 리소스 유형을 반환하는 메서드를 `HelloWorld` 구현합니다.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   HelloWorld.java [**에 대한 전체** 코드는 여기에서 찾을 수 있습니다.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

7. Apache Maven을 사용하여 코드를 AEM에 배포:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   OSGi 콘솔 [!DNL Sling Model] 에서 [[!UICONTROL 상태] > 모델 [!UICONTROL 으로 이동하여]](http://localhost:4502/system/console/status-slingmodels) 배포 및 등록을확인합니다.

   Sling Model은 Sling `HelloWorld` 리소스 유형에 속하며 `we-retail-journal/components/helloworld` Sling 리소스 유형으로 등록되어 있음을 확인해야 합니다. [!DNL Sling Model Exporter Servlet]

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## 반응 구성 요소 만들기 {#react-component}

**페르소나:프런트 엔드 개발자**

다음으로, React 구성 요소가 만들어집니다. 원하는 편집기를 사용하여 **반응형 앱** 모듈( `<src>/aem-sample-we-retail-journal/react-app`)을 엽니다.

>[!NOTE]
>
> 각도 개발에만 관심이 있는 경우 이 섹션을 [건너뛰십시오](#angular-component).

1. 폴더 내부에서 `react-app` src 폴더로 이동합니다. 구성 요소 폴더를 확장하여 기존 반응 구성 요소 파일을 봅니다.

   ![반응형 구성 요소 파일 구조](assets/spa-editor-helloworld-tutorial-use/react-components.png)

2. 이름이 지정된 구성 요소 폴더 아래에 새 파일을 추가합니다 `HelloWorld.js`.
3. 열기 `HelloWorld.js`. 가져오기 문을 추가하여 React 구성 요소 라이브러리를 가져옵니다. 두 번째 가져오기 문을 추가하여 Adobe에서 제공하는 `MapTo` 도우미를 가져옵니다. 도우미는 AEM 구성 요소의 JSON에 반응형 구성 요소의 매핑을 `MapTo` 제공합니다.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

4. 가져오기 아래에서 React 인터페이스를 확장하는 새 클래스 `HelloWorld` 를 만듭니다 `Component` . 필요한 `render()` 메서드를 `HelloWorld` 클래스에 추가합니다.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

5. 도우미에는 `MapTo` 반응 구성 요소의 prop `cqModel` 의 일부로 명명된 개체가 자동으로 포함됩니다. 여기에는 `cqModel` 에 의해 노출된 모든 속성이 포함됩니다 [!DNL Sling Model].

   이전에 [!DNL Sling Model] 만든 항목에 메서드가 포함되어 있음을 기억하십시오 `getDisplayMessage()`. `getDisplayMessage()` 는 출력 시 이름이 지정된 JSON 키로 `displayMessage` 변환됩니다.

   값을 포함하는 `render()` 태그를 출력하는 메서드를 `h1` `displayMessage`구현합니다. [JavaScript](https://reactjs.org/docs/introducing-jsx.html)의 구문 확장자인 JSX는 구성 요소의 최종 마크업을 반환하는 데 사용됩니다.

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

6. 편집 구성 방법을 구현합니다. 이 메서드는 `MapTo` 도우미를 통해 전달되며, 구성 요소가 비어 있는 경우 자리 표시자를 표시하는 정보를 AEM 편집기에 제공합니다. 이 문제는 구성 요소가 SPA에 추가되었지만 아직 작성되지 않은 경우에 발생합니다. 클래스 아래에 다음을 `HelloWorld` 추가합니다.

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

7. 파일 끝에 있는 `MapTo` 도우미를 호출하고 `HelloWorld` 클래스와 클래스를 전달합니다 `HelloWorldEditConfig`. AEM 구성 요소의 리소스 유형을 기반으로 AEM 구성 요소에 반응형 구성 요소를 매핑합니다. `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   HelloWorld.js에 대한 [**완성된** 코드는 여기에서 찾을 수 있습니다.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

8. Open the file `ImportComponents.js`. 에 있습니다 `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   컴파일된 JavaScript 번들의 다른 구성 요소와 함께 `HelloWorld.js` 필요한 줄을 추가합니다.

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

9. 폴더에서 `components` 파일 채우기 `HelloWorld.css` 의 동위 `HelloWorld.js.` 로 명명된 새 파일을 만들어 구성 요소에 대한 몇 가지 기본 스타일 `HelloWorld` 을 만듭니다.

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

10. 필요한 가져오기 문 `HelloWorld.js` 에서 다시 열고 업데이트합니다. `HelloWorld.css`

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

11. Apache Maven을 사용하여 코드를 AEM에 배포:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

12. CRXDE- [Lite에서](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) 엽니다 `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. app.js에서 HelloWorld를 빠르게 검색하여 Responsive 구성 요소가 컴파일된 앱에 포함되어 있는지 확인합니다.

   >[!NOTE]
   >
   > **app.js** 는 번들 React 앱입니다. 이 코드는 더 이상 사람이 읽을 수 없습니다. 이 `npm run build` 명령은 최신 브라우저에서 해석할 수 있는 컴파일된 JavaScript를 출력하는 최적화된 빌드를 트리거했습니다.


## 각도 구성 요소 만들기 {#angular-component}

**페르소나:프런트 엔드 개발자**

>[!NOTE]
>
> 반응형 개발에 관심이 있는 경우 이 섹션을 건너뛰십시오.

그런 다음 각도 컴포넌트가 생성됩니다. 원하는 편집기를 사용하여 **각도 앱** 모듈(`<src>/aem-sample-we-retail-journal/angular-app`)을 엽니다.

1. 폴더 안 `angular-app` 에서 해당 `src` 폴더로 이동합니다. 구성 요소 폴더를 확장하여 기존 각 구성 요소 파일을 봅니다.

   ![각 파일 구조](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

2. 이름이 지정된 구성 요소 폴더 아래에 새 폴더를 추가합니다 `helloworld`. 폴더 아래에 `helloworld` 이름이 새로 추가되었습니다 `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

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

3. 열기 `helloworld.component.ts`. 가져오기 문을 추가하여 각도 `Component` 및 `Input` 클래스를 가져옵니다. 및 를 가리키는 새 구성 요소 `styleUrls` 를 만들고, `templateUrl` and를 `helloworld.component.css` 지정합니다 `helloworld.component.html`. 마지막으로, 예상 입력 `HelloWorldComponent` 으로 클래스를 내보냅니다 `displayMessage`.

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
   > 이전에 [!DNL Sling Model] 만든 내용을 다시 호출할 경우 getDisplayMessage() **라는 메서드가 있었습니다**. 이 메서드의 직렬화된 JSON은 **displayMessage**&#x200B;가 되며, 이는 현재 Angular 앱에서 읽고 있습니다.

4. 속성 `helloworld.component.html` 을 인쇄하는 `h1` 태그를 포함하도록 열어서 `displayMessage` 다음을 수행합니다.

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

5. 구성 요소 `helloworld.component.css` 에 대한 몇 가지 기본 스타일을 포함하도록 업데이트합니다.

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

6. 다음 테스트 `helloworld.component.spec.ts` 로 업데이트합니다.

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

7. 다음 업데이트 `src/components/mapping.ts` 에 해당 항목이 `HelloWorldComponent`포함됩니다. 구성 요소를 구성하기 전에 AEM 편집기에서 자리 표시자를 표시하는 `HelloWorldEditConfig` 항목을 추가합니다. 마지막으로 한 줄을 추가하여 AEM 구성 요소를 도우미로 각도 구성 요소에 `MapTo` 매핑합니다.

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

   mapping.ts에 대한 전체 [**코드는** 여기에서 찾을 수 있습니다.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

8. NgModule `src/app.module.ts` 을 업데이트하도록 **업데이트합니다**. AppModule **`HelloWorldComponent`** 에 속하는 **선언으로** 추가합니다 ****. 또한 JSON 모델 `HelloWorldComponent` 이 처리되는 동안 **컴파일되고 동적으로 앱에 포함되도록 응모 구성** 요소로 해당 구성 요소를 추가합니다.

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

   app.module.ts에 대한 [**완료된** 코드는 여기에서 찾을 수 있습니다.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

9. Maven을 사용하여 코드를 AEM에 배포합니다.

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

10. CRXDE- [Lite에서](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) 엽니다 `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. HelloWorld **에** 대한 빠른 검색 `main.js` 을 수행하여 Angular 구성 요소가 포함되어 있는지 확인합니다.

   >[!NOTE]
   >
   > **main.js** 는 번들 Angular 앱입니다. 이 코드는 더 이상 사람이 읽을 수 없습니다. npm run build 명령은 최신 브라우저에서 해석할 수 있는 컴파일된 JavaScript를 출력하는 최적화된 빌드를 트리거했습니다.

## 템플릿 업데이트 {#template-update}

1. 반응 및/또는 각 버전에 대한 편집 가능한 템플릿으로 이동합니다.

   * (각도) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (반응) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

2. 기본 [!UICONTROL 레이아웃 컨테이너를] 선택하고 정책 [!UICONTROL 아이콘을 선택하여] 정책을 엽니다.

   ![레이아웃 정책 선택](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   속성 **** > **[!UICONTROL 허용된 구성 요소]**&#x200B;아래에서 **[!DNL Custom Components]**&#x200B;검색을수행합니다. 구성 요소가 **[!DNL Hello World]** 표시되면 선택합니다. 오른쪽 위 모서리에 있는 확인란을 클릭하여 변경 내용을 저장합니다.

   ![레이아웃 컨테이너 정책 구성](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

3. 저장 후 레이아웃 컨테이너에는 구성 **[!DNL HelloWorld]** 요소를 허용된 구성 요소로 [!UICONTROL 표시해야 합니다].

   ![허용된 구성 요소 업데이트됨](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > AEM 6.5 및 AEM 6.4.5만이 SPA 편집기의 편집 가능한 템플릿 기능을 지원합니다. AEM 6.4를 사용하는 경우 CRXDE Lite을 통해 허용된 구성 요소에 대한 정책을 수동으로 구성해야 합니다. `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` or `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   레이아웃 컨테이너에서 허용된 구성 요소에 대한 [!UICONTROL 업데이트된] 정책 구성을 보여주는 [!UICONTROL CRXDE Lite]:

   ![레이아웃 컨테이너에서 허용된 구성 요소에 대한 업데이트된 정책 구성을 보여주는 CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## 모든 것을 통합하여 {#putting-together}

1. 각도 또는 반응 페이지로 이동합니다.

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

2. 구성 요소를 **[!DNL Hello World]** 찾아 구성 요소를 페이지에 드래그하여 **[!DNL Hello World]** 놓습니다.

   ![hello world drag + drop](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   자리 표시자가 나타납니다.

   ![인사말](assets/spa-editor-helloworld-tutorial-use/fig10.png)

3. 구성 요소를 선택하고 대화 상자에 &quot;세계&quot; 또는 &quot;사용자 이름&quot;과 같은 메시지를 추가합니다. 변경 사항을 저장합니다.

   ![렌더링된 구성 요소](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   &quot;Hello&quot; 문자열은 항상 메시지에 접두사가 됩니다. 이것은 그 논리의 `HelloWorld.java` 결과이다 [!DNL Sling Model].

## 다음 단계 {#next-steps}

[HelloWorld 구성 요소에 대한 솔루션 완료](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* GitHub [[!DNL We.Retail Journal] 에 대한 전체 소스 코드](https://github.com/adobe/aem-sample-we-retail-journal)
* AEM SPA Editor - WKND Tutorial]를 사용하여 반응형 개발 [[!DNL 시작하기 자습서를 참조하십시오.](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## 문제 해결 {#troubleshooting}

### Eclipse에서 프로젝트를 빌드할 수 없음 {#unable-to-build-project-in-eclipse}

**오류:** 인식할 수 없는 목표 실행을 위해 프로젝트를 Eclipse로 [!DNL We.Retail Journal] 가져올 때 오류가 발생했습니다.

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![일식 오류 마법사](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**해상도**:마침을 클릭하여 나중에 확인합니다. 따라서 자습서가 완료되는 것을 방지할 수 없습니다.

**오류**:Aven 빌드 동안 React 모듈 `react-app`이 성공적으로 빌드되지 않습니다.

**해결 방법:** 반응형 앱 `node_modules` 아래의 **폴더를 삭제해 보십시오**. 프로젝트의 루트에서 Apache Maven 명령 `mvn  clean install -PautoInstallSinglePackage` 을 다시 실행합니다.

### AEM의 종속성 충족 {#unsatisfied-dependencies-in-aem}

![패키지 관리자 종속성 오류](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

AEM 종속성이 충족되지 않으면 **[!UICONTROL AEM 패키지 관리자]** 또는 **[!UICONTROL AEM 웹 콘솔]** (Felix Console)에서 SPA 편집기 기능을 사용할 수 없음을 나타냅니다.

### 구성 요소가 표시되지 않음

**오류**:배포 성공 후 컴파일된 버전의 React/Angular 앱에 업데이트된 구성 요소가 있는지 확인한 후에도 구성 요소를 페이지로 드래그하면 표시되지 않습니다. `helloworld` AEM UI에서 구성 요소를 볼 수 있습니다.

**해상도**:브라우저의 내역/캐시를 지우거나 새로운 브라우저를 열거나 익명으로 알려진 모드를 사용하십시오. 그렇지 않으면 로컬 AEM 인스턴스에서 클라이언트 라이브러리 캐시를 무효화합니다. AEM은 효율적인 구현을 위해 대형 클라이언트 라이브러리를 캐시하려고 합니다. 오래된 코드가 캐시되는 문제를 수정하기 위해 때로 캐시를 수동으로 무효화해야 합니다.

탐색: [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) 를 클릭하고 캐시 무효화를 클릭합니다. [반응/각도] 페이지로 돌아가서 페이지를 새로 고칩니다.

![클라이언트 라이브러리 다시 빌드](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
