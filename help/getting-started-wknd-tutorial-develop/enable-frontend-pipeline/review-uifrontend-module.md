---
title: 전체 스택 프로젝트의 ui.frontend 모듈 검토
description: Maven 기반 전체 스택 AEM Sites 프로젝트의 프론트엔드 개발, 배포 및 게재 수명 주기를 검토합니다.
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
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# 전체 스택 AEM 프로젝트의 &#39;ui.frontend&#39; 모듈 검토 {#aem-full-stack-ui-frontent}

이 장에서는 __WKND Sites 프로젝트__&#x200B;의 &#39;ui.frontend&#39; 모듈을 중심으로 전체 스택 AEM 프로젝트의 프론트엔드 아티팩트의 개발, 배포 및 게재를 검토합니다.


## 목표 {#objective}

* AEM 전체 스택 프로젝트의 프론트엔드 아티팩트의 빌드 및 배포 흐름 이해
* AEM 전체 스택 프로젝트의 `ui.frontend` 모듈 [webpack](https://webpack.js.org/) 구성을 검토하십시오.
* AEM 클라이언트 라이브러리(clientlibs라고도 함) 생성 프로세스

## AEM 전체 스택 및 빠른 사이트 생성 프로젝트에 대한 프론트엔드 배포 플로우

>[!IMPORTANT]
>
>이 비디오에서는 프론트엔드 리소스 빌드, 배포 및 게재 모델의 미묘한 차이점을 개략적으로 설명하기 위해 **전체 스택 및 빠른 사이트 만들기** 프로젝트에 대한 프론트엔드 흐름을 설명하고 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## 사전 요구 사항 {#prerequisites}


* [AEM WKND Sites 프로젝트 복제](https://github.com/adobe/aem-guides-wknd)
* 복제된 AEM WKND Sites 프로젝트를 빌드하고 AEM as a Cloud Service에 배포했습니다.

자세한 내용은 AEM WKND 사이트 프로젝트 [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md)를 참조하십시오.

## AEM 전체 스택 프로젝트 프론트엔드 아티팩트 흐름 {#flow-of-frontend-artifacts}

다음은 전체 스택 AEM 프로젝트에서 프론트엔드 아티팩트의 __개발, 배포 및 게재__ 흐름에 대한 높은 수준의 표현입니다.

![프론트엔드 아티팩트의 개발, 배포 및 게재](assets/Dev-Deploy-Delivery-AEM-Project.png)


개발 단계에서는 `ui.frontend/src/main/webpack` 폴더에서 CSS, JS 파일을 업데이트하여 스타일링 및 리브랜딩과 같은 프런트 엔드 변경 작업을 수행합니다. 그런 다음 빌드 시간 동안 [webpack](https://webpack.js.org/) 모듈 번들러 및 maven 플러그인은 이러한 파일을 `ui.apps` 모듈에서 최적화된 AEM clientlib으로 바꿉니다.

Cloud Manager[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=ko)에서 __전체 스택__ 파이프라인을 실행할 때 프론트엔드 변경 내용이 AEM as a Cloud Service 환경에 배포됩니다.

프론트엔드 리소스는 `/etc.clientlibs/`(으)로 시작하는 URI 경로를 통해 웹 브라우저에 전달되며 일반적으로 AEM Dispatcher 및 CDN에서 캐시됩니다.


>[!NOTE]
>
> 마찬가지로 __AEM 빠른 사이트 생성 여정__&#x200B;에서 __프론트엔드__ 파이프라인을 실행하여 [프론트엔드 변경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html?lang=ko)을(를) AEM as a Cloud Service 환경에 배포합니다. [파이프라인 설정](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html?lang=ko)을 참조하세요.

### WKND Sites 프로젝트에서 Webpack 구성 검토 {#development-frontend-webpack-clientlib}

* WKND 사이트 프론트엔드 리소스를 번들로 만드는 데 사용되는 세 개의 __webpack__ 구성 파일이 있습니다.

   1. `webpack.common` - WKND 리소스 번들 및 최적화를 지시하기 위한 __common__ 구성이 여기에 포함되어 있습니다. __output__ 속성은 통합 파일(JavaScript 번들이라고도 하지만 AEM OSGi 번들과 혼동하지 않도록 함)을 내보낼 위치를 알려줍니다. 기본 이름이 `clientlib-site/js/[name].bundle.js`(으)로 설정되어 있습니다.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js`은(는) webpack-dev-service에 대한 __development__ 구성을 포함하며, 사용할 HTML 템플릿을 가리킵니다. `localhost:4502`에서 실행 중인 AEM 인스턴스에 대한 프록시 구성도 포함되어 있습니다.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js`은(는) __프로덕션__ 구성을 포함하며 플러그인을 사용하여 개발 파일을 최적화된 번들로 변환합니다.

  ```javascript
      ...
      module.exports = merge(common, {
          mode: 'production',
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin(),
                  new CssMinimizerPlugin({ ...})
          }
      ...    
  ```


* `clientlib.config.js` 파일에서 관리되는 구성을 사용하여 [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) 플러그인을 사용하여 번들 리소스를 `ui.apps` 모듈로 이동시킵니다.

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* `ui.frontend/pom.xml`의 __frontend-maven-plugin__&#x200B;은(는) AEM 프로젝트 빌드 중 Webpack 번들 및 clientlib 생성을 조정합니다.

`$ mvn clean install -PautoInstallSinglePackage`

### AEM as a Cloud Service에 배포 {#deployment-frontend-aemaacs}

[__전체 스택__ 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=ko&#full-stack-pipeline)은(는) 이러한 변경 내용을 AEM as a Cloud Service 환경에 배포합니다.


### AEM as a Cloud Service 게재 {#delivery-frontend-aemaacs}

전체 스택 파이프라인을 통해 배포된 프론트엔드 리소스는 AEM 사이트에서 `/etc.clientlibs` 파일로 웹 브라우저에 전달됩니다. [공개적으로 호스팅된 WKND 사이트](https://wknd.site/content/wknd/us/en.html)를 방문하고 웹 페이지의 소스를 보면 이를 확인할 수 있습니다.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 축하합니다! {#congratulations}

축하합니다. 전체 스택 프로젝트의 ui.frontend 모듈을 검토했습니다.

## 다음 단계 {#next-steps}

다음 장 [프론트엔드 파이프라인을 사용하도록 프로젝트 업데이트](update-project.md)에서는 프론트엔드 파이프라인 계약에 사용하도록 AEM WKND Sites 프로젝트를 업데이트합니다.
