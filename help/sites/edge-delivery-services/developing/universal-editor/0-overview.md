---
title: Edge Delivery Services 및 범용 편집기 개발자 튜토리얼
description: AEM 범용 편집기로 작성되고 Edge Delivery Services로 게재되는 새 웹 사이트 개발의 기본 사항에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Catalog
jira: KT-15832
duration: 88
exl-id: aeac08a2-75a0-4adb-b32e-0e7f85e7eb1d
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '586'
ht-degree: 100%

---

# Edge Delivery Services 및 범용 편집기 개발자 튜토리얼

![Edge Delivery Services 및 범용 편집기 개발자 튜토리얼](./assets/0-overview/hero.png)

이 튜토리얼에서는 범용 편집기의 강력한 작성 기능과 Edge Delivery Services를 활용한 초고속 콘텐츠 게재 기능 결합하여 AEM 웹 사이트를 빌드하는 기본 사항에 대해 알아봅니다. 이 튜토리얼을 완료하면 새 프로젝트를 만들고, 로컬 개발 환경을 설정하고, 새로운 블록을 빌드하는 방법에 대한 기본적인 이해를 얻게 됩니다.

## 프로젝트 설정

AEM as a Cloud Service에서 코드 프로젝트를 만들고 새 사이트를 구성하는 방법을 알아봅니다. 이 설정을 사용하면 범용 편집기의 콘텐츠 생성 기능과 Edge Delivery Services를 통한 빠른 콘텐츠 게재 기능을 원활하게 연동하여 개발할 수 있습니다.

<!-- CARDS 

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
                    <p class="is-size-6">범용 편집기를 사용하여 편집 가능한 Edge Delivery Services용 코드 프로젝트를 만듭니다.</p>
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
                    <p class="is-size-6">범용 편집기를 사용하여 편집 가능한 Edge Delivery Services용 AEM Sites 사이트를 만듭니다.</p>
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

빠른 웹 사이트 개발을 위해 로컬 개발 환경을 구성하는 방법을 알아봅니다. 이 설정을 사용하면 범용 편집기를 통한 원활한 사이트 생성과 Edge Delivery Services를 통한 효율적인 콘텐츠 게재를 결합하여, 원활하고 최적화된 개발 워크플로가 확보됩니다.
<!-- CARDS 

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
                        <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3443978/?format=jpeg&nocache=1741027443737" alt="로컬 개발 환경 설정"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./3-local-development-environment.md" target="_blank" rel="referrer" title="로컬 개발 환경 설정">로컬 개발 환경 설정</a>
                    </p>
                    <p class="is-size-6">Edge Delivery Services를 통해 제공되고 범용 편집기로 편집 가능한 사이트에 대한 로컬 개발 환경을 설정합니다.</p>
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
                    <p class="is-size-6">Edge Delivery Services 사이트에 대한 글로벌 CSS, CSS 변수 및 웹 글꼴을 정의합니다.</p>
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

콘텐츠 모델을 정의하고 테스트 및 개발을 위한 샘플 콘텐츠를 설정하여 새 블록을 만드는 방법을 알아봅니다. 블록을 렌더링하는 두 가지 방법을 살펴보고, AEM 및 Edge Delivery Services에서 최적의 성능과 유연성을 위해 블록을 구성하는 방법을 이해합니다.

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
                        <img class="is-bordered-r-small" src="./assets/5-new-block/card.png" alt="블록 만들기"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./5-new-block.md" target="_blank" rel="referrer" title="블록 만들기">블록 만들기</a>
                    </p>
                    <p class="is-size-6">범용 편집기로 편집 가능한 Edge Delivery Services 웹 사이트용 블록을 빌드합니다.</p>
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
                        <img class="is-bordered-r-small" src="./assets/6-author-block/card.png" alt="블록 작성"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./6-author-block.md" target="_blank" rel="referrer" title="블록 작성">블록 작성</a>
                    </p>
                    <p class="is-size-6">범용 편집기를 사용하여 Edge Delivery Services 블록을 작성합니다.</p>
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
                    <a href="./7a-block-css.md" title="CSS로 블록 개발" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/7a-block-css/card.png" alt="CSS로 블록 개발"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7a-block-css.md" target="_blank" rel="referrer" title="CSS로 블록 개발">CSS로 블록 개발</a>
                    </p>
                    <p class="is-size-6">범용 편집기를 사용하여 편집 가능한 Edge Delivery Services용 CSS 블록을 개발합니다.</p>
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
                    <a href="./7b-block-js-css.md" title="CSS와 JS로 블록 개발" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/7b-block-js-css/card.png" alt="CSS와 JS로 블록 개발"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7b-block-js-css.md" target="_blank" rel="referrer" title="CSS와 JS로 블록 개발">CSS와 JS로 블록 개발</a>
                    </p>
                    <p class="is-size-6">범용 편집기를 사용하여 편집 가능한 Edge Delivery Services용 CSS와 JavaScript로 블록을 개발합니다.</p>
                </div>
                <a href="./7b-block-js-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 다음 단계

이 튜토리얼을 완료했으므로, 이제 학습한 내용을 바탕으로 다음의 집중적인 방법을 통해 작업을 진행해 보십시오. 이들 안내서에서는 여기서 다룬 코드와 개념을 확장하여 역할별 사용 사례, 고급 기술, Edge Delivery Services와 범용 편집기 개발 기술을 연마하기 위한 추가 팁을 살펴봅니다.

<!-- CARDS 

* ./how-to/block-options.md
* ./how-to/header-and-footer.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Block options">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/block-options.md" title="블록 옵션" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="how-to/assets/block-options/main.png" alt="블록 옵션"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/block-options.md" target="_blank" rel="referrer" title="블록 옵션">블록 옵션</a>
                    </p>
                    <p class="is-size-6">여러 가지 표시 옵션이 있는 블록을 빌드하는 방법을 알아봅니다.</p>
                </div>
                <a href="./how-to/block-options.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header and Footer">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/header-and-footer.md" title="머리글 및 바닥글" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="how-to/assets/header-and-footer/hero.png" alt="머리글 및 바닥글"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/header-and-footer.md" target="_blank" rel="referrer" title="머리글 및 바닥글">머리글 및 바닥글</a>
                    </p>
                    <p class="is-size-6">Edge Delivery Services와 범용 편집기에서 머리글 및 바닥글이 사용되는 방식을 알아봅니다.</p>
                </div>
                <a href="./how-to/header-and-footer.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
