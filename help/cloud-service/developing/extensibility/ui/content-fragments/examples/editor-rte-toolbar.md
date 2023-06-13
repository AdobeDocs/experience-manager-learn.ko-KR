---
title: 리치 텍스트 편집기(RTE) 도구 모음에 사용자 지정 단추 추가
description: AEM 콘텐츠 조각 편집기에서 사용자 지정 단추를 리치 텍스트 편집기(RTE) 도구 모음에 추가하는 방법을 알아봅니다
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
source-git-commit: e59c9d1f17c6ade169e834a21b9d5f50ac3a569e
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---


# 리치 텍스트 편집기(RTE) 도구 모음에 사용자 지정 단추 추가

![콘텐츠 조각 편집기 도구 모음 확장 예제](./assets/rte/rte-toolbar-hero.png){align="center"}

사용자 지정 단추를에 추가할 수 있습니다 **RTE 도구 모음** 를 사용하는 콘텐츠 조각 편집기 내 `rte` 확장 점입니다. 이 예에서는 이라는 사용자 지정 단추를 추가하는 방법을 보여 줍니다. _팁 추가_ 을 클릭하여 RTE 도구 모음을 실행하고 RTE 내의 콘텐츠를 수정합니다.

사용 `rte` 확장 지점 `getCustomButtons()` 메서드 하나 이상의 사용자 지정 단추를 **RTE 도구 모음**. 다음과 같이 표준 RTE 버튼을 추가하거나 제거할 수도 있습니다. _복사, 붙여넣기, 굵게, 기울임체_ 사용 `getCoreButtons()` 및 `removeButtons)` 메서드를 각각 사용합니다.

이 예에서는 사용자 지정을 사용하여 강조 표시된 메모 또는 팁을 삽입하는 방법을 보여 줍니다 _팁 추가_ 도구 모음 단추. 강조 표시된 메모 또는 팁 콘텐츠에는 HTML 요소 및 관련 CSS 클래스를 통해 적용되는 특수 서식이 있습니다. 자리 표시자 콘텐츠 및 HTML 코드는 `onClick()` 콜백 메서드 `getCustomButtons()`.

## 확장 지점

이 예제는 확장 지점까지 확장됩니다 `rte` 을 클릭하여 콘텐츠 조각 편집기의 RTE 도구 모음에 사용자 지정 단추를 추가합니다.

| AEM UI 확장 | 확장 지점 |
| ------------------------ | --------------------- | 
| [콘텐츠 조각 편집기](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [리치 텍스트 편집기 도구 모음](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## 확장 예

다음 예제에서는 _팁 추가_ rte 도구 모음의 사용자 지정 단추. 클릭 동작으로 RTE의 현재 캐럿 위치에 자리 표시자 텍스트가 삽입됩니다.

이 코드는 아이콘으로 사용자 지정 단추를 추가하고 클릭 처리기 함수를 등록하는 방법을 보여 줍니다.

### 확장 등록

`ExtensionRegistration.js`index.html 경로에 매핑되는 는 AEM 확장의 진입점이며 다음을 정의합니다.

+ 에서 RTE 도구 모음 버튼의 정의 `getCustomButtons()` 함수 `id, tooltip and icon` 속성.
+ 에서 단추의 클릭 처리기입니다. `onClick()` 함수.
+ 클릭 처리기 함수는 `state` 개체를 HTML 또는 텍스트 형식으로 RTE의 콘텐츠를 가져오는 인수로 사용하십시오. 그러나 이 예제에서는 사용되지 않습니다.
+ 클릭 처리기 함수는 명령 배열을 반환합니다. 이 배열에는 `type` 및 `value` 속성. 컨텐츠를 삽입하려면 `value` 속성 HTML 코드 조각, `type` 속성은 다음을 사용합니다. `insertContent`. 콘텐츠를 대체할 사용 사례가 있는 경우 `replaceContent` 명령 유형.

다음 `insertContent` 값은 HTML 문자열이고, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. CSS 클래스 `cmp-contentfragment__element-tip` 값을 표시하는 데 사용되는 는 위젯에서 정의되지 않고 이 콘텐츠 조각 필드가 표시되는 웹 경험에서 구현됩니다.


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {

  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {

          // RTE Toolbar custom button
          getCustomButtons: () => ([
            {
              id: "wknd-cf-tip",       // Provide a unique ID for the custom button
              tooltip: "Add Tip",      // Provide a label for the custom button
              icon: 'Note',            // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {    // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [{
                  type: "insertContent",
                  value: "<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>"
                }];
              },
            },
          ]),
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```
