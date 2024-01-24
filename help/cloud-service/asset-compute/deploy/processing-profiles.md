---
title: AEM 처리 프로필과 Asset compute 작업자 통합
description: AEM as a Cloud Service은 AEM Assets 처리 프로필을 통해 Adobe I/O Runtime에 배포된 Asset compute 작업자와 통합됩니다. 처리 프로필은 사용자 정의 작업자를 사용하여 특정 에셋을 처리하고 작업자가 생성한 파일을 에셋 변환으로 저장하도록 작성자 서비스에 구성됩니다.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# AEM 처리 프로필과 통합

asset compute 작업자가 AEMas a Cloud Service 에서 사용자 정의 렌디션을 생성하려면 처리 프로필을 통해 AEM as a Cloud Service Author 서비스에 등록해야 합니다. 해당 처리 프로필의 대상인 모든 에셋은 업로드 또는 재처리 시 작업자를 호출하고 사용자 지정 렌디션을 생성하여 에셋의 렌디션을 통해 사용할 수 있도록 합니다.

## 처리 프로필 정의

먼저 구성 가능한 매개 변수로 작업자를 호출하는 새 처리 프로필을 만듭니다.

![처리 프로필](./assets/processing-profiles/new-processing-profile.png)

1. AEM as a Cloud Service Author 서비스에 다음으로 로그인 __AEM 관리자__. 튜토리얼이므로 샌드박스의 개발 환경 또는 환경을 사용하는 것이 좋습니다.
1. 다음으로 이동 __도구 > 에셋 > 처리 프로필__
1. 누르기 __만들기__ 단추
1. 처리 프로필에 이름 지정, `WKND Asset Renditions`
1. 탭 __사용자 정의__ 탭, 탭 __새로 추가__
1. 새 서비스 정의
   + __렌디션 이름:__ `Circle`
      + AEM Assets에서 이 렌디션을 식별하는 데 사용된 렌디션의 파일 이름
   + __확장:__ `png`
      + 생성된 렌디션의 확장명입니다. 다음으로 설정 `png` 이는 작업자의 웹 서비스가 지원하는 출력 형식이므로 서클 자르기 뒤에 투명한 배경이 생깁니다.
   + __끝점:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 를 통해 얻은 작업자의 URL입니다. `aio app get-url`. URL이 AEM as a Cloud Service 환경을 기반으로 올바른 작업 영역을 가리켜야 합니다.
      + 작업자 URL이 올바른 작업 영역을 가리켜야 합니다. AEM as a Cloud Service Stage는 Stage 작업 영역 URL을 사용하고, AEM as a Cloud Service Production은 Production 작업 영역 URL을 사용해야 합니다.
   + __서비스 매개 변수__
      + 누르기 __매개 변수 추가__
         + 키: `size`
         + 값: `1000`
      + 누르기 __매개 변수 추가__
         + 키: `contrast`
         + 값: `0.25`
      + 누르기 __매개 변수 추가__
         + 키: `brightness`
         + 값: `0.10`
      + asset compute 작업자에게 전달되고 를 통해 사용할 수 있는 이러한 키/값 쌍 `rendition.instructions` JavaScript 개체입니다.
   + __Mime 유형__
      + __포함 사항:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + 이 MIME 유형은 작업자의 npm 모듈에만 해당됩니다. 이 목록은 사용자 정의 작업자가 처리하는 것을 제한합니다.
      + __제외:__ `Leave blank`
         + 이 서비스 구성을 사용하여 이러한 MIME 유형의 자산을 처리하지 마십시오. 이 경우 허용 목록만 사용합니다.
1. 누르기 __저장__ 오른쪽 상단에서

## 처리 프로필 적용 및 호출

1. 새로 생성된 처리 프로필을 선택합니다. `WKND Asset Renditions`
1. 누르기 __폴더에 프로필 적용__ 맨 위의 작업 표시줄에서
1. 처리 프로필을 적용할 폴더 선택: `WKND` 및 탭 __적용__
1. 를 통해 처리 프로필이 적용되지 않은 폴더로 이동합니다. __AEM > Assets > 파일__ 을 누릅니다. `WKND`.
1. 일부 새 이미지 자산 업로드 ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg), 및 [sample-3.jpg](../assets/samples/sample-3.jpg)) 폴더에 있는 모든 폴더에 처리 프로필이 적용되어 업로드된 에셋이 처리될 때까지 기다립니다.
1. 에셋을 탭하여 세부 정보 열기
   + 기본 렌디션은 사용자 지정 렌디션보다 AEM에서 더 빨리 생성되어 표시될 수 있습니다.
1. 를 엽니다. __표현물__ 왼쪽 사이드바에서 보기
1. 이름이 인 에셋 탭 `Circle.png` 생성된 렌디션 검토

   ![생성된 렌디션](./assets/processing-profiles/rendition.png)

## 완료되었습니다.

축하합니다! 다음을 완료했습니다. [튜토리얼](../overview.md) AEM as a Cloud Service Asset compute 마이크로서비스를 확장하는 방법에 대해 알아보십시오! 이제 AEM as a Cloud Service 작성자 서비스에서 사용할 사용자 정의 Asset compute 작업자를 설정, 개발, 테스트, 디버그 및 배포할 수 있는 권한이 있어야 합니다.

### Github에서 전체 프로젝트 소스 코드 검토

최종 Asset compute 프로젝트는 Github의 다음 위치에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github는 프로젝트의 최종 상태로, 작업자 및 테스트 사례로 완전히 채워져 있지만, 자격 증명은 포함하지 않습니다(예: ). `.env`, `.config.json` 또는 `.aio`._

## 문제 해결

+ [AEM의 에셋에서 사용자 정의 렌디션 누락](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM에서 자산 처리 실패](../troubleshooting.md#asset-processing-fails)
