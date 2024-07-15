---
title: 7장 - 모바일 앱에서 AEM Content Services 사용 - 콘텐츠 서비스
description: 자습서의 7장에서는 Android 모바일 앱을 실행하여 AEM Content Services에서 작성된 콘텐츠를 사용합니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 467
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# 7장 - 모바일 앱에서 AEM Content Services 사용

자습서의 7장에서는 기본 Android 모바일 앱을 사용하여 AEM Content Services의 콘텐츠를 사용합니다.

## Android 모바일 앱

이 자습서에서는 **간단한 기본 Android 모바일 앱**&#x200B;을 사용하여 AEM Content Services에서 노출하는 이벤트 콘텐츠를 사용하고 표시합니다.

[Android](https://developer.android.com/)을(를) 사용하는 것은 중요하지 않으며, 소비하는 모바일 앱은 모든 모바일 플랫폼(예: iOS)의 프레임워크에서 작성할 수 있습니다.

Android은 Windows, macOs 및 Linux에서 Android 에뮬레이터를 실행할 수 있는 기능, 그 인기 및 AEM 개발자가 잘 이해하는 언어인 Java로 작성할 수 있기 때문에 자습서에 사용됩니다.

*자습서의 Android 모바일 앱은 Android 모바일 앱을 빌드하거나 Android 개발 모범 사례를 전달하는 방법을 지시하기 위한 것이 아니라 모바일 애플리케이션에서 AEM Content Services를 사용하는 방법을 설명하기 위한 것입니다.*****

### AEM Content Services가 모바일 앱 경험을 제공하는 방법

![Mobile 앱과 Content Services 매핑](assets/chapter-7/content-services-mapping.png)

1. [!DNL Events API] 페이지의 **이미지 구성 요소**&#x200B;에 정의된 **로고**&#x200B;입니다.
1. [!DNL Events API] 페이지의 **텍스트 구성 요소**&#x200B;에 정의된 **태그 줄**.
1. 이 **이벤트 목록**&#x200B;은(는) 구성된 **콘텐츠 조각 목록 구성 요소**&#x200B;를 통해 노출된 이벤트 콘텐츠 조각의 직렬화에서 파생됩니다.

## 모바일 앱 데모

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### 로컬이 아닌 호스트 사용을 위한 모바일 앱 구성

AEM Publish이 **http://localhost:4503**&#x200B;에서 실행되지 않는 경우 모바일 앱의 [!DNL Settings]에서 AEM Publish 호스트/포트 속성을 가리키도록 호스트 및 포트를 업데이트할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## 로컬에서 모바일 앱 실행

1. [Android Studio](https://developer.android.com/studio/install)을(를) 다운로드하여 설치하여 Android 에뮬레이터를 설치하십시오.
1. **Android [!DNL APK] 파일 [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 다운로드**
1. **Android Studio** 열기
   * Android Studio를 처음 시작할 때 [!DNL Android SDK]을(를) 설치하라는 메시지가 표시됩니다. 기본값을 적용하고 설치를 완료합니다.
1. Android Studio를 열고 **프로필 또는 APK 디버그**&#x200B;를 선택합니다
1. 2단계에서 다운로드한 APK 파일(**wknd-mobile.x.x.apk**)을 선택하고 **확인**&#x200B;을 클릭합니다
   * **새 폴더 만들기** 또는 **기존 폴더 사용**&#x200B;을 묻는 메시지가 표시되면 **기존 폴더 사용**&#x200B;을 선택합니다.
1. Android Studio를 처음 시작할 때 프로젝트 목록에서 **wknd-mobile.x.x.x**&#x200B;을 마우스 오른쪽 단추로 클릭하고 **모듈 설정 열기**&#x200B;를 선택합니다.
   * **모듈 > wknd-mobile.x.x.x > 종속성 탭**&#x200B;에서 **Android API 29 플랫폼**&#x200B;을 선택합니다. 확인 을 탭하여 변경 내용을 닫고 저장합니다.
   * 이렇게 하지 않으면 에뮬레이터를 시작하려고 할 때 &quot;Android SDK를 선택하십시오.&quot; 오류가 표시됩니다.
1. **도구 > AVD 관리자**&#x200B;를 선택하거나 상단 막대에서 **AVD 관리자** 아이콘을 탭하여 **AVD 관리자**&#x200B;를 엽니다.
1. 아직 등록된 장치가 없으면 **AVD 관리자** 창에서 **+ 가상 장치 만들기...**&#x200B;를 클릭하십시오.
   1. 왼쪽에서 **전화** 범주를 선택합니다.
   1. **픽셀 2**&#x200B;을(를) 선택하십시오.
   1. **다음** 단추를 클릭합니다.
   1. **API 수준 29**&#x200B;을(를) 사용하여 **Q**&#x200B;을(를) 선택하십시오.
      * AVD Manager를 처음 실행하면 버전이 지정된 API를 다운로드하라는 메시지가 표시됩니다. &quot;Q&quot; 릴리스 옆에 있는 다운로드 링크를 클릭하고 다운로드 및 설치를 완료합니다.
   1. **다음** 단추를 클릭합니다.
   1. **완료** 단추를 클릭합니다.
1. **AVD 관리자** 창을 닫습니다.
1. 상단 메뉴 표시줄의 **구성 실행/편집** 드롭다운에서 **wknd-mobile.x.x.x**&#x200B;을(를) 선택합니다.
1. 선택한 **구성 실행/편집** 옆에 있는 **실행** 단추를 탭합니다.
1. 팝업에서 새로 만든 **[!DNL Pixel 2 API 29]** 가상 장치를 선택하고 **확인**&#x200B;을 누릅니다
1. [!DNL WKND Mobile] 앱이 즉시 로드되지 않으면 에뮬레이터의 Android 홈 화면에서 **[!DNL WKND]** 아이콘을 찾아 탭합니다.
   * 에뮬레이터가 시작되지만 에뮬레이터의 화면이 검정색으로 남아 있는 경우 에뮬레이터 창 옆에 있는 에뮬레이터의 도구 창에서 **전원** 단추를 탭합니다.
   * 가상 장치 내에서 스크롤하려면 길게 클릭하고 드래그합니다.
   * AEM에서 컨텐츠를 새로 고치려면 새로 고침 아이콘이 표시될 때까지 맨 위에서 아래로 끕니다
및 릴리스를 표시합니다.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## 모바일 앱 코드

이 섹션에서는 대부분의 상호 작용하고 AEM Content Services 및 해당 JSON 출력에 의존하는 Android Mobile 앱 코드를 중점적으로 다룹니다.

로드 시 모바일 앱은 모바일 앱을 실행할 콘텐츠를 제공하도록 구성된 AEM Content Services 끝점인 `HTTP GET`에서 `/content/wknd-mobile/en/api/events.model.json`까지 만듭니다.

이벤트 API(`/content/wknd-mobile/en/api/events.model.json`)의 편집 가능한 템플릿이 잠겨 있으므로 모바일 앱을 코딩하여 JSON 응답의 특정 위치에서 특정 정보를 찾을 수 있습니다.

### 높은 수준 코드 흐름

1. [!DNL WKND Mobile] 앱을 열면 `/content/wknd-mobile/en/api/events.model.json`에서 AEM Publish에 대한 `HTTP GET` 요청을 호출하여 모바일 앱의 UI를 채울 콘텐츠를 수집합니다.
2. AEM에서 콘텐츠를 받으면 모바일 앱의 세 가지 보기 요소인 **로고, 태그 줄 및 이벤트 목록**&#x200B;이 각각 AEM의 콘텐츠로 초기화됩니다.
   * AEM 콘텐츠에 모바일 앱의 보기 요소에 바인딩되기 위해 각 AEM 구성 요소를 나타내는 JSON은 Android 보기 요소에 바인딩되는 Java POJO에 매핑된 개체입니다.
      * 이미지 구성 요소 JSON → 로고 POJO → 로고 ImageView
      * 텍스트 구성 요소 JSON → TagLine POJO → Text ImageView
      * 컨텐츠 조각 목록 JSON → 이벤트 POJO →이벤트 RecyclerView
   * *더 큰 JSON 응답 내에서 잘 알려진 위치로 인해 모바일 앱 코드에서 JSON을 POJO에 매핑할 수 있습니다. &quot;image&quot;, &quot;text&quot; 및 &quot;contentfragmentlist&quot;의 JSON 키는 관련 AEM 구성 요소의 노드 이름에 따라 달라집니다. 이러한 노드 이름이 변경되면 모바일 앱에서 JSON 데이터에서 필요한 콘텐츠를 소싱하는 방법을 알 수 없게 되므로 중단됩니다.*

#### AEM Content Services 끝점 호출

다음은 모바일 앱 환경을 구현하는 콘텐츠를 수집하기 위해 AEM Content Services를 호출하는 모바일 앱 `MainActivity`의 코드 증류입니다.

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)`은(는) 모바일 앱의 초기화 후크이며 JSON을 구문 분석하고 값을 `View` 요소에 바인딩하는 역할을 하는 3개의 사용자 지정 `ViewBinders`을(를) 등록합니다.

그런 다음 `initApp(...)`을(를) 호출하여 AEM Publish의 AEM Content Services 끝점에 HTTP GET 요청을 수행하여 콘텐츠를 수집합니다. 유효한 JSON 응답을 받으면 JSON 응답을 JSON을 구문 분석하고 모바일 `View` 요소에 바인딩하는 각 `ViewBinder`에 JSON 응답이 전달됩니다.

#### JSON 응답 구문 분석

다음은 `LogoViewBinder`을(를) 살펴봅니다. 간단하지만 몇 가지 중요한 고려 사항을 강조합니다.

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

`bind(...)`의 첫 번째 줄은 구성 요소가 추가된 AEM 레이아웃 컨테이너를 나타내는 **:items → 루트 →:items** 키를 통해 JSON 응답을 탐색합니다.

여기에서는 이미지 구성 요소를 나타내는 **image** 키가 검사됩니다(즉, 이 노드 이름→ JSON 키가 안정적이어야 함). 이 개체가 있으면 Jackson `ObjectMapper` 라이브러리를 통해 [사용자 지정 이미지 POJO](#image-pojo)를 읽고 매핑합니다. 이미지 POJO는 아래에서 살펴봅니다.

마지막으로 [!DNL Glide] 도우미 라이브러리를 사용하여 로고의 `src`이(가) Android ImageView에 로드됩니다.

AEM Content Services는 JCR 경로(예: )만 제공하므로 AEM Publish 인스턴스에 `aemHost`을(를) 통해 AEM 스키마, 호스트 및 포트를 제공해야 합니다. `/content/dam/wknd-mobile/images/wknd-logo.png`)을(를) 참조된 콘텐츠로 복사합니다.

#### 이미지 POJO{#image-pojo}

선택 사항이지만 Gson과 같은 다른 라이브러리에서 제공하는 [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) 또는 유사한 기능을 사용하면 기본 JSON 개체 자체를 직접 처리하는 테디움 없이 복잡한 JSON 구조를 Java POJO에 매핑하는 데 도움이 됩니다. 이 간단한 경우 `@JSONProperty` 주석을 통해 `image` JSON 개체의 `src` 키를 이미지 POJO의 `src` 특성에 직접 매핑합니다.

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

JSON 개체에서 더 많은 데이터 포인트를 선택해야 하는 이벤트 POJO는 간단한 이미지보다 이 기술의 이점을 더 많이 얻습니다. 원하는 것은 `src`입니다.

## 모바일 앱 경험 살펴보기

AEM Content Services를 통해 기본 모바일 경험을 제공하는 방법을 이해했으므로, 학습한 내용을 사용하여 다음 단계를 수행하고 모바일 앱에 반영된 변경 사항을 확인하십시오.

각 단계 후에 모바일 앱을 풀다운하여 새로 고친 다음 모바일 경험에 대한 업데이트를 확인합니다.

1. **새 [!DNL Event] 콘텐츠 조각** 만들기 및 게시
1. **기존 [!DNL Event] 콘텐츠 조각** 게시 취소
1. Publish에서 **태그 줄** 업데이트

## 축하합니다.

**AEM Headless 자습서를 완료했습니다!**

AEM Content Services 및 AEM as a Headless CMS에 대한 자세한 내용은 Adobe의 다른 설명서 및 지원 자료를 참조하십시오.

* [콘텐츠 조각 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM 코어 구성 요소 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko-KR)
* [AEM WCM 핵심 구성 요소 구성 요소 라이브러리](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM 핵심 구성 요소 GitHub 프로젝트](https://github.com/adobe/aem-core-wcm-components)
* [구성 요소 내보내기의 코드 샘플](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
