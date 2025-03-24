---
title: Asset Compute 작업자와 AEM 처리 프로필 통합
description: AEM as a Cloud Service은 AEM Assets 처리 프로필을 통해 Adobe I/O Runtime에 배포된 Asset Compute 작업자와 통합됩니다. 처리 프로필은 사용자 정의 작업자를 사용하여 특정 에셋을 처리하고 작업자가 생성한 파일을 에셋 변환으로 저장하도록 작성자 서비스에 구성됩니다.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 126
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# AEM 처리 프로필과 통합

Asset Compute 작업자가 AEM as a Cloud Service에서 사용자 지정 렌디션을 생성하려면 처리 프로필 을 통해 AEM as a Cloud Service Author 서비스에 등록해야 합니다. 해당 처리 프로필의 대상인 모든 에셋은 업로드 또는 재처리 시 작업자를 호출하고 사용자 지정 렌디션을 생성하여 에셋의 렌디션을 통해 사용할 수 있도록 합니다.

## 처리 프로필 정의

먼저 구성 가능한 매개 변수로 작업자를 호출하는 새 처리 프로필을 만듭니다.

![프로필 처리 중](./assets/processing-profiles/new-processing-profile.png)

1. __AEM as a Cloud Service 관리자__(으)로 AEM 작성자 서비스에 로그인합니다. 튜토리얼이므로 샌드박스의 개발 환경 또는 환경을 사용하는 것이 좋습니다.
1. __도구 > Assets > 처리 프로필__(으)로 이동
1. __만들기__ 단추 누르기
1. 처리 프로필 이름 지정, `WKND Asset Renditions`
1. __사용자 지정__ 탭을 탭하고 __새로 추가__&#x200B;를 탭합니다.
1. 새 서비스 정의
   + __렌디션 이름:__ `Circle`
      + AEM Assets에서 이 렌디션을 식별하는 데 사용된 렌디션의 파일 이름
   + __확장:__ `png`
      + 생성된 렌디션의 확장명입니다. 작업자 웹 서비스에서 지원하는 출력 형식이므로 `png`(으)로 설정하면 서클 잘라내기 뒤에 투명 배경이 생깁니다.
   + __끝점:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + `aio app get-url`을(를) 통해 얻은 작업자의 URL입니다. URL이 AEM as a Cloud Service 환경을 기반으로 올바른 작업 영역을 가리켜야 합니다.
      + 작업자 URL이 올바른 작업 영역을 가리켜야 합니다. AEM as a Cloud Service Stage는 단계 작업 영역 URL을 사용하고 AEM as a Cloud Service Production은 프로덕션 작업 영역 URL을 사용해야 합니다.
   + __서비스 매개 변수__
      + __매개 변수 추가__ 탭
         + 키: `size`
         + 값: `1000`
      + __매개 변수 추가__ 탭
         + 키: `contrast`
         + 값: `0.25`
      + __매개 변수 추가__ 탭
         + 키: `brightness`
         + 값: `0.10`
      + Asset Compute 작업자에게 전달되고 `rendition.instructions` JavaScript 개체를 통해 사용할 수 있는 키/값 쌍입니다.
   + __MIME 유형__
      + __포함 항목:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + 이 MIME 유형은 작업자의 npm 모듈에만 해당됩니다. 이 목록은 사용자 정의 작업자가 처리하는 것을 제한합니다.
      + __제외:__ `Leave blank`
         + 이 서비스 구성을 사용하여 이러한 MIME 유형의 자산을 처리하지 마십시오. 이 경우 허용 목록만 사용합니다.
1. 오른쪽 상단에서 __저장__&#x200B;을 누릅니다.

## 처리 프로필 적용 및 호출

1. 새로 만든 처리 프로필 `WKND Asset Renditions`을(를) 선택하십시오.
1. 상단 작업 표시줄에서 __폴더에 프로필 적용__&#x200B;을 탭합니다.
1. 처리 프로필을 적용할 폴더(예: `WKND`)를 선택하고 __적용__&#x200B;을 누릅니다.
1. __AEM > Assets > 파일__&#x200B;을 통해 처리 프로필이 적용되지 않은 폴더로 이동한 다음 `WKND`을(를) 탭합니다.
1. 처리 프로필이 적용된 폴더 아래의 모든 폴더에서 일부 새 이미지 자산([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) 및 [sample-3.jpg](../assets/samples/sample-3.jpg))을 업로드하고 업로드된 자산이 처리될 때까지 기다립니다.
1. 에셋을 탭하여 세부 정보 열기
   + 기본 렌디션은 사용자 지정 렌디션보다 AEM에서 더 빨리 생성되어 표시될 수 있습니다.
1. 왼쪽 사이드바에서 __렌디션__ 보기를 엽니다.
1. `Circle.png` 자산을 탭하고 생성된 렌디션을 검토합니다.

   ![생성된 렌디션](./assets/processing-profiles/rendition.png)

## 완료되었습니다.

축하합니다! AEM as a Cloud Service Asset Compute 마이크로서비스를 확장하는 방법에 대한 [자습서](../overview.md)를 완료했습니다! 이제 AEM as a Cloud Service 작성자 서비스에서 사용할 사용자 정의 Asset Compute 작업자를 설정, 개발, 테스트, 디버그 및 배포할 수 있는 권한이 있어야 합니다.

### Github에서 전체 프로젝트 소스 코드 검토

최종 Asset Compute 프로젝트는 Github의 다음 위치에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github는 프로젝트의 최종 상태로, 작업자 및 테스트 사례로 완전히 채워져 있지만 자격 증명은 들어 있지 않습니다. `.env`, `.config.json` 또는 `.aio`._

## 문제 해결

+ [AEM의 에셋에서 사용자 정의 렌디션이 누락됨](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM에서 자산 처리 실패](../troubleshooting.md#asset-processing-fails)
