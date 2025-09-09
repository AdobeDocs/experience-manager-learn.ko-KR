---
title: 개인화 개요
description: Adobe Target 및 Adobe Experience Platform 애플리케이션을 사용하여 AEM as a Cloud Service 웹 사이트를 개인화하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
exl-id: c4fb11b9-b613-4522-b9da-18d7ae0826ec
source-git-commit: 5b91e7409ff0735bab40d78ad98410ac2ab006ed
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 11%

---

# 개인화 개요

AEM as a Cloud Service(AEMCS)를 Adobe Target 및 Adobe Experience Platform(AEP)와 통합하는 방법에 대해 알아봅니다. A/B 테스트를 사용하거나, 행동을 기반으로 사용자를 타겟팅하거나, 고객 프로필을 사용하여 콘텐츠를 개인화하는 방법을 통해 개인화된 경험을 전달하는 방법을 살펴보십시오.

## 사전 요구 사항

다양한 개인화 시나리오를 보여주기 위해 이 자습서에서는 샘플 [AEM WKND](https://github.com/adobe/aem-guides-wknd/) 프로젝트를 사용합니다. 따라가려면 다음이 필요합니다.

- 다음에 대한 액세스 권한이 있는 Adobe 조직:
   - **AEM as a Cloud Service 환경** - 콘텐츠 만들기 및 관리
   - **Adobe Target** - 개인화된 경험을 구성하고 전달합니다.
   - **Adobe Experience Platform 애플리케이션** - 고객 프로필 및 대상자 관리
   - **AEP의 태그(이전 Launch)** - 데이터 수집 및 개인화를 위해 Web SDK 및 사용자 지정 JavaScript을 배포합니다.

- AEM 구성 요소 및 경험 조각에 대한 기본 이해

- [AEM WKND](https://github.com/adobe/aem-guides-wknd/) 프로젝트가 AEM as a Cloud Service 환경에 배포되었습니다.

## 시작하기

특정 사용 사례를 살펴보기 전에 먼저 개인화를 위해 AEM as a Cloud Service을 구성합니다. Adobe Target 및 태그를 통합하여 AEP Web SDK을 사용하여 클라이언트측 개인화를 활성화함으로써 시작합니다. 이러한 기본 단계를 통해 AEM 페이지에서 실험, 대상 타기팅 및 실시간 개인화를 지원할 수 있습니다.

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content as Adobe Target offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="Adobe Target 통합" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="Adobe Target 통합"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="Adobe Target 통합">Adobe Target 통합</a>
                    </p>
                    <p class="is-size-6">AEMCS를 Adobe Target과 통합하여 Adobe Target이 제공하는 대로 개인화된 콘텐츠를 활성화합니다.</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Target 통합</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="태그 통합" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="태그 통합"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="태그 통합">태그 통합</a>
                    </p>
                    <p class="is-size-6">AEMCS를 태그와 통합하여 데이터 수집 및 개인화를 위해 웹 SDK 및 사용자 지정 JavaScript을 삽입합니다.</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">태그 통합</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## 사용 사례

AEMCS, Adobe Target 및 Adobe Experience Platform에서 지원하는 다음과 같은 일반적인 개인화 사용 사례를 살펴봅니다.

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
    {title = Experimentation (A/B Testing)}
    {description = Learn how to test different content variations in AEMCS using Adobe Target for A/B testing.}
    {image = ./assets/use-cases/experiment/experimentation.png}
    {cta = Learn Experimentation}

* ./use-cases/behavioral-targeting.md
    {title = Behavioral Targeting}
    {description = Learn how to personalize content based on user behavior using Adobe Experience Platform and Adobe Target.}
    {image = ./assets/use-cases/behavioral-targeting/behavioral-targeting.png}
    {cta = Learn Behavioral Targeting}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="실험 (A/B 테스트)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="실험 (A/B 테스트)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="실험 (A/B 테스트)">실험(A/B 테스트)</a>
                    </p>
                    <p class="is-size-6">A/B 테스트를 위해 Adobe Target을 사용하여 AEM의 다양한 콘텐츠 변형을 테스트하는 방법을 알아봅니다.</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">실험 학습</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Behavioral Targeting">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/behavioral-targeting.md" title="행동 타기팅" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/behavioral-targeting/behavioral-targeting.png" alt="행동 타기팅"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" title="행동 타기팅">동작 타깃팅</a>
                    </p>
                    <p class="is-size-6">Adobe Experience Platform 및 Adobe Target을 사용하여 사용자 행동을 기반으로 콘텐츠를 개인화하는 방법을 알아봅니다.</p>
                </div>
                <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">동작 타깃팅 학습</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
