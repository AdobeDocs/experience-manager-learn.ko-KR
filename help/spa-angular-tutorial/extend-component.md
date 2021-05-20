---
title: 구성 요소 확장 | AEM SPA 편집기 및 Angular 시작하기
description: AEM SPA 편집기에서 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 컨텐츠를 추가하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 확장하는 강력한 방법입니다. Sling Model 및 Sling Resource Merger 기능을 확장하는 위임 패턴을 사용하는 방법을 알아봅니다.
sub-product: 사이트
feature: SPA 편집기, 핵심 구성 요소
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1989'
ht-degree: 2%

---


# 코어 구성 요소 확장 {#extend-component}

AEM SPA 편집기에서 사용할 기존 코어 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소를 확장하는 방법을 이해하는 것은 AEM SPA 편집기 구현의 기능을 사용자 지정하고 확장하는 강력한 방법입니다.

## 목표

1. 추가 속성 및 콘텐츠로 기존 코어 구성 요소를 확장합니다.
2. `sling:resourceSuperType`을 사용하여 구성 요소 상속의 기본 사항을 이해합니다.
3. Sling 모델용 [위임 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)을 활용하여 기존 로직 및 기능을 다시 사용하는 방법을 알아봅니다.

## 빌드할 내용

이 장에서는 새 `Card` 구성 요소가 만들어집니다. `Card` 구성 요소는 [이미지 코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/image.html)를 추가하여 제목 및 클릭유도문안 단추와 같은 추가 컨텐츠 필드를 SPA 내의 다른 콘텐츠에 대한 티저 역할을 수행합니다.

![카드 구성 요소의 최종 작성](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 실제 구현에서는 [Teaser 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/teaser.html)를 사용한 다음 [이미지 코어 구성 요소](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)를 확장하여 프로젝트 요구 사항에 따라 `Card` 구성 요소를 만드는 것이 더 적절할 수 있습니다. 가능하면 항상 [코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)를 직접 사용하는 것이 좋습니다.

## 전제 조건

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

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

   [AEM 6.x](overview.md#compatibility)을 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에 대해 완료된 패키지를 설치합니다. [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에서 제공하는 이미지가 WKND SPA에서 다시 사용됩니다. 패키지는 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 설치할 수 있습니다.

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)에서 완료된 코드를 보거나 분기 `Angular/extend-component-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## Inspect 초기 카드 구현

초기 카드 구성 요소는 장 시작 코드에서 제공했습니다. Inspect 를 카드 구현의 시작점입니다.

1. 선택한 IDE에서 `ui.apps` 모듈을 엽니다.
2. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` 로 이동하여 `.content.xml` 파일을 봅니다.

   ![카드 구성 요소 AEM 정의 시작](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   `sling:resourceSuperType` 속성은 `Card` 구성 요소가 WKND SPA 이미지 구성 요소에서 모든 기능을 상속함을 나타내는 `wknd-spa-angular/components/image`을 가리킵니다.

3. Inspect `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml` 파일:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   `sling:resourceSuperType`이 `core/wcm/components/image/v2/image`을 가리킵니다. 이는 WKND SPA 이미지 구성 요소가 핵심 구성 요소 이미지의 모든 기능을 상속함을 나타냅니다.

   [프록시 패턴](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling 리소스 상속은 하위 구성 요소가 원하는 경우 기능을 상속하고 동작을 확장/무시할 수 있도록 해주는 강력한 디자인 패턴입니다. Sling 상속은 여러 수준의 상속을 지원하므로 궁극적으로 새 `Card` 구성 요소는 핵심 구성 요소 이미지의 기능을 상속합니다.

   많은 개발 팀은 D.R.Y.가 되기 위해 노력하고 있습니다(반복하지 마십시오). Sling 상속을 사용하면 AEM에서 이 작업을 수행할 수 있습니다.

4. `card` 폴더 아래에서 `_cq_dialog/.content.xml` 파일을 엽니다.

   이 파일은 `Card` 구성 요소의 구성 요소 대화 상자 정의입니다. Sling 상속을 사용하는 경우 [Sling Resource Merger](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html)의 기능을 사용하여 대화 상자의 부분을 대체하거나 확장할 수 있습니다. 이 샘플에서는 카드 구성 요소를 채우기 위해 작성자의 추가 데이터를 캡처하기 위해 대화 상자에 새 탭이 추가되었습니다.

   `sling:orderBefore` 과 같은 속성을 사용하면 개발자가 새 탭이나 양식 필드를 삽입할 위치를 선택할 수 있습니다. 이 경우 `Text` 탭이 `asset` 탭 앞에 삽입됩니다. Sling 리소스 병합을 완전히 사용하려면 [이미지 구성 요소 대화 상자](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)에 대한 원래 대화 상자 노드 구조를 알고 있어야 합니다.

5. `card` 폴더 아래에서 `_cq_editConfig.xml` 파일을 엽니다. 이 파일은 AEM 작성 UI의 드래그 앤 드롭 동작을 지시합니다. 이미지 구성 요소를 확장할 때 리소스 유형이 구성 요소 자체와 일치해야 합니다. `<parameters>` 노드를 검토합니다.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   대부분의 구성 요소는 `cq:editConfig`이 필요하지 않으며, 이미지 구성 요소의 이미지 및 하위 항목은 예외입니다.

6. IDE 스위치에서 `ui.frontend` 모듈로 이동하여 `ui.frontend/src/app/components/card`:

   ![Angular 구성 요소 시작](assets/extend-component/angular-card-component-start.png)

7. Inspect: `card.component.ts` 파일

   구성 요소가 이미 표준 `MapTo` 함수를 사용하여 AEM `Card` 구성 요소에 매핑되도록 업로드되었습니다.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   `src`, `alt` 및 `title`에 대한 클래스에서 세 개의 `@Input` 매개 변수를 검토하십시오. 이는 Angular 구성 요소에 매핑될 AEM 구성 요소의 JSON 값이어야 합니다.

8. `card.component.html` 파일을 엽니다.

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   이 예제에서는 `card.component.ts`에서 `@Input` 매개 변수를 전달하여 기존 Angular 이미지 구성 요소 `app-image`을 다시 사용하도록 선택했습니다. 튜토리얼의 후반부에 추가 속성이 추가되고 표시됩니다.

## 템플릿 정책 업데이트

이 초기 `Card` 구현에서 AEM SPA 편집기의 기능을 검토합니다. 초기 `Card` 구성 요소를 보려면 템플릿 정책에 대한 업데이트가 필요합니다.

1. 아직 수행하지 않았다면 AEM의 로컬 인스턴스에 시작 코드를 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)의 SPA 페이지 템플릿으로 이동합니다.
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 `Card` 구성 요소를 허용된 구성 요소로 추가합니다.

   ![레이아웃 컨테이너 정책 업데이트](assets/extend-component/card-component-allowed.png)

   정책에 대한 변경 사항을 저장하고 `Card` 구성 요소를 허용된 구성 요소로 관찰합니다.

   ![허용된 구성 요소로 카드 구성 요소](assets/extend-component/card-component-allowed-layout-container.png)

## 작성자 초기 카드 구성 요소

그런 다음 AEM SPA 편집기를 사용하여 `Card` 구성 요소를 작성합니다.

1. [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)로 이동합니다.
2. `Edit` 모드에서 `Card` 구성 요소를 `Layout Container`에 추가합니다.

   ![새 구성 요소 삽입](assets/extend-component/insert-custom-component.png)

3. 자산 파인더의 이미지를 `Card` 구성 요소로 끌어다 놓습니다.

   ![이미지 추가](assets/extend-component/card-add-image.png)

4. `Card` 구성 요소 대화 상자를 열고 **텍스트** 탭의 추가를 확인합니다.
5. **텍스트** 탭에 다음 값을 입력합니다.

   ![텍스트 구성 요소 탭](assets/extend-component/card-component-text.png)

   **카드 경로**  - SPA 홈 페이지 아래에서 페이지를 선택합니다.

   **CTA 텍스트**  - &quot;자세한 내용&quot;

   **카드 제목**  - 비워 둡니다.

   **연결된 페이지에서 제목 가져오기**  - 확인란을 선택하여 true를 나타냅니다.

6. **자산 메타데이터** 탭을 업데이트하여 **대체 텍스트** 및 **캡션**&#x200B;에 대한 값을 추가합니다.

   현재 대화 상자를 업데이트한 후에는 추가 변경 사항이 표시되지 않습니다. 새 필드를 Angular 구성 요소에 표시하려면 `Card` 구성 요소에 대한 Sling 모델을 업데이트해야 합니다.

7. 새 탭을 열고 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card)로 이동합니다. Inspect `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` 아래의 컨텐츠 노드를 사용하여 `Card` 구성 요소 컨텐츠를 찾습니다.

   ![CRXDE-Lite 구성 요소 속성](assets/extend-component/crxde-lite-properties.png)

   `cardPath`, `ctaText`, `titleFromPage` 속성이 대화 상자에 의해 유지되는지 확인합니다.

## 카드 Sling 모델 업데이트

궁극적으로 구성 요소 대화 상자의 값을 Angular 구성 요소에 노출하려면 `Card` 구성 요소에 대한 JSON을 채우는 Sling 모델을 업데이트해야 합니다. 또한 두 가지 비즈니스 로직을 구현할 수 있는 기회가 있습니다.

* `titleFromPage`이 **true**&#x200B;로 반환되면 `cardPath`에서 지정한 페이지의 제목을 반환하거나 `cardTitle` textfield의 값을 반환합니다.
* `cardPath`에서 지정한 페이지의 마지막 수정 날짜를 반환합니다.

원하는 IDE로 돌아가서 `core` 모듈을 엽니다.

1. `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`에서 `Card.java` 파일을 엽니다.

   `Card` 인터페이스는 현재 `com.adobe.cq.wcm.core.components.models.Image`을 확장하므로 `Image` 인터페이스의 모든 메서드를 상속합니다. `Image` 인터페이스는 이미 Sling 모델을 JSON으로 내보내고 SPA 편집기로 매핑할 수 있도록 하는 `ComponentExporter` 인터페이스를 확장합니다. 따라서 [사용자 지정 구성 요소 장](custom-component.md)에서 수행한 것과 같이 `ComponentExporter` 인터페이스를 명시적으로 확장할 필요가 없습니다.

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

3. 열기 `CardImpl.java`. `Card.java` 인터페이스의 구현입니다. 이 구현은 이미 자습서를 가속화하기 위해 부분적으로 시도되었습니다.  `@Model` 및 `@Exporter` 주석을 사용하여 Sling 모델을 Sling Model Exporter를 통해 JSON으로 직렬화할 수 있는지 확인합니다.

   `CardImpl.java` 또한 는 Sling  [Models에 위임 ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 패턴을 사용하여 이미지 코어 구성 요소에서 모든 논리를 다시 작성하지 않습니다.

4. 다음 줄을 준수합니다.

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   위의 주석은 `Card` 구성 요소의 `sling:resourceSuperType` 상속을 기반으로 `image`이라는 이미지 개체를 인스턴스화합니다.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   그런 다음 `image` 개체를 사용하여 논리를 직접 작성하지 않고도 `Image` 인터페이스에 정의된 메서드를 구현할 수 있습니다. 이 기법은 `getSrc()`, `getAlt()` 및 `getTitle()`에 사용됩니다.

5. 그런 다음 `initModel()` 메서드를 구현하여 `cardPath` 값을 기반으로 개인 변수 `cardPage`을 시작합니다

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Sling 모델이 초기화되면 `@PostConstruct initModel()`은 항상 호출되므로 모델의 다른 메서드에서 사용할 수 있는 개체를 초기화하는 것이 좋습니다. `pageManager`은 `@ScriptVariable` 주석을 통해 Sling 모델에서 사용할 수 있는 [Java 지원 전역 개체](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) 중 하나입니다. [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) 메서드는 경로를 가져와 AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) 개체를 반환하거나, 경로가 올바른 페이지를 가리키지 않으면 null을 반환합니다.

   이렇게 하면 `cardPage` 변수가 초기화됩니다. 이 변수는 기본적으로 연결된 페이지에 대한 데이터를 반환하기 위해 다른 새로운 방법으로 사용됩니다.

6. 작성자 대화 상자에 저장된 JCR 속성에 이미 매핑된 전역 변수를 검토하십시오. `@ValueMapValue` 주석은 매핑을 자동으로 수행하는 데 사용됩니다.

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
   > 여기서 [완성된 CardImpl.java를 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. 터미널 창을 열고 `core` 디렉토리의 Maven `autoInstallBundle` 프로필을 사용하여 `core` 모듈에 대한 업데이트만 배포합니다.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   [AEM 6.x](overview.md#compatibility)를 사용하는 경우 `classic` 프로필을 추가합니다.

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

   `CardImpl` Sling 모델에서 메서드를 업데이트하면 JSON 모델이 추가 키/값 쌍으로 업데이트됩니다.

## angular 구성 요소 업데이트

이제 JSON 모델이 `ctaLinkURL`, `ctaText`, `cardTitle` 및 `cardLastModified`에 대한 새 속성으로 채워지므로 Angular 구성 요소를 업데이트하여 표시할 수 있습니다.

1. IDE로 돌아가서 `ui.frontend` 모듈을 엽니다. 선택적으로 새 터미널 창에서 웹 팩 개발 서버를 시작하여 변경 사항을 실시간으로 확인합니다.

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. `ui.frontend/src/app/components/card/card.component.ts`에서 `card.component.ts` 을 엽니다. 추가 `@Input` 주석을 추가하여 새 모델을 캡처합니다.

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

3. 작업에 대한 호출이 준비되었는지 확인하고 `cardLastModified` 입력을 기반으로 날짜/시간 문자열을 반환하는 메서드를 추가합니다.

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

4. `card.component.html` 을 열고 다음 마크업을 추가하여 제목, 작업 호출 및 마지막 수정 날짜를 표시합니다.

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

   제목, 작업 호출 및 마지막 수정 날짜를 스타일을 지정하는 규칙이 이미 `card.component.scss`에 추가되었습니다.

   >[!NOTE]
   >
   > 완성된 [Angular 카드 구성 요소 코드를 여기](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card)에서 볼 수 있습니다.

5. Maven을 사용하여 프로젝트의 루트에서 AEM에 대한 전체 변경 사항을 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)로 이동하여 업데이트된 구성 요소를 확인합니다.

   ![AEM에서 업데이트된 카드 구성 요소](assets/extend-component/updated-card-in-aem.png)

7. 다음과 유사한 페이지를 만들려면 기존 컨텐츠를 다시 작성합니다.

   ![카드 구성 요소의 최종 작성](assets/extend-component/final-authoring-card.png)

## 축하합니다! {#congratulations}

축하합니다. 를 사용하여 AEM 구성 요소를 확장하는 방법과 Sling 모델 및 대화 상자가 JSON 모델로 작동하는 방법을 알아보았습니다.

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)에서 완료된 코드를 보거나 분기 `Angular/extend-component-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.
