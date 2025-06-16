---
title: 구성 요소 확장 | AEM SPA 편집기 및 Angular 시작하기
description: AEM SPA 편집기에서 사용할 기존 핵심 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 콘텐츠를 추가하는 방법을 이해하는 것은 AEM SPA Editor 구현의 기능을 확장하는 강력한 기술입니다. Sling 리소스 병합의 Sling 모델 및 기능을 확장하기 위해 위임 패턴을 사용하는 방법에 대해 알아봅니다.
feature: SPA Editor, Core Components
version: Experience Manager as a Cloud Service
jira: KT-5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
duration: 435
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1713'
ht-degree: 0%

---

# 핵심 구성 요소 확장 {#extend-component}

{{spa-editor-deprecation}}

AEM SPA 편집기에서 사용할 기존 핵심 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소를 확장하는 방법을 이해하는 것은 AEM SPA Editor 구현의 기능을 사용자 정의하고 확장하는 강력한 기술입니다.

## 목표

1. 추가 속성 및 콘텐츠를 사용하여 기존 핵심 구성 요소를 확장합니다.
2. `sling:resourceSuperType`을(를) 사용하여 구성 요소 상속의 기본 사항을 이해합니다.
3. Sling 모델용 [위임 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)을 사용하여 기존 논리 및 기능을 재사용하는 방법에 대해 알아봅니다.

## 빌드할 내용

이 장에서는 새 `Card` 구성 요소를 만듭니다. `Card` 구성 요소는 [이미지 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)를 확장하고 제목 및 Call to action 단추와 같은 추가 콘텐츠 필드를 추가하여 SPA 내의 다른 콘텐츠에 대한 티저 역할을 수행합니다.

![카드 구성 요소 최종 작성](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 실제 구현에서는 프로젝트 요구 사항에 따라 [이미지 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)를 확장하여 `Card` 구성 요소를 만드는 것보다 [티저 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html)를 사용하는 것이 더 적절할 수 있습니다. 가능하면 항상 [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)를 직접 사용하는 것이 좋습니다.

## 사전 요구 사항

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드하십시오.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)을(를) 사용하는 경우 `classic` 프로필을 추가하십시오.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0)에 대해 완료된 패키지를 설치합니다. [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)에서 제공한 이미지를 WKND SPA에서 다시 사용합니다. 패키지는 [AEM의 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 설치할 수 있습니다.

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)에서 완성된 코드를 항상 보거나 `Angular/extend-component-solution` 분기로 전환하여 코드를 로컬로 확인할 수 있습니다.

## 초기 카드 구현 검사

초기 카드 구성 요소는 챕터 스타터 코드에서 제공했습니다. 카드 구현의 시작 지점을 검사합니다.

1. 선택한 IDE에서 `ui.apps` 모듈을 엽니다.
2. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card`(으)로 이동하여 `.content.xml` 파일을 봅니다.

   ![카드 구성 요소 AEM 정의 시작](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   `sling:resourceSuperType` 속성은 `Card` 구성 요소가 WKND SPA 이미지 구성 요소에서 기능을 상속함을 나타내는 `wknd-spa-angular/components/image`을(를) 가리킵니다.

3. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml` 파일 검사:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   `sling:resourceSuperType`이(가) `core/wcm/components/image/v2/image`을(를) 가리킵니다. 이는 WKND SPA 이미지 구성 요소가 핵심 구성 요소 이미지의 기능을 상속함을 나타냅니다.

   [프록시 패턴](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) 슬링 리소스 상속이라고도 하는 Sling 리소스 상속은 하위 구성 요소가 기능을 상속하고 원하는 경우 동작을 확장/재정의할 수 있는 강력한 디자인 패턴입니다. Sling 상속은 여러 수준의 상속을 지원하므로 궁극적으로 새 `Card` 구성 요소는 핵심 구성 요소 이미지의 기능을 상속합니다.

   많은 개발 팀이 D.R.Y.가 되도록 노력합니다(반복하지 마십시오). Sling 상속을 사용하면 AEM에서 이 작업을 수행할 수 있습니다.

4. `card` 폴더 아래에서 `_cq_dialog/.content.xml` 파일을 엽니다.

   이 파일은 `Card` 구성 요소에 대한 구성 요소 대화 상자 정의입니다. Sling 상속을 사용하는 경우 [Sling 리소스 병합](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html)의 기능을 사용하여 대화 상자의 일부를 재정의하거나 확장할 수 있습니다. 이 샘플에서는 작성자의 추가 데이터를 캡처하여 카드 구성 요소를 채우기 위한 새 탭이 대화 상자에 추가되었습니다.

   개발자는 `sling:orderBefore`과(와) 같은 속성을 사용하여 새 탭이나 양식 필드를 삽입할 위치를 선택할 수 있습니다. 이 경우 `Text` 탭은 `asset` 탭 앞에 삽입됩니다. Sling 리소스 병합을 최대한 활용하려면 [이미지 구성 요소 대화 상자](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)의 원래 대화 상자 노드 구조를 알고 있어야 합니다.

5. `card` 폴더 아래에서 `_cq_editConfig.xml` 파일을 엽니다. 이 파일은 AEM 작성 UI의 드래그 앤 드롭 동작을 지시합니다. 이미지 구성 요소를 확장할 때 리소스 유형이 구성 요소 자체와 일치해야 합니다. `<parameters>` 노드 검토:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   대부분의 구성 요소에는 `cq:editConfig`이(가) 필요하지 않습니다. 이미지 및 이미지 구성 요소의 하위 항목은 예외입니다.

6. IDE에서 `ui.frontend` 모듈로 전환하여 `ui.frontend/src/app/components/card`(으)로 이동:

   ![Angular 구성 요소 시작](assets/extend-component/angular-card-component-start.png)

7. `card.component.ts` 파일을 검사합니다.

   표준 `MapTo` 함수를 사용하여 AEM `Card` 구성 요소에 매핑하기 위해 구성 요소를 이미 스터드했습니다.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   `src`, `alt` 및 `title`에 대한 클래스에서 세 개의 `@Input` 매개 변수를 검토하십시오. 이는 Angular 구성 요소에 매핑되는 AEM 구성 요소의 예상 JSON 값입니다.

8. `card.component.html` 파일을 엽니다.

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   이 예제에서는 `card.component.ts`의 `@Input` 매개 변수를 전달하여 기존 Angular 이미지 구성 요소 `app-image`을(를) 다시 사용하도록 선택했습니다. 튜토리얼 뒷부분에서 추가 속성이 추가되고 표시됩니다.

## 템플릿 정책 업데이트

이 초기 `Card` 구현에서는 AEM SPA 편집기의 기능을 검토합니다. 초기 `Card` 구성 요소를 보려면 템플릿 정책을 업데이트해야 합니다.

1. 아직 배포하지 않은 경우 AEM의 로컬 인스턴스에 시작 코드를 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)의 SPA 페이지 템플릿으로 이동합니다.
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 `Card` 구성 요소를 허용된 구성 요소로 추가합니다.

   ![레이아웃 컨테이너 정책 업데이트](assets/extend-component/card-component-allowed.png)

   정책에 대한 변경 내용을 저장하고 `Card` 구성 요소를 허용된 구성 요소로 관찰합니다.

   ![카드 구성 요소를 허용된 구성 요소로 사용](assets/extend-component/card-component-allowed-layout-container.png)

## 작성자 초기 카드 구성 요소

그런 다음 AEM SPA 편집기를 사용하여 `Card` 구성 요소를 작성합니다.

1. [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)&#x200B;(으)로 이동합니다.
2. `Edit` 모드에서 `Card` 구성 요소를 `Layout Container`에 추가합니다.

   ![새 구성 요소 삽입](assets/extend-component/insert-custom-component.png)

3. 자산 파인더에서 `Card` 구성 요소로 이미지를 끌어서 놓습니다.

   ![이미지 추가](assets/extend-component/card-add-image.png)

4. `Card` 구성 요소 대화 상자를 열고 **텍스트** 탭이 추가된 것을 확인합니다.
5. **텍스트** 탭에 다음 값을 입력하십시오.

   ![텍스트 구성 요소 탭](assets/extend-component/card-component-text.png)

   **카드 경로** - SPA 홈 페이지 아래에서 페이지를 선택하십시오.

   **CTA 텍스트** - &quot;자세한 내용&quot;

   **카드 제목** - 비워 둠

   **연결된 페이지에서 제목 가져오기** - 확인란을 선택하여 true를 나타냅니다.

6. **자산 메타데이터** 탭을 업데이트하여 **대체 텍스트** 및 **캡션**&#x200B;에 대한 값을 추가하십시오.

   현재 대화 상자를 업데이트한 후 추가 변경 사항이 표시되지 않습니다. 새 필드를 Angular 구성 요소에 노출하려면 `Card` 구성 요소에 대한 Sling 모델을 업데이트해야 합니다.

7. 새 탭을 열고 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card)&#x200B;(으)로 이동합니다. `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` 아래의 콘텐츠 노드를 검사하여 `Card` 구성 요소 콘텐츠를 찾으십시오.

   ![CRXDE-Lite 구성 요소 속성](assets/extend-component/crxde-lite-properties.png)

   `cardPath`, `ctaText`, `titleFromPage` 속성이 대화 상자에서 지속되는지 확인하십시오.

## 카드 Sling 모델 업데이트

궁극적으로 구성 요소 대화 상자의 값을 Angular 구성 요소에 노출하려면 `Card` 구성 요소에 대해 JSON을 채우는 Sling 모델을 업데이트해야 합니다. 또한 다음과 같은 두 가지 비즈니스 논리를 구현할 수 있습니다.

* `titleFromPage`에서 **true**&#x200B;이면 `cardPath`에 지정된 페이지의 제목을 반환하고, 그렇지 않으면 `cardTitle` 텍스트 필드의 값을 반환합니다.
* `cardPath`에서 지정한 페이지의 마지막 수정 날짜를 반환합니다.

선택한 IDE로 돌아가서 `core` 모듈을 엽니다.

1. `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`에서 `Card.java` 파일을 엽니다.

   `Card` 인터페이스가 현재 `com.adobe.cq.wcm.core.components.models.Image`을(를) 확장하므로 `Image` 인터페이스의 메서드를 상속합니다. `Image` 인터페이스는 Sling 모델을 JSON으로 내보내고 SPA 편집기에서 매핑할 수 있도록 하는 `ComponentExporter` 인터페이스를 이미 확장합니다. 따라서 [사용자 지정 구성 요소 챕터](custom-component.md)에서와 같이 `ComponentExporter` 인터페이스를 명시적으로 확장할 필요가 없습니다.

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

   이러한 메서드는 JSON 모델 API를 통해 노출되고 Angular 구성 요소에 전달됩니다.

3. `CardImpl.java` 열기 `Card.java` 인터페이스의 구현입니다. 이 구현은 자습서를 가속화하기 위해 부분적으로 자세히 설명되어 있습니다.  Sling 모델 내보내기를 통해 Sling 모델을 JSON으로 serialize할 수 있도록 `@Model` 및 `@Exporter` 주석을 사용하십시오.

   `CardImpl.java`은(는) 또한 이미지 핵심 구성 요소에서 논리를 다시 쓰지 않도록 Sling 모델에 대해 [위임 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)을 사용합니다.

4. 다음 줄을 확인하십시오.

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   위의 주석은 `Card` 구성 요소의 `sling:resourceSuperType` 상속을 기반으로 이름이 `image`인 이미지 개체를 인스턴스화합니다.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   그러면 논리를 직접 작성하지 않고도 `image` 개체를 사용하여 `Image` 인터페이스에서 정의한 메서드를 구현할 수 있습니다. 이 기술은 `getSrc()`, `getAlt()` 및 `getTitle()`에 사용됩니다.

5. 그런 다음 `initModel()` 메서드를 구현하여 `cardPath` 값을 기준으로 개인 변수 `cardPage`을(를) 시작합니다

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Sling 모델이 초기화될 때 `@PostConstruct initModel()`이(가) 호출되므로 모델의 다른 메서드에서 사용할 수 있는 개체를 초기화할 수 있습니다. `pageManager`은(는) `@ScriptVariable` 주석을 통해 Sling 모델에서 사용할 수 있는 여러 [Java™ 지원 전역 개체](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) 중 하나입니다. [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 메서드는 경로를 사용하여 AEM [Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) 개체를 반환하거나, 경로가 올바른 페이지를 가리키지 않으면 null을 반환합니다.

   이렇게 하면 `cardPage` 변수가 초기화됩니다. 이 변수는 다른 새 메서드에서 연결된 기본 페이지에 대한 데이터를 반환하는 데 사용됩니다.

6. 작성자 대화 상자에 저장된 JCR 속성에 이미 매핑된 전역 변수를 검토합니다. `@ValueMapValue` 주석은 매핑을 자동으로 수행하는 데 사용됩니다.

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
   > 여기에서 [완료된 CardImpl.java를 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. 터미널 창을 열고 `core` 디렉터리의 Maven `autoInstallBundle` 프로필을 사용하여 `core` 모듈에 대한 업데이트만 배포합니다.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   [AEM 6.x](overview.md#compatibility)을(를) 사용하는 경우 `classic` 프로필을 추가하십시오.

9. [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)에서 JSON 모델 응답을 보고 `wknd-spa-angular/components/card`을(를) 검색합니다.

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

   JSON 모델은 `CardImpl` Sling 모델에서 메서드를 업데이트한 후 추가 키/값 쌍으로 업데이트됩니다.

## Angular 구성 요소 업데이트

이제 JSON 모델이 `ctaLinkURL`, `ctaText`, `cardTitle` 및 `cardLastModified`에 대한 새 속성으로 채워졌으므로 이를 표시하도록 Angular 구성 요소를 업데이트할 수 있습니다.

1. IDE로 돌아가서 `ui.frontend` 모듈을 엽니다. 필요한 경우 새 터미널 창에서 Webpack 개발 서버를 시작하여 변경 사항을 실시간으로 확인합니다.

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. `ui.frontend/src/app/components/card/card.component.ts`에서 `card.component.ts`을(를) 엽니다. 새 모델을 캡처하려면 추가 `@Input` 주석을 추가하십시오.

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

3. call to action이 준비되었는지 확인하고 `cardLastModified` 입력을 기반으로 날짜/시간 문자열을 반환하는 메서드를 추가합니다.

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

4. `card.component.html`을(를) 열고 다음 태그를 추가하여 제목, call to action 및 마지막으로 수정한 날짜를 표시합니다.

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

   제목, call to action 및 마지막으로 수정한 날짜에 스타일을 지정하기 위해 `card.component.scss`에 Sass 규칙이 이미 추가되었습니다.

   >[!NOTE]
   >
   > 완료된 [Angular 카드 구성 요소 코드는 여기에서 볼 수 있습니다](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Maven을 사용하여 프로젝트의 루트에서 AEM에 대한 모든 변경 사항을 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. 업데이트된 구성 요소를 보려면 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)&#x200B;(으)로 이동하십시오.

   ![AEM에서 카드 구성 요소를 업데이트했습니다](assets/extend-component/updated-card-in-aem.png)

7. 다음과 유사한 페이지를 만들려면 기존 콘텐츠를 다시 작성할 수 있어야 합니다.

   ![카드 구성 요소 최종 작성](assets/extend-component/final-authoring-card.png)

## 축하합니다! {#congratulations}

축하합니다. AEM 구성 요소를 확장하는 방법과 Sling 모델 및 대화 상자가 JSON 모델과 함께 작동하는 방법을 배웠습니다.

[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution)에서 완성된 코드를 항상 보거나 `Angular/extend-component-solution` 분기로 전환하여 코드를 로컬로 확인할 수 있습니다.
