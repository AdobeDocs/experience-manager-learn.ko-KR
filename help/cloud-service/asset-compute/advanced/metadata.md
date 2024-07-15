---
title: asset compute 메타데이터 작업자 개발
description: 이미지 에셋에서 가장 일반적으로 사용되는 색상을 파생하고 색상 이름을 AEM의 에셋 메타데이터에 다시 작성하는 Asset compute 메타데이터 작업자를 만드는 방법을 알아봅니다.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 432
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# asset compute 메타데이터 작업자 개발

사용자 지정 Asset compute 작업자는 AEM으로 다시 전송되고 자산에 메타데이터로 저장되는 XMP (XML) 데이터를 생성할 수 있습니다.

일반적인 사용 사례는 다음과 같습니다.

+ 추가 메타데이터를 검색하고 자산에 저장해야 하는 PIM(제품 정보 관리 시스템)과 같은 타사 시스템과의 통합
+ 컨텐츠 및 Commerce AI와 같은 Adobe 서비스와 통합하여 추가 머신 러닝 속성으로 에셋 메타데이터를 증가시킵니다
+ 바이너리에서 에셋에 대한 메타데이터를 파생하고 AEM as a Cloud Service에 에셋 메타데이터로 저장

## 수행할 작업

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

이 자습서에서는 이미지 에셋에서 가장 일반적으로 사용되는 색상을 파생하고 색상 이름을 AEM의 에셋 메타데이터에 다시 작성하는 Asset compute 메타데이터 작업자를 만듭니다. 작업자 자체는 기본이지만 이 자습서에서는 이 작업자를 사용하여 Asset compute 작업자를 사용하여 AEM as a Cloud Service의 에셋에 메타데이터를 다시 쓰는 방법을 살펴봅니다.

## asset compute 메타데이터 작업자 호출의 논리 흐름

asset compute 메타데이터 작업자의 호출은 [작업자 생성 이진 렌디션](../develop/worker.md)의 호출과 거의 동일하며, 반환 형식의 기본 차이점은 값이 에셋의 메타데이터에도 기록되는 XMP(XML) 렌디션입니다.

Asset compute 작업자는 개념적으로 다음과 같은 `renditionCallback(...)` 함수에서 Asset compute SDK 작업자 API 계약을 구현합니다.

+ __입력:__ AEM 자산의 원본 이진 및 처리 프로필 매개 변수입니다.
+ __출력:__ AEM 에셋 및 에셋의 메타데이터로 지속되는 XMP(XML) 렌디션

![Asset compute 메타데이터 작업자 논리 흐름](./assets/metadata/logical-flow.png)

1. AEM 작성자 서비스는 Asset compute 메타데이터 작업자를 호출하여 자산의 __(1a)__ 원본 바이너리와 처리 프로필에 정의된 모든 매개 변수 __(1b)__&#x200B;을(를) 제공합니다.
1. asset compute SDK는 자산의 바이너리 __(1a)__ 및 처리 프로필 매개 변수 __(1b)__&#x200B;을(를) 기반으로 사용자 지정 Asset compute 메타데이터 작업자의 `renditionCallback(...)` 함수 실행을 조정하고 XMP(XML) 렌디션을 파생합니다.
1. asset compute 작업자가 XMP(XML) 표현을 `rendition.path`에 저장합니다.
1. `rendition.path`에 기록된 XMP(XML) 데이터가 Asset compute SDK를 통해 AEM 작성자 서비스로 전송되어 자산의 메타데이터 노드에 지속되는 텍스트 렌디션과 __(4b)__(으)로 노출됩니다.____

## manifest.yml 구성{#manifest}

모든 Asset compute 작업자는 [manifest.yml](../develop/manifest.md)에 등록되어야 합니다.

프로젝트의 `manifest.yml`을(를) 열고 새 작업자를 구성하는 작업자 항목(이 경우 `metadata-colors`)을 추가하십시오.

_`.yml`은(는) 공백에 민감합니다._

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

`function`은(는) [다음 단계](#metadata-worker)에서 만든 작업자 구현을 가리킵니다. [작업자 URL](#deploy)에 표시되는 것과 같이 의미상 `actions/worker/index.js`의 이름이 `actions/rendition-circle/index.js`(으)로 지정되었을 수 있으며 [작업자 테스트 도구 모음 폴더 이름](#test)도 결정합니다.

`limits` 및 `require-adobe-auth`은(는) 작업자마다 개별적으로 구성됩니다. 이 작업자에서 코드가 대용량 이진 이미지 데이터를 검사할 때 메모리 `512 MB`이(가) 할당됩니다. 다른 `limits`은(는) 기본값을 사용하도록 제거되었습니다.

## 메타데이터 작업자 개발{#metadata-worker}

`/actions/metadata-colors/index.js`의 [새 작업자에 대한 manifest.yml 경로에 있는 Asset compute 프로젝트에 새 메타데이터 작업자 JavaScript 파일을 만듭니다.](#manifest)

### npm 모듈 설치

이 Asset compute 작업자에서 사용하는 추가 npm 모듈([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors) 및 [color-name](https://www.npmjs.com/package/color-namer))을 설치하십시오.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### 메타데이터 작업자 코드

이 작업자는 [렌디션 생성 작업자](../develop/worker.md)와 매우 유사합니다. 가장 큰 차이점은 XMP(XML) 데이터를 `rendition.path`에 작성하여 AEM에 다시 저장한다는 것입니다.


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
      // These namespaces are automatically registered in AEM if they do not yet exist
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

asset compute 프로젝트에는 두 명의 작업자(이전 [서클 렌디션](../develop/worker.md) 및 이 `metadata-colors` 작업자)가 포함되어 있으므로 [Asset compute 개발 도구의 ](../develop/development-tool.md) 프로필 정의에는 두 작업자의 실행 프로필이 모두 나열됩니다. 두 번째 프로필 정의는 새 `metadata-colors` 작업자를 가리킵니다.

![XML 메타데이터 렌디션](./assets/metadata/metadata-rendition.png)

1. asset compute 프로젝트의 루트에서
1. `aio app run`을(를) 실행하여 Asset compute 개발 도구 시작
1. __파일 선택...__ 드롭다운에서 처리할 [샘플 이미지](../assets/samples/sample-file.jpg)를 선택하십시오.
1. `metadata-colors` 작업자를 가리키는 두 번째 프로필 정의 구성에서 이 작업자가 XMP(XML) 렌디션을 생성함에 따라 `"name": "rendition.xml"`을(를) 업데이트합니다. 필요한 경우 `colorsFamily` 매개 변수(`basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`) 추가

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

1. __실행__&#x200B;을 누르고 XML 렌디션이 생성될 때까지 기다립니다.
   + 두 작업자가 모두 프로필 정의에 나열되므로 두 표현물이 모두 생성됩니다. 필요한 경우 [서클 렌디션 작업자](../develop/worker.md)를 가리키는 상단 프로필 정의를 삭제하여 개발 도구에서 실행하지 않도록 할 수 있습니다.
1. __렌디션__ 섹션에서 생성된 렌디션을 미리 봅니다. `rendition.xml`을(를) 탭하여 다운로드하고 VS 코드(또는 즐겨 사용하는 XML/텍스트 편집기)에서 열어 검토하십시오.

## 작업자 테스트{#test}

메타데이터 작업자는 [이진 변환과 동일한 Asset compute 테스트 프레임워크](../test-debug/test.md)를 사용하여 테스트할 수 있습니다. 유일한 차이점은 테스트 사례의 `rendition.xxx` 파일이 예상 XMP(XML) 렌디션이어야 한다는 것입니다.

1. asset compute 프로젝트에서 다음 구조를 만듭니다.

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. [샘플 파일](../assets/samples/sample-file.jpg)을(를) 테스트 사례의 `file.jpg`(으)로 사용합니다.
3. `params.json`에 다음 JSON을 추가합니다.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   테스트 세트에서 `.xml` 텍스트 기반 렌디션을 생성하도록 하려면 `"fmt": "xml"`이(가) 필요합니다.

4. `rendition.xml` 파일에 필요한 XML을 입력하십시오. 이는 다음과 같은 방법으로 얻을 수 있습니다.
   + 개발 도구를 통해 테스트 입력 파일을 실행하고 (유효성이 확인된) XML 렌디션을 저장합니다.

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. asset compute 프로젝트의 루트에서 `aio app test`을(를) 실행하여 모든 테스트 세트를 실행합니다.

### Adobe I/O Runtime에 작업자 배포{#deploy}

AEM Assets에서 이 새 메타데이터 작업자를 호출하려면 다음 명령을 사용하여 Adobe I/O Runtime에 배포해야 합니다.

```
$ aio app deploy
```

![aio 앱 배포](./assets/metadata/aio-app-deploy.png)

이렇게 하면 프로젝트의 모든 작업자가 배포됩니다. 스테이지 및 프로덕션 작업 영역에 배포하는 방법은 [생략되지 않은 배포 지침](../deploy/runtime.md)을 검토하십시오.

### AEM 처리 프로필과 통합{#processing-profile}

새로 만들거나 이 배포된 작업자를 호출하는 기존 사용자 지정 처리 프로필 서비스를 수정하여 AEM에서 작업자를 호출합니다.

![프로필 처리 중](./assets/metadata/processing-profile.png)

1. __AEM 관리자__(으)로 AEM as a Cloud Service 작성자 서비스에 로그인합니다
1. __도구 > Assets > 처리 프로필__(으)로 이동
1. 새 프로필을 __만들거나__, 기존 처리 프로필을 __편집__
1. __사용자 지정__ 탭을 탭하고 __새로 추가__&#x200B;를 탭합니다.
1. 새 서비스 정의
   + __메타데이터 렌디션 만들기__: 활성으로 전환
   + __끝점:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + [deploy](#deploy) 중에 또는 `aio app get-url` 명령을 사용하여 얻은 작업자의 URL입니다. URL이 AEM as a Cloud Service 환경을 기반으로 올바른 작업 영역을 가리켜야 합니다.
   + __서비스 매개 변수__
      + __매개 변수 추가__ 탭
         + 키: `colorFamily`
         + 값: `pantone`
            + 지원되는 값: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __MIME 유형__
      + __포함 항목:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + 색상을 파생하는 데 사용되는 타사 npm 모듈에서 지원되는 유일한 MIME 유형입니다.
      + __제외:__ `Leave blank`
1. 오른쪽 상단에서 __저장__&#x200B;을 누릅니다.
1. 아직 적용하지 않은 경우 AEM Assets 폴더에 처리 프로필 적용

### 메타데이터 스키마 업데이트{#metadata-schema}

색상 메타데이터를 검토하려면 이미지의 메타데이터 스키마에 있는 두 개의 새 필드를 작업자가 채우는 새 메타데이터 데이터 속성에 매핑합니다.

![메타데이터 스키마](./assets/metadata/metadata-schema.png)

1. AEM Author 서비스에서 __도구 > Assets > 메타데이터 스키마__&#x200B;로 이동합니다.
1. __기본__(으)로 이동하여 __이미지__&#x200B;를 선택 및 편집하고 읽기 전용 양식 필드를 추가하여 생성된 색상 메타데이터를 표시합니다
1. __한 줄 텍스트 추가__
   + __필드 레이블__: `Colors Family`
   + __속성에 매핑__: `./jcr:content/metadata/wknd:colorsFamily`
   + __규칙 > 필드 > 편집 비활성화__: 선택됨
1. __다중 값 텍스트__ 추가
   + __필드 레이블__: `Colors`
   + __속성에 매핑__: `./jcr:content/metadata/wknd:colors`
1. 오른쪽 상단에서 __저장__&#x200B;을 누릅니다.

## 자산 처리 중

![자산 세부 정보](./assets/metadata/asset-details.png)

1. AEM Author 서비스에서 __Assets > 파일__(으)로 이동합니다.
1. 폴더 또는 하위 폴더로 이동하면 처리 프로필이 적용됩니다.
1. 폴더에 새 이미지(JPEG, PNG, GIF 또는 SVG)를 업로드하거나 업데이트된 [처리 프로필](#processing-profile)을 사용하여 기존 이미지를 다시 처리합니다.
1. 처리가 완료되면 자산을 선택하고 맨 위 작업 표시줄에서 __속성__&#x200B;을 탭하여 해당 메타데이터를 표시합니다
1. 사용자 지정 Asset compute 메타데이터 작업자에서 다시 작성한 메타데이터에 대한 `Colors Family` 및 `Colors` [메타데이터 필드](#metadata-schema)를 검토하십시오.

자산의 메타데이터에 기록된 색상 메타데이터를 사용하여 `[dam:Asset]/jcr:content/metadata` 리소스에서 이 메타데이터는 검색을 통해 이러한 용어를 사용하여 향상된 자산 검색 기능으로 인덱싱되며, __DAM 메타데이터 원본에 쓰기__ 워크플로우가 호출되는 경우 해당 메타데이터를 자산의 바이너리에 다시 쓸 수도 있습니다.

### AEM Assets의 메타데이터 렌디션

![AEM Assets 메타데이터 렌디션 파일](./assets/metadata/cqdam-metadata-rendition.png)

asset compute 메타데이터 작업자가 생성한 실제 XMP 파일도 에셋에 개별 렌디션으로 저장됩니다. 이 파일은 일반적으로 사용되지 않고, 자산의 메타데이터 노드에 적용된 값이 사용되지만 작업자의 원시 XML 출력은 AEM에서 사용할 수 있습니다.

## Github의 메타데이터 색상 작업자 코드

최종 `metadata-colors/index.js`은(는) Github의 다음 위치에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

최종 `test/asset-compute/metadata-colors` 테스트 도구 모음은 Github의 다음 위치에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
