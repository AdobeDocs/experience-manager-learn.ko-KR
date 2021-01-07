---
title: SPA 구성 요소를 AEM 구성 요소에 매핑 | AEM SPA 편집기 및 각도 시작하기
description: AEM SPA Editor JS SDK를 사용하여 Angular 구성 요소를 Adobe Experience Manager(AEM) 구성 요소에 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 AEM SPA Editor 내에서 SPA 구성 요소에 대한 동적 업데이트를 일반적인 AEM 작성과 유사합니다.
sub-product: 사이트
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
translation-type: tm+mt
source-git-commit: 28b5522e094a41d81116acb923dc0390478e2308
workflow-type: tm+mt
source-wordcount: '2387'
ht-degree: 1%

---


# SPA 구성 요소를 AEM 구성 요소 {#map-components}에 매핑

AEM SPA Editor JS SDK를 사용하여 Angular 구성 요소를 Adobe Experience Manager(AEM) 구성 요소에 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 AEM SPA Editor 내에서 SPA 구성 요소에 대한 동적 업데이트를 일반적인 AEM 작성과 유사합니다.

이 장에서는 AEM JSON 모델 API에 대해 자세히 설명하고 AEM 구성 요소에 의해 노출된 JSON 컨텐츠를 Angular 구성 요소에 prop으로 자동으로 주입하는 방법을 살펴봅니다.

## 목표

1. AEM 구성 요소를 SPA 구성 요소에 매핑하는 방법을 알아봅니다.
2. **Container** 구성 요소와 **Content** 구성 요소 간의 차이점을 이해합니다.
3. 기존 AEM 구성 요소에 매핑되는 새 각도 구성 요소를 만듭니다.

## 구축 분야

이 장에서는 제공된 `Text` SPA 구성 요소가 AEM `Text` 구성 요소에 매핑되는 방식을 검사합니다. SPA에서 사용할 수 있고 AEM에서 작성할 수 있는 새 `Image` SPA 구성 요소가 만들어집니다. **레이아웃 컨테이너** 및 **템플릿 편집기** 정책의 기본 기능을 사용하여 모양에 좀 더 다양한 보기를 만들 수도 있습니다.

![장 샘플 최종 저작](./assets/map-components/final-page.png)

## 전제 조건

[로컬 개발 환경 설정](overview.md#local-dev-environment)에 대한 필수 도구 및 지침을 검토하십시오.

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Maven을 사용하여 코드 베이스를 로컬 AEM 인스턴스에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)을 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution)에서 완료된 코드를 보거나 분기 `Angular/map-components-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## 매핑 방법

기본 개념은 SPA 구성 요소를 AEM 구성 요소에 매핑하는 것입니다. AEM 구성 요소, 서버측 실행, JSON 모델 API의 일부로 컨텐츠 내보내기 JSON 콘텐츠는 SPA이 브라우저에서 클라이언트측을 실행하여 사용합니다. SPA 구성 요소와 AEM 구성 요소 간 1:1 매핑이 만들어집니다.

![AEM 구성 요소를 각 구성 요소에 매핑하는 높은 수준의 개요](./assets/map-components/high-level-approach.png)

*AEM 구성 요소를 각 구성 요소에 매핑하는 높은 수준의 개요*

## Inspect the Text Component

[AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype)은 AEM [텍스트 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/text.html)에 매핑되는 `Text` 구성 요소를 제공합니다. AEM에서 *content*&#x200B;를 렌더링하기 위한 **content** 구성 요소의 예입니다.

구성 요소의 작동 방식을 살펴보겠습니다.

### Inspect JSON 모델

1. SPA 코드로 이동하기 전에 AEM에서 제공하는 JSON 모델을 이해하는 것이 중요합니다. [핵심 구성 요소 라이브러리](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)로 이동하고 텍스트 구성 요소에 대한 페이지를 봅니다. 핵심 구성 요소 라이브러리는 모든 AEM 코어 구성 요소의 예를 제공합니다.
2. 다음 예 중 하나에 대해 **JSON** 탭을 선택합니다.

   ![텍스트 JSON 모델](./assets/map-components/text-json.png)

   다음 세 가지 속성이 표시됩니다.`text`, `richText` 및 `:type`.

   `:type` 는 AEM 구성 요소의  `sling:resourceType` (또는 경로)를 나열하는 예약된 속성입니다. `:type` 값은 AEM 구성 요소를 SPA 구성 요소에 매핑하는 데 사용되는 값입니다.

   `text` spa  `richText` 구성 요소에 노출되는 추가 속성입니다.

### Inspect the Text 구성 요소

1. 새 터미널을 열고 프로젝트 내의 `ui.frontend` 폴더로 이동합니다. `npm install`을(를) 실행한 다음 `npm start`을(를) 실행하여 **webpack dev server**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   `ui.frontend` 모듈은 현재 [모의 JSON 모델](./integrate-spa.md#mock-json)을 사용하도록 설정되어 있습니다.

2. [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)에 새 브라우저 창이 열려 있는 것이 보입니다.

   ![Mock 컨텐츠가 포함된 Webpack 개발 서버](assets/map-components/initial-start.png)

3. 원하는 IDE에서 WKND SPA용 AEM 프로젝트를 엽니다. `ui.frontend` 모듈을 확장하고 `ui.frontend/src/app/components/text/text.component.ts` 아래의 **text.component.ts** 파일을 엽니다.

   ![Text.js 각 구성 요소 소스 코드](assets/map-components/vscode-ide-text-js.png)

4. 검사할 첫 번째 영역은 ~line 35의 `class TextComponent`입니다.

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

   [@Input()](https://angular.io/api/core/Input) decorator는 앞에서 검토한 매핑된 JSON 개체를 통해 값이 설정된 필드를 선언하는 데 사용됩니다.

   `@HostBinding('innerHtml') get content()` 은 의 값에서 제작된 텍스트 컨텐츠를 표시하는  `this.text`메서드입니다. 내용이 리치 텍스트인 경우(`this.richText` 플래그를 통해 결정) Angular의 내장 보안을 건너뜁니다. Angular의 [DomWidden](https://angular.io/api/platform-browser/DomSanitizer)은 원시 HTML을 &quot;스크러빙&quot;하고 크로스 사이트 스크립팅 취약점을 방지하는 데 사용됩니다. 이 메서드는 [@HostBinding](https://angular.io/api/core/HostBinding) 장식기를 사용하여 `innerHtml` 속성에 바인딩됩니다.

5. 다음 검사: ~line 24의 `TextEditConfig`:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   위의 코드는 AEM 작성 환경에서 자리 표시자를 렌더링할 시기를 결정합니다. `isEmpty` 메서드가 **true**&#x200B;를 반환하면 자리 표시자가 렌더링됩니다.

6. 마지막으로 ~line 53의 `MapTo` 호출을 확인합니다.

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **** MapTois는 AEM SPA Editor JS SDK(`@adobe/cq-angular-editable-components`)에서 제공합니다. 경로 `wknd-spa-angular/components/text`은 AEM 구성 요소의 `sling:resourceType`을 나타냅니다. 이 경로는 이전에 관찰된 JSON 모델에 의해 노출된 `:type`과 일치합니다. **MapMap** JSON 모델 응답을 표시하고 올바른 값을 SPA 구성 요소의  `@Input()` 변수에 전달합니다.

   `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`에서 AEM `Text` 구성 요소 정의를 찾을 수 있습니다.

7. `ui.frontend/src/mocks/json/en.model.json`에서 **en.model.json** 파일을 수정하여 실험해 봅니다.

   ~line 62에서 **`H1`** 및 **`u`** 태그를 사용하도록 첫 번째 `Text` 값을 업데이트합니다.

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   브라우저로 돌아가 **webpack dev server**&#x200B;에서 제공하는 효과를 확인합니다.

   ![업데이트된 텍스트 모델](assets/map-components/updated-text-model.png)

   렌더링 논리를 보려면 **true** / **false** 사이에 `richText` 속성을 전환하십시오.

8. Inspect **text.component.html**(`ui.frontend/src/app/components/text/text.component.html`에 있음)

   구성 요소의 전체 내용이 `innerHTML` 속성으로 설정되므로 이 파일은 비어 있습니다.

9. Inspect은 `ui.frontend/src/app/app.module.ts`에 있는 **app.module.ts**&#x200B;를 선택합니다.

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

   **TextComponent**&#x200B;은(는) 명시적으로 포함되어 있지 않지만 AEM SPA Editor JS SDK에서 제공하는 **AEMRescentiveGridComponent**&#x200B;를 통해 동적으로 포함됩니다. 따라서 **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components) 배열에 나열되어야 합니다.

## 이미지 구성 요소 만들기

다음으로 AEM [이미지 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/image.html)에 매핑되는 `Image` 각도 구성 요소를 만듭니다. `Image` 구성 요소는 **content** 구성 요소의 또 다른 예입니다.

### JSON을 위한 Inspect

SPA 코드로 이동하기 전에 AEM에서 제공하는 JSON 모델을 검사하십시오.

1. 핵심 구성 요소 라이브러리](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)의 [이미지 예로 이동합니다.

   ![이미지 코어 구성 요소 JSON](./assets/map-components/image-json.png)

   `src`, `alt` 및 `title`의 속성은 SPA `Image` 구성 요소를 채우는 데 사용됩니다.

   >[!NOTE]
   >
   > 개발자가 적응형 및 레이지 로드 구성 요소를 만들 수 있도록 하는 노출 이미지 속성(`lazyEnabled`, `widths`)이 있습니다. 이 자습서에 내장된 구성 요소는 간단하며 **이(가) 이러한 고급 속성을 사용하지 않습니다.**

2. IDE로 돌아가 `en.model.json`에서 `ui.frontend/src/mocks/json/en.model.json`을 엽니다. 이 구성 요소는 프로젝트에 대한 순 새 구성 요소이므로 이미지 JSON을 &quot;시크&quot;해야 합니다.

   ~line 70에서 `image` 모델에 대한 JSON 항목을 추가합니다(두 번째 `text_386303036` 뒤에 오는 쉼표 `,`에 대해 잊지 말고). 그리고 `:itemsOrder` 배열을 업데이트하십시오.

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

   프로젝트는 **webpack dev server**&#x200B;와 함께 사용할 `/mock-content/adobestock-140634652.jpeg`의 샘플 이미지를 포함합니다.

   전체 [en.model.json은 ](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json)에서 볼 수 있습니다.

3. 구성 요소에서 표시할 스톡 사진을 추가합니다.

   `ui.frontend/src/mocks` 아래에 **images**&#x200B;라는 새 폴더를 만듭니다. [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg)을 다운로드하고 새로 만든 **이미지** 폴더에 배치합니다. 원하는 경우 자신의 이미지를 자유롭게 사용할 수 있습니다.

### 이미지 구성 요소 구현

1. 시작하는 경우 **webpack dev server**&#x200B;를 중지합니다.
2. `ui.frontend` 폴더 내에서 Angular CLI `ng generate component` 명령을 실행하여 새 이미지 구성 요소를 만듭니다.

   ```shell
   $ ng generate component components/image
   ```

3. IDE에서 **image.component.ts**&#x200B;를 `ui.frontend/src/app/components/image/image.component.ts`에서 열고 다음과 같이 업데이트합니다.

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

   `ImageEditConfig` 는 속성이 채워지는지 여부를 기준으로 AEM에서 작성자 자리 표시자를 렌더링할지 여부를 결정하는  `src` 구성입니다.

   `@Input()` JSON API `src`에서 매핑되는 속성 `alt`과  `title` 같습니다.

   `hasImage()` 이미지를 렌더링할지 여부를 결정하는 방법입니다.

   `MapTo` 의 AEM 구성 요소에 SPA 구성 요소를 매핑합니다 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. **image.component.html**&#x200B;을 열고 다음과 같이 업데이트합니다.

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   `hasImage`이 **true**&#x200B;를 반환하면 `<img>` 요소가 렌더링됩니다.

5. **image.component.scss**&#x200B;을 열고 다음과 같이 업데이트합니다.

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
   > AEM SPA 편집기 자리 표시자가 제대로 작동하려면 `:host-context` 규칙은 **중요**&#x200B;입니다. AEM 페이지 편집기에서 작성하려는 모든 SPA 구성 요소에는 이 규칙이 최소한으로 필요합니다.

6. `app.module.ts`을 열고 `ImageComponent`을 `entryComponents` 배열에 추가합니다.

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   `TextComponent`과 마찬가지로 `ImageComponent`도 동적으로 로드되며 `entryComponents` 배열에 포함되어야 합니다.

7. `ImageComponent` 렌더링을 보려면 **webpack dev server**&#x200B;를 시작합니다.

   ```shell
   $ npm run start:mock
   ```

   ![이미지에 추가된 이미지](assets/map-components/image-added-mock.png)

   *SPA에 추가된 이미지*

   >[!NOTE]
   >
   > **보너스 당면 과제**:새 메서드를 구현하여 이미지 아래에 있는 캡션 `title` 으로 값을 표시합니다.

## AEM에서 정책 업데이트

`ImageComponent` 구성 요소는 **webpack dev server**&#x200B;에만 표시됩니다. 다음으로 업데이트된 SPA을 AEM에 배포하고 템플릿 정책을 업데이트합니다.

1. **webpack 개발 서버**&#x200B;를 중지하고 프로젝트의 **루트**&#x200B;에서 Maven 기술을 사용하여 AEM에 변경 내용을 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. AEM 시작 화면에서 **[!UICONTROL 도구]** > **[!UICONTROL 템플릿]** > **[WKND SPA 각도](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**&#x200B;로 이동합니다.

   **SPA 페이지**&#x200B;를 선택하고 편집합니다.

   ![SPA 페이지 템플릿 편집](assets/map-components/edit-spa-page-template.png)

3. **레이아웃 컨테이너**&#x200B;를 선택하고 **policy** 아이콘을 클릭하여 정책을 편집합니다.

   ![레이아웃 컨테이너 정책](./assets/map-components/layout-container-policy.png)

4. **허용된 구성 요소** > **WKND SPA 각도 - 컨텐츠** > **이미지** 구성 요소 선택:

   ![선택한 이미지 구성 요소](assets/map-components/check-image-component.png)

   **기본 구성 요소** > **매핑 추가**&#x200B;에서 **이미지 - WKND SPA 각도 - 컨텐츠** 구성 요소를 선택합니다.

   ![기본 구성 요소 설정](assets/map-components/default-components.png)

   `image/*`의 **mime 유형**&#x200B;을 입력합니다.

   정책 업데이트를 저장하려면 **완료**&#x200B;를 클릭합니다.

5. **레이아웃 컨테이너**&#x200B;에서 **텍스트** 구성 요소에 대한 **정책** 아이콘을 클릭합니다.

   ![텍스트 구성 요소 정책 아이콘](./assets/map-components/edit-text-policy.png)

   **WKND SPA Text**&#x200B;이라는 새 정책을 만듭니다. **플러그인** > **서식 지정** > 추가 서식 옵션을 활성화하려면 모든 상자를 선택합니다.

   ![RTE 서식 사용](assets/map-components/enable-formatting-rte.png)

   **플러그인** > **단락 스타일** > **단락 스타일 사용**:

   ![단락 스타일 사용](./assets/map-components/text-policy-enable-paragraphstyles.png)

   정책 업데이트를 저장하려면 **완료**&#x200B;를 클릭합니다.

6. **홈 페이지** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)로 이동합니다.

   `Text` 구성 요소를 편집하고 **전체 화면** 모드에서 추가 단락 스타일을 추가할 수도 있습니다.

   ![전체 화면 리치 텍스트 편집](assets/map-components/full-screen-rte.png)

7. **자산 파인더**&#x200B;에서 이미지를 드래그 앤 드롭할 수도 있습니다.

   ![이미지 드래그하여 놓기](./assets/map-components/drag-drop-image.gif)

8. [AEM Assets](http://localhost:4502/assets.html/content/dam)을(를) 통해 자신의 이미지를 추가하거나 표준 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에 대해 완료된 코드 베이스를 설치합니다. [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에는 WKND SPA에서 다시 사용할 수 있는 많은 이미지가 포함되어 있습니다. [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 패키지를 설치할 수 있습니다.

   ![Package Manager 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect the Layout Container

**레이아웃 컨테이너**&#x200B;에 대한 지원은 AEM SPA Editor SDK에서 자동으로 제공됩니다. 이름으로 표시된 **레이아웃 컨테이너**&#x200B;는 **컨테이너** 구성 요소입니다. 컨테이너 구성 요소는 *기타* 구성 요소를 나타내며 동적으로 인스턴스화하는 JSON 구조를 허용하는 구성 요소입니다.

레이아웃 컨테이너를 더 자세히 살펴보죠.

1. IDE에서 **responsive-grid.component.ts**&#x200B;를 엽니다.`ui.frontend/src/app/components/responsive-grid`

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   `AEMResponsiveGridComponent`은(는) AEM SPA Editor SDK의 일부로 구현되며 `import-components`을(를) 통해 프로젝트에 포함됩니다.

2. 브라우저에서 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)로 이동합니다.

   ![JSON 모델 API - 반응형 격자](./assets/map-components/responsive-grid-modeljson.png)

   **레이아웃 컨테이너** 구성 요소에는 `wcm/foundation/components/responsivegrid`의 `sling:resourceType`가 있으며 `Text` 및 `Image` 구성 요소처럼 `:type` 속성을 사용하여 SPA 편집기에서 인식됩니다.

   SPA Editor에서 [레이아웃 모드](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode)를 사용하여 구성 요소의 크기를 다시 지정하는 것과 동일한 기능을 사용할 수 있습니다.

3. [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)로 돌아갑니다. **이미지** 구성 요소를 추가하고 **레이아웃** 옵션을 사용하여 크기를 다시 조정해 보십시오.

   ![레이아웃 모드를 사용하여 이미지 크기 다시 조정](./assets/map-components/responsive-grid-layout-change.gif)

4. JSON 모델 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)을 다시 열고 JSON의 일부로 `columnClassNames`을 관찰합니다.

   ![열 클래스 이름](./assets/map-components/responsive-grid-classnames.png)

   클래스 이름 `aem-GridColumn--default--4`은 구성 요소가 12개의 열 격자를 기준으로 4개의 열 너비여야 함을 나타냅니다. [응답형 격자에 대한 자세한 내용은 ](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)에서 확인할 수 있습니다.

5. IDE로 돌아가고 `ui.apps` 모듈에는 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`에 정의된 클라이언트측 라이브러리가 있습니다. `less/grid.less` 파일을 엽니다.

   이 파일은 **레이아웃 컨테이너**&#x200B;에 사용되는 중단점(`default`, `tablet` 및 `phone`)을 결정합니다. 이 파일은 프로젝트 사양에 따라 맞춤화하기 위해 만들어졌습니다. 현재 중단점은 `1200px` 및 `650px`로 설정됩니다.

6. `Text` 구성 요소의 응답형 기능과 업데이트된 리치 텍스트 정책을 사용하여 다음과 같은 뷰를 작성할 수 있어야 합니다.

   ![장 샘플 최종 저작](assets/map-components/final-page.png)

## 축하합니다!{#congratulations}

축하합니다. SPA 구성 요소를 AEM 구성 요소에 매핑하는 방법을 알아냈고 새 `Image` 구성 요소를 구현했습니다. 또한 **레이아웃 컨테이너**&#x200B;의 응답형 기능을 살펴볼 수 있습니다.

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution)에서 완료된 코드를 보거나 분기 `Angular/map-components-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

### 다음 단계 {#next-steps}

[탐색 및 라우팅](navigation-routing.md)  - SPA Editor SDK를 사용하여 AEM 페이지에 매핑하여 SPA에서 여러 보기를 지원하는 방법을 알아봅니다. 동적 탐색은 각도 라우터를 사용하여 구현되고 기존 헤더 구성 요소에 추가됩니다.

## 추가 - 소스 제어 {#bonus}(으)로 구성 유지

대부분의 경우, 특히 AEM 프로젝트를 시작할 때는 템플릿 및 관련 컨텐츠 정책과 같은 구성을 소스 제어에 유지하는 것이 중요합니다. 이렇게 하면 모든 개발자가 동일한 컨텐츠 및 구성 세트를 사용하여 작업하고 환경 간에 추가적인 일관성을 유지할 수 있습니다. 프로젝트가 특정 수준의 성숙도에 도달하면 템플릿 관리 방식을 고급 사용자 그룹으로 전환할 수 있습니다.

다음 몇 단계는 Visual Studio 코드 IDE와 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)을 사용하여 수행되지만 **pull** 또는 **AEM의 로컬 인스턴스에서** 내용을 가져오기 위해 구성한 모든 도구와 IDE를 사용할 수 있습니다.

1. Visual Studio 코드 IDE에서 Marketplace 확장을 통해 **VSCode AEM 동기화**&#x200B;가 설치되어 있는지 확인합니다.

   ![VSCode AEM 동기화](./assets/map-components/vscode-aem-sync.png)

2. 프로젝트 탐색기에서 **ui.content** 모듈을 확장하고 `/conf/wknd-spa-angular/settings/wcm/templates`로 이동합니다.

3. **폴더를** 마우스 오른쪽 단추로  `templates` 클릭하고  **AEM 서버에서 가져오기**:

   ![VSCode 가져오기 템플릿](assets/map-components/import-aem-servervscode.png)

4. 내용을 가져오려면 단계를 반복하지만 `/conf/wknd-spa-angular/settings/wcm/policies`에 있는 **policies** 폴더를 선택합니다.

5. Inspect은 `ui.content/src/main/content/META-INF/vault/filter.xml`에 있는 `filter.xml` 파일을 반환합니다.

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

   `filter.xml` 파일은 패키지와 함께 설치할 노드의 경로를 식별합니다. 기존 컨텐츠가 수정되지 않고 새 컨텐츠만 추가됨을 나타내는 각 필터에 `mode="merge"`이 표시됩니다. 컨텐츠 작성자는 이러한 경로를 업데이트할 수 있으므로 코드 배포에서는 **이 컨텐츠를 덮어쓰지 않는**&#x200B;이(가) 있어야 합니다. 필터 요소 작업에 대한 자세한 내용은 [FileVault 설명서](https://jackrabbit.apache.org/filevault/filter.html)를 참조하십시오.

   각 모듈에서 관리하는 다른 노드를 이해하려면 `ui.content/src/main/content/META-INF/vault/filter.xml` 및 `ui.apps/src/main/content/META-INF/vault/filter.xml`을 비교하십시오.
