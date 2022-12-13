---
title: AEM 컨텐츠 조각 콘솔 확장
description: AEM as a Cloud Service 컨텐츠 조각 콘솔 확장을 빌드하고 배포하는 방법을 알아봅니다
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 4%

---


# AEM 컨텐츠 조각 콘솔 확장

[AEM 컨텐츠 조각 콘솔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) 확장은 두 개의 확장 지점을 통해 추가할 수 있습니다. 버튼 [컨텐츠 조각 콘솔의](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) 헤더 메뉴 또는 작업 표시줄. 확장 프로그램은 App Builder 앱으로 실행되는 JavaScript로 작성되며, 사용자 지정 웹 UI 및 서버리스 Adobe I/O Runtime 작업을 구현하여 보다 집중적이고 오래 실행되는 작업을 수행할 수 있습니다.

![AEM 컨텐츠 조각 콘솔 확장](./assets/overview/example.png){align="center"}

| 확장 유형 | 설명 | 매개 변수 |
| :--- | :--- | :--- |
| 헤더 메뉴 | 다음에 표시되는 헤더에 단추를 추가합니다 __제로__ 컨텐츠 조각이 선택됩니다. | 없음. |
| 작업 표시줄 | 다음 경우에 표시되는 작업 표시줄에 단추를 추가합니다 __하나 이상__ 컨텐츠 조각이 선택됩니다. | 선택한 컨텐츠 조각 경로의 배열입니다. |

단일 AEM 컨텐츠 조각 콘솔 확장에는 0 또는 하나의 헤더 메뉴와 0 또는 하나의 작업 표시줄 확장 유형이 포함될 수 있습니다. 동일한 유형의 여러 확장 유형이 필요한 경우 여러 AEM 컨텐츠 조각 콘솔 확장을 만들어야 합니다.

AEM 컨텐츠 조각 콘솔 확장 기능, [Adobe Developer 콘솔 프로젝트](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) 그리고 [App Builder 앱](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) 사용 `@adobe/aem-cf-admin-ui-ext-tpl` Adobe Developer 콘솔 프로젝트와 연결된 템플릿입니다.

확장에서 수행할 작업을 기반으로 App Builder 앱을 생성할 때 다음 기능 중에서 선택합니다. 확장에서는 모든 옵션 조합을 사용할 수 있습니다.

|  | 에 단추 추가 [헤더 메뉴](./header-menu.md) | 에 단추 추가 [작업 표시줄](./action-bar.md) | 표시 [모달](./modal.md) | 추가 [서버측 처리기](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| 컨텐츠 조각을 선택하지 않은 경우 사용할 수 있습니다 | ✔ |  |  |  |
| 하나 이상의 컨텐츠 조각을 선택한 경우 사용할 수 있습니다 |  | ✔ |  |  |
| 사용자로부터 사용자 지정 입력 수집 |  |  | ✔️ |  |
| 사용자에게 사용자 지정 피드백을 표시합니다. |  |  | ✔️ |  |
| AEM에 대한 HTTP 요청을 호출합니다 |  |  |  | ✔ |
| Adobe/타사 서비스에 대한 HTTP 요청을 호출합니다 |  |  |  | ✔ |


## Adobe Developer 설명서

Adobe Developer에는 AEM 컨텐츠 조각 콘솔 확장에 대한 개발자 세부 사항이 포함되어 있습니다. 다음 문서를 검토하십시오 [Adobe Developer 컨텐츠를 참조하십시오](https://developer.adobe.com/uix/docs/).

## 확장 개발

AEM as a Cloud Service용 AEM 컨텐츠 조각 콘솔 확장을 생성, 개발 및 배포하는 방법에 대해 알아보려면 아래 설명된 단계를 따르십시오.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console" title="Adobe Developer 프로젝트 만들기" tabindex="-1" target="_adobe-developer-com">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Adobe Developer 프로젝트 만들기">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. 프로젝트 만들기</p>
                    <p class="is-size-6">다른 Adobe 서비스에 대한 액세스 권한을 정의하고 해당 배포를 관리하는 Adobe Developer 콘솔 프로젝트를 만듭니다.</p>
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_adobe-developer-com">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Adobe Developer 프로젝트 만들기</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation/#launch-code-generation-during-project-initialization" title="확장 앱 생성" tabindex="-1" target="_adobe-developer-com">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="확장 앱 초기화">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. 확장 앱 초기화</p>
                    <p class="is-size-6">확장이 표시되는 위치와 이 확장이 수행하는 작업을 정의하는 AEM 컨텐츠 조각 콘솔 확장 App Builder 앱을 초기화합니다.</p>
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation/#launch-code-generation-during-project-initialization" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_adobe-developer-com">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">확장 앱 초기화</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="확장 등록" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="확장 등록">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. 연장등록</p>
                    <p class="is-size-6">AEM 컨텐츠 조각 콘솔에서 확장을 헤더 메뉴 또는 작업 표시줄 확장 유형으로 등록합니다.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">확장 등록</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="헤더 메뉴" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="헤더 메뉴">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. 헤더 메뉴</p>
                    <p class="is-size-6">AEM 컨텐츠 조각 콘솔 헤더 메뉴 확장을 만드는 방법을 알아봅니다.</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">머리글 메뉴 확장</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="작업 표시줄" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="작업 표시줄">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. 작업 표시줄</p>
                    <p class="is-size-6">AEM 컨텐츠 조각 콘솔 작업 표시줄 확장을 만드는 방법을 알아봅니다.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">작업 표시줄 확장</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="양식" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="양식">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5. 모달</p>
                    <p class="is-size-6">사용자를 위한 맞춤형 경험을 만드는 데 사용할 수 있는 확장에 사용자 지정 모달을 추가합니다. 모듈은 종종 사용자로부터 입력을 수집하고 작업 결과를 표시합니다.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">모달 추가</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Adobe I/O Runtime 작업" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Adobe I/O Runtime 작업">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Adobe I/O Runtime 작업</p>
                    <p class="is-size-6">확장 프로그램이 컨텐츠 조각 및 AEM과 상호 작용하기 위해 호출할 수 있는 서버를 사용하지 않는 Adobe I/O Runtime 작업을 추가하여 사용자 지정 비즈니스 작업을 수행합니다.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Adobe I/O Runtime 작업 추가</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="테스트" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="테스트">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7. 테스트</p>
                    <p class="is-size-6">개발 중에 확장을 테스트하고, 특수 URL을 사용하여 QA 또는 UAT 테스터에 완료된 확장을 공유할 수 있습니다.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">확장 테스트</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="확장 배포" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="확장 배포">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. 프로덕션 배포</p>
                    <p class="is-size-6">확장을 배포하여 AEM 사용자가 사용할 수 있도록 Adobe I/O. 확장은 업데이트 및 제거할 수도 있습니다.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">프로덕션에 배포</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## 예시 확장

AEM 컨텐츠 조각 콘솔 확장 예제 .

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="벌크 속성 업데이트 확장" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="벌크 속성 업데이트 확장">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">벌크 속성 업데이트 확장</p>
                    <p class="is-size-6">선택한 컨텐츠 조각에서 속성을 벌크로 업데이트하는 예제 작업 표시줄 확장을 살펴봅니다.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">예제 확장 살펴보기</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>
