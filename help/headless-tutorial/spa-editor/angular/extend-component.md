---
title: 구성 요소 확장 | AEM SPA 편집기 및 Angular 시작하기
description: AEM SPA 편집기에서 사용할 기존 핵심 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소에 속성 및 콘텐츠를 추가하는 방법을 이해하는 것은 AEM SPA Editor 구현의 기능을 확장하는 강력한 기술입니다. Sling 리소스 병합의 Sling 모델 및 기능을 확장하기 위해 위임 패턴을 사용하는 방법에 대해 알아봅니다.
feature: SPA Editor, Core Components
version: Cloud Service
jira: KT-5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
duration: 621
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1713'
ht-degree: 0%

---

# 핵심 구성 요소 확장 {#extend-component}

AEM SPA 편집기에서 사용할 기존 핵심 구성 요소를 확장하는 방법을 알아봅니다. 기존 구성 요소를 확장하는 방법을 이해하는 것은 AEM SPA Editor 구현의 기능을 사용자 정의하고 확장하는 강력한 기술입니다.

## 목표

1. 추가 속성 및 콘텐츠를 사용하여 기존 핵심 구성 요소를 확장합니다.
2. 을 사용하여 구성 요소 상속의 기본 사항 이해 `sling:resourceSuperType`.
3. 사용 방법 알아보기 [전달 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 기존 논리 및 기능을 재사용할 수 있도록 Sling 모델.

## 빌드할 내용

이 장에서는 `Card` 구성 요소가 생성됩니다. 다음 `Card` 구성 요소가 [이미지 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 제목 및 콜 투 액션 버튼과 같은 추가 콘텐츠 필드를 추가하여 SPA 내의 다른 콘텐츠에 대한 티저 역할을 수행합니다.

![카드 구성 요소의 최종 작성](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> 실제 구현에서는 를 사용하는 것이 더 적합할 수 있습니다. [티저 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html) 를 확장하는 것 보다 [이미지 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 을(를) 만들려면 `Card` 구성 요소는 프로젝트 요구 사항에 따라 달라집니다. 항상 를 사용하는 것이 좋습니다. [핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 가능한 경우 직접

## 사전 요구 사항

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment).

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

   사용 중인 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 기존 패키지를 위한 완료된 패키지 설치 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0). 에서 제공한 이미지 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest) 는 WKND SPA에서 재사용됩니다. 패키지를 설치하려면 [AEM 패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp).

   ![패키지 관리자 설치 wknd.all](./assets/map-components/package-manager-wknd-all.png)

에서 완성된 코드를 항상 볼 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) 또는 분기로 전환하여 코드를 로컬에서 확인합니다. `Angular/extend-component-solution`.

## Inspect 초기 카드 구현

초기 카드 구성 요소는 챕터 스타터 코드에서 제공했습니다. Inspect 는 카드 구현의 시작점입니다.

1. 선택한 IDE에서 `ui.apps` 모듈.
2. 다음으로 이동 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` 및 보기 `.content.xml` 파일.

   ![카드 구성 요소 AEM 정의 시작](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   속성 `sling:resourceSuperType` 포인트 대상 `wknd-spa-angular/components/image` 를 나타내는 `Card` 구성 요소는 WKND SPA 이미지 구성 요소에서 기능을 상속합니다.

3. 파일 Inspect `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   다음 사항에 주목하십시오. `sling:resourceSuperType` 포인트 대상 `core/wcm/components/image/v2/image`. 이는 WKND SPA 이미지 구성 요소가 핵심 구성 요소 이미지의 기능을 상속함을 나타냅니다.

   (으)로도 알려짐 [프록시 패턴](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling 리소스 상속은 하위 구성 요소가 기능을 상속하고 필요할 때 동작을 확장/무시할 수 있도록 하는 강력한 디자인 패턴입니다. Sling 상속은 여러 수준의 상속을 지원하므로 궁극적으로 새로운 `Card` 구성 요소는 핵심 구성 요소 이미지의 기능을 상속합니다.

   많은 개발 팀이 D.R.Y.가 되도록 노력합니다(반복하지 마십시오). Sling 상속을 사용하면 AEM에서 이 작업을 수행할 수 있습니다.

4. 아래 `card` 폴더, 파일을 엽니다. `_cq_dialog/.content.xml`.

   이 파일은 의 구성 요소 대화 상자 정의입니다. `Card` 구성 요소. Sling 상속을 사용하는 경우 [Sling 리소스 병합](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) 를 클릭하여 대화 상자의 일부를 재정의하거나 확장합니다. 이 샘플에서는 작성자의 추가 데이터를 캡처하여 카드 구성 요소를 채우기 위한 새 탭이 대화 상자에 추가되었습니다.

   다음과 같은 속성 `sling:orderBefore` 개발자가 새 탭이나 양식 필드를 삽입할 위치를 선택할 수 있도록 허용합니다. 이 경우 `Text` 탭이 다음 앞에 삽입됨: `asset` 탭. Sling 리소스 병합을 최대한 활용하려면 의 원래 대화 상자 노드 구조를 알고 있어야 합니다. [이미지 구성 요소 대화 상자](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. 아래 `card` 폴더, 파일을 엽니다. `_cq_editConfig.xml`. 이 파일은 AEM 작성 UI의 드래그 앤 드롭 동작을 지시합니다. 이미지 구성 요소를 확장할 때 리소스 유형이 구성 요소 자체와 일치해야 합니다. 리뷰 `<parameters>` 노드:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   대부분의 구성 요소에는 `cq:editConfig`, 이미지 및 이미지 구성 요소의 하위 항목은 예외입니다.

6. IDE에서 `ui.frontend` 모듈, 이동 `ui.frontend/src/app/components/card`:

   ![Angular 구성 요소 시작](assets/extend-component/angular-card-component-start.png)

7. 파일 Inspect `card.component.ts`.

   구성 요소를 AEM에 매핑하기 위해 이미 스터빙했습니다 `Card` 표준을 사용하는 구성 요소 `MapTo` 함수.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   세 가지 항목 검토 `@Input` 클래스의 매개 변수 `src`, `alt`, 및 `title`. 이는 Angular 구성 요소에 매핑되는 AEM 구성 요소의 예상 JSON 값입니다.

8. 파일 열기 `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   이 예제에서는 기존 Angular 이미지 구성 요소를 다시 사용하도록 선택했습니다 `app-image` 을(를) 단순히 `@Input` 매개 변수 `card.component.ts`. 튜토리얼 뒷부분에서 추가 속성이 추가되고 표시됩니다.

## 템플릿 정책 업데이트

이 이니셜로 `Card` 구현 AEM SPA Editor에서 기능을 검토합니다. 이니셜을 보려면 `Card` 구성 요소에 템플릿 정책을 업데이트해야 합니다.

1. 아직 배포하지 않은 경우 스타터 코드를 AEM의 로컬 인스턴스에 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 다음 SPA 페이지 템플릿으로 이동합니다. [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 레이아웃 컨테이너의 정책을 업데이트하여 새 항목 추가 `Card` 구성 요소를 허용된 구성 요소로 사용:

   ![레이아웃 컨테이너 정책 업데이트](assets/extend-component/card-component-allowed.png)

   정책에 대한 변경 사항을 저장하고 다음을 확인합니다. `Card` 구성 요소를 허용된 구성 요소로 사용:

   ![카드 구성 요소를 허용된 구성 요소로 사용](assets/extend-component/card-component-allowed-layout-container.png)

## 작성자 초기 카드 구성 요소

다음으로, 를 작성합니다. `Card` AEM SPA 편집기를 사용하는 구성 요소입니다.

1. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. 위치 `Edit` 모드, 추가 `Card` 구성 요소를 `Layout Container`:

   ![새 구성 요소 삽입](assets/extend-component/insert-custom-component.png)

3. 자산 파인더에서 로 이미지를 끌어다 놓습니다. `Card` 구성 요소:

   ![이미지 추가](assets/extend-component/card-add-image.png)

4. 를 엽니다. `Card` 구성 요소 대화 상자 및 추가 항목 확인 **텍스트** 탭.
5. 다음에 대한 값을 입력하십시오. **텍스트** 탭:

   ![텍스트 구성 요소 탭](assets/extend-component/card-component-text.png)

   **카드 경로** - SPA 홈 페이지 아래에서 페이지를 선택합니다.

   **CTA 텍스트** - &quot;자세히 보기&quot;

   **카드 제목** - 비워 둡니다.

   **링크된 페이지에서 제목 가져오기** - 확인란을 선택하여 true를 나타냅니다.

6. 업데이트 **자산 메타데이터** 값을 추가할 탭 **대체 텍스트** 및 **캡션**.

   현재 대화 상자를 업데이트한 후 추가 변경 사항이 표시되지 않습니다. 새 필드를 Angular 구성 요소에 노출하려면 의 슬링 모델을 업데이트해야 합니다. `Card` 구성 요소.

7. 새 탭을 열고 다음으로 이동 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). 아래 콘텐츠 노드 Inspect `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` 을(를) 찾으려면 `Card` 구성 요소 컨텐츠.

   ![CRXDE-Lite 구성 요소 속성](assets/extend-component/crxde-lite-properties.png)

   속성을 확인합니다. `cardPath`, `ctaText`, `titleFromPage` 대화 상자에서 지속됩니다.

## 카드 Sling 모델 업데이트

궁극적으로 구성 요소 대화 상자의 값을 Angular 구성 요소에 노출하려면 의 JSON을 채우는 Sling 모델을 업데이트해야 합니다. `Card` 구성 요소. 또한 다음과 같은 두 가지 비즈니스 논리를 구현할 수 있습니다.

* If `titleFromPage` 끝 **true**&#x200B;에 지정된 페이지의 제목을 반환합니다. `cardPath` 그렇지 않으면 값 반환 `cardTitle` textfield.
* 에 지정된 페이지의 마지막 수정 날짜 반환 `cardPath`.

선택한 IDE로 돌아가서 `core` 모듈.

1. 파일 열기 `Card.java` 위치: `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   다음을 확인합니다. `Card` 인터페이스가 현재 확장됨 `com.adobe.cq.wcm.core.components.models.Image` 따라서 의 메서드를 상속합니다 `Image` 인터페이스. 다음 `Image` 인터페이스가 이미 다음을 확장합니다. `ComponentExporter` 슬링 모델을 JSON으로 내보내고 SPA 편집기에서 매핑할 수 있는 인터페이스입니다. 따라서 명시적으로 확장할 필요가 없습니다 `ComponentExporter` 인터페이스(예: [사용자 지정 구성 요소 챕터](custom-component.md).

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

3. 열기 `CardImpl.java`. 이는 의 구현입니다. `Card.java` 인터페이스. 이 구현은 자습서를 가속화하기 위해 부분적으로 자세히 설명되어 있습니다.  의 사용에 주목하십시오. `@Model` 및 `@Exporter` 슬링 모델 익스포터를 통해 슬링 모델을 JSON으로 일련화할 수 있도록 하는 주석입니다.

   `CardImpl.java` 는 를 사용합니다 [Sling 모델의 전달 패턴](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 이미지 핵심 구성 요소에서 논리를 다시 작성하지 않도록 합니다.

4. 다음 줄을 확인하십시오.

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   위의 주석은 이라는 이미지 개체를 인스턴스화합니다. `image` 를 기반으로 함 `sling:resourceSuperType` 상속 `Card` 구성 요소.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   그런 다음 를 간단하게 사용할 수 있습니다. `image` 로 정의된 메서드를 구현할 개체 `Image` 직접 논리를 작성하지 않고도 인터페이스를 사용할 수 있습니다. 이 기법은 다음 용도로 사용됩니다. `getSrc()`, `getAlt()`, 및 `getTitle()`.

5. 그런 다음 를 구현합니다. `initModel()` 개인 변수를 시작하는 방법 `cardPage` 의 값을 기반으로 함 `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   다음 `@PostConstruct initModel()` Sling 모델이 초기화되면 가 호출되므로 모델의 다른 메서드에서 사용할 수 있는 개체를 초기화할 수 있습니다. 다음 `pageManager` 여러 개 중 하나 [Java™ 지원 전역 개체](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) 을(를) 통해 Sling 모델에 사용 가능 `@ScriptVariable` 주석. 다음 [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 메서드는 경로를 사용하고 AEM을 반환합니다. [페이지](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) 경로가 유효한 페이지를 가리키지 않으면 개체 또는 null입니다.

   이렇게 하면 `cardPage` 변수 : 다른 새 메서드에서 연결된 기본 페이지에 대한 데이터를 반환하는 데 사용됩니다.

6. 작성자 대화 상자에 저장된 JCR 속성에 이미 매핑된 전역 변수를 검토합니다. 다음 `@ValueMapValue` 주석은 매핑을 자동으로 수행하는 데 사용됩니다.

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

   이러한 변수는 다음에 대한 추가 메서드를 구현하는 데 사용됩니다. `Card.java` 인터페이스.

7. 에 정의된 추가 방법 구현 `Card.java` 인터페이스:

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
   > 다음을 볼 수 있습니다. [여기서 CardImpl.java를 완료했습니다.](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. 터미널 창을 열고 업데이트에 대한 `core` Maven을 사용한 모듈 `autoInstallBundle` 프로필의 `core` 디렉토리.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   사용 중인 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필.

9. 다음 위치에서 JSON 모델 응답을 봅니다. [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) 및 검색 `wknd-spa-angular/components/card`:

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

이제 JSON 모델에 의 새 속성이 채워집니다. `ctaLinkURL`, `ctaText`, `cardTitle`, 및 `cardLastModified` 이러한 구성 요소를 표시하도록 Angular 구성 요소를 업데이트할 수 있습니다.

1. IDE로 돌아가서 `ui.frontend` 모듈. 필요한 경우 새 터미널 창에서 Webpack 개발 서버를 시작하여 변경 사항을 실시간으로 확인합니다.

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. 열기 `card.component.ts` 위치: `ui.frontend/src/app/components/card/card.component.ts`. 추가 추가 `@Input` 새 모델을 캡처하는 주석:

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

3. 콜 투 액션이 준비되었는지 확인하고 를 기반으로 날짜/시간 문자열을 반환하는 메서드를 추가합니다. `cardLastModified` 입력:

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

4. 열기 `card.component.html` 그리고 다음 마크업을 추가하여 제목, 클릭 유도 문안 및 마지막 수정 날짜를 표시합니다.

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

   Sass 규칙이 이미 다음 위치에 추가되었습니다. `card.component.scss` 제목의 스타일을 지정하려면 클릭 유도 문안 및 마지막 수정 날짜.

   >[!NOTE]
   >
   > 완료된 항목을 볼 수 있습니다 [Angular 카드 구성 요소 코드가 여기에 있음](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Maven을 사용하여 프로젝트의 루트에서 AEM에 전체 변경 사항을 배포합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) 업데이트된 구성 요소를 보려면:

   ![업데이트된 AEM의 카드 구성 요소](assets/extend-component/updated-card-in-aem.png)

7. 다음과 유사한 페이지를 만들려면 기존 콘텐츠를 다시 작성할 수 있어야 합니다.

   ![카드 구성 요소의 최종 작성](assets/extend-component/final-authoring-card.png)

## 축하합니다! {#congratulations}

축하합니다. AEM 구성 요소를 확장하는 방법과 Sling 모델 및 대화 상자가 JSON 모델과 함께 작동하는 방법을 배웠습니다.

에서 완성된 코드를 항상 볼 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) 또는 분기로 전환하여 코드를 로컬에서 확인합니다. `Angular/extend-component-solution`.
