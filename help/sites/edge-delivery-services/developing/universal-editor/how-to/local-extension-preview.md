---
title: 유니버설 편집기 확장 미리 보기
description: 개발 중에 로컬에서 실행 중인 유니버설 편집기 확장을 미리 보는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner, Intermediate, Experienced
doc-type: Tutorial
jira: KT-18658
source-git-commit: f0ad5d66549970337118220156d7a6b0fd30fd57
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---


# 로컬 유니버설 편집기 확장 미리 보기

>[!TIP]
> [유니버설 편집기 확장을 만드는 방법](https://developer.adobe.com/uix/docs/services/aem-universal-editor/)을 알아보세요.

개발 중에 유니버설 편집기 확장을 미리 보려면 다음을 수행해야 합니다.

1. 로컬에서 확장을 실행합니다.
2. 자체 서명된 인증서를 수락합니다.
3. 범용 편집기에서 페이지를 엽니다.
4. 위치 URL을 업데이트하여 로컬 확장을 로드합니다.

## 로컬에서 확장 실행

이는 이미 [유니버설 편집기 확장](https://developer.adobe.com/uix/docs/services/aem-universal-editor/)을 만들었으며 로컬에서 테스트하고 개발하는 동안 미리 보려는 것으로 가정합니다.

유니버설 편집기 확장을 시작할 때:

```bash
$ aio app run
```

다음과 같은 출력이 표시됩니다.

```
To view your local application:
  -> https://localhost:9080
To view your deployed application in the Experience Cloud shell:
  -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
```

기본적으로 `https://localhost:9080`에서 확장이 실행됩니다.


## 자체 서명된 인증서 수락

유니버설 편집기에서 확장을 로드하려면 HTTPS가 필요합니다. 로컬 개발에서는 자체 서명된 인증서를 사용하므로 브라우저가 명시적으로 인증서를 신뢰해야 합니다.

새 브라우저 탭을 열고 `aio app run` 명령으로 로컬 확장 URL 출력으로 이동합니다.

```
https://localhost:9080
```

브라우저에 인증서 경고가 표시됩니다. 계속하려면 인증서를 수락하십시오.

![자체 서명된 인증서 수락](./assets/local-extension-preview/accept-certificate.png)

승인되면 로컬 확장의 자리 표시자 페이지가 표시됩니다.

![확장에 액세스할 수 있음](./assets/local-extension-preview/extension-accessible.png)


## 범용 편집기에서 페이지 열기

[유니버설 편집기 콘솔](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/)을 통해 또는 유니버설 편집기를 사용하는 AEM Sites의 페이지를 편집하여 유니버설 편집기를 엽니다.

![유니버설 편집기에서 페이지 열기](./assets/local-extension-preview/open-page-in-ue.png)


## 확장 로드

유니버설 편집기에서 인터페이스 가운데 맨 위에 있는 **위치** 필드를 찾습니다. 확장하고 **위치 필드**&#x200B;의 URL을 업데이트합니다.**브라우저 주소 표시줄이 아닙니다**.

다음 쿼리 매개 변수를 추가합니다.

* `devMode=true` - 유니버설 편집기의 개발 모드를 사용하도록 설정합니다.
* `ext=https://localhost:9080` - 로컬에서 실행 중인 확장을 로드합니다.

예:

```
https://author-pXXX-eXXX.adobeaemcloud.com/content/aem-ue-wknd/index.html?devMode=true&ext=https://localhost:9080
```

![유니버설 편집기 위치 URL 업데이트](./assets/local-extension-preview/update-location-url.png)


## 확장 미리 보기

업데이트된 URL이 사용되는지 확인하려면 브라우저의 **하드 다시 로드**&#x200B;를 수행하십시오.

이제 Universal Editor는 브라우저 세션에서만 로컬 확장을 로드합니다.

로컬에서 수행하는 모든 코드 변경 사항은 즉시 반영됩니다.

![로컬 확장이 로드됨](./assets/local-extension-preview/extension-loaded.png)

