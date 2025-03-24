---
title: Asset Compute 개발 도구
description: Asset Compute 개발 도구는 개발자가 Adobe I/O Runtime의 Asset Compute 리소스에 대해 AEM SDK 컨텍스트 외부에 있는 Asset Computer 작업자를 로컬로 구성 및 실행할 수 있도록 하는 로컬 웹 도구입니다.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Asset Compute 개발 도구

Asset Compute 개발 도구는 개발자가 Adobe I/O Runtime의 Asset Compute 리소스에 대해 AEM SDK 컨텍스트 외부에 있는 Asset Computer 작업자를 로컬로 구성 및 실행할 수 있도록 하는 로컬 웹 도구입니다.

## Asset Compute 개발 도구 실행

Asset Compute 개발 도구는 terminal 명령을 통해 Asset Compute 프로젝트의 루트에서 실행할 수 있습니다.

```
$ aio app run
```

__http://localhost:9000__&#x200B;에서 개발 도구를 시작하고 브라우저 창에서 자동으로 열립니다. 개발 도구를 실행하려면 [쿼리 매개 변수를 통해 올바른 자동 생성 devToolToken을 제공해야 합니다](#troubleshooting__devtooltoken).

## Asset Compute 개발 도구 인터페이스 이해{#interface}

![Asset Compute 개발 도구](./assets/development-tool/asset-compute-dev-tool.png)

1. __Source 파일:__ 소스 파일 선택 항목을 사용하여 다음을 수행할 수 있습니다.
   + Asset Compute 작업자에게 전달된 `source` 바이너리 역할을 하는 자산 바이너리를 선택했습니다.
   + 소스 파일 업로드
1. __Asset Compute 프로필 정의:__ 작업자 URL 끝점, 결과 렌디션 이름 및 모든 매개 변수를 포함하여 실행할 Asset Compute 작업자를 정의합니다.
1. __실행:__ 실행 단추는 Asset Compute 구성 프로필 편집기에 정의된 대로 Asset Compute 프로필을 실행합니다
1. __중단:__ 중단 단추를 사용하면 [실행] 단추를 누르기 시작한 실행이 취소됩니다
1. __요청/응답:__ Adobe I/O Runtime에서 실행 중인 Asset Compute 작업자에 대한 HTTP 요청 및 응답을 제공합니다. 이 기능은 디버깅에 유용합니다
1. __활성화 로그:__ Asset Compute 작업자의 실행을 설명하는 로그와 함께 모든 오류가 표시됩니다. 이 정보는 `aio app run` 표준 버전에서도 사용할 수 있습니다.
1. __렌디션:__ Asset Compute 작업자 실행으로 생성된 모든 렌디션을 표시합니다
1. __devToolToken 쿼리 매개 변수:__ Asset Compute 개발 도구 토큰에 유효한 `devToolToken` 쿼리 매개 변수가 있어야 합니다. 이 토큰은 새 개발 도구가 생성될 때마다 자동으로 생성됩니다

### 사용자 지정 작업자 실행

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_개발 도구에서 Asset Compute 작업 실행 클릭스루(오디오 없음)_

1. `aio app run` 명령을 사용하여 프로젝트 루트에서 Asset Compute 개발 도구가 시작되었는지 확인하십시오.
1. Asset Compute 개발 도구에서 [샘플 이미지 파일](../assets/samples/sample-file.jpg)을 업로드하거나 선택합니다.
   + __Source 파일__ 드롭다운에서 파일이 선택되어 있는지 확인합니다.
1. __Asset Compute 프로필 정의__ 텍스트 영역 검토
   + `worker` 키는 배포된 Asset Compute 작업자의 URL을 정의합니다.
   + `name` 키는 생성할 렌디션의 이름을 정의합니다
   + 다른 키/값은 이 JSON 개체에 제공할 수 있으며 `rendition.instructions` 개체 아래의 작업자에서 사용할 수 있습니다.
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

1. __실행__ 단추 탭
1. __렌디션 섹션__&#x200B;은(는) 렌디션 자리 표시자로 채워집니다
1. 작업자가 완료되면 렌디션 자리 표시자에 생성된 렌디션이 표시됩니다

개발 도구가 실행되는 동안 작업자 코드에 코드를 변경하면 변경 사항이 &quot;핫 배포&quot;됩니다. &quot;핫 배포&quot;는 몇 초 정도 걸리므로 개발 도구에서 작업자를 다시 실행하기 전에 배포를 완료할 수 있습니다.

## 문제 해결

+ [잘못된 YAML 들여쓰기](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize 제한이 너무 낮게 설정되어 있습니다.](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [private.key가 누락되어 개발 도구를 시작할 수 없습니다.](../troubleshooting.md#missing-private-key)
+ [Source 파일 드롭다운이 잘못됨](../troubleshooting.md#source-files-dropdown-incorrect)
+ [누락되었거나 잘못된 devToolToken 쿼리 매개 변수](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [소스 파일을 제거할 수 없음](../troubleshooting.md#unable-to-remove-source-files)
+ [렌디션이 부분적으로 그려졌거나 손상되었습니다.](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
