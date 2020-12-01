---
title: 구성 요소 확장 | AEM SPA 편집기 및 각도 시작하기
description: AEM SPA 편집기와 함께 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성과 컨텐츠를 추가하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 확장하는 강력한 방법입니다. Sling Resource Combination의 기능과 Sling Model을 확장하기 위해 위임 패턴을 사용하는 방법을 학습합니다.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1984'
ht-degree: 1%

---


# 핵심 구성 요소 확장 {#extend-component}

AEM SPA 편집기와 함께 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소를 확장하는 방법을 이해하는 것은 AEM SPA Editor 구현의 기능을 사용자 정의하고 확장하는 강력한 방법입니다.

## 목표

1. 추가 속성 및 컨텐츠로 기존 핵심 구성 요소를 확장합니다.
2. `sling:resourceSuperType`을 사용하여 구성 요소 상속의 기본 사항을 이해합니다.
3. Sling Models용 [위임 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)을 활용하여 기존 로직과 기능을 다시 사용하는 방법을 알아봅니다.

## 구축 내용

이 장에서 새 `Card` 구성 요소가 만들어집니다. `Card` 구성 요소는 [이미지 코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/image.html)를 확장하여 제목 및 클릭유도문안 버튼과 같은 추가 컨텐츠 필드를 SPA 내의 다른 컨텐츠에 대한 티저 역할을 수행합니다.

![카드 구성 요소의 최종 작성](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 실제 구현에서는 프로젝트 요구 사항에 따라 [Teaser Component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html)를 사용한 다음 [Image Core Component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)를 확장하여 `Card` 구성 요소를 만드는 것이 더 적절할 수 있습니다. 가능하면 항상 [코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)를 직접 사용하는 것이 좋습니다.

## 전제 조건

[로컬 개발 환경 설정](overview.md#local-dev-environment)에 대한 필수 도구 및 지침을 검토하십시오.

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드하십시오.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Maven을 사용하여 코드 베이스를 로컬 AEM 인스턴스에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)을 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에 대해 완료된 패키지를 설치합니다. [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에서 제공한 이미지가 WKND SPA에서 다시 사용됩니다. [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 패키지를 설치할 수 있습니다.

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)에서 완료된 코드를 보거나 분기 `Angular/extend-component-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## Inspect 초기 카드 구현

초기 카드 구성 요소는 장 시작 코드로 제공되었습니다. 카드 구현의 시작점 Inspect

1. 선택한 IDE에서 `ui.apps` 모듈을 엽니다.
2. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card`으로 이동하여 `.content.xml` 파일을 봅니다.

   ![카드 구성 요소 AEM 정의 시작](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   `sling:resourceSuperType` 속성은 `wknd-spa-angular/components/image`을 가리킵니다. 즉, `Card` 구성 요소는 WKND SPA 이미지 구성 요소의 모든 기능을 상속합니다.

3. Inspect 파일 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   `sling:resourceSuperType`이 `core/wcm/components/image/v2/image`을(를) 가리킵니다. 이는 WKND SPA 이미지 구성 요소가 핵심 구성 요소 이미지의 모든 기능을 상속함을 나타냅니다.

   [프록시 패턴](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling 리소스 상속은 하위 구성 요소가 기능을 상속하고 원하는 경우 동작을 확장/무시할 수 있도록 하는 강력한 디자인 패턴입니다. Sling 상속은 여러 수준의 상속을 지원하므로, 궁극적으로 새로운 `Card` 구성 요소는 핵심 구성 요소 이미지의 기능을 상속합니다.

   많은 개발 팀들은 D.R.Y.가 되려고 노력한다(반복하지 마라). 상속을 끊으면 AEM에서 가능합니다.

4. `card` 폴더 아래에서 `_cq_dialog/.content.xml` 파일을 엽니다.

   이 파일은 `Card` 구성 요소의 구성 요소 대화 상자 정의입니다. Sling 상속을 사용하는 경우 [Sling 리소스 합병](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html)의 기능을 사용하여 대화 상자의 부분을 대체하거나 확장할 수 있습니다. 이 샘플에서는 카드 구성 요소를 채우기 위해 작성자의 추가 데이터를 캡처하기 위해 대화 상자에 새 탭이 추가되었습니다.

   `sling:orderBefore`과 같은 속성을 사용하면 개발자는 새 탭이나 양식 필드를 삽입할 위치를 선택할 수 있습니다. 이 경우 `Text` 탭이 `asset` 탭 앞에 삽입됩니다. Sling 리소스 합병을 완전히 사용하려면 [이미지 구성 요소 대화 상자](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)에 대한 원래 대화 상자 노드 구조를 알고 있어야 합니다.

5. `card` 폴더 아래에서 `_cq_editConfig.xml` 파일을 엽니다. 이 파일은 AEM 작성 UI의 드래그 앤 드롭 동작을 나타냅니다. 이미지 구성 요소를 확장할 때는 리소스 유형이 구성 요소 자체와 일치해야 합니다. `<parameters>` 노드를 검토합니다.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   대부분의 구성 요소에는 `cq:editConfig`이 필요하지 않으며 이미지 구성 요소의 이미지 및 하위 하위 하위 하위 항목은 예외입니다.

6. IDE 스위치에서 `ui.frontend` 모듈로 이동하고 `ui.frontend/src/app/components/card`:

   ![각도 구성 요소 시작](assets/extend-component/angular-card-component-start.png)

7. Inspect 파일 `card.component.ts`

   표준 `MapTo` 함수를 사용하여 AEM `Card` 구성 요소에 매핑하기 위해 구성 요소가 이미 출력되었습니다.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   `src`, `alt` 및 `title`에 대해 클래스에 있는 세 개의 `@Input` 매개 변수를 검토하십시오. 이러한 값은 Angular 구성 요소에 매핑될 AEM 구성 요소의 JSON 값이어야 합니다.

8. `card.component.html` 파일을 엽니다.

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   이 예에서는 `card.component.ts`의 `@Input` 매개 변수를 단순히 전달하여 기존 각도 이미지 구성 요소 `app-image`을 다시 사용하도록 선택했습니다. 나중에 튜토리얼에서 추가 속성이 추가되고 표시됩니다.

## 템플릿 정책 업데이트

이 초기 `Card` 구현에서는 AEM SPA 편집기의 기능을 검토합니다. 초기 `Card` 구성 요소를 보려면 템플릿 정책을 업데이트해야 합니다.

1. 아직 설치하지 않은 경우 시작 코드를 AEM의 로컬 인스턴스에 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)의 SPA 페이지 템플릿으로 이동합니다.
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 `Card` 구성 요소를 허용된 구성 요소로 추가합니다.

   ![레이아웃 컨테이너 정책 업데이트](assets/extend-component/card-component-allowed.png)

   정책에 대한 변경 내용을 저장하고 `Card` 구성 요소를 허용된 구성 요소로 확인합니다.

   ![카드 구성 요소를 허용된 구성 요소로 사용](assets/extend-component/card-component-allowed-layout-container.png)

## 초기 카드 구성 요소 작성

다음으로 AEM SPA 편집기를 사용하여 `Card` 구성 요소를 작성합니다.

1. [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)으로 이동합니다.
2. `Edit` 모드에서 `Card` 구성 요소를 `Layout Container`에 추가합니다.

   ![새 구성 요소 삽입](assets/extend-component/insert-custom-component.png)

3. 자산 파인더의 이미지를 `Card` 구성 요소로 드래그하여 놓습니다.

   ![이미지 추가](assets/extend-component/card-add-image.png)

4. `Card` 구성 요소 대화 상자를 열고 **Text** 탭이 추가되었음을 확인합니다.
5. **텍스트** 탭에 다음 값을 입력합니다.

   ![텍스트 구성 요소 탭](assets/extend-component/card-component-text.png)

   **카드 경로**  - SPA 홈 페이지 아래에서 페이지를 선택합니다.

   **CTA 텍스트**  - &quot;자세히 보기&quot;

   **카드 제목**  - 비워 둡니다.

   **연결된 페이지에서**  제목 가져오기 - 확인란을 선택하여 true로 지정합니다.

6. **자산 메타데이터** 탭을 업데이트하여 **대체 텍스트** 및 **캡션**&#x200B;의 값을 추가합니다.

   현재 대화 상자를 업데이트한 후에는 추가 변경 사항이 표시되지 않습니다. 새 필드를 각도 구성 요소에 표시하려면 `Card` 구성 요소에 대한 슬링 모델을 업데이트해야 합니다.

7. 새 탭을 열고 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card)로 이동합니다. `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` 아래에 있는 컨텐트 노드를 Inspect하여 `Card` 구성 요소 컨텐츠를 찾습니다.

   ![CRXDE-Lite 구성 요소 속성](assets/extend-component/crxde-lite-properties.png)

   `cardPath`, `ctaText`, `titleFromPage` 속성이 대화 상자에 의해 유지되는지 확인합니다.

## 카드 슬링 모델 업데이트

궁극적으로 구성 요소 대화 상자의 값을 각 구성 요소에 표시하려면 `Card` 구성 요소의 JSON을 채우는 슬링 모델을 업데이트해야 합니다. Adobe는 다음 두 가지 비즈니스 로직을 구현할 수 있는 기회를 가지고 있습니다.

* `titleFromPage`이 **true**&#x200B;인 경우 `cardPath`에서 지정한 페이지의 제목을 반환하거나 `cardTitle` textfield의 값을 반환합니다.
* `cardPath`에서 지정한 페이지의 마지막 수정 날짜를 반환합니다.

원하는 IDE로 돌아가 `core` 모듈을 엽니다.

1. `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`에서 `Card.java` 파일을 엽니다.

   `Card` 인터페이스는 현재 `com.adobe.cq.wcm.core.components.models.Image`을(를) 확장하므로 `Image` 인터페이스의 모든 메서드를 상속합니다. `Image` 인터페이스는 이미 `ComponentExporter` 인터페이스를 확장하여 Sling 모델을 JSON으로 내보내고 SPA 편집기로 매핑할 수 있습니다. 따라서 [사용자 지정 구성 요소 장](custom-component.md)에서와 같이 `ComponentExporter` 인터페이스를 명시적으로 확장할 필요는 없습니다.

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

3. 열기 `CardImpl.java`. 이것은 `Card.java` 인터페이스의 구현입니다. 이 구현은 자습서를 가속화하기 위해 이미 부분적으로 시도되었습니다.  `@Model` 및 `@Exporter` 주석을 사용하여 Sling 모델을 Sling Model Exporter를 통해 JSON으로 직렬화할 수 있도록 합니다.

   `CardImpl.java` 또한 Sling  [Models용 위임 패턴을 사용하여 ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 이미지 코어 구성 요소의 모든 논리를 재작성하지 않습니다.

4. 다음 줄을 준수하십시오.

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   위의 주석은 `Card` 구성 요소의 `sling:resourceSuperType` 상속을 기준으로 `image`이라는 이미지 개체를 인스턴스화합니다.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   그런 다음 `image` 개체를 사용하여 논리를 직접 작성하지 않고도 `Image` 인터페이스에 의해 정의된 메서드를 구현할 수 있습니다. 이 기술은 `getSrc()`, `getAlt()` 및 `getTitle()`에 사용됩니다.

5. 다음으로 `initModel()` 메서드를 구현하여 `cardPath` 값을 기반으로 개인 변수 `cardPage`을(를) 시작합니다.

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Sling 모델이 초기화되면 `@PostConstruct initModel()`은 항상 호출되므로 모델의 다른 메서드에서 사용할 수 있는 개체를 초기화하는 것이 좋습니다. `pageManager`은 `@ScriptVariable` 주석을 통해 Sling 모델에서 사용할 수 있는 [Java 지원 전역 개체](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) 수 중 하나입니다. [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) 메서드는 경로를 가져와서 AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) 개체를 반환하거나 경로가 올바른 페이지를 가리키지 않으면 null을 반환합니다.

   이렇게 하면 `cardPage` 변수가 초기화됩니다. 이 변수는 기본 연결된 페이지에 대한 데이터를 반환하는 다른 새 메서드에서 사용됩니다.

6. 작성 대화 상자를 저장한 JCR 속성에 이미 매핑된 전역 변수를 검토합니다. `@ValueMapValue` 주석은 매핑을 자동으로 수행하는 데 사용됩니다.

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

   이러한 변수는 `Card.java` 인터페이스에 대한 추가 메서드를 구현하는 데 사용됩니다.

7. `Card.java` 인터페이스에 정의된 추가 메서드를 구현합니다.

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
   > [완성된 CardImpl.java는 여기에서 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. 터미널 창을 열고 `core` 디렉터리의 Maben `autoInstallBundle` 프로필을 사용하여 `core` 모듈에 대한 업데이트만 배포합니다.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   [AEM 6.x](overview.md#compatibility)을 사용하는 경우 `classic` 프로필을 추가합니다.

9. 다음 위치에서 JSON 모델 응답을 봅니다.[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) 및 `wknd-spa-angular/components/card` 검색:

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

   JSON 모델은 `CardImpl` Sling Model의 메서드를 업데이트한 후 추가 키/값 쌍으로 업데이트됩니다.

## 각도 구성 요소 업데이트

이제 JSON 모델이 `ctaLinkURL`, `ctaText`, `cardTitle` 및 `cardLastModified`에 대한 새 속성으로 채워지므로 이러한 속성을 표시하도록 Angular 구성 요소를 업데이트할 수 있습니다.

1. IDE로 돌아가 `ui.frontend` 모듈을 엽니다. 원할 경우, 새 터미널 창에서 webpack 개발 서버를 시작하여 변경 사항을 실시간으로 확인합니다.

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. `ui.frontend/src/app/components/card/card.component.ts`에서 `card.component.ts`을(를) 엽니다. 추가 `@Input` 주석을 추가하여 새 모델을 캡처합니다.

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

3. 클릭유도문안이 준비되었는지 확인하고 `cardLastModified` 입력을 기반으로 날짜/시간 문자열을 반환하는 방법을 추가합니다.

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

4. `card.component.html`을(를) 열고 다음 마크업을 추가하여 제목, 작업 호출 및 마지막으로 수정한 날짜를 표시합니다.

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

   제목, 클릭유도문안 및 마지막으로 수정한 날짜에 스타일을 지정하기 위해 sass 규칙이 이미 `card.component.scss`에 추가되었습니다.

   >[!NOTE]
   >
   > 완성된 [각 카드 구성 요소 코드는 여기에서 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Maven을 사용하여 프로젝트의 루트에서 AEM에 전체 변경 사항을 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)으로 이동하여 업데이트된 구성 요소를 확인합니다.

   ![AEM에서 업데이트된 카드 구성 요소](assets/extend-component/updated-card-in-aem.png)

7. 기존 컨텐츠를 재저작하여 다음과 유사한 페이지를 만들 수 있어야 합니다.

   ![카드 구성 요소의 최종 작성](assets/extend-component/final-authoring-card.png)

## 축하합니다!{#congratulations}

축하합니다. JSON 모델에서 Sling Models 및 대화 상자가 작동하는 방식과 함께 AEM 구성 요소를 확장하는 방법을 알아보았습니다.

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)에서 완료된 코드를 보거나 분기 `Angular/extend-component-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.
