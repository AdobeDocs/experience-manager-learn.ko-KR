---
title: 구성 요소 확장 | AEM SPA 편집기 및 Angular 시작하기
description: AEM SPA 편집기에서 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 컨텐츠를 추가하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 확장하는 강력한 방법입니다. Sling Model 및 Sling Resource Merger 기능을 확장하는 위임 패턴을 사용하는 방법을 알아봅니다.
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1935'
ht-degree: 2%

---

# 코어 구성 요소 확장 {#extend-component}

AEM SPA 편집기에서 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소를 확장하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 사용자 지정하고 확장하는 강력한 방법입니다.

## 목표

1. 추가 속성 및 콘텐츠로 기존 코어 구성 요소를 확장합니다.
2. 을 사용하여 구성 요소 상속의 기본 사항을 이해합니다 `sling:resourceSuperType`.
3. 사용 방법을 알아봅니다 [위임 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Sling Models에서 기존 로직 및 기능을 재사용할 수 있습니다.

## 빌드할 내용

이 장에서는 `Card` 구성 요소가 만들어집니다. 다음 `Card` 구성 요소 확장 [이미지 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 제목 및 클릭유도문안 단추와 같은 추가 컨텐츠 필드를 추가하여 SPA 내의 다른 콘텐츠에 대한 티저 역할을 수행합니다.

![카드 구성 요소의 최종 작성](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 실제 구현에서는 을 사용하는 것이 더 적절할 수 있습니다 [티저 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html) 확장 [이미지 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 만들다 `Card` 구성 요소 는 프로젝트 요구 사항에 따라 다릅니다. 항상 을 사용하는 것이 좋습니다 [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 가능한 한 직접 가져옵니다.

## 사전 요구 사항

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   를 사용하는 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 패키지에 대해 완료된 패키지 설치 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0). 에서 제공하는 이미지 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest) 는 WKND SPA에서 재사용됩니다. 패키지는 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp).

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `Angular/extend-component-solution`.

## Inspect 초기 카드 구현

초기 카드 구성 요소는 장 시작 코드에서 제공했습니다. Inspect 를 카드 구현의 시작점입니다.

1. 선택한 IDE에서 `ui.apps` 모듈.
2. 다음으로 이동 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` 그리고 `.content.xml` 파일.

   ![카드 구성 요소 AEM 정의 시작](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   속성 `sling:resourceSuperType` 점 `wknd-spa-angular/components/image` 다음을 나타냅니다. `Card` 구성 요소는 WKND SPA 이미지 구성 요소의 기능을 상속합니다.

3. Inspect 파일 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   다음 사항에 주의하십시오. `sling:resourceSuperType` 점 `core/wcm/components/image/v2/image`. 이는 WKND SPA 이미지 구성 요소가 핵심 구성 요소 이미지의 기능을 상속함을 나타냅니다.

   라고도 함 [프록시 패턴](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling 리소스 상속은 하위 구성 요소가 기능을 상속하고 원하는 경우 동작을 확장/무시할 수 있도록 해주는 강력한 디자인 패턴입니다. Sling 상속은 여러 수준의 상속을 지원하므로 궁극적으로 새로운 `Card` 구성 요소는 핵심 구성 요소 이미지의 기능을 상속합니다.

   많은 개발 팀은 D.R.Y.가 되기 위해 노력하고 있습니다(반복하지 마십시오). Sling 상속을 사용하면 AEM에서 이 작업을 수행할 수 있습니다.

4. 아래 `card` 폴더에서 파일을 엽니다. `_cq_dialog/.content.xml`.

   이 파일은 `Card` 구성 요소. Sling 상속을 사용하는 경우에는 의 기능을 사용할 수 있습니다 [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) 를 클릭하여 대화 상자의 부분을 대체하거나 확장합니다. 이 샘플에서 카드 구성 요소를 채우기 위해 작성자의 추가 데이터를 캡처하기 위해 대화 상자에 새 탭이 추가되었습니다.

   과 같은 속성 `sling:orderBefore` 개발자가 새 탭이나 양식 필드를 삽입할 위치를 선택할 수 있도록 허용합니다. 이 경우 `Text` 탭 앞에 삽입 `asset` 탭. Sling Resource Merger를 완전히 사용하려면 의 원래 대화 상자 노드 구조를 알고 있어야 합니다 [이미지 구성 요소 대화 상자](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. 아래 `card` 폴더에서 파일을 엽니다. `_cq_editConfig.xml`. 이 파일은 AEM 작성 UI의 드래그 앤 드롭 동작을 지시합니다. 이미지 구성 요소를 확장할 때 리소스 유형이 구성 요소 자체와 일치해야 합니다. 를 검토합니다. `<parameters>` 노드:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   대부분의 구성 요소에는 `cq:editConfig`, 이미지 구성 요소의 하위 항목 및 이미지 는 예외입니다.

6. IDE에서 `ui.frontend` 모듈, 이동 `ui.frontend/src/app/components/card`:

   ![Angular 구성 요소 시작](assets/extend-component/angular-card-component-start.png)

7. Inspect 파일 `card.component.ts`.

   구성 요소는 AEM에 매핑하기 위해 이미 로그아웃되었습니다 `Card` 표준을 사용하는 구성 요소 `MapTo` 함수 위에 있어야 합니다.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   세 가지 사항을 검토합니다 `@Input` 에 대한 클래스의 매개 변수 `src`, `alt`, 및 `title`. 이는 Angular 구성 요소에 매핑되는 AEM 구성 요소의 JSON 값이어야 합니다.

8. 파일을 엽니다. `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   이 예에서는 기존 Angular 이미지 구성 요소를 다시 사용하도록 선택했습니다 `app-image` 그냥 `@Input` 매개 변수 `card.component.ts`. 튜토리얼의 후반부에 추가 속성이 추가되고 표시됩니다.

## 템플릿 정책 업데이트

초기 `Card` 구현은 AEM SPA 편집기에서 기능을 검토합니다. 초기 항목을 보려면 `Card` 구성 요소 및 템플릿 정책에 대한 업데이트가 필요합니다.

1. 아직 수행하지 않았다면 AEM의 로컬 인스턴스에 시작 코드를 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 의 SPA 페이지 템플릿으로 이동합니다. [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 `Card` 구성 요소를 허용된 구성 요소로 사용:

   ![레이아웃 컨테이너 정책 업데이트](assets/extend-component/card-component-allowed.png)

   정책에 대한 변경 사항을 저장하고 `Card` 구성 요소를 허용된 구성 요소로 사용:

   ![허용된 구성 요소로 카드 구성 요소](assets/extend-component/card-component-allowed-layout-container.png)

## 작성자 초기 카드 구성 요소

다음으로, `Card` 구성 요소를 생성하지 않습니다.

1. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. in `Edit` 모드, 추가 `Card` 구성 요소를 `Layout Container`:

   ![새 구성 요소 삽입](assets/extend-component/insert-custom-component.png)

3. 자산 파인더의 이미지를 `Card` 구성 요소:

   ![이미지 추가](assets/extend-component/card-add-image.png)

4. 를 엽니다. `Card` 구성 요소 대화 상자 및 추가 **텍스트** 탭.
5. 에 다음 값을 입력합니다. **텍스트** 탭:

   ![텍스트 구성 요소 탭](assets/extend-component/card-component-text.png)

   **카드 경로** - SPA 홈 페이지 아래에서 페이지를 선택합니다.

   **CTA 텍스트** - &quot;자세한 내용&quot;

   **카드 제목** - 비워 둡니다.

   **연결된 페이지에서 제목 가져오기** - true를 표시하려면 확인란을 선택합니다.

6. 업데이트 **자산 메타데이터** 탭하여 다음 값 추가 **대체 텍스트** 및 **캡션**.

   현재 대화 상자를 업데이트한 후에는 추가 변경 사항이 표시되지 않습니다. 새 필드를 Angular 구성 요소에 표시하려면 의 Sling 모델을 업데이트해야 합니다. `Card` 구성 요소.

7. 새 탭을 열고 다음 위치로 이동합니다. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). 아래의 컨텐츠 노드 Inspect `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` 찾기 `Card` 구성 요소 컨텐츠.

   ![CRXDE-Lite 구성 요소 속성](assets/extend-component/crxde-lite-properties.png)

   해당 속성을 확인합니다 `cardPath`, `ctaText`, `titleFromPage` 는 대화 상자에 의해 유지됩니다.

## 카드 Sling 모델 업데이트

궁극적으로 구성 요소 대화 상자의 값을 Angular 구성 요소로 표시하려면 의 JSON을 채우는 Sling 모델을 업데이트해야 합니다. `Card` 구성 요소. 또한 두 가지 비즈니스 로직을 구현할 수 있는 기회가 있습니다.

* If `titleFromPage` to **true**&#x200B;에 지정된 페이지의 제목을 반환합니다. `cardPath` 그렇지 않으면 값 반환 `cardTitle` 텍스트 필드.
* 지정한 페이지의 마지막 수정 날짜 반환 `cardPath`.

선택한 IDE로 돌아가서 를 엽니다. `core` 모듈.

1. 파일을 엽니다. `Card.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   다음을 확인합니다. `Card` 인터페이스 현재 확장 `com.adobe.cq.wcm.core.components.models.Image` 따라서 는 `Image` 인터페이스. 다음 `Image` 인터페이스는 이미 확장되어 있습니다. `ComponentExporter` Sling 모델을 JSON으로 내보내고 SPA 편집기로 매핑할 수 있는 인터페이스입니다. 따라서 명시적으로 확장할 필요가 없습니다 `ComponentExporter` 우리가 했던 것과 같은 인터페이스 [사용자 지정 구성 요소 장](custom-component.md).

2. 인터페이스에 다음 메서드를 추가합니다.

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   이러한 메서드는 JSON 모델 API를 통해 노출되고 Angular 구성 요소로 전달됩니다.

3. 열기 `CardImpl.java`. 구현입니다 `Card.java` 인터페이스. 이 구현은 자습서를 가속화하기 위해 부분적으로 시도되었습니다.  의 사용에 주목하십시오 `@Model` 및 `@Exporter` Sling Model Exporter를 통해 Sling Model을 JSON으로 직렬화할 수 있는 주석.

   `CardImpl.java` 또한 [Sling 모델에 대한 위임 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 이미지 코어 구성 요소에서 논리를 다시 작성하지 않도록 합니다.

4. 다음 줄을 준수합니다.

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   위의 주석은 이름이 인 Image 객체를 인스턴스화합니다 `image` 기준 `sling:resourceSuperType` 상속 `Card` 구성 요소.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   그런 다음 `image` 개체 `Image` 직접 논리를 쓰지 않고도 인터페이스를 사용할 수 있습니다. 이 기법은 `getSrc()`, `getAlt()`, 및 `getTitle()`.

5. 다음으로, 를 구현합니다 `initModel()` 비공개 변수 시작 방법 `cardPage` 의 값에 따라 `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   다음 `@PostConstruct initModel()` Sling 모델이 초기화될 때 를 호출하여 모델의 다른 메서드에서 사용할 수 있는 개체를 초기화할 수 있는 좋은 기회입니다. 다음 `pageManager` 다음 중 하나 [Java™ 지원 전역 개체](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) 를 통해 Sling 모델에서 을 사용할 수 있게 되었습니다. `@ScriptVariable` 주석. 다음 [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 메서드는 경로를 취하여 AEM을 반환합니다 [페이지](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) 유효한 페이지를 가리키지 않는 경우에는 null 또는 객체입니다.

   이렇게 하면 `cardPage` 변수에 대한 반환 값을 지정할 수 있습니다.

6. 작성자 대화 상자에 저장된 JCR 속성에 이미 매핑된 전역 변수를 검토하십시오. 다음 `@ValueMapValue` 주석은 매핑을 자동으로 수행하는 데 사용됩니다.

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   이러한 변수는 `Card.java` 인터페이스.

7. 에 정의된 추가 메서드를 구현합니다 `Card.java` 인터페이스:

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > 을 볼 수 있습니다 [여기서 CardImpl.java를 완료했습니다.](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. 터미널 창을 열고 업데이트를 `core` Maven을 사용한 모듈 `autoInstallBundle` 프로필의 `core` 디렉토리.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   를 사용하는 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필 참조.

9. 다음 위치에서 JSON 모델 응답을 봅니다. [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) 그리고 `wknd-spa-angular/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-angular/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-angular/us/en/home/page-1.html",
       ":type": "wknd-spa-angular/components/card"
   }
   ```

   JSON 모델은 의 메서드를 업데이트한 후 추가 키/값 쌍으로 업데이트됩니다. `CardImpl` Sling 모델.

## angular 구성 요소 업데이트

이제 JSON 모델이 의 새 속성으로 채워지므로 `ctaLinkURL`, `ctaText`, `cardTitle`, 및 `cardLastModified` angular 구성 요소를 업데이트하여 표시할 수 있습니다.

1. IDE로 돌아가서 를 엽니다. `ui.frontend` 모듈. 선택적으로 새 터미널 창에서 웹 팩 개발 서버를 시작하여 변경 사항을 실시간으로 확인합니다.

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. 열기 `card.component.ts` at `ui.frontend/src/app/components/card/card.component.ts`. 추가 `@Input` 새 모델을 캡처하기 위한 주석:

   ```diff
   export class CardComponent implements OnInit {
   
        @Input() src: string;
        @Input() alt: string;
        @Input() title: string;
   +    @Input() cardTitle: string;
   +    @Input() cardLastModified: number;
   +    @Input() ctaLinkURL: string;
   +    @Input() ctaText: string;
   ```

3. 클릭유도문안이 준비되었는지 확인하고, 다음을 기반으로 날짜/시간 문자열을 반환하는 메서드를 추가합니다 `cardLastModified` 입력:

   ```js
   export class CardComponent implements OnInit {
       ...
       get hasCTA(): boolean {
           return this.ctaLinkURL && this.ctaLinkURL.trim().length > 0 && this.ctaText && this.ctaText.trim().length > 0;
       }
   
       get lastModifiedDate(): string {
           const lastModifiedDate = this.cardLastModified ? new Date(this.cardLastModified) : null;
   
           if (lastModifiedDate) {
           return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

4. 열기 `card.component.html` 그리고 다음 마크업을 추가하여 제목, 작업 호출 및 마지막 수정 날짜를 표시합니다.

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
       <div class="card__content">
           <h2 class="card__title">
               {{cardTitle}}
               <span class="card__lastmod" *ngIf="lastModifiedDate">{{lastModifiedDate}}</span>
           </h2>
           <div class="card__action-container" *ngIf="hasCTA">
               <a [routerLink]="ctaLinkURL" class="card__action-link" [title]="ctaText">
                   {{ctaText}}
               </a>
           </div>
       </div>
   </div>
   ```

   Sass 규칙은에 이미 추가되었습니다 `card.component.scss` 제목 스타일을 지정하려면 작업 및 마지막 수정 날짜를 호출합니다.

   >[!NOTE]
   >
   > 완료된 를 볼 수 있습니다 [Angular 카드 구성 요소 코드 여기](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Maven을 사용하여 프로젝트의 루트에서 AEM에 대한 전체 변경 사항을 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) 업데이트된 구성 요소를 보려면 다음을 수행하십시오.

   ![AEM에서 업데이트된 카드 구성 요소](assets/extend-component/updated-card-in-aem.png)

7. 다음과 유사한 페이지를 만들려면 기존 컨텐츠를 다시 작성하면 됩니다.

   ![카드 구성 요소의 최종 작성](assets/extend-component/final-authoring-card.png)

## 축하합니다! {#congratulations}

축하합니다. AEM 구성 요소를 확장하는 방법과 Sling 모델 및 대화 상자가 JSON 모델과 어떻게 작동하는지 배웠습니다.

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `Angular/extend-component-solution`.
