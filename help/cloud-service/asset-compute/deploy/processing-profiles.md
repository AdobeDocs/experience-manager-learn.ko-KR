---
title: asset compute 작업자와 AEM 처리 프로필 통합
description: AEM as a Cloud Service은 AEM Assets 처리 프로필을 통해 Adobe I/O Runtime에 배포된 Asset compute 작업자와 통합됩니다. 처리 프로필은 사용자 정의 작업자를 사용하여 특정 자산을 처리하고 작업자가 생성한 파일을 자산 변환으로 저장하도록 작성자 서비스에 구성됩니다.
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 1%

---


# AEM 처리 프로필과 통합

asset compute 작업자가 AEM에서 Cloud Service으로 사용자 정의 렌디션을 생성하려면 처리 프로필을 통해 Cloud Service 작성자 서비스로 AEM에 등록해야 합니다. 해당 처리 프로필의 모든 자산은 업로드 또는 재처리 시 작업자를 호출하고, 자산의 변환을 통해 생성 및 사용할 수 있는 사용자 정의 렌디션을 갖도록 합니다.

## 처리 프로필 정의

먼저 구성 가능한 매개변수로 작업자를 호출하는 새 처리 프로파일을 생성합니다.

![처리 프로필](./assets/processing-profiles/new-processing-profile.png)

1. AEM as a0/>AEM Administrator __로 Cloud Service 작성자 서비스에 로그인합니다.__ 자습서이므로 샌드박스에서 개발 환경 또는 환경을 사용하는 것이 좋습니다.
1. __도구 > 자산 > 처리 프로필__&#x200B;로 이동합니다.
1. __만들기__ 단추를 누릅니다
1. 처리 프로필의 이름을 `WKND Asset Renditions` 로 지정합니다.
1. __사용자 지정__ 탭을 탭하고 __새로 추가__&#x200B;를 탭합니다
1. 새 서비스 정의
   + __변환 이름:__ `Circle`
      + AEM Assets에서 이 변환을 식별하는 데 사용할 파일 이름 표현물
   + __확장:__ `png`
      + 생성될 표현물의 확장. 작업자 웹 서비스가 지원하는 지원되는 출력 형식이므로 `png`로 설정하고, 이 경우 원 뒤에 투명한 배경이 표시됩니다.
   + __끝점:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + `aio app get-url`을 통해 가져온 작업자의 URL입니다. URL이 AEM as a Cloud Service 환경을 기반으로 하는 올바른 작업 영역에서 표시되는지 확인하십시오.
      + 작업자 URL이 올바른 작업 영역을 가리키는지 확인합니다. Cloud Service 스테이지로 AEM은 단계 작업 공간 URL을 사용해야 하며, AEM as a Cloud Service 프로덕션은 프로덕션 작업 공간 URL을 사용해야 합니다.
   + __서비스 매개 변수__
      + __매개 변수 추가__ 를 누릅니다
         + 키: `size`
         + 값: `1000`
      + __매개 변수 추가__ 를 누릅니다
         + 키: `contrast`
         + 값: `0.25`
      + __매개 변수 추가__ 를 누릅니다
         + 키: `brightness`
         + 값: `0.10`
      + 이러한 키/값 쌍은 Asset compute 작업자로 전달되고 `rendition.instructions` JavaScript 개체를 통해 사용할 수 있습니다.
   + __MIME 유형__
      + __포함:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + 이러한 MIME 유형은 작업자의 npm 모듈에서만 사용할 수 있습니다. 이 목록은 사용자 정의 작업자가 처리할 자산을 제한합니다.
      + __제외:__ `Leave blank`
         + 이 서비스 구성을 사용하여 이러한 MIME 유형으로 자산을 처리하지 마십시오. 이 경우 허용 목록만 사용합니다.
1. 오른쪽 상단에 있는 __저장__&#x200B;을 탭합니다.

## 처리 프로필 적용 및 호출

1. 새로 만든 처리 프로필 `WKND Asset Renditions` 을 선택합니다.
1. 맨 위 작업 표시줄에서 __프로필 적용__&#x200B;을 누릅니다
1. 처리 프로필을 적용할 폴더(예: `WKND`)를 선택하고 __적용__
1. __AEM > Assets > Files__&#x200B;을 통해 처리 프로필이 적용되지 않은 폴더로 이동하고 `WKND` 을 탭합니다.
1. 처리 프로필이 적용된 폴더 아래의 폴더에 새 이미지 자산([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) 및 [sample-3.jpg](../assets/samples/sample-3.jpg))을 업로드하고 업로드된 자산이 처리될 때까지 기다립니다.
1. 자산을 탭하여 세부 사항을 엽니다
   + 기본 표현물은 사용자 지정 표현물보다 AEM에서 더 빨리 생성되고 나타날 수 있습니다.
1. 왼쪽 사이드바에서 __표현물__ 보기를 엽니다
1. `Circle.png` 이라는 자산을 탭하고 생성된 렌디션을 검토합니다

   ![생성된 표현물](./assets/processing-profiles/rendition.png)

## 완료됨!

축하합니다! AEM을 Cloud Service Asset compute 마이크로 서비스로 확장하는 방법에 대한 [자습서](../overview.md)를 완료했습니다! 이제 AEM에서 Cloud Service 작성자 서비스로 사용할 사용자 지정 Asset compute 작업자를 설정, 개발, 테스트, 디버그 및 배포할 수 있습니다.

### Github에서 전체 프로젝트 소스 코드 검토

최종 Asset compute 프로젝트는 Github에서 사용할 수 있습니다.

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github 포함 은 프로젝트의 최종 상태이며, 작업자 및 테스트 사례로 완전히 채워지지만 자격 증명(예: )은 포함하지 않습니다. `.env`,  `.config.json` 또는  `.aio`._

## 문제 해결

+ [AEM의 자산에서 사용자 지정 표현물이 누락됨](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM에서 자산 처리가 실패합니다](../troubleshooting.md#asset-processing-fails)
