---
title: 전체 스택 프로젝트의 ui.frontend 모듈 검토
description: maven 기반 전체 스택 AEM Sites 프로젝트의 프런트 엔드 개발, 배포 및 게재 수명 주기를 검토합니다.
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
source-wordcount: '614'
ht-degree: 2%

---


# 전체 스택 AEM 프로젝트의 &#39;ui.frontend&#39; 모듈을 검토합니다. {#aem-full-stack-ui-frontent}

이 장에서는 전체 스택 AEM 프로젝트의 &#39;ui.frontend&#39; 모듈에 초점을 맞추어 전체 스택 프로젝트의 프런트 엔드 아티팩트의 개발, 배포 및 전달을 검토합니다 __WKND Sites 프로젝트__.


## 목표 {#objective}

* AEM 전체 스택 프로젝트에서 프런트 엔드 아티팩트의 빌드 및 배포 흐름 이해
* AEM 전체 스택 프로젝트의 `ui.frontend` 모듈 [웹 팩](https://webpack.js.org/) 구성
* AEM 클라이언트 라이브러리(clientlibs라고도 함) 생성 프로세스

## AEM 전체 스택 및 빠른 사이트 만들기 프로젝트를 위한 프런트 엔드 배포 흐름

>[!IMPORTANT]
>
>이 비디오에서는 두 가지 모두에 대한 프런트엔드 흐름을 설명하고 보여 줍니다 **전체 스택 및 빠른 사이트 작성** 프런트 엔드 리소스 빌드, 배포 및 제공 모델의 미묘한 차이를 설명하는 프로젝트입니다.

>[!VIDEO](https://video.tv.adobe.com/v/3409344/)

## 사전 요구 사항 {#prerequisites}


* 복제 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)
* 복제된 AEM WKND Sites 프로젝트를 AEM as a Cloud Service에 구축 및 배포합니다.

AEM WKND Site 프로젝트를 참조하십시오 [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) 자세한 내용

## AEM 전체 스택 프로젝트 프런트 엔드 아티팩트 흐름 {#flow-of-frontend-artifacts}

아래는 __개발, 배포 및 제공__ 전체 스택 AEM 프로젝트에서 프런트 엔드 아티팩트의 흐름.

![프런트엔드 객체 개발, 배치 및 전달](assets/Dev-Deploy-Delivery-AEM-Project.png)


개발 단계 동안, 스타일링 및 리브랜딩과 같은 프런트 엔드 변경 사항은 에서 CSS, JS 파일을 업데이트하여 수행합니다 `ui.frontend/src/main/webpack` 폴더를 입력합니다. 그런 다음 빌드 시간 동안 [웹 팩](https://webpack.js.org/) module bundler 및 maven 플러그인은 아래의 최적화된 AEM clientlibs로 전환합니다. `ui.apps` 모듈.

프런트 엔드 변경 사항은 를 실행할 때 AEM as a Cloud Service 환경에 배포됩니다 [__전체 스택__ Cloud Manager의 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

프런트엔드 리소스는 `/etc.clientlibs/`및 는 일반적으로 AEM Dispatcher 및 CDN에 캐시됩니다.


>[!NOTE]
>
> 마찬가지로, __AEM 빠른 사이트 만들기 여정__, [프런트엔드 변경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) 를 실행하여 AEM as a Cloud Service 환경에 배포됩니다. __프런트엔드__ 파이프라인 참조: [파이프라인 설정](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### WKND Sites 프로젝트에서 웹 팩 구성 검토 {#development-frontend-webpack-clientlib}

* 3개 있습니다 __웹 팩__ WKND 사이트 프런트 엔드 리소스를 번들로 제공하는 데 사용되는 구성 파일입니다.

   1. `webpack.common` - 여기에는 다음 항목이 포함됩니다 __공통__ WKND 리소스 번들 및 최적화를 지시하는 구성. 다음 __출력__ 속성은 생성된 통합 파일(JavaScript 번들이라고도 하지만 AEM OSGi 번들과 혼동하지 않도록 할 위치를 알려줍니다.) 기본 이름은 (으)로 설정되어 있습니다 `clientlib-site/js/[name].bundle.js`.

   ```javascript
       ...
       output: {
               filename: 'clientlib-site/js/[name].bundle.js',
               path: path.resolve(__dirname, 'dist')
           }
       ...    
   ```

   1. `webpack.dev.js` 다음 포함 __개발__ webpack-dev-serve 구성 및 사용할 HTML 템플릿을 가리킵니다. 또한,에서 실행되는 AEM 인스턴스에 대한 프록시 구성이 포함되어 있습니다 `localhost:4502`.

   ```javascript
       ...
       devServer: {
           proxy: [{
               context: ['/content', '/etc.clientlibs', '/libs'],
               target: 'http://localhost:4502',
           }],
       ...    
   ```

   1. `webpack.prod.js` 다음 포함 __production__ 구성 및 은 플러그인을 사용하여 개발 파일을 최적화된 번들로 변환합니다.

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


* 번들 리소스는 로 이동됩니다. `ui.apps` 모듈 사용 [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) 플러그인을 사용하여 `clientlib.config.js` 파일.

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

* 다음 __frontend-maven-plugin__ 변환 전: `ui.frontend/pom.xml` AEM 프로젝트 빌드 중 webpack bundling 및 clientlib 생성을 오케스트레이션합니다.

`$ mvn clean install -PautoInstallSinglePackage`

### AEM as a Cloud Service에 배포 {#deployment-frontend-aemaacs}

다음 [__전체 스택__ 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) AEM as a Cloud Service 환경에 이러한 변경 사항을 배포합니다.


### AEM에서 게재 as a Cloud Service {#delivery-frontend-aemaacs}

전체 스택 파이프라인을 통해 배포되는 프런트 엔드 리소스는 AEM 사이트에서 웹 브라우저로 전달됩니다 `/etc.clientlibs` 파일. 을(를) 방문하여 확인할 수 있습니다 [공개적으로 호스팅된 WKND 사이트](https://wknd.site/content/wknd/us/en.html) 웹 페이지의 소스 보기

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 축하합니다! {#congratulations}

축하합니다. 전체 스택 프로젝트의 ui.frontend 모듈을 검토했습니다

## 다음 단계 {#next-steps}

다음 장에서 [프런트 엔드 파이프라인을 사용하도록 프로젝트 업데이트](update-project.md)를 업데이트하여 프런트 엔드 파이프라인 계약에 사용할 수 있도록 AEM WKND Sites 프로젝트를 업데이트합니다.
