---
title: AEM 적응형 Forms에서 자동화된 테스트 사용
description: Calvin SDK를 사용한 적응형 Forms 자동 테스트
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 124
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# AEM 적응형 Forms에서 자동화된 테스트 사용 {#using-automated-tests-with-aem-adaptive-forms}

Calvin SDK를 사용한 적응형 Forms 자동 테스트

Calvin SDK는 적응형 Forms 개발자가 적응형 Forms을 테스트할 수 있는 유틸리티 API입니다. Calvin SDK는 [Hobbes.js 테스트 프레임워크](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html). Calvin SDK는 AEM Forms 6.3 이상에서 사용할 수 있습니다.

이 자습서에서는 다음을 만듭니다.

* 테스트 세트
* 테스트 세트에는 하나 이상의 테스트 사례가 포함됩니다.
* 테스트 사례에는 하나 이상의 작업이 포함됩니다.

## 시작 {#getting-started}

[패키지 관리자를 사용하여 자산 다운로드 및 설치](assets/testingadaptiveformsusingcalvinsdk1.zip)패키지에는 샘플 스크립트와 여러 적응형 Forms이 포함되어 있습니다. 이러한 적응형 Forms은 AEM Forms 6.3 버전을 사용하여 작성됩니다. AEM Forms 6.4 이상에서 테스트하는 경우 AEM Forms 버전과 관련된 새 양식을 만드는 것이 좋습니다. 샘플 스크립트는 적응형 Forms을 테스트할 수 있는 다양한 Calvin SDK API를 보여 줍니다. AEM 적응형 Forms을 테스트하는 일반 단계는 다음과 같습니다.

* 테스트해야 하는 양식으로 이동합니다
* 필드 값 설정
* 적응형 양식 제출
* 오류 메시지 확인

패키지의 샘플 스크립트는 위의 모든 작업을 보여 줍니다.
의 코드를 살펴보겠습니다. `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

위의 코드는 새 테스트 세트를 만듭니다.

* 이 경우 TestSuite의 이름은 입니다. `Mortgage Form Test` &#39;.
* 테스트 세트가 포함된 js 파일에 대한 AEM의 절대 경로가 제공됩니다.
* &#39;(으)로 설정된 경우 register 매개 변수 `true` &#39;, 테스트 UI에서 테스트 세트를 사용할 수 있도록 합니다.

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>AEM Forms 6.4 이상에서 이 기능을 테스트하는 경우 새 적응형 양식을 만들고 이 양식을 사용하여 테스트하십시오. 패키지와 함께 제공된 적응형 양식은 사용하지 않는 것이 좋습니다.

테스트 사례를 적응형 양식에 대해 실행할 테스트 세트에 추가할 수 있습니다.

* 테스트 세트에 테스트 사례를 추가하려면 `addTestCase` TestSuite 개체의 메서드입니다.
* 다음 `addTestCase` 메서드는 TestCase 개체를 매개 변수로 사용합니다.
* 테스트 사례를 만들려면 다음을 사용하십시오. `hobs.TestCase(..)` 메서드를 사용합니다.
* 참고: 첫 번째 매개 변수는 UI에 표시되는 테스트 사례의 이름입니다.
* 테스트 사례를 만든 후에는 테스트 사례에 작업을 추가할 수 있습니다.
* 다음을 포함한 작업 `navigateTo`, `asserts.isTrue` 은 테스트 사례에 작업으로 추가할 수 있습니다.

## 자동화된 테스트 실행 {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)테스트 세트를 확장하고 테스트를 실행합니다. 모든 것이 성공적으로 실행되면 다음 출력이 표시됩니다.

![칼빈sdk](assets/calvinimage.png)

## 샘플 테스트 세트를 사용해 보십시오. {#try-out-the-sample-test-suites}

샘플 패키지의 일부로 3개의 추가 테스트 세트가 있습니다. 다음과 같이 클라이언트 라이브러리의 js.txt 파일에 적절한 파일을 포함하여 시도할 수 있습니다.

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
