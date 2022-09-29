---
title: asset compute 작업자와 AEM 처리 프로필 통합
description: AEM as a Cloud Service은 AEM Assets 처리 프로필을 통해 Adobe I/O Runtime에 배포된 Asset compute 작업자와 통합됩니다. 처리 프로필은 사용자 정의 작업자를 사용하여 특정 자산을 처리하고 작업자가 생성한 파일을 자산 변환으로 저장하도록 작성자 서비스에 구성됩니다.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 2%

---

# AEM 처리 프로필과 통합

asset compute 작업자가 AEM as a Cloud Service에서 사용자 지정 변환을 생성하려면 처리 프로필을 통해 AEM as a Cloud Service 작성자 서비스에 등록해야 합니다. 해당 처리 프로필에 적용되는 모든 자산은 업로드하거나 재처리 시 작업자를 호출하고, 생성된 사용자 정의 렌디션을 생성하여 자산의 렌디션을 통해 사용할 수 있도록 합니다.

## 처리 프로필 정의

먼저 구성 가능한 매개변수로 작업자를 호출하는 새 처리 프로파일을 생성합니다.

![처리 프로필](./assets/processing-profiles/new-processing-profile.png)

1. AEM as a Cloud Service 작성자 서비스에 로그인하십시오. __AEM 관리자__. 자습서이므로 샌드박스에서 개발 환경 또는 환경을 사용하는 것이 좋습니다.
1. 다음으로 이동 __도구 > 자산 > 처리 프로필__
1. 탭 __만들기__ 버튼
1. 처리 프로필의 이름을 지정합니다. `WKND Asset Renditions`
1. 탭하기 __사용자 지정__ 탭, 탭 __새로 추가__
1. 새 서비스 정의
   + __변환 이름:__ `Circle`
      + AEM Assets에서 이 변환을 식별하는 데 사용되는 표현물의 파일 이름입니다
   + __확장:__ `png`
      + 생성된 표현물의 확장. 을 로 설정합니다. `png` 이는 작업자 웹 서비스가 지원하는 지원되는 출력 포맷이며, 원 뒤에 투명한 배경이 나타납니다.
   + __끝점:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 을 통해 가져온 작업자의 URL입니다 `aio app get-url`. AEM as a Cloud Service 환경을 기반으로 올바른 작업 영역에서 URL을 가리키는지 확인합니다.
      + 작업자 URL이 올바른 작업 영역을 가리키는지 확인합니다. AEM as a Cloud Service 스테이지는 스테이지 작업 공간 URL을 사용해야 하며 AEM as a Cloud Service 프로덕션은 프로덕션 작업 공간 URL을 사용해야 합니다.
   + __서비스 매개 변수__
      + 탭 __매개 변수 추가__
         + 키: `size`
         + 값: `1000`
      + 탭 __매개 변수 추가__
         + 키: `contrast`
         + 값: `0.25`
      + 탭 __매개 변수 추가__
         + 키: `brightness`
         + 값: `0.10`
      + asset compute 작업자로 전달되고 을 통해 사용할 수 있는 이러한 키/값 쌍입니다 `rendition.instructions` JavaScript 개체.
   + __MIME 유형__
      + __다음을 포함합니다.__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + 이러한 MIME 유형은 작업자의 npm 모듈에서만 사용할 수 있습니다. 이 목록은 사용자 정의 작업자가 처리하는 것을 제한합니다.
      + __제외:__ `Leave blank`
         + 이 서비스 구성을 사용하여 이러한 MIME 유형으로 자산을 처리하지 마십시오. 이 경우 허용 목록만 사용합니다.
1. 탭 __저장__ 오른쪽 상단에서

## 처리 프로필 적용 및 호출

1. 새로 만든 처리 프로필 을 선택합니다. `WKND Asset Renditions`
1. 탭 __폴더에 프로필 적용__ 상단 작업 모음에서
1. 처리 프로필을 적용할 폴더(예: )를 선택합니다 `WKND` 탭 __적용__
1. 를 통해 처리 프로필이 적용되지 않은 폴더로 이동합니다 __AEM > 자산 > 파일__ 및 `WKND`.
1. 새 이미지 자산 업로드([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg), 및 [sample-3.jpg](../assets/samples/sample-3.jpg))을 처리 프로필이 적용된 폴더 아래의 모든 폴더에서 사용할 수 있고 업로드된 자산이 처리될 때까지 기다립니다.
1. 자산을 탭하여 세부 사항을 엽니다
   + 기본 표현물은 사용자 지정 표현물보다 AEM에서 더 빨리 생성되고 나타날 수 있습니다.
1. 를 엽니다. __표현물__ 왼쪽 사이드바에서 보기
1. 이름이 인 자산을 탭합니다. `Circle.png` 생성된 변환 및 검토

   ![생성된 표현물](./assets/processing-profiles/rendition.png)

## 완료됨!

축하합니다! 을(를) 완료했습니다 [튜토리얼](../overview.md) AEM as a Cloud Service Asset compute 마이크로 서비스를 확장하는 방법에 대해 알아보십시오! 이제 AEM as a Cloud Service 작성자 서비스에서 사용할 사용자 지정 Asset compute 작업자를 설정, 개발, 테스트, 디버그 및 배포할 수 있습니다.

### Github에서 전체 프로젝트 소스 코드 검토

최종 Asset compute 프로젝트는 Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github 포함 은 프로젝트의 최종 상태이며, 작업자 및 테스트 사례로 완전히 채워지지만 자격 증명(예: )은 포함하지 않습니다. `.env`, `.config.json` 또는 `.aio`._

## 문제 해결

+ [AEM의 자산에서 사용자 지정 표현물이 누락됨](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM에서 자산 처리가 실패합니다](../troubleshooting.md#asset-processing-fails)
