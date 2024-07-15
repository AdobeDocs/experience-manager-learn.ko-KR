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
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 345
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 리치 텍스트 편집기(RTE) 도구 모음에 사용자 지정 단추 추가

AEM 콘텐츠 조각 편집기의 리치 텍스트 편집기(RTE) 도구 모음에 사용자 지정 단추를 추가하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

사용자 지정 단추는 `rte` 확장 지점을 사용하여 콘텐츠 조각 편집기의 **RTE 도구 모음**&#x200B;에 추가할 수 있습니다. 이 예에서는 RTE 도구 모음에 _팁 추가_&#x200B;라는 사용자 지정 단추를 추가하고 RTE 내에서 콘텐츠를 수정하는 방법을 보여 줍니다.

`rte` 확장 지점의 `getCustomButtons()` 메서드를 사용하면 하나 이상의 사용자 지정 단추를 **RTE 도구 모음**&#x200B;에 추가할 수 있습니다. `getCoreButtons()` 및 `removeButtons)` 메서드를 각각 사용하여 _복사, 붙여넣기, 굵게, 기울임꼴_&#x200B;과 같은 표준 RTE 단추를 추가하거나 제거할 수도 있습니다.

이 예제에서는 사용자 지정 _팁 추가_ 도구 모음 단추를 사용하여 강조 표시된 메모나 팁을 삽입하는 방법을 보여 줍니다. 강조 표시된 메모 또는 팁 콘텐츠에는 HTML 요소 및 관련 CSS 클래스를 통해 적용되는 특수 서식이 있습니다. 자리 표시자 콘텐츠 및 HTML 코드는 `getCustomButtons()`의 `onClick()` 콜백 메서드를 사용하여 삽입됩니다.

## 확장 지점

이 예제는 확장 지점 `rte`까지 확장하여 콘텐츠 조각 편집기의 RTE 도구 모음에 사용자 지정 단추를 추가합니다.

| AEM UI 확장 | 확장 지점 |
| ------------------------ | --------------------- | 
| [콘텐츠 조각 편집기](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [리치 텍스트 편집기 도구 모음](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## 확장 예

다음 예제에서는 RTE 도구 모음에 _팁 추가_ 사용자 지정 단추를 만듭니다. 클릭 작업은 RTE의 현재 캐럿 위치에 자리 표시자 텍스트를 삽입합니다.

이 코드는 아이콘으로 사용자 지정 단추를 추가하고 클릭 처리기 함수를 등록하는 방법을 보여 줍니다.

### 확장 등록

index.html 경로에 매핑된 `ExtensionRegistration.js`은(는) AEM 확장의 진입점이며 다음을 정의합니다.

+ `id, tooltip and icon` 특성이 있는 `getCustomButtons()` 함수의 RTE 도구 모음 단추 정의입니다.
+ `onClick()` 함수에서 단추의 클릭 처리기입니다.
+ 클릭 처리기 함수는 `state` 개체를 인수로 수신하여 RTE의 콘텐츠를 HTML 또는 텍스트 형식으로 가져옵니다. 그러나 이 예제에서는 사용되지 않습니다.
+ 클릭 처리기 함수는 명령 배열을 반환합니다. 이 배열에는 `type` 및 `value` 특성이 있는 개체가 있습니다. `value` 특성 HTML 코드 조각인 콘텐츠를 삽입하기 위해 `type` 특성은 `insertContent`을(를) 사용합니다. 콘텐츠를 대체할 사용 사례가 있으면 `replaceContent` 명령 유형을 사용하십시오.

`insertContent` 값은 HTML 문자열 `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`입니다. 값을 표시하는 데 사용되는 CSS 클래스 `cmp-contentfragment__element-tip`이(가) 위젯에 정의되지 않고 이 콘텐츠 조각 필드가 표시되는 웹 경험에 구현됩니다.


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
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
          getCustomButtons: () => [
            {
              id: "wknd-cf-tip", // Provide a unique ID for the custom button
              tooltip: "Add Tip", // Provide a label for the custom button
              icon: "Note", // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {
                // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [
                  {
                    type: "insertContent",
                    value:
                      '<div class="cmp-contentfragment__element-tip"><div>TIP</div><div>Add your tip text here...</div></div>',
                  },
                ];
              },
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```
