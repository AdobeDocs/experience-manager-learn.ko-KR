---
title: 자산 계산 개발 도구
description: Asset Compute Development Tool은 개발자가 Adobe I/O Runtime의 Asset Compute Resources에 대해 AEM SDK의 컨텍스트 외부에 있는 에셋 컴퓨터 작업자를 로컬로 구성 및 실행할 수 있도록 하는 로컬 웹 도구입니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---


# 자산 계산 개발 도구

Asset Compute Development Tool은 개발자가 Adobe I/O Runtime의 Asset Compute Resources에 대해 AEM SDK의 컨텍스트 외부에 있는 에셋 컴퓨터 작업자를 로컬로 구성 및 실행할 수 있도록 하는 로컬 웹 도구입니다.

## 자산 계산 개발 도구 실행

자산 계산 개발 도구는 터미널 명령을 통해 자산 계산 프로젝트의 루트에서 실행할 수 있습니다.

```
$ aio app run
```

개발 도구는 http://localhost:9000에서 __시작되며__&#x200B;브라우저 창에서 자동으로 열립니다. 개발 도구를 실행하려면 쿼리 매개 변수 [를 통해 자동 생성된 유효한 devToolToken을 제공해야 합니다](#troubleshooting__devtooltoken).

## 자산 컴퓨팅 개발 도구 인터페이스 이해{#interface}

![자산 계산 개발 도구](./assets/development-tool/asset-compute-dev-tool.png)

1. __소스 파일:__ 소스 파일 선택
   + 자산 계산 작업자에게 전달된 `source` 바이너리인 자산 이진 선택
   + 소스 파일 업로드
1. __자산 계산 프로필 정의:__ 매개 변수를 포함하여 실행할 자산 계산 작업자를 정의합니다.작업자 URL 끝점, 결과 변환 이름 및 모든 매개 변수 포함
1. __실행:__ 실행(Run) 단추는 자산 계산 구성 프로필 편집기에 정의된 대로 자산 계산 프로필을 실행합니다
1. __중단:__ 중단 버튼을 누르면 실행 버튼을 탭하여 시작된 실행이 취소됩니다
1. __요청/응답:__ Adobe I/O Runtime에서 실행 중인 자산 계산 작업자에 대한/의 HTTP 요청 및 응답을 제공합니다. 디버깅에 유용할 수 있습니다.
1. __활성화 로그:__ 모든 오류와 함께 자산 계산 작업자의 실행을 설명하는 로그입니다. 이 정보는 `aio app run` 표준 출력
1. __표현물:__ 자산 계산 작업자 실행으로 생성된 모든 표현물을 표시합니다.
1. __devToolToken 쿼리 매개 변수:__ 자산 계산 개발 도구 토큰에는 유효한 쿼리 매개 변수가 `devToolToken` 있어야 합니다. 이 토큰은 새 개발 도구가 생성될 때마다 자동으로 생성됩니다

### 사용자 정의 작업자 실행

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)
_개발 도구에서 에셋 계산 작업을 실행하는 클릭스루(오디오 없음)_

1. 명령을 사용하여 프로젝트 루트에서 에셋 계산 개발 도구가 시작되었는지 `aio app run` 확인합니다.
1. 자산 계산 개발 도구에서 [샘플 이미지 파일을 업로드하거나 선택합니다](../assets/samples/sample-file.jpg)
   + 소스 파일 드롭다운에서 __파일이 선택되었는지__ 확인
1. 자산 계산 __프로필 정의__ 텍스트 영역 검토
   + 키 `worker` 는 배포된 자산 계산 작업자에 대한 URL을 정의합니다
   + 키는 생성할 변환의 이름을 정의합니다. `name`
   + 다른 키/값은 이 JSON 개체에서 제공할 수 있으며 개체 아래의 작업자에서 사용할 수 `rendition.instructions` 있습니다
      + 선택적으로, `size`및 `contrast` `brightness`다음 값을 추가할 수 있습니다.

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

1. Tap the __Run__ button
1. 표현물 __섹션이__ 표현물 자리 표시자로 채워집니다.
1. 작업자가 완료되면 변환 자리 표시자에 생성된 변환이 표시됩니다

개발 도구가 실행되는 동안 작업자 코드에 코드를 변경하면 변경 사항이 &quot;핫 배포&quot;됩니다. &quot;핫 배포&quot;는 몇 초 정도 소요되므로 개발 도구에서 작업자를 다시 실행하기 전에 배포를 완료할 수 있습니다.

## 문제 해결

### 소스 파일 드롭다운이 잘못되었습니다.{#troubleshooting__dev-tool-application-cache}

자산 계산 개발 도구는 오래된 데이터를 가져오는 상태에 들어갈 수 있으며, 소스 파일 ____ 드롭다운에서 잘못된 항목을 표시하는 것이 가장 눈에 띄었습니다.

+ __오류:__ 소스 파일 드롭다운에 잘못된 항목이 표시됩니다.
+ __원인:__ 오래된 캐시된 브라우저 상태로 인해
+ __해결 방법:__ 브라우저에서 브라우저 탭의 &quot;응용 프로그램 상태&quot;, 브라우저 캐시, 로컬 저장소 및 서비스 작업자를 완전히 지웁니다.

### devToolToken 쿼리 매개 변수가 없거나 잘못되었습니다.{#troubleshooting__devtooltoken}

+ __오류:__ 자산 계산 개발 도구의 &quot;권한 없음&quot; 알림
+ __원인:__`devToolToken` 없거나 잘못되었습니다.
+ __해결 방법:__ 자산 계산 개발 도구 브라우저 창을 닫고, 명령을 통해 시작된 모든 실행 중인 개발 도구 프로세스를 `aio app run` 종료하고 개발 도구를 다시 시작합니다(사용 `aio app run`).

### 소스 파일을 제거할 수 없습니다.{#troubleshooting__remove-source-files}

+ __오류:__ 개발 도구 UI에서 추가된 소스 파일을 제거할 방법이 없습니다.
+ __원인:__ 이 기능은 구현되지 않았습니다.
+ __해결 방법:__ 에 정의된 자격 증명을 사용하여 클라우드 스토리지 공급자에 로그인합니다 `.env`. 개발 도구(에 지정됨)에서 사용하는 컨테이너 `.env`를 찾아 __소스__ 폴더로 이동한 다음 소스 이미지를 삭제합니다. 삭제된 소스 파일이 개발 도구 &quot;응용 프로그램 상태&quot;에서 로컬에 캐싱될 수 있으므로 드롭다운에 계속 표시되는 경우 [소스 파일 드롭다운에](#troubleshooting__dev-tool-application-cache) 설명된 단계를 수행해야 할 수 있습니다.

   ![Microsoft Azure Blob 저장소](./assets/development-tool/troubleshooting__remove-source-files.png)
