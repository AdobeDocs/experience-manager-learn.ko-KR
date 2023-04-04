---
title: 7장 - 모바일 앱에서 AEM Content Services 사용 - Content Services
description: 자습서의 7장에서 Android 모바일 앱을 실행하여 AEM Content Services에서 작성된 콘텐츠를 사용합니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 0%

---

# 7장 - 모바일 앱에서 AEM Content Services 사용

이 자습서의 7장에서는 기본 Android 모바일 앱을 사용하여 AEM Content Services의 콘텐츠를 사용합니다.

## Android 모바일 앱

이 자습서에서는 **간단한 기본 Android 모바일 앱** AEM Content Services에 의해 노출된 이벤트 컨텐츠를 사용하고 표시할 수 있습니다.

의 사용 [Android](https://developer.android.com/) 은(는) 크게 중요하지 않으며, 소비되는 모바일 앱을 모바일 플랫폼용 프레임워크(예: iOS)에 쓸 수 있습니다.

Android는 Windows, macOs 및 Linux에서 Android 에뮬레이터를 실행하는 기능, 인기도, AEM 개발자가 잘 이해하는 언어인 Java로 쓸 수 있기 때문에 자습서에 사용됩니다.

*이 자습서의 Android 모바일 앱은&#x200B;**not**Android 모바일 앱을 빌드하거나 Android 개발 우수 사례를 전달하는 방법을 지시하는 대신, 모바일 애플리케이션에서 AEM Content Services를 사용할 수 있는 방법을 보여 주기 위한 것입니다.*

### AEM Content Services가 모바일 앱 경험을 구동하는 방법

![모바일 앱과 Content Services 매핑](assets/chapter-7/content-services-mapping.png)

1. 다음 **로고** 에 의해 정의됨 [!DNL Events API] 페이지 **이미지 구성 요소**.
1. 다음 **태그 라인** 에 정의된 대로 [!DNL Events API] 페이지 **텍스트 구성 요소**.
1. 이 **이벤트 목록** 은 구성된 을 통해 노출된 이벤트 컨텐츠 조각의 직렬화에서 파생됩니다 **컨텐츠 조각 목록 구성 요소**.

## 모바일 앱 데모

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### 비로컬 호스트를 사용하도록 모바일 앱 구성

AEM 게시가 실행되고 있지 않은 경우 **http://localhost:4503** 호스트 및 포트는 모바일 앱의 [!DNL Settings] AEM 게시 호스트/포트를 가리킵니다.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## 로컬에서 모바일 앱 실행

1. 를 다운로드하여 설치합니다. [Android Studio](https://developer.android.com/studio/install) android 에뮬레이터를 설치하려면 다음을 수행하십시오.
1. **다운로드** android [!DNL APK] 파일 [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 열기 **Android Studio**
   * Android Studio를 처음 시작할 때 설치 메시지가 표시됩니다 [!DNL Android SDK] 이 표시됩니다. 기본값을 적용하고 설치를 완료합니다.
1. Android Studio를 열고 을(를) 선택합니다. **프로필 또는 디버그 APK**
1. APK 파일(**wknd-mobile.x.x.apk**)를 2단계에서 다운로드한 다음 를 클릭합니다 **확인**
   * 메시지가 표시되면 **새 폴더 만들기**, 또는 **기존 사용**, 선택 **기존 사용**.
1. Android Studio를 처음 시작할 때 **wknd-mobile.x.x** 프로젝트 목록에서 **모듈 설정 열기**.
   * 아래 **모듈 > wknd-mobile.x.x > 종속성 탭**, 선택 **Android API 29 Platform**. 확인 을 눌러 변경 사항을 닫고 저장합니다.
   * 이렇게 하지 않으면 에뮬레이터를 실행하려고 할 때 &quot;Android SDK를 선택하십시오&quot; 오류가 표시됩니다.
1. 를 엽니다. **AVD 관리자** 선택 **도구 > AVD 관리자** 또는 **AVD 관리자** 상단 막대의 아이콘.
1. 에서 **AVD 관리자** 창 **+ 가상 장치 만들기...** 아직 장치가 등록되지 않은 경우.
   1. 왼쪽에서 **전화** 카테고리.
   1. 선택 **픽셀 2**.
   1. 을(를) 클릭합니다. **다음** 버튼을 클릭합니다.
   1. 선택 **Q** with **API 레벨 29**.
      * AVD Manager가 처음 실행되면 버전이 지정된 API를 다운로드하라는 메시지가 표시됩니다. &quot;Q&quot; 릴리스 옆에 있는 다운로드 링크를 클릭하고 다운로드 및 설치를 완료합니다.
   1. 을(를) 클릭합니다. **다음** 버튼을 클릭합니다.
   1. 을(를) 클릭합니다. **완료** 버튼을 클릭합니다.
1. 닫기 **AVD 관리자** 창을 엽니다.
1. 상단 메뉴 막대에서 을(를) 선택합니다 **wknd-mobile.x.x** 에서 **구성 실행/편집** 드롭다운
1. 탭하기 **실행** 선택 항목 옆에 있는 단추 **구성 실행/편집**
1. 팝업에서 새로 만든 을 선택합니다 **[!DNL Pixel 2 API 29]** 가상 장치 및 탭 **확인**
1. 만약 [!DNL WKND Mobile] 앱이 즉시 로드되지 않고 **[!DNL WKND]** 아이콘을 사용합니다.
   * 에뮬레이터가 실행되었지만 에뮬레이터의 화면이 검정색으로 남아 있는 경우 **전원** 에뮬레이터 창 옆에 있는 에뮬레이터의 도구 창에 있는 단추입니다.
   * 가상 장치 내에서 스크롤하려면 마우스 오른쪽 단추를 누르고 드래그합니다.
   * AEM에서 컨텐츠를 새로 고치려면 새로 고침 아이콘이 표시되고 릴리스가 표시될 때까지 맨 위에서 아래로 당기십시오.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## 모바일 앱 코드

이 섹션에서는 가장 상호 작용하며 AEM Content Services 및 JSON 출력에 따라 다른 Android 모바일 앱 코드를 강조 표시합니다.

로드 시 모바일 앱에서 다음을 수행합니다 `HTTP GET` to `/content/wknd-mobile/en/api/events.model.json` 모바일 앱을 구동할 컨텐츠를 제공하도록 구성된 AEM Content Services 끝점입니다.

이벤트 API(`/content/wknd-mobile/en/api/events.model.json`가 잠겨 있는 경우 모바일 앱을 코딩하여 JSON 응답의 특정 위치에서 특정 정보를 찾을 수 있습니다.

### 높은 수준 코드 흐름

1. 열기 [!DNL WKND Mobile] 앱이 을 호출합니다 `HTTP GET` 의 AEM Publish에 요청 `/content/wknd-mobile/en/api/events.model.json` 콘텐츠를 수집하여 모바일 앱의 UI를 채우십시오.
2. AEM에서 콘텐츠를 받으면 모바일 앱의 세 가지 보기 요소 각각인 **로고, 태그 라인 및 이벤트 목록**&#x200B;는 AEM의 콘텐츠로 초기화됩니다.
   * AEM 컨텐츠에 모바일 앱의 보기 요소에 바인딩하기 위해 각 AEM 구성 요소를 나타내는 JSON은 Java POJO에 매핑된 개체이며, 이 개체는 Android 보기 요소에 바인딩됩니다.
      * 이미지 구성 요소 JSON → 로고 POJO → 로고 ImageView
      * 텍스트 구성 요소 JSON → TagLine POJO → 텍스트 이미지 보기
      * 컨텐츠 조각 목록 JSON → 이벤트 POJO →Events RecyclerView
   * *더 큰 JSON 응답 내에서 잘 알려진 위치 때문에 모바일 앱 코드는 JSON을 POJO에 매핑할 수 있습니다. &quot;image&quot;, &quot;text&quot; 및 &quot;contentfragmentlist&quot;의 JSON 키는 지원되는 AEM 구성 요소의 노드 이름에 의해 결정됩니다. 이러한 노드 이름이 변경되면 JSON 데이터에서 필수 콘텐츠를 소싱하는 방법을 알 수 없으므로 모바일 앱이 중단됩니다.*

#### AEM Content Services 끝점 호출

다음은 모바일 앱의 코드를 증류하는 것입니다 `MainActivity` 는 모바일 앱 경험을 구동하는 컨텐츠를 수집하기 위해 AEM Content Services를 호출해야 합니다.

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

`onCreate(..)` 는 모바일 앱에 대한 초기화 후크이며 3개의 사용자 지정을 등록합니다 `ViewBinders` json을 구문 분석하고 값을 `View` 요소를 생성하지 않습니다.

`initApp(...)` 그런 다음 이 호출됩니다. 이 호출되면 AEM Publish에서 AEM Content Services 종단점에 HTTP GET을 요청하여 컨텐츠를 수집합니다. 유효한 JSON 응답을 수신하면 JSON 응답이 각 JSON에 전달됩니다 `ViewBinder` JSON을 구문 분석하고 모바일에 바인딩하는 책임이 있습니다 `View` 요소를 생성하지 않습니다.

#### JSON 응답 구문 분석

다음으로, `LogoViewBinder`과 같은 두 가지 주요 고려 사항은 간단하지만 몇 가지 중요한 고려 사항이 있습니다.

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

의 첫 번째 줄 `bind(...)` 키를 통해 JSON 응답을 탐색합니다 **항목→ 루트 →:항목** 구성 요소가 추가된 AEM 레이아웃 컨테이너를 나타냅니다.

여기에서 이름이 인 키에 대해 확인이 수행됩니다 **이미지**&#x200B;이미지 구성 요소를 나타내는 : 이 노드 이름→ JSON 키가 안정해야 합니다. 이 개체가 있으면 읽기 및 매핑됩니다 [사용자 지정 이미지 POJO](#image-pojo) 잭슨 `ObjectMapper` 라이브러리. 이미지 POJO는 아래에 탐색되었습니다.

마지막으로 로고는 `src` 를 사용하여 Android ImageView에 로드됩니다. [!DNL Glide] 도우미 라이브러리입니다.

Adobe에서는 AEM 스키마, 호스트 및 포트를( `aemHost`)를 AEM Content Services로 AEM Publish 인스턴스에 추가하면 JCR 경로(예: `/content/dam/wknd-mobile/images/wknd-logo.png`)를 참조한 컨텐츠에 추가할 수 있습니다.

#### 이미지 POJO{#image-pojo}

선택 사항이지만 [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) Gson과 같은 다른 라이브러리에서 제공하는 유사한 기능을 사용하면 기본 JSON 개체 자체를 직접 처리하지 않고도 복잡한 JSON 구조를 Java POJO에 매핑할 수 있습니다. 이 간단한 경우에는 `src` 키 `image` JSON 개체, 대상 `src` 를 통해 직접 이미지 POJO에 있는 속성 `@JSONProperty` 주석.

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

JSON 개체에서 더 많은 데이터 포인트를 선택해야 하는 이벤트 POJO는 간단한 이미지보다 이 기법의 이점을 더 많이 활용하며 우리가 원하는 것은 `src`.

## 모바일 앱 경험 살펴보기

이제 AEM Content Services에서 기본 모바일 경험을 구현하는 방법을 이해했으므로 다음 단계를 수행하고 모바일 앱에 반영된 변경 사항을 볼 수 있습니다.

각 단계 후에 모바일 앱을 새로 고침하고 모바일 경험에 대한 업데이트를 확인합니다.

1. 만들기 및 게시 **새 [!DNL Event] 컨텐츠 조각**
1. 게시 취소 **기존 [!DNL Event] 컨텐츠 조각**
1. 에 업데이트 게시 **태그 라인**

## 축하합니다

**AEM Headless 자습서를 완료했습니다!**

AEM Content Services 및 AEM as a Headless CMS에 대해 자세히 알아보려면 Adobe의 다른 설명서 및 지원 자료를 참조하십시오.

* [컨텐츠 조각 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM 코어 구성 요소 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)
* [AEM WCM 코어 구성 요소 구성 요소 라이브러리](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM 코어 구성 요소 GitHub 프로젝트](https://github.com/adobe/aem-core-wcm-components)
* [구성 요소 익스포터의 코드 샘플](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
