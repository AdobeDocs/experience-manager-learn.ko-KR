---
title: OpenAPI 설명서 살펴보기 | AEM Headless Part 3
description: Adobe Experience Manager(AEM) 및 OpenAPI를 시작합니다. 기본 제공 API 설명서를 사용하여 AEM의 OpenAPI 기반 콘텐츠 조각 전달 API를 살펴보십시오. AEM이 콘텐츠 조각 모델을 기반으로 OpenAPI 스키마를 자동으로 생성하는 방법을 알아봅니다. API 설명서를 사용하여 기본 쿼리를 구성해 보십시오.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 400
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 0%

---


# AEM OpenAPI 기반 콘텐츠 조각 배달 API 탐색

AEM의 [OpenAPI를 통한 AEM 콘텐츠 조각 배달](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/)은(는) 구조화된 콘텐츠를 모든 애플리케이션이나 채널에 전달할 수 있는 강력한 방법을 제공합니다. 이 장에서는 OpenAPI를 사용하여 설명서의 **사용해 보기** 기능을 통해 콘텐츠 조각을 검색하는 방법에 대해 알아봅니다.

## 사전 요구 사항 {#prerequisites}

이 튜토리얼은 여러 부분으로 구성되어 있으며 [콘텐츠 조각 작성](./2-author-content-fragments.md)에 설명된 단계가 완료되었다고 가정합니다.

다음을 확인하십시오.

* `https://publish-<PROGRAM_ID>-e<ENVIRONMENT_ID >.adobeaemcloud.com/`콘텐츠 조각이 [에 게시되는 AEM Publish 서비스(예: &#x200B;](./2-author-content-fragments.md#publish-content-fragments))의 호스트 이름입니다. AEM 미리보기 서비스를 게시하는 경우 해당 호스트 이름을 사용할 수 있습니다(예: `https://preview-<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com/`).

## 목표 {#objectives}

* OpenAPI를 사용하여 [AEM 콘텐츠 조각 배달](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/)을 숙지하십시오.
* API 문서의 **시도** 기능을 사용하여 API를 호출합니다.

## 게재 API

OpenAPI API를 사용하는 AEM 콘텐츠 조각 전달은 콘텐츠 조각을 검색하는 RESTful 인터페이스를 제공합니다. 이 자습서에서 설명하는 API는 AEM Publish 및 미리보기 서비스에서만 사용할 수 있으며 Author 서비스에서는 사용할 수 없습니다. [AEM 작성자 서비스에서 콘텐츠 조각과 상호 작용](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/)하기 위한 다른 OpenAPI가 있습니다.

## API 살펴보기

[OpenAPI 설명서를 통한 AEM 콘텐츠 조각 배달](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/)에는 API를 탐색하고 브라우저에서 직접 테스트할 수 있는 &quot;사용해 보기&quot; 기능이 있습니다. 이는 API 엔드포인트 및 해당 기능을 숙지하는 좋은 방법입니다.

브라우저에서 [AEM Sites API 문서](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/)를 엽니다.

API는 **조각 배달** 섹션 아래의 왼쪽 탐색에 나열됩니다. 이 섹션을 확장하여 사용 가능한 API를 볼 수 있습니다. API를 선택하면 기본 패널에 API 세부 정보가 표시되고, 오른쪽 레일의 **사용해 보기** 섹션에서 브라우저에서 직접 API를 테스트하고 탐색할 수 있습니다.

![OpenAPI API 문서를 사용한 AEM 콘텐츠 조각 배달](./assets/3/docs-overview.png)

## 콘텐츠 조각 나열

1. 브라우저에서 [OpenAPI 개발자 문서를 사용하여 AEM 콘텐츠 조각 배달](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/)을 엽니다.
1. 왼쪽 탐색에서 **조각 배달** 섹션을 확장하고 **모든 콘텐츠 조각 나열** API를 선택합니다.

이 API를 사용하면 AEM에서 모든 콘텐츠 조각의 페이지가 매겨진 목록을 폴더별로 검색할 수 있습니다. 이 API를 사용하는 가장 간단한 방법은 콘텐츠 조각이 포함된 폴더의 경로를 제공하는 것입니다.

1. 오른쪽 레일 상단에서 **시도**&#x200B;를 선택합니다.
1. API가 콘텐츠 조각을 검색하기 위해 연결할 AEM 서비스의 식별자를 입력합니다. 버킷은 AEM 게시(또는 미리보기) 서비스 URL의 첫 번째 부분이며 일반적으로 `publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>` 또는 `preview-p<PROGRAM_ID>-e<ENVIRONMENT_ID>` 형식입니다.

AEM Publish 서비스를 사용하고 있으므로 버킷을 AEM Publish 서비스 식별자로 설정하십시오. 예:

* **버킷**: `publish-p138003-e1400351`

![버킷으로 시도](./assets/3/try-it-bucket.png)

버킷이 설정되면 **Target 서버** 필드가 `https://publish-p138003-e1400351.adobeaemcloud.com/adobe/contentFragments`과(와) 같은 AEM Publish 서비스의 전체 API URL로 자동 업데이트됩니다.

1. **보안** 섹션을 확장하고 **보안 체계**&#x200B;을(를) **없음**(으)로 설정합니다. AEM Publish 서비스(및 미리보기 서비스)에서는 OpenAPI를 사용하여 AEM 콘텐츠 조각 전달을 위한 인증이 필요하지 않기 때문입니다.

1. 가져올 콘텐츠 조각의 세부 정보를 제공하려면 **매개 변수** 섹션을 확장하세요.

* **cursor**: 비워 둡니다. 페이지 매김에 사용되며 초기 요청입니다.
* **limit**: 비워 두십시오. 결과 페이지당 반환되는 결과 수를 제한하는 데 사용됩니다.
* **경로**: `/content/dam/my-project/en`

  >[!TIP]
  > 경로를 입력할 때 접두사가 `/content/dam/`이고 **not**&#x200B;이(가) 뒤쪽 슬래시 `/`(으)로 끝나는지 확인하십시오.

  ![매개 변수 사용](assets/3/try-it-parameters.png)

1. **보내기** 단추를 선택하여 API 호출을 실행합니다.
1. **시도** 패널의 **응답** 탭에는 지정된 폴더에 콘텐츠 조각 목록이 포함된 JSON 응답이 표시됩니다. 응답은 다음과 비슷합니다.

   ![응답 시도](./assets/3/try-it-response.png)

1. 응답에는 `path`개인`/content/dam/my-project` 및 **팀** 콘텐츠 조각을 모두 포함하는 하위 폴더를 포함하여 **매개 변수의** 폴더 아래에 모든 콘텐츠 조각이 포함되어 있습니다.
1. `items` 배열을 클릭하고 `Team Alpha` 항목의 `id` 값을 찾았습니다. 이 ID는 다음 섹션에서 단일 콘텐츠 조각의 세부 정보를 검색하는 데 사용됩니다.
1. **시도** 패널의 맨 위에서 **요청 편집**&#x200B;을 선택하고 API 호출의 다양한 매개 변수를 선택하여 응답이 어떻게 변경되는지 확인합니다. 예를 들어 콘텐츠 조각이 포함된 다른 폴더로 경로를 변경하거나 쿼리 매개 변수를 추가하여 결과를 필터링할 수 있습니다. 예를 들어 `path` 매개 변수를 해당 폴더(및 하위 폴더)의 콘텐츠 조각만 사용하도록 `/content/dam/my-project/teams`(으)로 변경합니다.

## 콘텐츠 조각 세부 정보 가져오기

**모든 콘텐츠 조각 나열** API와 마찬가지로 **콘텐츠 조각 가져오기** API는 선택적 참조와 함께 해당 ID로 단일 콘텐츠 조각을 검색합니다. 이 API를 탐색하기 위해 여러 개인 콘텐츠 조각을 참조하는 팀 콘텐츠 조각을 요청합니다.

1. 왼쪽 레일에서 **조각 배달** 섹션을 확장하고 **콘텐츠 조각 가져오기** API를 선택합니다.
1. 오른쪽 레일 상단에서 **시도**&#x200B;를 선택합니다.
1. AEM as a Cloud Service 게시 또는 미리보기 서비스에 대한 `bucket` 포인트를 확인합니다.
1. **보안** 섹션을 확장하고 **보안 체계**&#x200B;을(를) **없음**(으)로 설정합니다. AEM Publish 서비스에서는 OpenAPI를 사용하여 AEM 콘텐츠 조각 전달을 위한 인증이 필요하지 않기 때문입니다.
1. **매개 변수** 섹션을 확장하여 가져올 콘텐츠 조각의 세부 정보를 제공합니다.

이 예제에서는 이전 섹션에서 검색한 팀 콘텐츠 조각의 ID를 사용합니다. 예를 들어 **모든 콘텐츠 조각 나열**&#x200B;의 이 콘텐츠 조각 응답에 대해 `id`의 `b954923a-0368-4fa2-93ea-2845f599f512` 필드에 있는 값을 사용합니다. `id`이(가) 자습서에 사용된 값과 달라집니다.

```json
{
    "path": "/content/dam/my-project/teams/team-alpha",
    "name": "",
    "title": "Team Alpha",
    "id": "50f28a14-fec7-4783-a18f-2ce2dc017f55", // This is the Content Fragment ID
    "description": "",
    "model": {},
    "fields": {} 
}
```

* **fragmentId**: `50f28a14-fec7-4783-a18f-2ce2dc017f55`
* **참조**: `none`
* **깊이**: 비워 두십시오. **참조** 매개 변수는 참조된 조각의 깊이를 나타냅니다.
* **하이드레이션됨**: 비워 둡니다. **참조** 매개 변수는 참조된 조각의 하이드레이션을 나타냅니다.
* **If-None-Match**: 비워 둡니다.

1. **보내기** 단추를 선택하여 API 호출을 실행합니다.
1. **시도** 패널의 **응답** 탭에서 응답을 검토하십시오. 속성 및 보유하고 있는 모든 참조를 포함한 콘텐츠 조각의 세부 정보가 포함된 JSON 응답이 표시되어야 합니다.
1. **시도** 패널의 맨 위에서 **요청 편집**&#x200B;을 선택하고 **매개 변수** 섹션에서 `references` 매개 변수를 `all-hydrated`(으)로 조정하여 참조된 모든 콘텐츠 조각의 콘텐츠를 API 호출에 포함시킵니다.

   * **fragmentId**: `50f28a14-fec7-4783-a18f-2ce2dc017f55`
   * **참조**: `all-hydrated`
   * **깊이**: 비워 두십시오. **참조** 매개 변수는 참조된 조각의 깊이를 나타냅니다.
   * **하이드레이션됨**: 비워 둡니다. **참조** 매개 변수는 참조된 조각의 하이드레이션을 나타냅니다.
   * **If-None-Match**: 비워 둡니다.

1. API 호출을 다시 실행하려면 **다시 보내기** 단추를 선택하세요.
1. **시도** 패널의 **응답** 탭에서 응답을 검토하십시오. 속성 및 참조된 개인 콘텐츠 조각의 세부 정보를 포함한 콘텐츠 조각의 세부 정보가 포함된 JSON 응답이 표시되어야 합니다.

이제 `teamMembers` 배열에 참조된 개인 콘텐츠 조각의 세부 정보가 포함됩니다. 참조 하이드레이팅을 사용하면 단일 API 호출에서 필요한 모든 데이터를 검색할 수 있으므로 클라이언트 애플리케이션에서 수행하는 요청 수를 줄이는 데 특히 유용합니다.

## 축하합니다!

축하합니다. AEM 설명서의 **사용해 보기** 기능을 사용하여 OpenAPI API 호출을 사용하여 여러 AEM 콘텐츠 조각 배달을 만들고 실행했습니다.

## 다음 단계

다음 장인 [React 앱 빌드](./4-react-app.md)에서는 외부 애플리케이션이 OpenAPI API를 사용하여 AEM 콘텐츠 조각 전달과 상호 작용하는 방법을 살펴봅니다.

