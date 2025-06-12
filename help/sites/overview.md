---
title: AEM Sites 비디오 및 튜토리얼
description: Adobe Experience Manager Sites의 특징과 기능에 대한 비디오와 튜토리얼을 살펴봅니다. AEM Sites는 대표적인 경험 관리 플랫폼입니다.
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 36917be459162e5399620c976bfe953cc5553c82
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 62%

---

# AEM Sites 비디오 및 튜토리얼 {#overview}

{{edge-delivery-services}}

Adobe Experience Manager(AEM) Sites는 선도적인 경험 관리 플랫폼입니다. 이 사용 안내서에는 AEM Sites의 다양한 기능과 성능에 대한 비디오 및 튜토리얼이 포함되어 있습니다.

## AEM Sites를 활용한 3가지 빌드 방법

AEM Sites는 경험을 빌드하고, 작성하고, 전달하는 세 가지 방법을 제공합니다. 전체 페이지를 빌드하든, 에지 성능을 최적화하든, Headless 앱을 구동하든, AEM Sites는 프로젝트 요구사항에 맞춘 유연한 옵션을 제공합니다.

1. **Edge Delivery Services** 웹 사이트는 문서 기반 작성 또는 Adobe 유니버설 편집기를 사용하여 콘텐츠를 작성한 후 활성화된 다음 Edge Delivery Services as HTML 웹 페이지를 통해 최종 사용자에게 전달됩니다. 이 옵션은 주로 고성능, 확장성 및 속도를 필요로 하는 _신규 및 기존 프로젝트_&#x200B;에 사용됩니다.
1. **Headless/API-first** 웹 경험은 콘텐츠 조각 편집기 또는 유니버설 편집기를 사용하여 콘텐츠를 만든 다음 활성화하여 AEM Publish에서 JSON으로 제공합니다. 이 옵션은 주로 모바일 앱, 단일 페이지 애플리케이션(SPA) 또는 기타 Headless 애플리케이션에 Headless 콘텐츠 전달이 필요한 _신규 및 기존 프로젝트_&#x200B;에 사용됩니다.
1. **기존 AEM**&#x200B;은(는) AEM Sites을 사용하여 빌더 웹 경험을 만드는 최신 방법이 아닙니다. 기존 AEM은 AEM 작성자의 페이지 편집기 를 사용하여 콘텐츠를 작성하고, 이 콘텐츠가 활성화되고 AEM Publish as HTML 웹 페이지를 통해 최종 사용자에게 전달됩니다. _기존 프로젝트_&#x200B;에는 기존 AEM을 사용하는 것이 좋습니다.

이러한 옵션은 마케팅 조직의 다양한 요구를 충족하도록 설계되었으며, 어떤 채널이나 디바이스에서도 고속으로 확장 가능한 맞춤형 경험을 제공할 수 있게 합니다.

>[!IMPORTANT]
>
> **Edge Delivery Services**&#x200B;은(는) AEM Sites을 사용하여 빌드하는 최신 방법입니다. Adobe Edge Network의 강력한 기능을 활용하여 규모에 맞게 고성능 웹 사이트를 제공하도록 설계되었습니다.

다음 다이어그램은 다양한 경로를 보여 줍니다.

![AEM-Sites-콘텐츠-작성-및-경험-전달-경로.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### AEM Sites를 활용한 빌드 방법 비교

다음 표는 세 가지 경로를 개략적으로 비교한 것입니다. 각 경로에서의 콘텐츠 작성 방식과 경험 게재 방식의 차이점을 중심으로 설명합니다.

|            | Edge Delivery Services | Headless/API 중심 | 기존 AEM |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **추천 항목** | 높은 트래픽, 성능 및 확장성 요구 사항을 가진 웹 사이트 | 모바일 앱, SPA 및 기타 Headless 애플리케이션 | 기존 프로젝트(최신 접근 방식 아님) |
| **작성 도구** | 문서 기반 작성, 범용 편집기 | 콘텐츠 조각, 범용 편집기 | 페이지 편집기 |
| **작성된 콘텐츠 저장소** | 문서 또는 AEM 작성자 인스턴스 (JCR) | AEM 작성자 인스턴스 (JCR) | AEM 작성자 인스턴스 (JCR) |
| **게재** | Edge Delivery Services | AEM 게시 인스턴스 (Adobe CDN + Dispatcher 포함) | AEM 게시 인스턴스 (Adobe CDN + Dispatcher 포함) |
| **게재 콘텐츠 스토어** | Edge Delivery Services | AEM 게시 인스턴스 (JCR) | AEM 게시 인스턴스 (JCR) |
| **게재 형식** | HTML | JSON | HTML |
| **개발 기술** | JavaScript, CSS | 모두 (예: Swift, React 등) | Java™, JavaScript, CSS |
| **구현 단계** | 신규 및 기존 프로젝트 | 신규 및 기존 프로젝트 | 기존 프로젝트만 |

## 튜토리얼

다음 튜토리얼을 통해 AEM Sites를 활용한 세 가지 빌드 경로에 대해 알아보십시오.

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with EDS.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
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
                    <p class="is-size-6">포괄적인 안내서를 통해 Edge Delivery Services를 살펴봅니다. 빌드, 게시 및 출시 안내서에서는 EDS를 시작하는 데 필요한 모든 내용을 다룹니다.</p>
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
                    <p class="is-size-6">AEM 콘텐츠를 기반으로 Headless 애플리케이션을 만드는 방법을 알아봅니다. 튜토리얼에서는 iOS, Android 및 React와 같은 프레임워크를 다루며 스택에 맞는 항목을 선택합니다.</p>
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
                    <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="기존 AEM - WKND 자습서" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="기존 AEM - WKND 자습서"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ko/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="기존 AEM - WKND 자습서">기존 AEM - WKND 자습서</a>
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
