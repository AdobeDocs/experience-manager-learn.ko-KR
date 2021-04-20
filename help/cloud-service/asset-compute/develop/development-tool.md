---
title: asset compute 개발 도구
description: asset compute 개발 도구는 개발자가 Adobe I/O Runtime의 Asset compute 리소스에 대해 AEM SDK의 컨텍스트에서 벗어나 로컬로 에셋 컴퓨터 작업자를 구성 및 실행할 수 있도록 하는 로컬 웹 도구입니다.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---


# asset compute 개발 도구

asset compute 개발 도구는 개발자가 Adobe I/O Runtime의 Asset compute 리소스에 대해 AEM SDK의 컨텍스트에서 벗어나 로컬로 에셋 컴퓨터 작업자를 구성 및 실행할 수 있도록 하는 로컬 웹 도구입니다.

## asset compute 개발 도구 실행

asset compute 개발 도구는 터미널 명령을 통해 Asset compute 프로젝트의 루트에서 실행할 수 있습니다.

```
$ aio app run
```

이렇게 하면 __http://localhost:9000__&#x200B;에서 개발 도구가 시작되고 브라우저 창에서 자동으로 열립니다. 개발 도구를 실행하려면 쿼리 매개 변수](#troubleshooting__devtooltoken)를 통해 자동 생성된 유효한 devToolToken을 제공해야 합니다.[

## asset compute 개발 도구 인터페이스 이해{#interface}

![asset compute 개발 도구](./assets/development-tool/asset-compute-dev-tool.png)

1. __소스 파일:__ 소스 파일 선택을 사용하여 다음을 수행합니다.
   + asset compute 워커에 전달되는 `source` 바이너리인 자산 바이너리를 선택했습니다.
   + 소스 파일 업로드
1. __asset compute 프로필 정의:매개 변수를 포함하여 실행할__ Asset compute 워커를 정의합니다.근로자의 URL 끝점, 결과 변환 이름 및 매개 변수 포함
1. __실행:__ [실행] 단추는 Asset compute 구성 프로필 편집기에 정의된 대로 Asset compute 프로필을 실행합니다
1. __중단:__ 중단 버튼을 누르면 실행 버튼을 탭하여 시작된 실행이 취소됩니다.
1. __요청/응답:__ Adobe I/O Runtime에서 실행 중인 Asset compute 워커에 대한 HTTP 요청 및 응답을 제공합니다. 디버깅에 유용할 수 있습니다.
1. __활성화 로그:__ Asset compute 근로자의 실행을 설명하는 로그와 오류가 있습니다. 이 정보는 `aio app run` standard out에서도 사용할 수 있습니다.
1. __표현물:__ Asset compute 근로자의 실행으로 생성된 모든 표현물을 표시합니다.
1. __devToolToken 쿼리 매개 변수:__ Asset compute 개발 도구 토큰에는 유효한  `devToolToken` 쿼리 매개 변수가 있어야 합니다. 이 토큰은 새 개발 도구를 생성할 때마다 자동으로 생성됩니다

### 사용자 지정 작업자 실행

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_개발 도구에서 Asset compute 작업을 실행하는 클릭스루(오디오 없음)_

1. `aio app run` 명령을 사용하여 프로젝트 루트에서 Asset compute 개발 도구가 시작되었는지 확인합니다.
1. asset compute 개발 도구에서 [샘플 이미지 파일](../assets/samples/sample-file.jpg)을 업로드하거나 선택합니다.
   + __소스 파일__ 드롭다운에서 파일을 선택했는지 확인합니다.
1. __Asset compute 프로필 정의__ 텍스트 영역 검토
   + `worker` 키는 배포된 Asset compute 워커에 대한 URL을 정의합니다.
   + `name` 키는 생성할 변환의 이름을 정의합니다.
   + 다른 키/값은 이 JSON 개체에서 제공할 수 있으며, `rendition.instructions` 개체 아래의 워커에서 사용할 수 있습니다.
      + 선택적으로 `size`, `contrast` 및 `brightness`에 대한 값을 추가합니다.

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

1. __실행__ 단추를 누릅니다.
1. __표현물 섹션__&#x200B;이 변환 자리 표시자로 채워집니다.
1. 워커가 완료되면 변환 자리 표시자에 생성된 표현물이 표시됩니다

개발 도구가 실행되는 동안 작업자 코드에 코드를 변경하면 변경 내용이 &quot;핫 배포&quot;됩니다. &quot;핫 배포&quot;는 몇 초 정도 소요되므로 개발 도구에서 작업자를 다시 실행하기 전에 배포를 완료할 수 있습니다.

## 문제 해결

+ [잘못된 YAML 들여쓰기](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize 제한이 너무 낮게 설정되어 있습니다.](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [private.key가 누락되어 개발 도구를 시작할 수 없습니다.](../troubleshooting.md#missing-private-key)
+ [소스 파일 드롭다운이 잘못되었습니다.](../troubleshooting.md#source-files-dropdown-incorrect)
+ [devToolToken 쿼리 매개 변수가 없거나 잘못되었습니다.](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [소스 파일을 제거할 수 없습니다.](../troubleshooting.md#unable-to-remove-source-files)
+ [표현물이 부분적으로 그려짐/손상됨](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
