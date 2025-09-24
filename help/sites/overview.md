---
title: AEM Sites 비디오 및 튜토리얼
description: Adobe Experience Manager Sites의 특징과 기능에 대한 비디오와 튜토리얼을 살펴봅니다. AEM Sites는 대표적인 경험 관리 플랫폼입니다.
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 14ca2ba3d5b6c116e3fa8b437aa9ed90375ae468
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 38%

---

# AEM Sites 비디오 및 튜토리얼 {#overview}

{{edge-delivery-services}}

Adobe Experience Manager(AEM) Sites는 Adobe의 경험 관리 플랫폼으로, 웹 사이트, 모바일 앱 또는 기타 디지털 채널을 통해 디지털 경험을 작성, 관리 및 제공할 수 있습니다.

## AEM Sites을 통해 경험을 전달하는 세 가지 방법

AEM Sites는 경험을 빌드하고, 작성하고, 전달하는 세 가지 방법을 제공합니다. 웹 사이트를 구축하든, 에지 성능을 최적화하든, 헤드리스 앱을 강화하든 상관없이 AEM Sites은 프로젝트 요구 사항에 맞는 유연한 옵션을 제공합니다.

1. **Edge Delivery Services** 경험은 Adobe의 Edge Network을 사용하여 빠르고 짧은 지연 시간으로 콘텐츠를 제공합니다. 이 서비스는 소비하는 디바이스, 검색 엔진 및 GenAI 에이전트에 대한 콘텐츠를 자동으로 최적화합니다. 작성자는 Adobe Universal Editor 또는 문서 기반 작성을 사용하여 컨텐츠를 작성합니다.
1. **헤드리스/API 우선** 경험은 AEM Publish를 사용하여 모바일 앱, 단일 페이지 애플리케이션(SPA) 또는 기타 헤드리스 클라이언트에 대한 HTTP API를 통해 JSON으로 콘텐츠를 제공합니다. 작성자가 콘텐츠 조각 편집기 또는 범용 편집기를 사용하여 콘텐츠를 만듭니다.
1. **기존 AEM** 경험은 AEM Publish를 사용하여 콘텐츠를 HTML 웹 페이지로 전달합니다. 작성자는 AEM 작성자의 페이지 편집기를 사용하여 컨텐츠를 만듭니다. 이 옵션은 기존 프로젝트 또는 이미 마이그레이션된 프로젝트에 가장 적합합니다.

세 가지 옵션 모두 강력한 접근 방식이며 최상의 선택은 사용 사례와 조직의 요구 사항에 따라 다릅니다. 각 접근 방식을 통해 팀은 모든 채널 또는 디바이스에서 개인화되고 흥미로운 경험을 빠르고 확장하여 제공할 수 있습니다.

>[!IMPORTANT]
>
> **Edge Delivery Services**&#x200B;은(는) AEM을 사용하여 웹 사이트를 제공하는 가장 최신 고급 방법입니다. Adobe Edge Network의 속도와 확장성을 현대적인 제작 옵션과 결합합니다. 새 프로젝트에는 Edge Delivery Services을 권장하지만 AEM Sites은 Headless 및 기존 접근 방식을 계속 지원하므로 요구 사항에 가장 적합한 경로를 선택할 수 있습니다.

다음 다이어그램은 AEM Sites을 사용하여 경험을 구축하기 위한 다양한 옵션을 보여 줍니다.

![AEM-Sites-콘텐츠-작성-및-경험-전달-경로.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### AEM Sites를 활용한 빌드 방법 비교

다음 표는 세 가지 경로를 개략적으로 비교한 것입니다. 각 경로에서의 콘텐츠 작성 방식과 경험 게재 방식의 차이점을 중심으로 설명합니다.

|            | Edge Delivery Services | Headless/API 중심 | 기존 AEM |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **추천 항목** | 트래픽, 성능 및 확장성 요구 사항이 높은 웹 사이트 | 모바일 앱, SPA 및 기타 Headless 애플리케이션 | 기존 프로젝트 또는 마이그레이션된 프로젝트 |
| **작성 도구** | 문서 기반 작성, 범용 편집기, 페이지 편집기 | 콘텐츠 조각, 범용 편집기 | 페이지 편집기, 유니버설 편집기 |
| **작성된 콘텐츠 저장소** | 문서 또는 AEM 작성자 인스턴스 (JCR) | AEM 작성자 인스턴스 (JCR) | AEM 작성자 인스턴스 (JCR) |
| **게재** | Edge Delivery Services | AEM 게시 인스턴스 (Adobe CDN + Dispatcher 포함) | AEM 게시 인스턴스 (Adobe CDN + Dispatcher 포함) |
| **게재 콘텐츠 저장소** | Edge Delivery Services | AEM 게시 인스턴스 (JCR) | AEM 게시 인스턴스 (JCR) |
| **게재 형식** | HTML | JSON | HTML |
| **개발 기술** | JavaScript, CSS | 모두 (예: Swift, React 등) | Java™, HTL, JavaScript, CSS |
| **검색 보트 및 GenAI 에이전트 지원** | 봇, 검색 엔진 및 GenAI 에이전트에 최적화 | 보트 및 에이전트에서 작동하지만 SSR 또는 추가 설정이 필요할 수 있음 | 봇에 적합하지만 Edge Delivery Services에 비해 성능이 느릴 수 있습니다 |

## AMS 또는 On-Premise에서 마이그레이션

AMS 또는 온프레미스(OTP)에서 AEM as a Cloud Service으로 마이그레이션하는 경우 Adobe에서는 Edge Delivery Services으로 직접 이동하는 것을 평가할 것을 권장합니다. 이러한 노력은 일반적으로 더 빠른 성능과 더 큰 확장성을 제공하면서도 AEM as a Cloud Service Publish로 마이그레이션하는 것보다 크지 않습니다. 현재 Edge Delivery Services이 적합하지 않다고 판단되거나 다른 접근 방식이 사용자의 요구를 더 잘 충족한다면 프로젝트에 대해 완전히 지원되는 유효한 옵션으로 유지됩니다.

## 튜토리얼

AEM Sites을 사용하여 빌드하기 위한 세 가지 접근 방식을 자세히 살펴보십시오. 아래 튜토리얼에서는 각 옵션의 작동 방식, 관련 도구 및 사용 시기를 안내합니다.

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with Edge Delivery Services.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional AEM - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services - 안내서" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services - 안내서"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - 안내서">Edge Delivery Services - 안내서</a>
                    </p>
                    <p class="is-size-6">포괄적인 안내서를 통해 Edge Delivery Services를 살펴봅니다. 빌드, 게시 및 실행 안내서는 Edge Delivery Services을 시작하는 데 필요한 모든 것을 다룹니다.</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API 중심 - 튜토리얼" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API 중심 - 튜토리얼"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API 중심 - 튜토리얼">Headless/API 중심 - 튜토리얼</a>
                    </p>
                    <p class="is-size-6">AEM 콘텐츠를 기반으로 Headless 애플리케이션을 만드는 방법을 알아봅니다. 튜토리얼에서는 iOS, Android 및 React와 같은 프레임워크를 다루므로, 스택에 맞는 것을 선택할 수 있습니다.</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional AEM - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="기존 AEM - WKND 튜토리얼" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="기존 AEM - WKND 튜토리얼"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="기존 AEM - WKND 튜토리얼">기존 AEM - WKND 튜토리얼</a>
                    </p>
                    <p class="is-size-6">WKND 튜토리얼을 사용하여 샘플 AEM Sites 프로젝트를 빌드하는 방법을 알아봅니다. 이 안내서에서는 프로젝트 설정, 핵심 구성 요소, 편집 가능한 템플릿, 클라이언트측 라이브러리 및 구성 요소 개발에 대해 안내합니다.</p>
                </div>
                <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## 추가 리소스

* [AEM Sites 작성 설명서](https://experienceleague.adobe.com/ko/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [AEM Sites 개발 설명서](https://experienceleague.adobe.com/ko/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [AEM Sites 관리 설명서](https://experienceleague.adobe.com/ko/docs/experience-manager-65/content/sites/administering/home)
* [AEM Sites 배포 설명서](https://experienceleague.adobe.com/ko/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [AEM as a Cloud Service 튜토리얼](/help/cloud-service/overview.md)
* [AEM Assets 튜토리얼](/help/assets/overview.md)
* [AEM Forms 튜토리얼](/help/forms/overview.md)
* [AEM Foundation 튜토리얼](/help/foundation/overview.md)
