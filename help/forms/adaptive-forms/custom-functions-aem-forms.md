---
title: AEM Forms의 사용자 정의 함수
description: 적응형 양식에서 사용자 정의 함수 만들기 및 사용
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
duration: 322
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---

# 사용자 정의 함수

AEM Forms 6.5에는 규칙 편집기를 사용하여 복잡한 비즈니스 규칙을 정의하는 데 사용할 수 있는 JavaScript 함수를 정의하는 기능이 도입되었습니다.
AEM Forms에서는 이러한 다양한 사용자 정의 함수를 기본적으로 제공하지만, 사용자 정의 함수를 정의하고 여러 양식에서 사용해야 합니다.

첫 번째 사용자 지정 기능을 정의하려면 다음 단계를 따르십시오.
* [crx에 로그인](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* experience-league라는 앱에 새 폴더를 만듭니다(이 폴더 이름은 선택한 이름일 수 있음).
* 변경 사항을 저장합니다.
* experience-league 폴더에서 clientlibs라는 cq:ClientLibraryFolder 유형의 새 노드를 만듭니다.
* 새로 만든 clientlibs 폴더를 선택하고 스크린샷에 표시된 대로 allowProxy 및 categories 속성을 추가한 다음 변경 사항을 저장합니다.

![client-lib](assets/custom-functions.png)
* 라는 폴더 만들기 **js** 다음 아래에 **clientlibs** 폴더
* 라는 파일 만들기 **functions.js** 다음 아래에 **js** 폴더
* 라는 파일 만들기 **js.txt** 다음 아래에 **clientlibs** 폴더를 삭제합니다. 변경 사항을 저장합니다.
* 폴더 구조는 아래 스크린샷과 같아야 합니다.

![규칙 편집기](assets/folder-structure.png)

* functions.js를 두 번 클릭하여 편집기를 엽니다.
다음 코드를 functions.js에 복사하고 변경 사항을 저장합니다.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @return {OPTIONS} drop down options 
 */
function getCountyNamesList()
{
    var countyNames= [];
    countyNames[0] = "Santa Clara";
    countyNames[1] = "Alameda";
    countyNames[2] = "Buxor";
    countyNames[3] = "Contra Costa";
    countyNames[4] = "Merced";

    return countyNames;

}
/**
* Covert UTC to Local Time
* @name convertUTC Convert UTC Time to Local Time
* @param {string} strUTCString in Stringformat
* @return {string}
*/
function convertUTC(strUTCString)
{
    var dt = new Date(strUTCString);
    console.log(dt.toLocaleString());
    return dt.toLocaleString();
}
```

다음을 수행하세요. [jsdoc 참조](https://jsdoc.app/index.html)javascript 함수 주석 추가에 대한 자세한 내용은 을 참조하십시오.
위의 코드에는 두 가지 함수가 있습니다.
**getCountyNamesList** - 문자열의 배열을 반환합니다.
**전환 UTC** - UTC 타임스탬프를 로컬 시간대로 변환

js.txt를 열고 다음 코드를 붙여넣고 변경 사항을 저장합니다.

```javascript
#base=js
functions.js
```

#base=js 행은 JavaScript 파일이 있는 디렉토리를 지정합니다.
아래 줄은 기본 위치를 기준으로 JavaScript 파일의 위치를 나타냅니다.

사용자 지정 함수를 만드는 데 문제가 있는 경우 언제든지 다음 작업을 수행할 수 있습니다. [이 패키지 다운로드 및 설치](assets/custom-functions.zip) AEM 인스턴스에서 다음을 수행합니다.

## 사용자 정의 함수 사용

다음 비디오에서는 적응형 양식의 규칙 편집기에서 사용자 지정 기능을 사용하는 것과 관련된 단계를 설명합니다
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=12&learn=on)
