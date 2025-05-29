---
title: 프론트엔드 파이프라인을 사용하도록 전체 스택 AEM 프로젝트 업데이트
description: 프론트엔드 아티팩트만 빌드하고 배포할 수 있도록 전체 스택 AEM 프로젝트를 업데이트하여 프론트엔드 파이프라인에 활성화하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
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
duration: 307
source-git-commit: b395b3b84e63fe6c24e597d1628f4aed5ba47469
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# 프론트엔드 파이프라인을 사용하도록 전체 스택 AEM 프로젝트 업데이트 {#update-project-enable-frontend-pipeline}

이 장에서는 전체 스택 파이프라인 실행이 필요하지 않고 프론트엔드 파이프라인을 사용하여 JavaScript 및 CSS를 배포하도록 __WKND Sites 프로젝트__&#x200B;의 구성을 변경합니다. 이렇게 하면 프론트엔드 및 백엔드 객체의 개발 및 배포 라이프사이클을 분리하므로 전체적으로 보다 빠르고 반복적인 개발 프로세스가 가능합니다.

## 목표 {#objectives}

* 프론트엔드 파이프라인을 사용하도록 전체 스택 프로젝트 업데이트

## 전체 스택 AEM 프로젝트의 구성 변경 개요

>[!VIDEO](https://video.tv.adobe.com/v/3453616?quality=12&learn=on&captions=kor)

## 사전 요구 사항 {#prerequisites}

여러 부분으로 구성된 자습서이며 [&#39;ui.frontend&#39; 모듈](./review-uifrontend-module.md)을(를) 검토했다고 가정합니다.


## 전체 스택 AEM 프로젝트 변경 사항

세 가지 프로젝트 관련 구성 변경 사항과 테스트 실행을 위해 배포할 스타일 변경이 있으므로 WKND 프로젝트에서 프론트엔드 파이프라인 계약에 사용할 수 있도록 총 4개의 특정 변경 사항이 있습니다.

1. 전체 스택 빌드 주기에서 `ui.frontend` 모듈 제거

   * 에서 WKND Sites 프로젝트의 루트 `pom.xml`이(가) `<module>ui.frontend</module>` 하위 모듈 항목에 주석을 답니다.

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

   * `ui.apps/pom.xml`에서 관련 종속성을 댓글로 남깁니다.

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

1. 새 Webpack 구성 파일 두 개를 추가하여 프론트엔드 파이프라인 계약에 대한 `ui.frontend` 모듈을 준비합니다.

   * 기존 `webpack.common.js`을(를) `webpack.theme.common.js`(으)로 복사하고 `output` 속성과 `MiniCssExtractPlugin`, `CopyWebpackPlugin` 플러그 인 구성 매개 변수를 다음과 같이 변경합니다.

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
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './theme' }
           ]
       })
   ...
   ```

   * 기존 `webpack.prod.js`을(를) `webpack.theme.prod.js`(으)로 복사하고 `common` 변수의 위치를 위의 파일로 변경합니다.

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


   * `package.json` 파일에서 `name` 속성 값이 `/conf` 노드의 사이트 이름과 같은지 확인합니다. `scripts` 속성 아래에 이 모듈에서 프론트엔드 파일을 빌드하는 방법을 지시하는 `build` 스크립트가 있습니다.

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

1. Sling 구성을 두 개 추가하여 프론트엔드 파이프라인에 대한 `ui.content` 모듈을 준비합니다.

   * `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`에 파일 만들기 - 여기에는 `ui.frontend` 모듈이 Webpack 빌드 프로세스를 사용하여 `dist` 폴더에서 생성하는 모든 프론트엔드 파일이 포함됩니다.

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
   >    __AEM WKND Sites 프로젝트__&#x200B;에서 전체 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml)를 참조하십시오.


   * `themePackageName` 값이 `package.json` 및 `name` 속성 값과 같고 `siteTemplatePath`이(가) `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` 스텁 경로 값을 가리키는 `com.adobe.aem.wcm.site.manager.config.SiteConfig`을(를) 두 번째로 사용합니다.

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
   >    __AEM WKND Sites 프로젝트__&#x200B;에서 전체 [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml)을(를) 참조하십시오.

1. 테스트 실행을 위해 프론트엔드 파이프라인을 통해 배포하기 위해 테마 또는 스타일이 변경되었으며, `ui.frontend/src/main/webpack/base/sass/_variables.scss`을(를) 업데이트하여 `text-color`을(를) Adobe 빨간색으로 변경하고 있습니다(또는 직접 선택할 수 있음).

   ```css
       $black:     #a40606;
       ...
   ```

마지막으로 이러한 변경 사항을 프로그램의 Adobe git 저장소에 푸시합니다.


>[!AVAILABILITY]
>
> 이러한 변경 사항은 __AEM WKND Sites 프로젝트__&#x200B;의 [__프론트엔드 파이프라인__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 분기 내에 있는 GitHub에서 사용할 수 있습니다.


## 주의 - _프론트엔드 파이프라인 사용_ 단추

[레일 선택기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html?lang=ko)의 [사이트](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html?lang=ko) 옵션은 사이트 루트 또는 사이트 페이지를 선택할 때 **프론트엔드 파이프라인 사용** 단추를 표시합니다. **프론트엔드 파이프라인 사용** 단추를 클릭하면 위의 **Sling 구성**&#x200B;을 재정의합니다. Cloud Manager 파이프라인 실행을 통해 위의 변경 내용을 배포한 후 **이 단추를 클릭하지 마십시오**.

![프론트엔드 파이프라인 사용 단추](assets/enable-front-end-Pipeline-button.png)

실수로 클릭한 경우 파이프라인을 다시 실행하여 프론트엔드 파이프라인 계약 및 변경 사항이 복원되도록 해야 합니다.

## 축하합니다! {#congratulations}

축하합니다. 프론트엔드 파이프라인 계약에 대해 WKND Sites 프로젝트를 활성화하도록 업데이트했습니다.

## 다음 단계 {#next-steps}

다음 장인 [프론트엔드 파이프라인을 사용하여 배포](create-frontend-pipeline.md)에서는 프론트엔드 파이프라인을 만들고 실행하고 __이동__&#x200B;하는 방법을 &#39;/etc.clientlibs&#39; 기반 프론트엔드 리소스 게재에서 확인합니다.
