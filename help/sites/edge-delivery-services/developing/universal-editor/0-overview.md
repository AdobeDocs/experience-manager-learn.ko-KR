---
title: Edge Delivery Services 및 유니버설 편집기 개발자 자습서
description: AEM Universal Editor에서 작성되고 Edge Delivery Services을 사용하여 제공되는 새 웹 사이트 개발의 기본 사항에 대해 알아봅니다.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Catalog
jira: KT-15832
duration: 89
exl-id: aeac08a2-75a0-4adb-b32e-0e7f85e7eb1d
source-git-commit: aa8ea183639c4c63be74f7ef1ce099c89454c099
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# Edge Delivery Services 및 유니버설 편집기 개발자 자습서

![Edge Delivery Services 및 유니버설 편집기 개발자 자습서](./assets/0-overview/hero.png)

이 튜토리얼에서는 강력한 작성, 범용 편집기 및 Edge Delivery Services을 사용한 빠른 전달을 결합하는 AEM 웹 사이트 구축의 기본 사항에 대해 알아봅니다. 마지막으로 새 프로젝트를 만들고, 로컬 개발 환경을 설정하고, 새 블록을 만드는 방법에 대한 기본 사항을 이해할 수 있습니다.

## 프로젝트 설정

AEM as a Cloud Service에서 코드 프로젝트를 만들고 새 사이트를 구성하는 방법에 대해 알아봅니다. 이 설정을 사용하면 범용 편집기를 사용하여 컨텐츠를 원활하게 개발하고 Edge Delivery Services을 통해 컨텐츠를 빠르게 전달할 수 있습니다.

<!-- XCARDS 

* ./1-new-code-project.md
* ./2-new-aem-site.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a code project">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./1-new-code-project.md" title="코드 프로젝트 만들기" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/1-new-project/new-project.png" alt="코드 프로젝트 만들기"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./1-new-code-project.md" target="_blank" rel="referrer" title="코드 프로젝트 만들기">코드 프로젝트 만들기</a>
                    </p>
                    <p class="is-size-6">범용 편집기를 사용하여 편집할 수 있는 Edge Delivery Services 코드 프로젝트를 만듭니다.</p>
                </div>
                <a href="./1-new-code-project.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create an AEM site">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./2-new-aem-site.md" title="AEM 사이트 만들기" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/2-new-aem-site/new-site.png" alt="AEM 사이트 만들기"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./2-new-aem-site.md" target="_blank" rel="referrer" title="AEM 사이트 만들기">AEM 사이트 만들기</a>
                    </p>
                    <p class="is-size-6">범용 편집기를 사용하여 편집할 수 있는 Edge Delivery Services을 위한 AEM Sites 사이트를 만듭니다.</p>
                </div>
                <a href="./2-new-aem-site.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 개발 설정

빠른 웹 사이트 개발을 활성화하기 위해 로컬 개발 환경을 구성하는 방법에 대해 알아봅니다. 이 설정을 통해 범용 편집기로 원활한 사이트 생성 및 Edge Delivery Services을 통한 효율적인 콘텐츠 전달을 가능하게 하여 원활하고 최적화된 개발 워크플로를 보장합니다.
<!-- XCARDS 

* ./3-local-development-environment.md
* ./4-website-branding.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up a local development environment">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./3-local-development-environment.md" title="로컬 개발 환경 설정" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/3-local-development-environment/github-clone.png" alt="로컬 개발 환경 설정"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./3-local-development-environment.md" target="_blank" rel="referrer" title="로컬 개발 환경 설정">로컬 개발 환경 설정</a>
                    </p>
                    <p class="is-size-6">Edge Delivery Services과 함께 제공되고 범용 편집기로 편집할 수 있는 사이트에 대한 로컬 개발 환경을 설정합니다.</p>
                </div>
                <a href="./3-local-development-environment.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Add website branding">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./4-website-branding.md" title="웹 사이트 브랜딩 추가" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/4-website-branding/github-issues.png" alt="웹 사이트 브랜딩 추가"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./4-website-branding.md" target="_blank" rel="referrer" title="웹 사이트 브랜딩 추가">웹 사이트 브랜딩 추가</a>
                    </p>
                    <p class="is-size-6">Edge Delivery Services 사이트에 대한 전역 CSS, CSS 변수 및 웹 글꼴을 정의합니다.</p>
                </div>
                <a href="./4-website-branding.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 블록 개발

콘텐츠 모델을 정의하고 테스트 및 개발을 위한 샘플 콘텐츠를 설정하여 새 블록을 만드는 방법을 알아봅니다. 블록을 렌더링하는 두 가지 방법을 살펴보고 AEM 및 Edge Delivery Services에서 최적의 성능과 유연성을 위해 구성하는 방법을 이해합니다.

<!-- CARDS 

* ./5-new-block.md {image = ./assets/5-new-block/card.png}
* ./6-author-block.md {image = ./assets/6-author-block/card.png}
* ./7a-block-css.md {image = ./assets/7a-block-css/card.png}
* ./7b-block-js-css.md {image = ./assets/7b-block-js-css/card.png}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./5-new-block.md" title="블록 만들기" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/5-new-block/teaser-block.png" alt="블록 만들기"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./5-new-block.md" target="_blank" rel="referrer" title="블록 만들기">블록 만들기</a>
                    </p>
                    <p class="is-size-6">범용 편집기로 편집할 수 있는 Edge Delivery Services 웹 사이트용 블록을 빌드합니다.</p>
                </div>
                <a href="./5-new-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Author a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./6-author-block.md" title="블록 작성" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/6-author-block/open-new-site.png" alt="블록 작성"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./6-author-block.md" target="_blank" rel="referrer" title="블록 작성">블록 작성</a>
                    </p>
                    <p class="is-size-6">범용 편집기로 Edge Delivery Services 블록을 작성합니다.</p>
                </div>
                <a href="./6-author-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7a-block-css.md" title="CSS를 사용하여 블록 개발" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/7a-block-css/inspect-block-dom.png" alt="CSS를 사용하여 블록 개발"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7a-block-css.md" target="_blank" rel="referrer" title="CSS를 사용하여 블록 개발">CSS로 블록 개발</a>
                    </p>
                    <p class="is-size-6">범용 편집기를 사용하여 편집할 수 있는 Edge Delivery Services용 CSS를 사용하여 블록을 개발합니다.</p>
                </div>
                <a href="./7a-block-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS and JS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7b-block-js-css.md" title="CSS 및 JS를 사용하여 블록 개발" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/7a-block-css/inspect-block-dom.png" alt="CSS 및 JS를 사용하여 블록 개발"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7b-block-js-css.md" target="_blank" rel="referrer" title="CSS 및 JS를 사용하여 블록 개발">CSS 및 JS를 사용하여 블록 개발</a>
                    </p>
                    <p class="is-size-6">CSS와 JavaScript for Edge Delivery Services을 사용하여 블록을 개발하고 범용 편집기를 사용하여 편집할 수 있습니다.</p>
                </div>
                <a href="./7b-block-js-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
