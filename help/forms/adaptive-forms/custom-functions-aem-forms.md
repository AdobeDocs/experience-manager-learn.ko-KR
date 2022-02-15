---
title: AEM Forms의 사용자 지정 함수
description: 적응형 양식에서 사용자 지정 함수 만들기 및 사용
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
kt: 9685
source-git-commit: 15b57ec6792bc47d0041946014863b13867adf22
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# 사용자 지정 함수

AEM Forms 6.5에서는 규칙 편집기를 사용하여 복잡한 비즈니스 규칙을 정의하는 데 사용할 수 있는 JavaScript 함수를 정의하는 기능을 도입했습니다.
AEM Forms에서는 이러한 여러 사용자 지정 기능을 기본적으로 제공하지만, 고유한 사용자 지정 기능을 정의하고 여러 양식에서 사용해야 합니다.

첫 번째 사용자 지정 함수를 정의하려면 다음 단계를 수행합니다.
* [crx에 로그인](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* experience-league라는 앱에서 새 폴더를 만듭니다(이 폴더 이름은 원하는 이름일 수 있음)
* 변경 사항을 저장합니다.
* experience-league 폴더 아래에 clientlibs라는 cq:ClientLibraryFolder 유형의 새 노드를 만듭니다.
* 새로 clientlibs 폴더를 선택하고 스크린샷에 표시된 대로 allowProxy 및 categories 속성을 추가하고 변경 사항을 저장합니다.

![client-lib](assets/custom-functions.png)
* 라는 폴더를 만듭니다. **js** 아래에 **clientlibs** 폴더
* 라는 파일을 만듭니다. **function.js** 아래에 **js** 폴더
* 라는 파일을 만듭니다. **js.txt** 아래에 **clientlibs** 폴더를 입력합니다. 변경 사항을 저장합니다.
* 폴더 구조가 아래의 스크린샷과 유사해야 합니다.

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

제발 [jsdoc 를 참조하십시오. ](https://jsdoc.app/index.html)javascript 함수에 주석을 달 수 있는 자세한 내용.
위의 코드에는 두 가지 함수가 있습니다.
**getCountyNamesList** - 문자열 배열을 반환합니다
**convertUTC** - UTC 타임스탬프를 로컬 시간대로 변환

js.txt를 열고 다음 코드를 붙여넣고 변경 사항을 저장합니다.

```javascript
#base=js
functions.js
```

#base=js 줄은 JavaScript 파일이 있는 디렉토리를 지정합니다.
아래 줄은 기본 위치를 기준으로 JavaScript 파일의 위치를 나타냅니다.

사용자 지정 기능을 만드는 데 문제가 있다면 언제든지 [이 패키지 다운로드 및 설치](assets/custom-functions.zip) 참조하십시오.

## 사용자 지정 함수 사용

다음 비디오에서는 적응형 양식의 규칙 편집기에서 사용자 지정 함수를 사용하는 것과 관련된 단계를 안내합니다
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=9&learn=on)
