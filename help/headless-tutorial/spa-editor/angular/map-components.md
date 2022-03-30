---
title: AEM 구성 요소에 SPA 구성 요소 매핑 | AEM SPA 편집기 및 Angular 시작하기
description: AEM SPA Editor JS SDK를 사용하여 AEM(Adobe Experience Manager) 구성 요소에 Angular 구성 요소를 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 기존 AEM 작성과 유사하게 AEM SPA 편집기 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '2380'
ht-degree: 0%

---

# AEM 구성 요소에 SPA 구성 요소 매핑 {#map-components}

AEM SPA Editor JS SDK를 사용하여 AEM(Adobe Experience Manager) 구성 요소에 Angular 구성 요소를 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 기존 AEM 작성과 유사하게 AEM SPA 편집기 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다.

이 장에서는 AEM JSON 모델 API와 AEM 구성 요소에 의해 노출된 JSON 컨텐츠를 Angular 구성 요소에 자동으로 prop으로 주입하는 방법을 자세히 살펴봅니다.

## 목표

1. AEM 구성 요소를 SPA 구성 요소에 매핑하는 방법을 알아봅니다.
2. 차이점 이해 **컨테이너** 구성 요소 및 **컨텐츠** 구성 요소.
3. 기존 AEM 구성 요소에 매핑되는 새 Angular 구성 요소를 만듭니다.

## 빌드할 내용

이 장에서는 `Text` SPA 구성 요소가 AEM에 매핑됩니다 `Text`구성 요소. 새로운 `Image` SPA에서 사용하고 AEM에서 작성할 수 있는 SPA 구성 요소가 만들어집니다. 기본 제공되는 **레이아웃 컨테이너** 및 **템플릿 편집기** 모양새가 조금씩 다르다.

![장 샘플 최종 작성](./assets/map-components/final-page.png)

## 전제 조건

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   를 사용하는 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `Angular/map-components-solution`.

## 매핑 방법

기본 개념은 SPA 구성 요소를 AEM 구성 요소에 매핑하는 것입니다. AEM 구성 요소를 실행하고, 서버측을 실행하고, JSON 모델 API의 일부로 컨텐츠를 내보냅니다. JSON 콘텐츠는 브라우저에서 클라이언트측을 실행하는 SPA에 의해 사용됩니다. SPA 구성 요소와 AEM 구성 요소 간의 1:1 매핑이 만들어집니다.

![angular 구성 요소에 AEM 구성 요소 매핑에 대한 높은 수준의 개요](./assets/map-components/high-level-approach.png)

*angular 구성 요소에 AEM 구성 요소 매핑에 대한 높은 수준의 개요*

## 텍스트 구성 요소의 Inspect

다음 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype) 제공 `Text` AEM에 매핑된 구성 요소 [텍스트 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). 이것은 **콘텐츠** 구성 요소를 렌더링합니다. *콘텐츠* AEM에서 가져옵니다.

구성 요소가 어떻게 작동하는지 살펴보겠습니다.

### JSON 모델을 Inspect 합니다.

1. SPA 코드로 이동하기 전에 AEM에서 제공하는 JSON 모델을 이해하는 것이 중요합니다. 로 이동합니다 [코어 구성 요소 라이브러리](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) 텍스트 구성 요소의 페이지를 확인합니다. 코어 구성 요소 라이브러리는 모든 AEM 코어 구성 요소의 예를 제공합니다.
2. 을(를) 선택합니다 **JSON** 탭 - 예제 중 하나에 대해

   ![Text JSON model](./assets/map-components/text-json.png)

   다음 세 가지 속성이 표시됩니다. `text`, `richText`, 및 `:type`.

   `:type` 는 `sling:resourceType` AEM 구성 요소의 (또는 경로). 다음 값 `:type` 는 AEM 구성 요소를 SPA 구성 요소에 매핑하는 데 사용되는 것입니다.

   `text` and `richText` are additional properties that will be exposed to the SPA component.

### Inspect the Text component

1. 새 터미널을 열고 `ui.frontend` 폴더 아래에 표시됩니다. 실행 `npm install` 그리고 `npm start` 시작하려면 **웹 팩 개발 서버**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   The `ui.frontend` module is currently set up to use the [mock JSON model](./integrate-spa.md#mock-json).

2. 에 새 브라우저 창이 열려 있어야 합니다. [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![샘플 컨텐츠가 있는 웹 팩 개발 서버](assets/map-components/initial-start.png)

3. In the IDE of your choice open up the AEM Project for the WKND SPA. 를 확장합니다. `ui.frontend` 모듈 및 파일 열기 **text.component.ts** 아래에 `ui.frontend/src/app/components/text/text.component.ts`:

   ![Text.js Angular 구성 요소 소스 코드](assets/map-components/vscode-ide-text-js.png)

4. 검사할 첫 번째 영역은 `class TextComponent` ~35행:

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input()](https://angular.io/api/core/Input) decorator는 이전에 검토했던 매핑된 JSON 개체를 통해 값이 설정된 필드를 선언하는 데 사용됩니다.

   `@HostBinding('innerHtml') get content()` 는 다음 값에서 작성된 텍스트 컨텐츠를 표시하는 메서드입니다 `this.text`. 컨텐츠가 리치 텍스트(에 의해 결정됨)인 경우 `this.richText` 플래그) Angular의 기본 제공 보안이 무시됩니다. Angular [DomHidenzer](https://angular.io/api/platform-browser/DomSanitizer) 는 원시 HTML을 &quot;스크러빙&quot;하고 교차 사이트 스크립팅 취약점을 방지하는 데 사용됩니다. 메서드는 `innerHtml` 속성을 사용하여 [@HostBinding](https://angular.io/api/core/HostBinding) 장식가

5. 다음 검사 `TextEditConfig` ~24행:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   The above code is responsible for determining when to render the placeholder in the AEM author environment. 만약 `isEmpty` 메서드 반환 **true** 그러면 자리 표시자가 렌더링됩니다.

6. 마지막으로 `MapTo` ~line 53에서 호출:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** is provided by the AEM SPA Editor JS SDK (`@adobe/cq-angular-editable-components`). 경로 `wknd-spa-angular/components/text` 는 를 나타냅니다 `sling:resourceType` AEM 구성 요소의 일부입니다. 이 경로는 `:type` 이전에 살펴본 JSON 모델에 의해 노출됩니다. **MapTo** 는 JSON 모델 응답을 구문 분석하고 올바른 값을 `@Input()` SPA 구성 요소의 변수입니다.

   AEM을 찾을 수 있습니다 `Text` 구성 요소 정의 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. 을 수정하여 실험 **en.model.json** 파일 위치 `ui.frontend/src/mocks/json/en.model.json`.

   ~62행에서 첫 번째 업데이트 `Text` 사용할 값 **`H1`** 및 **`u`** 태그:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   브라우저로 돌아가 **웹 팩 개발 서버**:

   ![업데이트된 텍스트 모델](assets/map-components/updated-text-model.png)

   토글링 시도 `richText` 속성 사이의 **true** / **false** 렌더링 로직을 확인할 수 있습니다.

8. Inspect **text.component.html** at `ui.frontend/src/app/components/text/text.component.html`.

   구성 요소의 전체 컨텐츠가 `innerHTML` 속성을 사용합니다.

9. Inspect **app.module.ts** at `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   다음 **텍스트 구성 요소** 는 명시적으로 포함되지 않지만, 보다 동적으로 **AEMRresponsiveGridComponent** SPA Editor JS SDK에서 제공합니다. 따라서 다음과 같이 나열해야 합니다. **app.module.ts**` [entryComponents](https://angular.io/guide/entry-components) 배열입니다.

## 이미지 구성 요소 만들기

다음으로, `Image` AEM에 매핑된 angular 구성 요소 [이미지 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). 다음 `Image` 구성 요소는 **콘텐츠** 구성 요소.

### JSON의 Inspect

SPA 코드로 이동하기 전에 AEM에서 제공하는 JSON 모델을 검사하십시오.

1. 로 이동합니다 [코어 구성 요소 라이브러리의 이미지 예](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![이미지 코어 구성 요소 JSON](./assets/map-components/image-json.png)

   속성 `src`, `alt`, 및 `title` 는 SPA을 채우는 데 사용됩니다. `Image` 구성 요소.

   >[!NOTE]
   >
   > 다른 이미지 속성이 노출되어 있습니다(`lazyEnabled`, `widths`)을 클릭하여 개발자가 적응형 및 지연 로드 구성 요소를 만들 수 있습니다. 이 자습서에 포함된 구성 요소는 간단하며 **not** 이러한 고급 속성을 사용합니다.

2. IDE로 돌아가서 `en.model.json` at `ui.frontend/src/mocks/json/en.model.json`. 이 구성 요소는 프로젝트용 net-new 구성 요소이므로 이미지 JSON을 &quot;mock&quot;해야 합니다.

   ~70줄에서 `image` 모델(후행 쉼표에 대해 잊지 않음) `,` 두 번째 `text_386303036`) 및 를 업데이트합니다. `:itemsOrder` 배열입니다.

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   프로젝트에는 샘플 이미지가 포함되어 있습니다. `/mock-content/adobestock-140634652.jpeg` 와 함께 사용됩니다 **웹 팩 개발 서버**.

   You can view the full [en.model.json here](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. 구성 요소에서 표시할 스톡 사진을 추가합니다.

   이름이 인 새 폴더 만들기 **이미지** 아래 `ui.frontend/src/mocks`. 다운로드 [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) 새로 만든 **이미지** 폴더를 입력합니다. 원하는 경우 자유롭게 자신의 이미지를 사용할 수 있습니다.

### 이미지 구성 요소 구현

1. 를 중지합니다. **웹 팩 개발 서버** 시작하는 경우
2. angular CLI를 실행하여 새 이미지 구성 요소 생성 `ng generate component` 내에서 명령 `ui.frontend` 폴더:

   ```shell
   $ ng generate component components/image
   ```

3. IDE에서 **image.component.ts** at `ui.frontend/src/app/components/image/image.component.ts` 및 를 다음과 같이 업데이트합니다.

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` 는 AEM에서 작성자 자리 표시자를 렌더링할지 여부를 결정하는 구성입니다. `src` 속성이 채워집니다.

   `@Input()` 의 `src`, `alt`, 및 `title` 는 JSON API에서 매핑된 속성입니다.

   `hasImage()` 는 이미지를 렌더링해야 하는지 여부를 결정하는 메서드입니다.

   `MapTo` SPA 구성 요소를 의 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. 열기 **image.component.html** 다음과 같이 업데이트합니다.

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   이 경우 `<img>` 요소인 경우 `hasImage` 반환 **true**.

5. 열기 **image.component.scss** 다음과 같이 업데이트합니다.

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > 다음 `:host-context` 규칙: **중요** AEM SPA 편집기 자리 표시자가 올바르게 작동하도록 합니다. AEM 페이지 편집기에서 작성되기 위한 모든 SPA 구성 요소는 최소한으로 이 규칙이 필요합니다.

6. Open `app.module.ts` and add the `ImageComponent` to the `entryComponents` array:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Like the `TextComponent`, the `ImageComponent` is dynamically loaded, and must be included in the `entryComponents` array.

7. 시작 **웹 팩 개발 서버** 다음을 참조하십시오 `ImageComponent` 렌더링.

   ```shell
   $ npm run start:mock
   ```

   ![이미지에 이미지가 추가되었습니다](assets/map-components/image-added-mock.png)

   *SPA에 추가된 이미지*

   >[!NOTE]
   >
   > **Bonus challenge**: Implement a new method to display the value of `title` as a caption beneath the image.

## AEM에서 정책 업데이트

The `ImageComponent` component is only visible in the **webpack dev server**. Next, deploy the updated SPA to AEM and update the template policies.

1. 를 중지합니다. **웹 팩 개발 서버** 그리고 **루트** 프로젝트 중 Maven 기술을 사용하여 AEM에 변경 사항을 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. AEM 시작 화면에서 로 이동합니다. **[!UICONTROL 도구]** > **[!UICONTROL 템플릿]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   을(를) 선택하고 편집합니다 **SPA 페이지**:

   ![Edit SPA Page Template](assets/map-components/edit-spa-page-template.png)

3. Select the **Layout Container** and click it&#39;s **policy** icon to edit the policy:

   ![Layout Container Policy](./assets/map-components/layout-container-policy.png)

4. 아래 **허용된 구성 요소** > **WKND SPA Angular - 컨텐츠** > check **이미지** 구성 요소:

   ![이미지 구성 요소 선택](assets/map-components/check-image-component.png)

   Under **Default Components** > **Add mapping** and choose the **Image - WKND SPA Angular - Content** component:

   ![Set default components](assets/map-components/default-components.png)

   을(를) 입력합니다. **mime 유형** 의 `image/*`.

   클릭 **완료** 정책 업데이트를 저장하려면

5. 에서 **레이아웃 컨테이너** 를 클릭합니다. **정책** 아이콘 **텍스트** 구성 요소:

   ![텍스트 구성 요소 정책 아이콘](./assets/map-components/edit-text-policy.png)

   이름이 인 새 정책 만들기 **WKND SPA 텍스트**. 아래 **Plugins** > **서식** > 모든 상자를 선택하여 추가 서식 옵션을 활성화합니다.

   ![RTE 서식 사용](assets/map-components/enable-formatting-rte.png)

   아래 **Plugins** > **단락 스타일** > 확인란을 선택합니다. **단락 스타일 활성화**:

   ![단락 스타일 사용](./assets/map-components/text-policy-enable-paragraphstyles.png)

   클릭 **완료** 정책 업데이트를 저장하려면

6. 로 이동합니다 **홈페이지** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   또한 `Text` 구성 요소 및 추가 단락 스타일 추가 **전체 화면** 모드.

   ![전체 화면 리치 텍스트 편집](assets/map-components/full-screen-rte.png)

7. 이미지를 **자산 파인더**:

   ![이미지 드래그 앤 드롭](./assets/map-components/drag-drop-image.gif)

8. 를 통해 자체 이미지 추가 [AEM Assets](http://localhost:4502/assets.html/content/dam) 또는 standard용 완료된 코드 베이스를 설치하거나 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest). 다음 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest) 에는 WKND SPA에서 다시 사용할 수 있는 많은 이미지가 포함되어 있습니다. 패키지는 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp).

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

## 레이아웃 컨테이너 Inspect

지원 대상 **레이아웃 컨테이너** 는 AEM SPA Editor SDK에서 자동으로 제공됩니다. 다음 **레이아웃 컨테이너**&#x200B;로 표시된 대로 는 입니다. **컨테이너** 구성 요소. 컨테이너 구성 요소는 다음을 나타내는 JSON 구조를 허용하는 구성 요소입니다 *기타* 구성 요소를 동적으로 인스턴스화하고 생성할 수 있습니다.

레이아웃 컨테이너를 더 살펴보겠습니다.

1. IDE에서 **responsive-grid.component.ts** at `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   다음 `AEMResponsiveGridComponent` 는 AEM SPA Editor SDK의 일부로 구현되었으며 을 통해 프로젝트에 포함됩니다 `import-components`.

2. 브라우저에서 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON 모델 API - 응답형 그리드](./assets/map-components/responsive-grid-modeljson.png)

   다음 **레이아웃 컨테이너** 구성 요소에 `sling:resourceType` 의 `wcm/foundation/components/responsivegrid` 및 는 `:type` 속성, `Text` 및 `Image` 구성 요소.

   구성 요소를 사용하여 다시 크기 조정하는 동일한 기능 [레이아웃 모드](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) SPA 편집기에서 사용할 수 있습니다.

3. 로 돌아가기 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). 추가 **이미지** 구성 요소를 사용하여 크기 조정을 다시 시도하십시오 **레이아웃** 옵션:

   ![레이아웃 모드를 사용한 이미지 크기 조정](./assets/map-components/responsive-grid-layout-change.gif)

4. JSON 모델 다시 열기 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) 그리고 `columnClassNames` json의 일부로:

   ![열 클래스 이름](./assets/map-components/responsive-grid-classnames.png)

   클래스 이름 `aem-GridColumn--default--4` 구성 요소가 12열 격자를 기반으로 하여 4열 너비여야 함을 나타냅니다. 에 대한 자세한 내용 [응답형 그리드는 여기에서 찾을 수 있습니다.](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. IDE로 돌아가서 `ui.apps` 모듈에는 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. 파일을 엽니다. `less/grid.less`.

   이 파일은 중단점(`default`, `tablet`, 및 `phone`)에서 사용할 수 있습니다. **레이아웃 컨테이너**. 이 파일은 프로젝트 사양에 따라 사용자 지정되기 위한 것입니다. 현재 중단점이 `1200px` 및 `650px`.

6. 반응형 기능과 업데이트된 리치 텍스트 정책을 `Text` 구성 요소를 사용하여 다음과 같은 보기를 작성합니다.

   ![장 샘플 최종 작성](assets/map-components/final-page.png)

## 축하합니다! {#congratulations}

축하합니다. AEM 구성 요소에 SPA 구성 요소를 매핑하고 새 구성 요소를 구현하는 방법을 알아보았습니다 `Image` 구성 요소. 또한, **레이아웃 컨테이너**.

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `Angular/map-components-solution`.

### 다음 단계 {#next-steps}

[탐색 및 라우팅](navigation-routing.md) - SPA Editor SDK를 사용하여 AEM 페이지에 매핑하여 SPA에서 여러 개의 보기를 지원하는 방법을 알아봅니다. Dynamic navigation is implemented using Angular Router and added to an existing Header component.

## 보너스 - 소스 제어에 구성 유지 {#bonus}

대부분의 경우, 특히 AEM 프로젝트 시작 시 템플릿 및 관련 컨텐츠 정책과 같은 구성을 소스 제어에 유지하는 것이 중요합니다. 이렇게 하면 모든 개발자가 동일한 컨텐츠 및 구성 세트에 대해 작업하고 있으므로 환경 간에 추가적인 일관성을 유지할 수 있습니다. Once a project reaches a certain level of maturity, the practice of managing templates can be turned over to a special group of power users.

다음 몇 단계는 Visual Studio 코드 IDE를 사용하여 수행됩니다. [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 그러나 사용자가 구성했던 모든 도구 및 IDE를 사용하여 **가져오기** 또는 **가져오기** AEM의 로컬 인스턴스에 있는 컨텐츠입니다.

1. Visual Studio 코드 IDE에서 **VSCode AEM 동기화** marketplace 확장을 통해 설치:

   ![VSCode AEM 동기화](./assets/map-components/vscode-aem-sync.png)

2. 를 확장합니다. **ui.content** 프로젝트 탐색기에서 `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **마우스 오른쪽 단추 클릭** a `templates` 폴더를 선택하고 **AEM 서버에서 가져오기**:

   ![VSCode 가져오기 템플릿](assets/map-components/import-aem-servervscode.png)

4. 단계를 반복하여 컨텐츠를 가져오지만 **정책** 폴더 위치 `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspect the `filter.xml` file located at `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   The `filter.xml` file is responsible for identifying the paths of nodes that will be installed with the package. Notice the `mode="merge"` on each of the filters which indicates that existing content will not be modified, only new content is added. Since content authors may be updating these paths, it is important that a code deployment does **not** overwrite content. See the [FileVault documentation](https://jackrabbit.apache.org/filevault/filter.html) for more details on working with filter elements.

   비교 `ui.content/src/main/content/META-INF/vault/filter.xml` 및 `ui.apps/src/main/content/META-INF/vault/filter.xml` 각 모듈에서 관리하는 서로 다른 노드를 이해하기 위해
