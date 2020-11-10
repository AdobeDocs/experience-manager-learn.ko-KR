---
title: asset compute 메타데이터 작업자 개발
description: 이미지 자산에서 가장 일반적으로 사용되는 색상을 파생하고 색상 이름을 AEM의 자산 메타데이터에 다시 쓰는 Asset compute 메타데이터 작업자를 만드는 방법을 알아봅니다.
feature: asset-compute
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
translation-type: tm+mt
source-git-commit: c2a8e6c3ae6dcaa45816b1d3efe569126c6c1e60
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 1%

---


# asset compute 메타데이터 작업자 개발

사용자 정의 Asset compute 작업자는 AEM으로 다시 전송되고 에셋에 메타데이터로 저장되는 XMP(XML) 데이터를 생성할 수 있습니다.

일반적인 사용 사례는 다음과 같습니다.

+ PIM(제품 정보 관리 시스템)과 같은 타사 시스템과의 통합(추가 메타데이터를 검색하여 자산에 저장해야 함)
+ 콘텐츠 및 커머스 AI와 같은 Adobe 서비스와의 통합을 통해 추가 머신 러닝 특성으로 에셋 메타데이터 추가
+ 이진 파일에서 에셋에 대한 메타데이터를 추출하여 AEM에 Cloud Service으로 에셋 메타데이터로 저장

## 무엇을 할 것인가

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

이 자습서에서는 이미지 자산에서 가장 일반적으로 사용되는 색상을 파생하고 색상 이름을 AEM의 자산 메타데이터에 다시 쓰는 Asset compute 메타데이터 작업자를 만듭니다. 작업자 자체는 기본이지만 이 튜토리얼에서는 Asset compute 작업자가 메타데이터를 AEM의 자산에 다시 쓰는 데 Cloud Service을 사용하는 방법을 살펴봅니다.

## asset compute 메타데이터 작업자 호출의 논리적 흐름

asset compute 메타데이터 작업자의 호출은 작업자 [생성](../develop/worker.md)이진 변환의 호출과 거의 동일하며 반환 유형은 자산의 메타데이터에도 값이 기록되는 XMP(XML) 변환입니다.

asset compute 작업자는 개념적으로 다음과 같은 함수에서 Asset compute SDK 작업자 API 계약을 `renditionCallback(...)` 구현합니다.

+ __입력:__ AEM 자산의 원래 이진 및 처리 프로필 매개 변수
+ __출력:__ AEM 자산에 대한 변환과 자산의 메타데이터로 지속된 XMP(XML) 변환

![asset compute 메타데이터 작업자 논리 흐름](./assets/metadata/logical-flow.png)

1. AEM 작성자 서비스는 Asset compute 메타데이터 작업자를 호출하여 자산의 __(1a)__ 원래 바이너리 및 __(1b)__ 처리 프로필에 정의된 모든 매개 변수를 제공합니다.
1. asset compute SDK는 자산의 이진 `renditionCallback(...)` (1a) __및 모든 처리 프로필 매개 변수__ (1b) __에 따라 XMP(XML) 변환을 도출하여 사용자 정의 Asset compute 메타데이터 작업자__&#x200B;의 기능 실행을 오케스트레이션합니다.
1. asset compute 작업자는 XMP(XML) 표현을 에 저장합니다 `rendition.path`.
1. XMP(XML) 데이터 `rendition.path` 는 Asset compute SDK를 통해 AEM 작성자 서비스로 전송되며 __(4a)__ 텍스트 변환 및 __(4b)__ 에셋의 메타데이터 노드에 지속되도록 노출됩니다.

## manifest.html 구성{#manifest}

모든 Asset compute 작업자는 manifest. [yml에 등록되어 있어야 합니다](../develop/manifest.md).

프로젝트 `manifest.yml` 를 열고 새 작업자를 구성하는 작업자 항목을 추가합니다(이 경우) `metadata-colors`.

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

`function` 는 [다음 단계에서 생성된 작업자 구현을 가리킵니다](#metadata-worker). 근로자의 URL에 표시된 것처럼 이름 작업자의 이름 `actions/worker/index.js` (예: 이름이 더 `actions/rendition-circle/index.js`나았을 수 있음)을 의미있게 지정하고 [근로자의 테스트 세트 폴더 이름을 결정합니다](#deploy) [](#test).

및 `limits` 는 작업자별로 `require-adobe-auth` 독립적으로 구성됩니다. 이 작업자에서 메모리 `512 MB` 는 큰 이진 이미지 데이터를 검사(잠재적으로)하는 코드로 할당됩니다. 기본값 `limits` 을 사용하도록 다른 항목이 제거됩니다.

## 메타데이터 작업자 개발{#metadata-worker}

새 작업자에 대해 [](#manifest)정의된 manifest.yml 경로의 Asset compute 프로젝트에 새 메타데이터 작업자 JavaScript 파일을 만듭니다( `/actions/metadata-colors/index.js`

### npm 모듈 설치

이 Asset compute 작업자에 사용할 추가 npm 모듈([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-color](https://www.npmjs.com/package/get-image-colors)및 [color-namer](https://www.npmjs.com/package/color-namer))을설치합니다.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 메타데이터 작업자 코드

이 작업자는 [변환 생성 작업자와](../develop/worker.md)매우 유사해 보입니다. 주된 차이점은 XMP(XML) 데이터를 AEM에 다시 저장하여 다시 `rendition.path` 저장한다는 것입니다.


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

## 로컬에서 메타데이터 작업자 실행{#development-tool}

작업자 코드가 완료되면 로컬 Asset compute 개발 도구를 사용하여 실행할 수 있습니다.

asset compute 프로젝트에는 두 명의 작업자(이전 [원 변환](../develop/worker.md) 및 이 근로자)가 포함되어 있으므로 `metadata-colors` Asset compute 개발 도구의 [프로파일 정의에는 두 작업자 모두에 대한 실행 프로필이](../develop/development-tool.md) 나열됩니다. 두 번째 프로파일 정의는 새 `metadata-colors` 작업자를 가리킵니다.

![XML 메타데이터 변환](./assets/metadata/metadata-rendition.png)

1. asset compute 프로젝트의 루트에서
1. 실행 `aio app run` 을 통해 Asset compute 개발 도구 시작
1. 파일 __선택..__ 드롭다운에서 처리할 [샘플 이미지](../assets/samples/sample-file.jpg) 선택
1. 작업자를 가리키는 두 번째 프로필 정의 구성에서 이 작업자는 XMP(XML) 변환 `metadata-colors` `"name": "rendition.xml"` 을 생성하므로 업데이트합니다. 매개 변수(지원되는 값 `colorsFamily` , `basic`, `hex`, `html``ntc`, `pantone`등)를 추가할 수도 `roygbiv`있습니다.

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
1. Run __을__ 누르고 XML 변환이 생성되기를 기다립니다
   + 두 작업자 모두 프로필 정의에 나열되므로 두 표현물 모두 생성됩니다. 또한 원 변환 작업자를 가리키는 [상단 프로필 정의를 삭제하여 개발 도구에서](../develop/worker.md) 실행하지 않을 수 있습니다.
1. 표현물 __섹션은__ 생성된 표현물을 미리 봅니다. 을 눌러 `rendition.xml` 다운로드하고 VS 코드(또는 자주 사용하는 XML/텍스트 편집기)에서 열어 검토할 수 있습니다.

## 작업자 테스트{#test}

메타데이터 작업자는 이진 표현물과 [동일한 Asset compute 테스트 프레임워크를 사용하여 테스트할 수 있습니다](../test-debug/test.md). 유일한 차이점은 테스트 케이스의 `rendition.xxx` 파일이 예상 XMP(XML) 변환이어야 한다는 것입니다.

1. asset compute 프로젝트에서 다음 구조를 만듭니다.

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. 샘플 파일 [을](../assets/samples/sample-file.jpg) 테스트 케이스의 파일 `file.jpg`로 사용합니다.
3. 다음 JSON을 에 추가합니다 `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   테스트 세트 `"fmt": "xml"` 에서 텍스트 기반 변환을 생성하도록 지시하는 데 `.xml` 필요합니다.

4. 예상 XML을 `rendition.xml` 파일에 제공합니다. 다음을 통해 얻을 수 있습니다.
   + 개발 도구를 통해 테스트 입력 파일을 실행하고 (유효성이 검사된) XML 변환을 저장합니다.

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. asset compute 프로젝트 `aio app test` 의 루트에서 실행하여 모든 테스트 세트를 실행합니다.

### 작업자를 Adobe I/O Runtime에 배포{#deploy}

AEM Assets에서 이 새 메타데이터 작업자를 호출하려면 다음 명령을 사용하여 Adobe I/O Runtime에 배포해야 합니다.

```
$ aio app deploy
```

![aio app deploy](./assets/metadata/aio-app-deploy.png)

그러면 프로젝트의 모든 작업자가 배포됩니다. 스테이지 및 프로덕션 작업 영역에 배포하는 방법에 대한 [요약되지 않은 배포 지침을](../deploy/runtime.md) 검토하십시오.

### AEM 처리 프로필과 통합{#processing-profile}

이 배포된 작업자를 호출하는 기존 사용자 지정 처리 프로필 서비스를 수정하거나 새로 만들거나 수정하여 AEM에서 작업자를 불러옵니다.

![처리 프로필](./assets/metadata/processing-profile.png)

1. AEM 관리자로 Cloud Service 작성자 서비스로 __AEM에 로그인__
1. 도구 > __자산 > 처리 프로필로 이동합니다.__
1. __새__ 만들기, __편집__ 및 기존 처리 프로필
1. 사용자 __지정__ 탭을 누르고 새로 __추가를 누릅니다__
1. 새 서비스 정의
   + __메타데이터 변환 만들기__:활성으로 전환
   + __끝점:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + 이 URL은 [배포](#deploy) 또는 명령 사용 중에 얻은 작업자의 URL입니다 `aio app get-url`. AEM을 기준으로 올바른 작업 영역에서 URL을 Cloud Service 환경으로 확인합니다.
   + __서비스 매개 변수__
      + 매개 변수 __추가를 누릅니다.__
         + 키: `colorFamily`
         + 값: `pantone`
            + 지원되는 값: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __MIME 유형__
      + __포함:__`image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + 색상을 파생하는 데 사용되는 타사 npm 모듈에서 지원하는 유일한 MIME 형식입니다.
      + __제외:__ `Leave blank`
1. 오른쪽 __상단에 있는 저장을__ 누릅니다.
1. 아직 처리하지 않은 경우 처리 프로필을 AEM Assets 폴더에 적용

### 메타데이터 스키마 업데이트{#metadata-schema}

색상 메타데이터를 검토하려면, 이미지의 메타데이터 스키마에 있는 두 개의 새 필드를 작업자가 채우는 새 메타데이터 데이터 속성에 매핑합니다.

![메타데이터 스키마](./assets/metadata/metadata-schema.png)

1. AEM 작성자 서비스에서 __도구 > 자산 > 메타데이터 스키마로 이동합니다__
1. 기본 __으로__ 이동하여 __이미지를__ 선택 및 편집하고 읽기 전용 양식 필드를 추가하여 생성된 색상 메타데이터를 표시합니다
1. 단일 __행 텍스트 추가__
   + __필드 레이블__: `Colors Family`
   + __속성에 매핑__: `./jcr:content/metadata/wknd:colorsFamily`
   + __규칙 > 필드 > 편집 비활성화__:선택
1. 다중 값 __텍스트 추가__
   + __필드 레이블__: `Colors`
   + __속성에 매핑__: `./jcr:content/metadata/wknd:colors`
1. 오른쪽 __상단에 있는 저장을__ 누릅니다.

## 자산 처리

![자산 세부 정보](./assets/metadata/asset-details.png)

1. AEM 작성자 서비스에서 __자산 > 파일로 이동합니다__
1. 폴더 또는 하위 폴더로 이동하여 처리 프로필이
1. 새 이미지(JPEG, PNG, GIF 또는 SVG)를 폴더에 업로드하거나 업데이트된 처리 프로필을 사용하여 기존 이미지를 [다시 처리합니다](#processing-profile)
1. 처리가 완료되면 자산을 선택하고 상단 작업 표시줄의 __속성을__ 눌러 해당 메타데이터를 표시합니다
1. 사용자 지정 Asset compute 메타데이터 작업자로부터 `Colors Family` 다시 작성된 메타데이터에 대한 `Colors` 및 [메타데이터 필드를](#metadata-schema) 검토합니다.

자산의 메타데이터에 기록된 색상 메타데이터로 `[dam:Asset]/jcr:content/metadata` 리소스에 의해 이 메타데이터는 검색을 통해 이러한 용어를 사용하여 자산 검색 기능이 증가하는 인덱싱되며, DAM 메타데이터 쓰기 ____ 작업 과정이 호출되면 자산의 바이너리에 다시 기록될 수도 있습니다.

### AEM Assets의 메타데이터 변환

![AEM Assets 메타데이터 변환 파일](./assets/metadata/cqdam-metadata-rendition.png)

asset compute 메타데이터 작업자가 생성한 실제 XMP 파일도 자산에 대해 개별 변환으로 저장됩니다. 이 파일은 일반적으로 사용되지 않으며, 자산의 메타데이터 노드에 적용된 값이 사용되지만 작업자의 원시 XML 출력은 AEM에서 사용할 수 있습니다.

## Github의 메타데이터 색상 작업자 코드

결승전은 다음 위치의 Github에서 볼 수 있습니다. `metadata-colors/index.js`

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

최종 `test/asset-compute/metadata-colors` 테스트 세트는 다음 위치의 Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
