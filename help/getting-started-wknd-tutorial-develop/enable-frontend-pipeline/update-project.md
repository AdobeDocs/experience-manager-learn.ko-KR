---
title: 프런트엔드 파이프라인을 사용하도록 전체 스택 AEM 프로젝트 업데이트
description: 프런트 엔드 파이프라인에 사용하도록 전체 스택 AEM 프로젝트를 업데이트하여 프런트 엔드 아티팩트만 빌드하고 배포하는 방법을 알아봅니다.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---


# 프런트엔드 파이프라인을 사용하도록 전체 스택 AEM 프로젝트 업데이트 {#update-project-enable-frontend-pipeline}

이 장에서는 __WKND Sites 프로젝트__ 전체 스택 파이프라인 실행을 필요로 하지 않고 프런트 엔드 파이프라인을 사용하여 JavaScript 및 CSS를 배포할 수 있습니다. 이를 통해 프런트 엔드 및 백 엔드 아티팩트의 개발 및 배포 주기를 명확하게 함으로써 전반적인 보다 빠르고 반복적인 개발 프로세스를 수행할 수 있습니다.

## 목표 {#objectives}

* 프런트 엔드 파이프라인을 사용하도록 전체 스택 프로젝트를 업데이트합니다

## 전체 스택 AEM 프로젝트의 구성 변경 개요

>[!VIDEO](https://video.tv.adobe.com/v/3409419/)

## 사전 요구 사항 {#prerequisites}

이 튜토리얼은 여러 부분으로 구성된 튜토리얼이며 [&#39;ui.frontend&#39; 모듈](./review-uifrontend-module.md).


## 전체 스택 AEM 프로젝트 변경

테스트 실행에 대해 배포할 프로젝트 관련 구성 변경 사항과 스타일 변경 사항이 3개 있으므로 프런트 엔드 파이프라인 계약에 이를 활성화하기 위해 WKND 프로젝트에서 총 4개의 특정 변경 사항이 있습니다.

1. 제거 `ui.frontend` 전체 스택 빌드 사이클의 모듈

   * 에서 WKND Sites 프로젝트의 루트입니다 `pom.xml` 댓글 달기 `<module>ui.frontend</module>` 하위 모듈 항목.

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * 및에서 관련 종속성을 설명합니다. `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. 준비 `ui.frontend` 두 개의 새 webpack 구성 파일을 추가하여 프런트엔드 파이프라인 계약에 대한 모듈입니다.

   * 기존 `webpack.common.js` 로서의 `webpack.theme.common.js`, 변경 `output` 속성 및 `MiniCssExtractPlugin`, `CopyWebpackPlugin` 플러그인 구성 매개 변수:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'clientlib-[name]/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * 기존 `webpack.prod.js` 로서의 `webpack.theme.prod.js`, 및 를 변경하고 `common` 변수를으로 위의 파일에 추가합니다.

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >위의 두 &#39;webpack&#39; 구성 변경 사항은 서로 다른 출력 파일과 폴더 이름을 가지도록 되어 있으므로 clientlib(Full-stack)과 파이프라인 프런트 엔드 아티팩트를 생성한(프런트 엔드) 테마를 쉽게 구분할 수 있습니다.
   >
   >짐작했듯이 위의 변경 사항을 건너뛰어 기존 웹 팩 구성도 사용할 수 있지만 아래 변경 사항이 필요합니다.
   >
   >이름을 지정하거나 구성하는 방법은 사용자가 결정합니다.


   * 에서 `package.json` 파일, 확인,  `name` 속성 값은 `/conf` 노드 아래에 있어야 합니다. 그리고 `scripts` 속성, `build` 이 모듈에서 프런트 엔드 파일을 빌드하는 방법을 설명하는 스크립트입니다.

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. 준비 `ui.content` 두 개의 Sling 구성을 추가하여 프런트엔드 파이프라인에 대한 모듈.

   * 에서 새 파일을 만듭니다. `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - 여기에는 `ui.frontend` 모듈은 `dist` webpack 빌드 프로세스를 사용한 폴더.

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    전체 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) 에서 __AEM WKND Sites 프로젝트__.


   * 두 번째 `com.adobe.aem.wcm.site.manager.config.SiteConfig` 사용 `themePackageName` 값과 동일한 값 `package.json` 및 `name` 속성 값 및 `siteTemplatePath` 가리키기 `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` stub 경로 값.

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    전체 [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) 에서 __AEM WKND Sites 프로젝트__.

1. 테스트 실행을 위해 프런트 엔드 파이프라인을 통해 배포하도록 테마 또는 스타일이 변경되었으며, `text-color` 를 업데이트하여 빨간색으로 Adobe(또는 직접 선택할 수 있음)하려면 `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

마지막으로 이러한 변경 사항을 프로그램의 Adobe git 리포지토리에 푸시합니다.


>[!AVAILABILITY]
>
> 이러한 변경 사항은 [__프런트엔드 파이프라인__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 의 분기 __AEM WKND Sites 프로젝트__.


## 축하합니다! {#congratulations}

축하합니다. 프런트 엔드 파이프라인 계약에 사용할 수 있도록 WKND Sites 프로젝트를 업데이트했습니다.

## 다음 단계 {#next-steps}

다음 장에서 [프런트엔드 파이프라인을 사용하여 배포](create-frontend-pipeline.md)로 지정하는 경우 프런트 엔드 파이프라인을 만들고 실행하고 Adobe의 방법을 확인합니다. __이동__ &#39;/etc.clientlibs&#39; 기반 프런트 엔드 리소스 게재에서 확인할 수 있습니다.
