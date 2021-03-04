---
title: asset compute 직원과 AEM 처리 프로필 통합
description: Cloud Service으로 AEM은 AEM Assets 처리 프로필을 통해 Adobe I/O Runtime에 배포된 Asset compute 근로자와 통합됩니다. 처리 프로필은 사용자 지정 작업자를 사용하여 특정 자산을 처리하고 작업자가 생성한 파일을 자산 표현물로 저장하도록 작성자 서비스에 구성됩니다.
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: 통합, 개발
role: 개발자
level: 중간, 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 2%

---


# AEM 처리 프로필과 통합

asset compute 근로자가 Cloud Service으로 AEM에서 사용자 정의 변환을 생성하려면 처리 프로필을 통해 AEM에 Cloud Service 작성자 서비스로 등록해야 합니다. 처리 프로필의 모든 자산은 업로드 또는 재처리 시 호출된 워커를, 자산의 표현물을 통해 생성 및 사용할 수 있도록 사용자 지정 변환을 가집니다.

## 처리 프로필 정의

먼저 구성 가능한 매개 변수와 함께 작업자를 호출할 새 처리 프로파일을 만듭니다.

![처리 프로필](./assets/processing-profiles/new-processing-profile.png)

1. Cloud Service 작성자 서비스로 AEM에 로그인하고 __AEM 관리자__ 이 자습서이므로 샌드박스의 개발 환경 또는 환경을 사용하는 것이 좋습니다.
1. __도구 > 자산 > 처리 프로필__&#x200B;으로 이동합니다.
1. __만들기__ 단추를 누릅니다.
1. 처리 프로필의 이름을 `WKND Asset Renditions` 지정합니다.
1. __사용자 지정__ 탭을 누르고 __새로 추가__&#x200B;를 탭합니다.
1. 새 서비스 정의
   + __변환 이름:__ `Circle`
      + AEM Assets에서 이 변환을 식별하는 데 사용할 파일 이름 변환
   + __확장:__ `png`
      + 생성할 변환의 확장. 이 형식은 워커의 웹 서비스가 지원하는 출력 형식이며 원 뒤에 투명한 배경이 잘려서 `png`으로 설정합니다.
   + __끝점:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 이 URL은 `aio app get-url`을 통해 얻은 워커의 URL입니다. AEM을 Cloud Service 환경으로 기반으로 올바른 작업 영역에서 URL 점을 확인합니다.
      + 작업자 URL이 올바른 작업 영역을 가리키는지 확인합니다. Cloud Service 스테이지로 AEM은 스테이지 작업 영역 URL을 사용해야 하며 Cloud Service 프로덕션으로 AEM은 프로덕션 작업 공간 URL을 사용해야 합니다.
   + __서비스 매개 변수__
      + __매개 변수 추가__&#x200B;를 탭합니다.
         + 키: `size`
         + 값: `1000`
      + __매개 변수 추가__&#x200B;를 탭합니다.
         + 키: `contrast`
         + 값: `0.25`
      + __매개 변수 추가__&#x200B;를 탭합니다.
         + 키: `brightness`
         + 값: `0.10`
      + 이러한 키/값 쌍은 Asset compute 워커으로 전달되고 `rendition.instructions` JavaScript 객체를 통해 사용할 수 있습니다.
   + __MIME 유형__
      + __포함 사항:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + 이 MIME 유형은 워커의 npm 모듈에만 해당합니다. 이 목록은 사용자 지정 워커에서 처리할 자산을 제한합니다.
      + __제외:__ `Leave blank`
         + 이 서비스 구성을 사용하여 MIME 형식을 사용하여 자산을 처리하지 마십시오. 이 경우 허용 목록만 사용합니다.
1. 오른쪽 상단에서 __저장__&#x200B;을 탭합니다.

## 처리 프로필 적용 및 호출

1. 새로 만든 처리 프로필 `WKND Asset Renditions` 선택
1. 위쪽 작업 표시줄의 __폴더에 프로필 적용__&#x200B;을 탭합니다.
1. 처리 프로필을 적용할 폴더(예: `WKND`)를 선택하고 __적용__&#x200B;을 탭합니다.
1. __AEM > 자산 > 파일__&#x200B;을 통해 처리 프로필이 적용되지 않은 폴더로 이동하여 `WKND` 키를 누릅니다.
1. 처리 프로필이 적용된 폴더 아래의 폴더에 있는 새 이미지 자산([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) 및 [sample-3.jpg](../assets/samples/sample-3.jpg))을 업로드하고 업로드된 에셋이 처리될 때까지 기다립니다.
1. 자산을 탭하여 세부 정보를 엽니다.
   + 기본 변환은 사용자 정의 변환보다 AEM에서 보다 빠르게 생성 및 나타날 수 있습니다.
1. 왼쪽 사이드바에서 __변환__ 보기를 엽니다.
1. `Circle.png` 자산을 누르고 생성된 변환을 검토합니다.

   ![생성된 변환](./assets/processing-profiles/rendition.png)

## 완료됨!

축하합니다! Cloud Service Asset compute 마이크로서비스로 AEM을 확장하는 방법에 대한 [tutorial](../overview.md)을(를) 완료했습니다. 이제 AEM에서 Cloud Service 작성자 서비스로 사용할 사용자 정의 Asset compute 작업자를 설정, 개발, 테스트, 디버그 및 배포할 수 있습니다.

### Github에서 전체 프로젝트 소스 코드 검토

최종 Asset compute 프로젝트는 Github(

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains는 프로젝트의 최종 상태이며, 워커 및 테스트 케이스로 완전히 채워지지만 자격 증명(예:)은 포함하지 않습니다. `.env`,  `.config.json` 또는 `.aio`._

## 문제 해결

+ [AEM의 자산에서 사용자 지정 표현물이 없음](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM에서 자산 처리 실패](../troubleshooting.md#asset-processing-fails)
