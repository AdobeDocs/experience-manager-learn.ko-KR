---
title: AEM 콘텐츠 조각 콘솔 헤더 메뉴 확장
description: AEM 콘텐츠 조각 콘솔 헤더 메뉴 확장을 만드는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 92d6e98e-24d0-4229-9d30-850f6b72ab43
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# 헤더 메뉴 확장

![헤더 메뉴 확장](./assets/header-menu/header-menu.png){align="center"}

헤더 메뉴가 포함된 확장에서는 다음과 같은 경우에 표시되는 AEM 콘텐츠 조각 콘솔의 헤더에 버튼을 도입합니다. __아니요__ 콘텐츠 조각이 선택되어 있습니다. 헤더 메뉴 확장 단추는 콘텐츠 조각을 선택하지 않은 경우에만 표시되므로 일반적으로 기존 콘텐츠 조각에 영향을 주지 않습니다. 대신 헤더 메뉴 확장은 일반적으로 다음과 같습니다.

+ 콘텐츠 참조를 통해 연결된 콘텐츠 조각 집합 생성과 같은 사용자 지정 논리를 사용하여 새 콘텐츠 조각을 만듭니다.
+ 지난 주에 만든 모든 콘텐츠 조각 내보내기와 같이 프로그래밍 방식으로 선택한 콘텐츠 조각 집합에 작동합니다.

## 확장 등록

`ExtensionRegistration.js` 는 AEM 확장의 진입점이며 다음을 정의합니다.

1. 확장 유형. 이 경우 헤더 메뉴 단추.
1. 확장 버튼의 정의, 위치 `getButton()` 함수.
1. 에서 단추의 클릭 처리기입니다. `onClick()` 함수.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## 양식

![양식](./assets/modal/modal.png)

AEM 콘텐츠 조각 콘솔 헤더 메뉴 확장에는 다음이 필요할 수 있습니다.

+ 원하는 작업을 수행하기 위한 사용자의 추가 입력입니다.
+ 작업 결과에 대한 사용자 세부 정보를 제공하는 기능입니다.

이러한 요구 사항을 지원하기 위해 AEM 콘텐츠 조각 콘솔 확장을 사용하여 React 애플리케이션으로 렌더링되는 사용자 지정 모달을 사용할 수 있습니다.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">모달 만들기로 건너뛰기</p>
      <p class="has-text-blackest">헤더 메뉴 확장 단추를 클릭할 때 표시되는 모달을 만드는 방법을 알아봅니다.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="모달을 작성하는 방법 알아보기">모달을 작성하는 방법 알아보기</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 모달 없음

경우에 따라 AEM 콘텐츠 조각 콘솔 헤더 메뉴 확장은 다음과 같이 사용자와 더 이상의 상호 작용이 필요하지 않습니다.

+ 가져오기 또는 내보내기와 같이 사용자 입력이 필요하지 않은 백엔드 프로세스 호출.
+ 콘텐츠 지침에 대한 내부 설명서와 같은 새 웹 페이지 열기

이러한 경우 AEM 콘텐츠 조각 콘솔 확장은 [모달](#modal)헤더 메뉴 단추에서 직접 작업을 수행할 수 있습니다. `onClick` 핸들러입니다.

AEM 콘텐츠 조각 콘솔 확장을 사용하면 작업이 수행되는 동안 진행 표시기가 AEM 콘텐츠 조각 콘솔에 오버레이되어 사용자가 추가 작업을 수행할 수 없습니다. 진행 표시기의 사용은 선택 사항이지만, 동기 작업의 진행 상황을 사용자에게 전달하는 데 유용합니다.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
