---
title: 프론트엔드 파이프라인을 사용하도록 전체 스택 AEM 프로젝트 업데이트
description: 프론트엔드 아티팩트만 빌드하고 배포할 수 있도록 전체 스택 AEM 프로젝트를 업데이트하여 프론트엔드 파이프라인에 활성화하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# 프론트엔드 파이프라인을 사용하도록 전체 스택 AEM 프로젝트 업데이트 {#update-project-enable-frontend-pipeline}

이 장에서는 구성을 __WKND Sites 프로젝트__ 전체 스택 파이프라인 실행이 필요 없이 프론트엔드 파이프라인을 사용하여 JavaScript 및 CSS를 배포합니다. 이렇게 하면 프론트엔드 및 백엔드 객체의 개발 및 배포 라이프사이클을 분리하므로 전체적으로 보다 빠르고 반복적인 개발 프로세스가 가능합니다.

## 목표 {#objectives}

* 프론트엔드 파이프라인을 사용하도록 전체 스택 프로젝트 업데이트

## 전체 스택 AEM 프로젝트의 구성 변경 개요

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## 사전 요구 사항 {#prerequisites}

이 자습서는 여러 부분으로 구성되어 있으며 다음을 검토했다고 가정합니다. [&#39;ui.frontend&#39; 모듈](./review-uifrontend-module.md).


## 전체 스택 AEM 프로젝트 변경 사항

세 가지 프로젝트 관련 구성 변경 사항과 테스트 실행을 위해 배포할 스타일 변경이 있으므로 WKND 프로젝트에서 프론트엔드 파이프라인 계약에 사용할 수 있도록 총 4개의 특정 변경 사항이 있습니다.

1. 제거 `ui.frontend` 전체 스택 빌드 주기의 모듈

   * 에서 WKND Sites 프로젝트의 루트 `pom.xml` 댓글 달기 `<module>ui.frontend</module>` 하위 모듈 항목입니다.

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

   * 및 의 관련 종속성에 주석 달기 `ui.apps/pom.xml`

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

1. 준비 `ui.frontend` 두 개의 새 webpack 구성 파일을 추가하여 프론트엔드 파이프라인 계약에 대한 모듈입니다.

   * 기존 항목 복사 `webpack.common.js` 다음으로: `webpack.theme.common.js`, 및 변경 `output` 속성 및 `MiniCssExtractPlugin`, `CopyWebpackPlugin` 다음과 같은 플러그인 구성 매개변수:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * 기존 항목 복사 `webpack.prod.js` 다음으로: `webpack.theme.prod.js`, 및 변경 `common` 위의 파일에 대한 변수의 위치입니다.

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >위의 두 &#39;webpack&#39; 구성 변경 내용은 출력 파일 및 폴더 이름이 다르므로 clientlib(전체 스택)와 테마 생성(프론트엔드) 파이프라인 프론트엔드 아티팩트를 쉽게 구분할 수 있습니다.
   >
   >예상한 대로 기존 Webpack 구성을 사용하기 위해 위의 변경 사항을 건너뛸 수 있지만 아래 변경 사항이 필요합니다.
   >
   >이름을 지정하거나 구성하는 방법은 사용자가 결정합니다.


   * 다음에서 `package.json` 파일, 다음을 확인합니다.  `name` 속성 값이 의 사이트 이름과 같습니다. `/conf` 노드. 및 `scripts` 속성, a `build` 이 모듈에서 프론트엔드 파일을 빌드하는 방법을 지시하는 스크립트.

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

1. 준비 `ui.content` 두 개의 Sling 구성을 추가하여 프론트엔드 파이프라인용 모듈입니다.

   * 다음 위치에 파일 만들기 `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - 여기에는 다음과 같은 모든 프론트엔드 파일이 포함됩니다 `ui.frontend` 모듈은 `dist` webpack 빌드 프로세스를 사용하는 폴더.

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
   >    전체 보기 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) 다음에서 __AEM WKND Sites 프로젝트__.


   * 두 번째 `com.adobe.aem.wcm.site.manager.config.SiteConfig` (으)로 `themePackageName` 값이 와 같음 `package.json` 및 `name` 속성 값 및 `siteTemplatePath` 을(를) 가리키기 `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` 스텁 경로 값.

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
   >    참조: 완료 [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) 다음에서 __AEM WKND Sites 프로젝트__.

1. 테스트 실행을 위해 프론트엔드 파이프라인을 통해 배포하기 위해 테마 또는 스타일이 변경됨, 변경 중 `text-color` 를 업데이트하여 빨간색을 Adobe(또는 직접 선택할 수 있음)하려면 `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

마지막으로 이러한 변경 사항을 프로그램의 Adobe git 저장소에 푸시합니다.


>[!AVAILABILITY]
>
> 이러한 변경 사항은 내의 GitHub에서 사용할 수 있습니다. [__프론트엔드 파이프라인__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 의 분기 __AEM WKND Sites 프로젝트__.


## 주의 - _프론트엔드 파이프라인 활성화_ 단추

다음 [레일 선택기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) 의 [Site](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) 옵션은 **프론트엔드 파이프라인 활성화** 사이트 루트 또는 사이트 페이지 선택 시 버튼. 클릭 **프론트엔드 파이프라인 활성화** 단추는 위의 을(를) 재정의합니다. **Sling 구성**, 다음을 확인합니다. **클릭하지 않음** Cloud Manager 파이프라인 실행을 통해 위의 변경 사항 배포 후 이 버튼을 클릭합니다.

![프론트엔드 파이프라인 활성화 버튼](assets/enable-front-end-Pipeline-button.png)

실수로 클릭한 경우 파이프라인을 다시 실행하여 프론트엔드 파이프라인 계약 및 변경 사항이 복원되도록 해야 합니다.

## 축하합니다! {#congratulations}

축하합니다. 프론트엔드 파이프라인 계약에 대해 WKND Sites 프로젝트를 활성화하도록 업데이트했습니다.

## 다음 단계 {#next-steps}

다음 장에서는 [프론트엔드 파이프라인을 사용하여 배포](create-frontend-pipeline.md), 프론트엔드 파이프라인을 만들고 실행하고 실행 방법을 확인합니다 __멀리 이동함__ /etc.clientlibs 기반 프론트엔드 리소스 게재.
