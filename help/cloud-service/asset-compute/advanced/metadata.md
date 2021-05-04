---
title: asset compute 메타데이터 작업자 개발
description: 이미지 자산에서 가장 일반적으로 사용되는 색상을 파생하고 색상 이름을 AEM의 자산 메타데이터에 다시 쓰는 Asset compute 메타데이터 워커를 만드는 방법을 알아봅니다.
feature: asset compute 마이크로서비스
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 1%

---


# asset compute 메타데이터 작업자 개발

사용자 정의 Asset compute 작업자는 AEM으로 다시 전송되고 에셋에 메타데이터로 저장되는 XMP(XML) 데이터를 생성할 수 있습니다.

일반적인 사용 사례는 다음과 같습니다.

+ 추가 메타데이터를 검색하여 자산에 저장해야 하는 PIM(제품 정보 관리 시스템)과 같은 타사 시스템과의 통합
+ 콘텐츠 및 커머스 AI와 같은 Adobe 서비스와의 통합을 통해 추가 머신 러닝 특성으로 에셋 메타데이터 추가
+ 이진 파일에서 에셋에 대한 메타데이터를 추출하고 Cloud Service으로 AEM에 에셋 메타데이터로 저장합니다.

## 무엇을 할 것인가

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

이 튜토리얼에서는 이미지 자산에서 가장 일반적으로 사용되는 색상을 파생하고 색상 이름을 AEM의 자산 메타데이터에 다시 쓰는 Asset compute 메타데이터 작업자를 만듭니다. 작업자 자체는 기본이지만 이 튜토리얼에서는 Asset compute 작업자가 Cloud Service으로 AEM의 자산에 메타데이터를 다시 쓰는 데 어떻게 사용할 수 있는지 살펴봅니다.

## asset compute 메타데이터 작업자 호출의 논리 흐름

asset compute 메타데이터 워커의 호출은 작업자](../develop/worker.md)를 생성하는 [이진 변환과 거의 동일하며 반환 유형은 자산의 메타데이터에도 값이 기록되는 XMP(XML) 변환입니다.

asset compute 근로자는 개념적인 개념인 `renditionCallback(...)` 함수에서 Asset compute SDK 작업자 API 계약을 구현합니다.

+ __입력:__ AEM 자산의 원래 이진 및 처리 프로필 매개 변수
+ __출력:__ XMP(XML) 변환이 AEM 에셋에 표현물과 에셋의 메타데이터로 지속되었습니다.

![asset compute 메타데이터 작업자 논리 흐름](./assets/metadata/logical-flow.png)

1. AEM 작성자 서비스는 자산의 __(1a)__ 원본 이진 및 __(1b)__ 처리 프로필에 정의된 모든 매개 변수를 제공하여 Asset compute 메타데이터 작업자를 호출합니다.
1. asset compute SDK는 자산의 이진 __(1a)__ 및 처리 프로필 매개 변수 __(1b)__&#x200B;에 따라 사용자 지정 Asset compute 메타데이터 근로자의 `renditionCallback(...)` 함수 실행을 구성하며 XMP(XML) 변환을 파생합니다.
1. asset compute 워커는 XMP(XML) 표현을 `rendition.path`에 저장합니다.
1. `rendition.path`에 기록된 XMP(XML) 데이터는 Asset compute SDK를 통해 AEM 작성자 서비스로 전송되고 텍스트 변환과 __(4b)__(4b)__가 자산의 메타데이터 노드에 지속되도록 노출됩니다.__

## manifest.yml{#manifest} 구성

모든 Asset compute 근로자는 [manifest.yml](../develop/manifest.md)에 등록되어 있어야 합니다.

프로젝트의 `manifest.yml`을 열고 새 워커를 구성하는 작업자 항목을 추가합니다(이 경우 `metadata-colors`).

_공백 `.yml` 에 민감합니다._

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` 는  [다음 단계에서 생성된 작업자 구현을 가리킵니다](#metadata-worker). [근로자의 URL](#deploy)에 표시된 것처럼 이름 작업자의 이름을 의미있게 지정합니다(예: `actions/worker/index.js`이(가) `actions/rendition-circle/index.js`). 또한 [근로자의 테스트 세트 폴더 이름](#test)을 결정합니다.

`limits` 및 `require-adobe-auth`은(는) 작업자별로 별도로 구성됩니다. 이 작업자에서 메모리 `512 MB`은(는) 큰 이진 이미지 데이터를 검사(잠재적으로)하는 코드로 할당됩니다. 기본값을 사용하기 위해 다른 `limits`이(가) 제거됩니다.

## 메타데이터 작업자 현상{#metadata-worker}

새 작업자](#manifest) 경로에 대해 [정의된 manifest.yml 경로의 Asset compute 프로젝트에 새 메타데이터 작업자 JavaScript 파일을 만듭니다( `/actions/metadata-colors/index.js`).

### npm 모듈 설치

이 Asset compute 워커에서 사용할 추가 npm 모듈([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors) 및 [color-name](https://www.npmjs.com/package/color-namer))을 설치합니다.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 메타데이터 작업자 코드

이 워커는 [변환 생성 작업자](../develop/worker.md)와 매우 유사해 보입니다. 주된 차이점은 XMP(XML) 데이터를 `rendition.path`에 작성하여 다시 AEM에 저장한다는 것입니다.


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces will be automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## 메타데이터 작업자를 로컬로 실행{#development-tool}

작업자 코드가 완료되면 로컬 Asset compute 개발 도구를 사용하여 실행할 수 있습니다.

asset compute 프로젝트에 2명의 작업자(이전 [원 변환](../develop/worker.md) 및 이 `metadata-colors` 근로자)가 포함되어 있으므로 [Asset compute 개발 도구의 ](../develop/development-tool.md) 프로필 정의에는 두 작업자 모두에 대한 실행 프로필이 나열됩니다. 두 번째 프로필 정의는 새 `metadata-colors` 작업자를 가리킵니다.

![XML 메타데이터 변환](./assets/metadata/metadata-rendition.png)

1. asset compute 프로젝트의 루트에서
1. asset compute 개발 도구를 시작하려면 `aio app run` 실행
1. __파일 선택...__ 드롭다운, 처리할 [샘플 이미지](../assets/samples/sample-file.jpg)을 선택합니다.
1. `metadata-colors` 작업자를 가리키는 두 번째 프로필 정의 구성에서 이 워커가 XMP(XML) 변환을 생성하므로 `"name": "rendition.xml"`을(를) 업데이트합니다. 원하는 경우 `colorsFamily` 매개 변수(지원되는 값 `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`)를 추가합니다.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```

1. __실행__&#x200B;을 누르고 XML 변환이 생성될 때까지 기다립니다
   + 두 작업자 모두 프로필 정의에 나열되므로 두 표현물 모두 생성됩니다. 선택적으로, 개발 도구에서 실행하지 않도록 [원 변환 작업자](../develop/worker.md)를 가리키는 최상위 프로필 정의를 삭제할 수 있습니다.
1. __표현물__ 섹션에서는 생성된 표현물을 미리 봅니다. `rendition.xml`을 눌러 다운로드하고 VS 코드(또는 선호하는 XML/텍스트 편집기)에서 열어 검토합니다.

## 작업자 테스트{#test}

메타데이터 작업자는 이진 표현물](../test-debug/test.md)과 동일한 Asset compute 테스트 프레임워크를 사용하여 테스트할 수 있습니다. [ 유일한 차이는 테스트 케이스의 `rendition.xxx` 파일이 예상 XMP(XML) 변환이어야 합니다.

1. asset compute 프로젝트에서 다음 구조를 만듭니다.

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 테스트 케이스의 `file.jpg`로 [샘플 파일](../assets/samples/sample-file.jpg)을 사용합니다.
3. 다음 JSON을 `params.json`에 추가합니다.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   테스트 세트에 `.xml` 텍스트 기반 변환을 생성하도록 지시하려면 `"fmt": "xml"`이(가) 필요합니다.

4. `rendition.xml` 파일에 예상 XML을 제공합니다. 다음 방법을 통해 얻을 수 있습니다.
   + 개발 도구를 통해 테스트 입력 파일을 실행하고 (유효성이 검사된) XML 변환을 저장합니다.

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. asset compute 프로젝트의 루트에서 `aio app test`을 실행하여 모든 테스트 세트를 실행합니다.

### Worker를 Adobe I/O Runtime{#deploy}에 배포

AEM Assets에서 이 새 메타데이터 작업자를 호출하려면 다음 명령을 사용하여 Adobe I/O Runtime에 배포해야 합니다.

```
$ aio app deploy
```

![aio 앱 배포](./assets/metadata/aio-app-deploy.png)

프로젝트의 모든 작업자를 배포합니다. 스테이지 및 프로덕션 작업 영역에 배포하는 방법에 대해서는 [요약되지 않은 배포 지침](../deploy/runtime.md)을 검토하십시오.

### AEM 처리 프로필과 통합{#processing-profile}

이 배포된 작업자를 호출하는 기존 사용자 지정 처리 프로필 서비스를 수정하거나 새로 만드는 방법으로 AEM에서 작업자를 호출합니다.

![처리 프로필](./assets/metadata/processing-profile.png)

1. Cloud Service 작성자 서비스로 AEM에 로그인하고 __AEM 관리자__
1. __도구 > 자산 > 처리 프로필__&#x200B;으로 이동합니다.
1. __처리__ 프로파일을 새로 만들거나 기존  ____ 편집 및 만듭니다.
1. __사용자 지정__ 탭을 누르고 __새로 추가__&#x200B;를 탭합니다.
1. 새 서비스 정의
   + __메타데이터 변환 만들기__:활성으로 전환
   + __끝점:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 이 URL은 [배포](#deploy) 또는 `aio app get-url` 명령을 사용하는 동안 얻은 워커의 URL입니다. AEM을 Cloud Service 환경으로 기반으로 올바른 작업 영역에서 URL 점을 확인합니다.
   + __서비스 매개 변수__
      + __매개 변수 추가__&#x200B;를 탭합니다.
         + 키: `colorFamily`
         + 값: `pantone`
            + 지원되는 값:`basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __MIME 유형__
      + __포함 사항:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/svg`
         + 색상을 파생하는 데 사용되는 타사 npm 모듈에서 지원하는 유일한 MIME 형식입니다.
      + __제외:__ `Leave blank`
1. 오른쪽 상단에서 __저장__&#x200B;을 탭합니다.
1. 처리 프로필을 아직 수행하지 않은 경우 AEM Assets 폴더에 적용합니다.

### 메타데이터 스키마 업데이트{#metadata-schema}

색상 메타데이터를 검토하려면 이미지의 메타데이터 스키마에 있는 두 개의 새 필드를 작업자가 채우는 새 메타데이터 데이터 속성에 매핑합니다.

![메타데이터 스키마](./assets/metadata/metadata-schema.png)

1. AEM 작성자 서비스에서 __도구 > 자산 > 메타데이터 스키마__&#x200B;로 이동합니다.
1. __default__&#x200B;로 이동하여 __image__&#x200B;을 선택하고 편집하고 읽기 전용 양식 필드를 추가하여 생성된 색상 메타데이터를 표시합니다.
1. __단일 행 텍스트 추가__
   + __필드 레이블__: `Colors Family`
   + __속성에 매핑__: `./jcr:content/metadata/wknd:colorsFamily`
   + __규칙 > 필드 > 편집__ 비활성화:선택됨
1. __복수 값 텍스트 추가__
   + __필드 레이블__: `Colors`
   + __속성에 매핑__: `./jcr:content/metadata/wknd:colors`
1. 오른쪽 상단에서 __저장__&#x200B;을 탭합니다.

## 자산 처리 중

![자산 세부 정보](./assets/metadata/asset-details.png)

1. AEM 작성자 서비스에서 __자산 > 파일__&#x200B;로 이동합니다.
1. 폴더 또는 하위 폴더로 이동하여 처리 프로필이
1. 새 이미지(JPEG, PNG, GIF 또는 SVG)를 폴더에 업로드하거나 업데이트된 [처리 프로필](#processing-profile)을(를) 사용하여 기존 이미지를 다시 처리합니다.
1. 처리가 완료되면 자산을 선택하고 맨 위 작업 표시줄에서 __속성__&#x200B;을 탭하여 해당 메타데이터를 표시합니다
1. 사용자 지정 Asset compute 메타데이터 워커에서 다시 쓴 메타데이터에 대해서는 `Colors Family` 및 `Colors` [ 메타데이터 필드](#metadata-schema)를 검토하십시오.

에셋의 메타데이터에 기록된 색상 메타데이터가 있는 `[dam:Asset]/jcr:content/metadata` 리소스에서 이 메타데이터는 검색을 통해 이러한 용어를 사용하여 자산 검색 기능이 증가된 색인화되어 에셋의 바이너리에 다시 기록할 수 있습니다. 그런 다음 __DAM 메타데이터 쓰기 저장__ 워크플로우가 해당 에셋에서 호출될 경우 이 메타데이터는 에셋의 바이너리에 다시 쓸 수도 있습니다.

### AEM Assets의 메타데이터 변환

![AEM Assets 메타데이터 변환 파일](./assets/metadata/cqdam-metadata-rendition.png)

asset compute 메타데이터 워커에서 생성된 실제 XMP 파일도 자산에 대해 개별 변환으로 저장됩니다. 이 파일은 일반적으로 사용되지 않으며, 자산의 메타데이터 노드에 적용된 값이 사용되지만 워커의 원시 XML 출력은 AEM에서 사용할 수 있습니다.

## Github의 메타데이터 색상 작업자 코드

최종 `metadata-colors/index.js`은(는) Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

최종 `test/asset-compute/metadata-colors` 테스트 세트는 다음 위치의 Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
