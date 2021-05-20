---
title: asset compute 개발 도구
description: asset compute 개발 도구는 개발자가 Adobe I/O Runtime의 Asset compute 리소스에 대해 AEM SDK 컨텍스트 외부에 있는 Asset Computer 작업자를 로컬로 구성 및 실행할 수 있도록 해주는 로컬 웹 도구입니다.
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%

---


# asset compute 개발 도구

asset compute 개발 도구는 개발자가 Adobe I/O Runtime의 Asset compute 리소스에 대해 AEM SDK 컨텍스트 외부에 있는 Asset Computer 작업자를 로컬로 구성 및 실행할 수 있도록 해주는 로컬 웹 도구입니다.

## asset compute 개발 도구 실행

터미널 명령을 통해 Asset compute 프로젝트의 루트에서 Asset compute 개발 도구를 실행할 수 있습니다.

```
$ aio app run
```

이렇게 하면 __http://localhost:9000__&#x200B;에서 개발 도구가 시작되고 브라우저 창에서 자동으로 열립니다. 개발 도구를 실행하려면 쿼리 매개 변수](#troubleshooting__devtooltoken)을 통해 유효하고 자동 생성된 devToolToken을 제공해야 합니다.[

## asset compute 개발 도구 인터페이스 이해{#interface}

![asset compute 개발 도구](./assets/development-tool/asset-compute-dev-tool.png)

1. __소스 파일:__  소스 파일을 선택하여 다음을 수행할 수 있습니다.
   + asset compute 작업자에게 전달된 `source` 바이너리인 자산 바이너리를 선택했습니다
   + 소스 파일 업로드
1. __asset compute 프로필 정의:__  매개 변수를 포함하여 실행할 Asset compute 작업자를 정의합니다.작업자의 URL 끝점, 결과 변환 이름 및 모든 매개 변수 포함
1. __실행:__ 실행 단추는 Asset compute 구성 프로필 편집기에 정의된 대로 Asset compute 프로필을 실행합니다
1. __Abort:__  Abort 버튼을 클릭하면 실행 버튼을 탭하지 않은 실행 취소
1. __요청/응답:__  Adobe I/O Runtime에서 실행되는 Asset compute 작업자에 대한 HTTP 요청 및 응답을 제공합니다. 이 기능은 디버깅에 유용할 수 있습니다
1. __활성화 로그:__ 오류와 함께 Asset compute 작업자의 실행을 설명하는 로그입니다. 이 정보는 `aio app run` 표준에서도 확인할 수 있습니다
1. __표현물:__ Asset compute 작업자 실행에서 생성된 모든 표현물을 표시합니다
1. __devToolToken 쿼리 매개 변수__ : Asset compute 개발 도구 토큰에는 유효한  `devToolToken` 쿼리 매개 변수가 있어야 합니다. 이 토큰은 새 개발 도구가 생성될 때마다 자동으로 생성됩니다

### 사용자 정의 작업자 실행

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_개발 도구에서 Asset compute 작업을 실행하는 클릭스루(오디오 없음)_

1. `aio app run` 명령을 사용하여 프로젝트 루트에서 Asset compute 개발 도구가 시작되었는지 확인합니다.
1. asset compute 개발 도구에서 [샘플 이미지 파일](../assets/samples/sample-file.jpg)을 업로드하거나 선택합니다
   + __소스 파일__ 드롭다운에서 파일을 선택했는지 확인합니다
1. __Asset compute 프로필 정의__ 텍스트 영역을 검토합니다
   + `worker` 키는 배포된 Asset compute 작업자에 대한 URL을 정의합니다
   + `name` 키는 생성할 표현물의 이름을 정의합니다
   + 다른 키/값은 이 JSON 개체에 제공할 수 있으며 `rendition.instructions` 개체 아래의 작업자에서 사용할 수 있습니다
      + 선택 사항으로 `size`, `contrast` 및 `brightness`에 대한 값을 추가할 수 있습니다.

         ```json
         {
             "renditions": [
                 {
                     "worker": "...",
                     "name": "rendition.png",
                     "size":"800",
                     "contrast": "0.30",
                     "brightness": "-0.15"
                 }
             ]
         }
         ```

1. __실행__ 단추를 누릅니다
1. __표현물 섹션__&#x200B;은 표현물 자리 표시자로 채워집니다
1. 작업자가 완료되면 변환 자리 표시자에 생성된 표현물이 표시됩니다

개발 도구가 실행되는 동안 작업자 코드에 코드를 변경하면 변경 사항이 &quot;핫 배포&quot;됩니다. &quot;핫 배포&quot;가 몇 초 정도 소요되므로 개발 도구에서 작업자를 다시 실행하기 전에 배포가 완료될 수 있습니다.

## 문제 해결

+ [잘못된 YAML 들여쓰기](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize 제한이 너무 낮게 설정되어 있습니다.](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [private.key가 누락되어 개발 도구를 시작할 수 없습니다.](../troubleshooting.md#missing-private-key)
+ [소스 파일 드롭다운이 잘못되었습니다.](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolToken 쿼리 매개 변수가 없거나 잘못되었습니다.](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [소스 파일을 제거할 수 없습니다.](../troubleshooting.md#unable-to-remove-source-files)
+ [부분적으로 그려진/손상된 표현물이 반환되었습니다.](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
