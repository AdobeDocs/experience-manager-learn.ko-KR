---
title: 탐색 및 라우팅 추가 | AEM SPA 편집기 및 Angular 시작하기
description: AEM 페이지 및 SPA Editor SDK를 사용하여 SPA에서 여러 개의 보기가 지원되는 방법을 알아봅니다. 동적 탐색은 Angular 경로를 사용하여 구현되고 기존 헤더 구성 요소에 추가됩니다.
sub-product: 사이트
feature: SPA 편집기
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: bf9ab30f57faa23721d7d27b837d8e0f0e8cf4f1
workflow-type: tm+mt
source-wordcount: '2723'
ht-degree: 1%

---


# 탐색 및 라우팅 추가 {#navigation-routing}

AEM 페이지 및 SPA Editor SDK를 사용하여 SPA에서 여러 개의 보기가 지원되는 방법을 알아봅니다. 동적 탐색은 Angular 경로를 사용하여 구현되고 기존 헤더 구성 요소에 추가됩니다.

## 목표

1. SPA 편집기를 사용할 때 사용할 수 있는 SPA 모델 라우팅 옵션을 이해합니다.
2. [Angular 라우팅](https://angular.io/guide/router)을 사용하여 SPA의 다른 보기 사이를 탐색하는 방법을 알아봅니다.
3. AEM 페이지 계층 구조에 의해 구동되는 동적 탐색을 구현합니다.

## 빌드할 내용

이 장은 기존 `Header` 구성 요소에 탐색 메뉴를 추가합니다. 탐색 메뉴는 AEM 페이지 계층 구조에 의해 제어되며, [탐색 코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/navigation.html)에서 제공하는 JSON 모델을 사용합니다.

![구현된 탐색](assets/navigation-routing/final-navigation-implemented.gif)

## 전제 조건

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)을 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에 대해 완료된 패키지를 설치합니다. [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에서 제공하는 이미지가 WKND SPA에서 다시 사용됩니다. 패키지는 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 설치할 수 있습니다.

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution)에서 완료된 코드를 보거나 분기 `Angular/navigation-routing-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## Inspect HeaderComponent 업데이트 {#inspect-header}

이전 장에서 `HeaderComponent` 구성 요소는 `app.component.html`을 통해 포함된 순수 Angular 구성 요소로 추가되었습니다. 이 장에서 `HeaderComponent` 구성 요소는 앱에서 제거되고 [템플릿 편집기](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)를 통해 추가됩니다. 이를 통해 AEM 내에서 `HeaderComponent` 의 탐색 메뉴를 구성할 수 있습니다.

>[!NOTE]
>
> 이 장을 시작하기 위해 코드 베이스에 이미 여러 CSS 및 JavaScript 업데이트가 있습니다. 코어 개념에 집중하기 위해 코드 변경 사항의 모든&#x200B;**이 설명되어 있지 않습니다.** 전체 변경 사항 [을 여기에서 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. 선택한 IDE에서 이 장의 SPA 시작 프로젝트를 엽니다.
2. `ui.frontend` 모듈 아래의 `header.component.ts` 파일은 다음 위치에서 검사합니다.`ui.frontend/src/app/components/header/header.component.ts`

   구성 요소를 AEM 구성 요소 `wknd-spa-angular/components/header`에 매핑할 수 있도록 `HeaderEditConfig` 및 `MapTo` 를 추가하는 등 몇 가지 업데이트가 수행되었습니다.

   ```js
   /* header.component.ts */
   ...
   const HeaderEditConfig = {
       ...
   };
   
   @Component({
   selector: 'app-header',
   templateUrl: './header.component.html',
   styleUrls: ['./header.component.scss']
   })
   export class HeaderComponent implements OnInit {
   @Input() items: object[];
       ...
   }
   ...
   MapTo('wknd-spa-angular/components/header')(withRouter(Header), HeaderEditConfig);
   ```

   `items`에 대한 `@Input()` 주석을 참고하십시오. `items` 에는 AEM에서 전달된 탐색 개체 배열이 포함되어 있습니다.

3. `ui.apps` 모듈에서 AEM `Header` 구성 요소의 구성 요소 정의를 검사합니다.`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header` 구성 요소는 `sling:resourceSuperType` 속성을 통해 [탐색 코어 구성 요소](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)의 모든 기능을 상속합니다.

## SPA 템플릿 {#add-header-template}에 HeaderComponent 추가

1. 브라우저를 열고 AEM, [http://localhost:4502/](http://localhost:4502/)에 로그인합니다. 시작 코드 베이스를 이미 배포해야 합니다.
2. **[!UICONTROL SPA 페이지 템플릿]**&#x200B;으로 이동합니다.[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)
3. 가장 바깥쪽 **[!UICONTROL 루트 레이아웃 컨테이너]**&#x200B;를 선택하고 해당 **[!UICONTROL 정책]** 아이콘을 클릭합니다. 작성을 위해 **[!UICONTROL 레이아웃 컨테이너]**&#x200B;가 잠겨 있지 않은 **을 선택하려면 주의하십시오.**

   ![루트 레이아웃 컨테이너 정책 아이콘을 선택합니다](assets/navigation-routing/root-layout-container-policy.png)

4. 현재 정책을 복사하고 **[!UICONTROL SPA 구조]**&#x200B;라는 새 정책을 만듭니다.

   ![SPA 구조 정책](assets/map-components/spa-policy-update.png)

   **[!UICONTROL 허용된 구성 요소]** > **[!UICONTROL 일반]** 아래에서 **[!UICONTROL 레이아웃 컨테이너]** 구성 요소를 선택합니다.

   **[!UICONTROL 허용된 구성 요소]** > **[!UICONTROL WKND SPA ANGULAR - STRUCTURE]** 아래에서 **[!UICONTROL 헤더]** 구성 요소를 선택합니다.

   ![헤더 구성 요소 선택](assets/map-components/select-header-component.png)

   **[!UICONTROL 허용된 구성 요소]** > **[!UICONTROL WKND SPA ANGULAR - Content]** >에서 **[!UICONTROL 이미지]** 및 **[!UICONTROL 텍스트]** 구성 요소를 선택합니다. 총 4개의 구성 요소가 선택되어 있어야 합니다.

   **[!UICONTROL 완료]**&#x200B;를 클릭하여 변경 내용을 저장합니다.

5. **페이지를 새로 고칩니다.** 잠금 해제된 **[!UICONTROL 레이아웃 컨테이너]** 위에 **[!UICONTROL Header]** 구성 요소를 추가합니다.

   ![템플릿에 헤더 구성 요소 추가](./assets/navigation-routing/add-header-component.gif)

6. **[!UICONTROL 헤더]** 구성 요소를 선택하고 **정책** 아이콘을 클릭하여 정책을 편집합니다.

   ![헤더 정책 클릭](assets/navigation-routing/header-policy-icon.png)

7. **&quot;WKND SPA Header&quot;**&#x200B;의 **[!UICONTROL 정책 제목]**&#x200B;을 사용하여 새 정책을 만듭니다.

   **[!UICONTROL 속성]**&#x200B;에서 다음을 수행합니다.

   * **[!UICONTROL 탐색 루트]**&#x200B;를 `/content/wknd-spa-angular/us/en`로 설정합니다.
   * **[!UICONTROL 루트 수준 제외]**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **[!UICONTROL 모든 하위 페이지 수집]**&#x200B;의 선택을 취소합니다.
   * **[!UICONTROL 탐색 구조 깊이]**&#x200B;를 **3**&#x200B;로 설정합니다.

   ![헤더 정책 구성](assets/navigation-routing/header-policy.png)

   이렇게 하면 `/content/wknd-spa-angular/us/en` 아래의 탐색 2 수준이 수집됩니다.

8. 변경 사항을 저장한 후에는 채워진 `Header`이 템플릿의 일부로 표시됩니다.

   ![채워진 헤더 구성 요소](assets/navigation-routing/populated-header.png)

## 하위 페이지 만들기

다음으로, SPA에서 다른 보기 역할을 하는 AEM에서 추가 페이지를 만듭니다. 또한 AEM에서 제공하는 JSON 모델의 계층 구조도 검사합니다.

1. **사이트** 콘솔로 이동합니다.[http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home) **WKND SPA Angular 홈 페이지**&#x200B;를 선택하고 **[!UICONTROL 만들기]** > **[!UICONTROL 페이지]**&#x200B;를 클릭합니다.

   ![새 페이지 만들기](assets/navigation-routing/create-new-page.png)

2. **[!UICONTROL 템플릿]** SPA 페이지&#x200B;]**를 선택합니다.**[!UICONTROL  **[!UICONTROL 속성]** 아래에 **[!UICONTROL 제목]** 및 **&quot;page-1&quot;**&#x200B;의 **&quot;Page 1&quot;**&#x200B;을 이름으로 입력합니다.

   ![초기 페이지 속성 입력](assets/navigation-routing/initial-page-properties.png)

   **[!UICONTROL 만들기]**&#x200B;를 클릭하고 대화 상자 팝업에서 **[!UICONTROL 열기]**&#x200B;를 클릭하여 AEM SPA 편집기에서 페이지를 엽니다.

3. 새 **[!UICONTROL Text]** 구성 요소를 기본 **[!UICONTROL 레이아웃 컨테이너]**&#x200B;에 추가합니다. 구성 요소를 편집하고 텍스트를 입력합니다.**&quot;Page 1&quot;**(RTE 및 **H1** 요소 사용)(단락 요소를 변경하려면 전체 화면 모드로 전환해야 함)

   ![샘플 컨텐츠 페이지 1](assets/navigation-routing/page-1-sample-content.png)

   이미지와 같은 추가 컨텐츠를 자유롭게 추가할 수 있습니다.

4. AEM Sites 콘솔으로 돌아가서 위의 단계를 반복하여 **&quot;Page 2&quot;**(이)라는 두 번째 페이지를 **페이지 1**&#x200B;의 동위 멤버로 만듭니다. 컨텐츠를 쉽게 식별할 수 있도록 **페이지 2**&#x200B;에 추가하십시오.
5. 마지막으로 세 번째 페이지인 **&quot;Page 3&quot;** 를 만들되, **Page 2**&#x200B;의 **child**&#x200B;로 만듭니다. 사이트 계층 구조가 완료되면 다음과 같이 표시됩니다.

   ![샘플 사이트 계층](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. 새 탭에서 AEM에서 제공하는 JSON 모델 API를 엽니다.[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) SPA이 처음 로드될 때 이 JSON 컨텐츠가 요청됩니다. 외부 구조는 다음과 같습니다.

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {},
       "/content/wknd-spa-angular/us/en/home/page-2/page-3": {}
       }
   }
   ```

   `:children` 아래에 작성된 각 페이지에 대한 항목이 표시됩니다. 모든 페이지의 컨텐츠는 이 초기 JSON 요청에 있습니다. 탐색 라우팅이 구현되면 콘텐츠를 이미 클라이언트측에서 사용할 수 있으므로 SPA의 후속 보기가 빠르게 로드됩니다.

   초기 JSON 요청에 SPA의 컨텐츠 **ALL**&#x200B;을 로드하는 것이 초기 페이지 로드 속도를 저하하므로 현명한 방법은 아닙니다. 다음으로, 페이지의 계층 깊이가 수집되는 방식을 살펴보겠습니다.

7. 다음 위치에서 **SPA Root** 템플릿으로 이동합니다.[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html)

   **[!UICONTROL 페이지 속성 메뉴]** > **[!UICONTROL 페이지 정책]**&#x200B;을 클릭합니다.

   ![SPA 루트에 대한 페이지 정책을 엽니다.](assets/navigation-routing/open-page-policy.png)

8. **SPA Root** 템플릿에는 수집된 JSON 컨텐츠를 제어하는 추가 **[!UICONTROL 계층 구조]** 탭이 있습니다. **[!UICONTROL 구조 깊이]**&#x200B;는 **루트** 아래에 있는 하위 페이지를 수집하기 위해 사이트 계층 구조에서 깊이가 얼마나 되는지 결정합니다. **[!UICONTROL 구조 패턴]** 필드를 사용하여 정규 표현식에 따라 추가 페이지를 필터링할 수도 있습니다.

   **[!UICONTROL 구조 깊이]**&#x200B;를 **&quot;2&quot;**&#x200B;로 업데이트하십시오.

   ![구조 깊이 업데이트](assets/navigation-routing/update-structure-depth.png)

   **[!UICONTROL 완료]**&#x200B;를 클릭하여 정책에 대한 변경 사항을 저장합니다.

9. JSON 모델 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)을 다시 엽니다.

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {}
       }
   }
   ```

   **페이지 3** 경로가 제거되었습니다.초기 JSON 모델의 `/content/wknd-spa-angular/us/en/home/page-2/page-3`.

   나중에 AEM SPA Editor SDK에서 추가 컨텐츠를 동적으로 로드하는 방법을 관찰할 수 있습니다.

## 탐색 구현

그런 다음 새 `NavigationComponent` 을 사용하여 탐색 메뉴를 구현합니다. `header.component.html`에 직접 코드를 추가할 수 있지만, 더 좋은 방법은 큰 구성 요소를 사용하지 않는 것입니다. 대신 나중에 다시 사용할 수 있는 `NavigationComponent`을 구현하십시오.

1. [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)에서 AEM `Header` 구성 요소에 의해 노출된 JSON을 검토하십시오.

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-angular/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-angular/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA Angular Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-angular/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-angular/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-angular/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-angular/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-angular/components/header"
   ```

   AEM 페이지의 계층 특성은 JSON에서 모델링되며 탐색 메뉴를 채우는 데 사용할 수 있습니다. `Header` 구성 요소는 [탐색 코어 구성 요소](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html)의 모든 기능을 상속하며 JSON을 통해 노출된 컨텐츠는 Angular `@Input` 주석에 자동으로 매핑됩니다.

2. 새 터미널 창을 열고 SPA 프로젝트의 `ui.frontend` 폴더로 이동합니다. angular CLI 도구를 사용하여 새 `NavigationComponent` 만들기:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. 그런 다음 새로 만든 `components/navigation` 디렉토리에서 Angular CLI를 사용하여 `NavigationLink` 클래스를 만듭니다.

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. 선택한 IDE로 돌아가서 `/src/app/components/navigation/navigation-link.ts`에 있는 `navigation-link.ts`에서 파일을 엽니다.

   ![navigation-link.ts 파일을 엽니다.](assets/navigation-routing/ide-navigation-link-file.png)

5. `navigation-link.ts` 을 다음과 같이 채웁니다.

   ```js
   export class NavigationLink {
   
       title: string;
       path: string;
       url: string;
       level: number;
       children: NavigationLink[];
       active: boolean;
   
       constructor(data) {
           this.path = data.path;
           this.title = data.title;
           this.url = data.url;
           this.level = data.level;
           this.active = data.active;
           this.children = data.children.map( item => {
               return new NavigationLink(item);
           });
       }
   }
   ```

   개별 탐색 링크를 나타내는 간단한 클래스입니다. 클래스 생성자에서는 `data`이 AEM에서 전달된 JSON 개체여야 합니다. 이 클래스는 `NavigationComponent` 및 `HeaderComponent` 내에서 모두 사용하여 탐색 구조를 쉽게 채웁니다.

   데이터 변환은 수행되지 않으며, 이 클래스는 주로 JSON 모델을 강력하게 입력할 수 있도록 생성됩니다. `this.children`이 `NavigationLink[]`로 입력되었고 생성자가 `children` 배열의 각 항목에 대해 새 `NavigationLink` 개체를 재귀적으로 작성한다는 점에 주목하십시오. `Header`에 대한 JSON 모델이 계층적임을 기억하십시오.

6. `navigation-link.spec.ts` 파일을 엽니다. `NavigationLink` 클래스의 테스트 파일입니다. 다음과 같이 업데이트합니다.

   ```js
   import { NavigationLink } from './navigation-link';
   
   describe('NavigationLink', () => {
       it('should create an instance', () => {
           const data = {
               children: [],
               level: 1,
               active: false,
               path: '/content/wknd-spa-angular/us/en/home/page-1',
               description: null,
               url: '/content/wknd-spa-angular/us/en/home/page-1.html',
               lastModified: 1589429385100,
               title: 'Page 1'
           };
           expect(new NavigationLink(data)).toBeTruthy();
       });
   });
   ```

   `const data` 은 단일 링크에 대해 이전에 검사한 동일한 JSON 모델을 따릅니다. 이는 강력한 단위 테스트와는 거리가 있지만 `NavigationLink` 생성자를 테스트하는 것으로 충분합니다.

7. `navigation.component.ts` 파일을 엽니다. 다음과 같이 업데이트합니다.

   ```js
   import { Component, OnInit, Input } from '@angular/core';
   import { NavigationLink } from './navigation-link';
   
   @Component({
   selector: 'app-navigation',
   templateUrl: './navigation.component.html',
   styleUrls: ['./navigation.component.scss']
   })
   export class NavigationComponent implements OnInit {
   
       @Input() items: object[];
   
       constructor() { }
   
       get navigationLinks(): NavigationLink[] {
   
           if (this.items && this.items.length > 0) {
               return this.items.map(item => {
                   return new NavigationLink(item);
               });
           }
   
           return null;
       }
   
       ngOnInit() {}
   
   }
   ```

   `NavigationComponent` AEM의  `object[]` JSON 모델인  `items` 가 필요합니다. 이 클래스는 `NavigationLink` 개체의 배열을 반환하는 단일 메서드 `get navigationLinks()`을 노출합니다.

8. `navigation.component.html` 파일을 열고 다음과 같이 업데이트합니다.

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   이렇게 하면 초기 `<ul>`이 생성되고 `navigation.component.ts`에서 `get navigationLinks()` 메서드가 호출됩니다. `<ng-container>`은 `recursiveListTmpl`이라는 템플릿을 호출하고 이 템플릿을 `navigationLinks`라는 변수로 전달하는 데 사용됩니다.`links`

   다음 `recursiveListTmpl` 추가:

   ```html
   <ng-template #recursiveListTmpl let-links="links">
       <li *ngFor="let link of links" class="{{'navigation__item navigation__item--' + link.level}}">
           <a [routerLink]="link.url" class="navigation__item-link" [title]="link.title" [attr.aria-current]="link.active">
               {{link.title}}
           </a>
           <ul *ngIf="link.children && link.children.length > 0">
               <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: link.children }"></ng-container>
           </ul>
       </li>
   </ng-template>
   ```

   여기서는 탐색 링크에 대한 나머지 렌더링이 구현됩니다. `link` 변수는 `NavigationLink` 유형이며, 해당 클래스에서 만든 모든 메서드/속성을 사용할 수 있습니다. [`[routerLink]`](https://angular.io/api/router/RouterLink) 가 일반  `href` 속성 대신 사용됩니다. 이렇게 하면 전체 페이지를 새로 고치지 않고 앱의 특정 경로에 연결할 수 있습니다.

   현재 `link`에 비어 있지 않은 `children` 배열이 있는 경우 다른 `<ul>`을 만들어 탐색의 재귀 부분도 구현됩니다.

9. `RouterTestingModule`에 대한 지원을 추가하려면 `navigation.component.spec.ts`을 업데이트하십시오.

   ```diff
    ...
   + import { RouterTestingModule } from '@angular/router/testing';
    ...
    beforeEach(async(() => {
       TestBed.configureTestingModule({
   +   imports: [ RouterTestingModule ],
       declarations: [ NavigationComponent ]
       })
       .compileComponents();
    }));
    ...
   ```

   구성 요소가 `[routerLink]`을(를) 사용하므로 `RouterTestingModule`을 추가해야 합니다.

10. `navigation.component.scss`을 업데이트하여 일부 기본 스타일을 `NavigationComponent`에 추가합니다.

   ```scss
   @import "~src/styles/variables";
   
   $link-color: $black;
   $link-hover-color: $white;
   $link-background: $black;
   
   :host-context {
       display: block;
       width: 100%;
   }
   
   .navigation__item {
       list-style: none;
   }
   
   .navigation__item-link {
       color: $link-color;
       font-size: $font-size-large;
       text-transform: uppercase;
       padding: $gutter-padding;
       display: flex;
       border-bottom: 1px solid $gray;
   
       &:hover {
           background: $link-background;
           color: $link-hover-color;
       }
   
   }
   ```

## 헤더 구성 요소 업데이트

이제 `NavigationComponent`이 구현되었으므로 `HeaderComponent`을 업데이트하여 참조해야 합니다.

1. 터미널을 열고 SPA 프로젝트 내의 `ui.frontend` 폴더로 이동합니다. **웹 팩 개발 서버**&#x200B;를 시작합니다.

   ```shell
   $ npm start
   ```

2. 브라우저 탭을 열고 [http://localhost:4200/](http://localhost:4200/)로 이동합니다.

   **웹 팩 개발 서버**&#x200B;는 AEM의 로컬 인스턴스에서 JSON 모델을 프록시하도록 구성해야 합니다(`ui.frontend/proxy.conf.json`). 이렇게 하면 자습서의 앞부분에서 AEM에서 만든 컨텐츠에 대해 직접 코드를 작성할 수 있습니다.

   ![메뉴 작업 전환](./assets/navigation-routing/nav-toggle-static.gif)

   현재 `HeaderComponent`에 이미 구현된 메뉴 전환 기능이 있습니다. 그런 다음 탐색 구성 요소를 추가합니다.

3. 선택한 IDE로 돌아가서 `header.component.ts` 파일을 `ui.frontend/src/app/components/header/header.component.ts`에서 엽니다.
4. 하드 코딩된 문자열을 제거하고 AEM 구성 요소에서 전달한 동적 prop을 사용하려면 `setHomePage()` 메서드를 업데이트하십시오.

   ```js
   /* header.component.ts */
   import { NavigationLink } from '../navigation/navigation-link';
   ...
    setHomePage() {
       if (this.hasNavigation) {
           const rootNavigationLink: NavigationLink = new NavigationLink(this.items[0]);
           this.isHome = rootNavigationLink.path === this.route.snapshot.data.path;
           this.homePageUrl = rootNavigationLink.url;
       }
   }
   ...
   ```

   AEM에서 전달된 탐색 JSON 모델의 루트인 `items[0]`을 기반으로 `NavigationLink`의 새 인스턴스가 만들어집니다. `this.route.snapshot.data.path` 현재 Angular 경로의 경로를 반환합니다. 이 값은 현재 경로가 **홈 페이지**&#x200B;인지 확인하는 데 사용됩니다. `this.homePageUrl` 로고에서 앵커 링크를 채우는 데  **사용됩니다**.

5. `header.component.html` 을 열고 탐색에 대한 정적 자리 표시자를 새로 만든 `NavigationComponent`에 대한 참조로 바꿉니다.

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` 속성은  `@Input() items` 에서  `HeaderComponent` 로 `NavigationComponent` 를 전달하여 탐색을 작성합니다.

6. `header.component.spec.ts` 을 열고 `NavigationComponent`에 대한 선언을 추가합니다.

   ```diff
       /* header.component.spect.ts */
   +   import { NavigationComponent } from '../navigation/navigation.component';
   
       describe('HeaderComponent', () => {
       let component: HeaderComponent;
       let fixture: ComponentFixture<HeaderComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           imports: [ RouterTestingModule ],
   +       declarations: [ HeaderComponent, NavigationComponent ]
           })
           .compileComponents();
       }));
   ```

   이제 `NavigationComponent`이(가) `HeaderComponent`의 일부로 사용되므로 테스트 베드의 일부로 선언해야 합니다.

7. 열려 있는 파일에 대한 변경 내용을 저장하고 **webpack 개발 서버**&#x200B;로 돌아갑니다.[http://localhost:4200/](http://localhost:4200/)

   ![헤더 탐색 완료](assets/navigation-routing/completed-header.png)

   메뉴 토글을 클릭하여 탐색을 엽니다. 그러면 채워진 탐색 링크가 표시됩니다. SPA의 다른 보기로 이동할 수 있어야 합니다.

## SPA 라우팅 이해

탐색이 구현되었으므로 이제 AEM에서 라우팅을 검사합니다.

1. IDE에서 `ui.frontend/src/app`에 있는 `app-routing.module.ts` 파일을 엽니다.

   ```js
   /* app-routing.module.ts */
   import { AemPageDataResolver, AemPageRouteReuseStrategy } from '@adobe/cq-angular-editable-components';
   import { NgModule } from '@angular/core';
   import { RouteReuseStrategy, RouterModule, Routes, UrlMatchResult, UrlSegment } from '@angular/router';
   import { PageComponent } from './components/page/page.component';
   
   export function AemPageMatcher(url: UrlSegment[]): UrlMatchResult {
       if (url.length) {
           return {
               consumed: url,
               posParams: {
                   path: url[url.length - 1]
               }
           };
       }
   }
   
   const routes: Routes = [
       {
           matcher: AemPageMatcher,
           component: PageComponent,
           resolve: {
               path: AemPageDataResolver
           }
       }
   ];
   @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule],
       providers: [
           AemPageDataResolver,
           {
           provide: RouteReuseStrategy,
           useClass: AemPageRouteReuseStrategy
           }
       ]
   })
   export class AppRoutingModule {}
   ```

   `routes: Routes = [];` 배열은 Angular 구성 요소 매핑에 대한 경로 또는 탐색 경로를 정의합니다.

   `AemPageMatcher` 는 이 Angular 애플리케이션의 일부인 AEM의  [&quot;유사&quot;페이지와 일치하는 사용자 지정 Angular 라우터 UrlMatcher](https://angular.io/api/router/UrlMatcher)입니다.

   `PageComponent` 는 AEM의 페이지를 나타내는 Angular 구성 요소이며 일치하는 경로가 호출됩니다. `PageComponent`이(가) 더 검사됩니다.

   `AemPageDataResolver`AEM SPA Editor JS SDK에서 제공하는 은  [경로 URL을 AEM에 있는 경로 URL로, 확장자를 ](https://angular.io/api/router/Resolve) 뺀 페이지 경로인 AEM의 리소스 경로로 변환하는 데 사용되는 사용자 지정 Angular 라우터 입니다.

   예를 들어 `AemPageDataResolver`은 라우트의 URL이 `content/wknd-spa-angular/us/en/home.html`인 `/content/wknd-spa-angular/us/en/home`의 경로로 변환됩니다. JSON 모델 API의 경로를 기반으로 페이지의 컨텐츠를 해결하는 데 사용됩니다.

   `AemPageRouteReuseStrategy`AEM SPA Editor JS SDK에서 제공하는 는 여러 경로 간에  [](https://angular.io/api/router/RouteReuseStrategy) 의 재사용을 방지하는 사용자 지정 RouteReuseStrategit `PageComponent` 입니다. 그렇지 않으면 페이지 &quot;B&quot;로 이동할 때 페이지 &quot;A&quot;의 컨텐츠가 표시될 수 있습니다.

2. `ui.frontend/src/app/components/page/`에서 `page.component.ts` 파일을 엽니다.

   ```js
   ...
   export class PageComponent {
       items;
       itemsOrder;
       path;
   
       constructor(
           private route: ActivatedRoute,
           private modelManagerService: ModelManagerService
       ) {
           this.modelManagerService
           .getData({ path: this.route.snapshot.data.path })
           .then(data => {
               this.path = data[Constants.PATH_PROP];
               this.items = data[Constants.ITEMS_PROP];
               this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
           });
       }
   }
   ```

   `PageComponent` 은 AEM에서 검색된 JSON을 처리하는 데 필요하며, 경로를 렌더링하기 위해 Angular 구성 요소로 사용됩니다.

   `ActivatedRoute`angular 라우터 모듈에서 제공하는 에는 이 Angular 페이지 구성 요소 인스턴스에 어떤 AEM 페이지의 JSON 콘텐츠를 로드해야 하는지 나타내는 상태가 포함되어 있습니다.

   `ModelManagerService`는 경로를 기반으로 JSON 데이터를 가져오고 데이터를 클래스 변수  `path`,  `items`,  `itemsOrder`에 매핑합니다. 그런 다음 [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)에 전달됩니다

3. `ui.frontend/src/app/components/page/`에서 `page.component.html` 파일을 엽니다.

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` 는  [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)를 포함합니다. `path`, `items` 및 `itemsOrder` 변수가 `AEMPageComponent`에 전달됩니다. 그런 다음 SPA Editor JavaScript SDK를 통해 제공된 `AemPageComponent`은 이 데이터를 반복하고 [구성 요소 매핑 자습서](./map-components.md)에 표시된 대로 JSON 데이터를 기반으로 Angular 구성 요소를 동적으로 인스턴스화합니다.

   `PageComponent`은(는) 실제로 `AEMPageComponent`에 대한 프록시이며, 대부분의 JSON 모델을 Angular 구성 요소에 올바르게 매핑하기 위해 무거운 리프팅을 수행하는 `AEMPageComponent`입니다.

## AEM에서 SPA 라우팅 Inspect

1. 터미널을 열고 **webpack 개발 서버**&#x200B;를 시작하는 경우 중지합니다. 프로젝트의 루트로 이동하고 Maven 기술을 사용하여 프로젝트를 AEM에 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > angular 프로젝트에 매우 엄격한 라인 지정 규칙이 활성화되어 있습니다. Maven 빌드가 실패하면 오류를 확인하고 나열된 파일에서 발견된 **라인 오류를 찾습니다.**. Linter에서 발견된 문제를 해결하고 Maven 명령을 다시 실행하십시오.

2. AEM에서 SPA 홈 페이지로 이동합니다.[http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) 브라우저의 개발자 도구를 엽니다. 아래 스크린샷은 Google Chrome 브라우저에서 촬영됩니다.

   페이지를 새로 고치면 SPA 루트인 `/content/wknd-spa-angular/us/en.model.json`에 대한 XHR 요청이 표시됩니다. 자습서에서 이전에 만든 SPA 루트 템플릿에 대한 계층 구조 깊이 구성을 기준으로 세 개의 하위 페이지만 포함됩니다. 여기에는 **페이지 3**&#x200B;이 포함되지 않습니다.

   ![초기 JSON 요청 - SPA 루트](assets/navigation-routing/initial-json-request.png)

3. 개발자 도구가 열려 있으면 **페이지 3**&#x200B;으로 이동합니다.

   ![3페이지 탐색](assets/navigation-routing/page-three-navigation.png)

   다음에 대한 새 XHR 요청이 수행되는지 확인합니다.`/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![3페이지 XHR 요청](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager는 **페이지 3** JSON 콘텐츠를 사용할 수 없으며 추가 XHR 요청을 자동으로 트리거함을 이해합니다.

4. 다양한 탐색 링크를 사용하여 SPA을 계속 탐색합니다. 추가적인 XHR 요청이 수행되지 않으며 전체 페이지 새로 고침이 발생하지 않음을 확인합니다. 이렇게 하면 최종 사용자가 SPA을 빠르게 사용할 수 있으며, AEM에 대한 불필요한 요청을 다시 줄일 수 있습니다.

   ![구현된 탐색](assets/navigation-routing/final-navigation-implemented.gif)

5. 다음으로 직접 이동하여 딥 링크를 실험합니다.[http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html) 브라우저의 뒤로 단추가 계속 작동하는 것을 확인합니다.

## 축하합니다! {#congratulations}

축하합니다. SPA Editor SDK를 사용하여 AEM 페이지에 매핑하여 SPA에서 여러 개의 보기를 지원하는 방법을 알아보았습니다. 동적 내비게이션은 Angular 라우팅을 사용하여 구현되었으며 `Header` 구성 요소에 추가되었습니다.

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution)에서 완료된 코드를 보거나 분기 `Angular/navigation-routing-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

### 다음 단계 {#next-steps}

[사용자 지정 구성 요소 만들기](custom-component.md)  - AEM SPA 편집기에 사용할 사용자 지정 구성 요소를 만드는 방법을 알아봅니다. 작성자 대화 상자 및 Sling 모델을 개발하여 JSON 모델을 확장하여 사용자 지정 구성 요소를 채우는 방법을 알아봅니다.