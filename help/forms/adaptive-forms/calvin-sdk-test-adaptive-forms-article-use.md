---
title: 'AEM Adaptive Forms에서 자동화된 테스트 사용 '
seo-title: 'AEM Adaptive Forms에서 자동화된 테스트 사용 '
description: Calvin SDK를 사용한 적응형 Forms 자동 테스트
seo-description: Calvin SDK를 사용한 적응형 Forms 자동 테스트
feature: 적응형 양식
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: 개발
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 1%

---


# AEM Adaptive Forms에서 자동 테스트 사용 {#using-automated-tests-with-aem-adaptive-forms}

Calvin SDK를 사용한 적응형 Forms 자동 테스트

Calvin SDK는 적응형 Forms 개발자를 위한 유틸리티 API로 Adaptive Forms을 테스트합니다. Calvin SDK는 [Hobbes.js 테스트 프레임워크](https://docs.adobe.com/docs/en/aem/6-3/develop/ref/test-api/index.html)의 맨 위에 구축됩니다. Calvin SDK는 AEM Forms 6.3부터 사용할 수 있습니다.

이 자습서에서는 다음을 만듭니다.

* 테스트 세트
* 테스트 제품에는 하나 이상의 테스트 케이스가 포함됩니다.
* 테스트 케이스에 하나 이상의 작업이 포함됩니다.

## 시작하기 {#getting-started}

[패키지 관리자를 사용하여 에셋 다운로드 및 ](assets/testingadaptiveformsusingcalvinsdk1.zip)설치이 패키지에는 샘플 스크립트와 여러 가지 응용 Forms이 포함되어 있습니다. 이러한 응용 Forms은 AEM Forms 6.3 버전을 사용하여 빌드됩니다. AEM Forms 6.4 이상에서 테스트하는 경우 현재 사용 중인 AEM Forms 버전에 해당하는 새 양식을 만드는 것이 좋습니다. 샘플 스크립트에서는 적응형 Forms을 테스트하는 데 사용할 수 있는 다양한 Calvin SDK API를 보여 줍니다. AEM Adaptive Forms 테스트의 일반적인 단계는 다음과 같습니다.

* 테스트해야 하는 양식으로 이동합니다.
* 필드 값 설정
* 적응형 양식 제출
* 오류 메시지 확인

패키지의 샘플 스크립트는 위의 모든 작업을 보여 줍니다.
`mortgageForm.js` 코드를 살펴보겠습니다.

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

위의 코드는 새 테스트 세트를 만듭니다.

* 이 경우 TestSuite의 이름은 &#39; `Mortgage Form Test` &#39;입니다.
* 테스트 세트가 포함된 js 파일의 AEM의 절대 경로입니다.
* &#39; `true` &#39;(으)로 설정된 register 매개 변수를 사용하면 테스트 UI에서 테스트 세트를 사용할 수 있습니다.

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
>AEM Forms 6.4 이상에서 이 기능을 테스트하는 경우 새 적응형 양식을 만들어 테스트용으로 사용하십시오.패키지와 함께 제공된 적응형 양식을 사용하는 것은 권장되지 않습니다.

응용 양식에 대해 실행할 테스트 세트에 테스트 케이스를 추가할 수 있습니다.

* 테스트 세트에 테스트 케이스를 추가하려면 TestSuite 객체의 `addTestCase` 메서드를 사용합니다.
* `addTestCase` 메서드는 TestCase 개체를 매개 변수로 사용합니다.
* TestCase를 만들려면 `hobs.TestCase(..)` 메서드를 사용합니다.
* 참고:첫 번째 매개 변수는 UI에 나타나는 테스트 케이스 이름입니다.
* 테스트 케이스를 만들면 테스트 케이스에 작업을 추가할 수 있습니다.
* `navigateTo`, `asserts.isTrue`을(를) 포함하는 작업을 테스트 케이스에 작업으로 추가할 수 있습니다.

## 자동화된 테스트 실행 {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)Opentestsuite테스트 세트를 확장하고 테스트를 실행합니다. 모든 것이 성공적으로 실행되면 다음 출력이 표시됩니다.

![calvinsdk](assets/calvinimage.png)

## 샘플 테스트 세트 {#try-out-the-sample-test-suites} 시험버전

샘플 패키지의 일부로 3개의 추가 테스트 세트가 있습니다. 아래와 같이 clientlibrary의 js.txt 파일에 적절한 파일을 포함시켜 사용해 볼 수 있습니다.

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
