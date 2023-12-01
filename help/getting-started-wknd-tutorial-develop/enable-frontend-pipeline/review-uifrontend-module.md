---
title: 전체 스택 프로젝트의 ui.frontend 모듈 검토
description: Maven 기반 전체 스택 AEM Sites 프로젝트의 프론트엔드 개발, 배포 및 게재 수명 주기를 검토합니다.
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
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 2%

---

# 전체 스택 AEM 프로젝트의 &#39;ui.frontend&#39; 모듈 검토 {#aem-full-stack-ui-frontent}

이 장에서는 의 &#39;ui.frontend&#39; 모듈에 초점을 맞추어 전체 스택 AEM 프로젝트의 프론트엔드 아티팩트의 개발, 배포 및 게재를 검토합니다 __WKND Sites 프로젝트__.


## 목표 {#objective}

* AEM 전체 스택 프로젝트에서 프론트엔드 아티팩트의 빌드 및 배포 흐름 이해
* AEM 전체 스택 프로젝트 검토 `ui.frontend` 모듈 [webpack](https://webpack.js.org/) 구성
* AEM 클라이언트 라이브러리(clientlibs라고도 함) 생성 프로세스

## AEM 전체 스택 및 빠른 사이트 생성 프로젝트에 대한 프론트엔드 배포 플로우

>[!IMPORTANT]
>
>이 비디오에서는 두 기능의 프론트엔드 흐름을 설명하고 보여 줍니다 **전체 스택 및 빠른 사이트 생성** 프론트엔드 리소스 빌드, 배포 및 제공 모델의 미묘한 차이점을 개략적으로 설명하는 프로젝트.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## 사전 요구 사항 {#prerequisites}


* 복제 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)
* 복제된 AEM WKND Sites 프로젝트를 빌드하고 AEMas a Cloud Service 에 배포했습니다.

AEM WKND 사이트 프로젝트 를 참조하십시오 [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) 을 참조하십시오.

## AEM 전체 스택 프로젝트 프론트엔드 아티팩트 흐름 {#flow-of-frontend-artifacts}

다음은 의 상위 수준 표현입니다. __개발, 배포 및 제공__ 전체 스택 AEM 프로젝트에서의 프론트엔드 아티팩트 흐름.

![프론트엔드 아티팩트 개발, 배포 및 전달](assets/Dev-Deploy-Delivery-AEM-Project.png)


개발 단계에서는 CSS와 JS 파일을 업데이트하여 스타일링 및 리브랜딩과 같은 프론트엔드 변경 작업을 수행합니다. `ui.frontend/src/main/webpack` 폴더를 삭제합니다. 그런 다음 작성 시간 동안 [webpack](https://webpack.js.org/) module-bundler 및 maven 플러그인은 이러한 파일을 다음에서 최적화된 AEM clientlib으로 전환합니다. `ui.apps` 모듈.

AEM 프론트엔드 변경 사항은 를 실행할 때 as a Cloud Service 환경에 배포됩니다. [__전체 스택__ cloud Manager의 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

프론트엔드 리소스는 로 시작하는 URI 경로를 통해 웹 브라우저에 전달됩니다. `/etc.clientlibs/`, 및 는 일반적으로 AEM Dispatcher 및 CDN에 캐시됩니다.


>[!NOTE]
>
> 마찬가지로 __AEM 빠른 사이트 생성 여정__, [프론트엔드 변경 사항](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) 를 실행하여 AEM as a Cloud Service 환경에 배포됩니다. __프론트엔드__ 파이프라인, 참조 [파이프라인 설정](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### WKND Sites 프로젝트에서 Webpack 구성 검토 {#development-frontend-webpack-clientlib}

* 세 개 있습니다 __webpack__ wknd 사이트 프론트엔드 리소스를 번들로 만드는 데 사용되는 구성 파일입니다.

   1. `webpack.common` - 여기에는 __일반__ wknd 리소스 번들링 및 최적화를 지시하기 위한 구성. 다음 __출력__ 속성은 통합된 파일(JavaScript 번들이라고도 하며, AEM OSGi 번들과 혼동하지 않도록 함)을 방출하는 위치를 알려줍니다. 기본 이름은 로 설정됩니다. `clientlib-site/js/[name].bundle.js`.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` 다음을 포함: __개발__ webpack-dev-serve에 대한 구성과 사용할 HTML 템플릿을 가리킵니다. 또한에서 실행 중인 AEM 인스턴스에 대한 프록시 구성이 포함되어 있습니다. `localhost:4502`.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` 다음을 포함: __production__ 를 구성하고 플러그인을 사용하여 개발 파일을 최적화된 번들로 변환합니다.

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


* 번들된 리소스는 `ui.apps` 을 사용하는 모듈 [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) 플러그인(에서 관리되는 구성 사용) `clientlib.config.js` 파일.

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

* 다음 __frontend-maven-plugin__ 출처: `ui.frontend/pom.xml` AEM 프로젝트 빌드 중 webpack 번들 및 clientlib 생성을 조정합니다.

`$ mvn clean install -PautoInstallSinglePackage`

### AEM에 as a Cloud Service 배포 {#deployment-frontend-aemaacs}

다음 [__전체 스택__ 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) 이러한 변경 사항을 AEM as a Cloud Service 환경에 배포합니다.


### AEM에서 as a Cloud Service 게재 {#delivery-frontend-aemaacs}

전체 스택 파이프라인을 통해 배포된 프론트엔드 리소스는 AEM 사이트에서 다음과 같이 웹 브라우저로 전달됩니다. `/etc.clientlibs` 파일. 다음을 방문하여 확인할 수 있습니다. [공개적으로 호스팅된 WKND 사이트](https://wknd.site/content/wknd/us/en.html) 웹 페이지의 소스 보기.

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

다음 장에서는 [프론트엔드 파이프라인을 사용하도록 프로젝트 업데이트](update-project.md)를 설치한 후 AEM WKND Sites 프로젝트를 업데이트하여 프론트엔드 파이프라인 계약에 대해 활성화합니다.
