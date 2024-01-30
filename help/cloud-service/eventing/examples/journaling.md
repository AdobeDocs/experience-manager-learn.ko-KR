---
title: 저널링 및 AEM 이벤트
description: 저널에서 AEM 이벤트의 초기 세트를 검색하고 각 이벤트에 대한 세부 사항을 탐색하는 방법을 알아봅니다.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 163
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# 저널링 및 AEM 이벤트

저널에서 AEM 이벤트의 초기 세트를 검색하고 각 이벤트에 대한 세부 사항을 탐색하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

저널링은 AEM 이벤트를 사용하는 가져오기 메서드이며 저널은 순서가 지정된 이벤트 목록입니다. Adobe I/O 이벤트 저널링 API를 사용하여 저널에서 AEM 이벤트를 가져와 애플리케이션에서 처리할 수 있습니다. 이 접근 방식을 사용하면 지정된 케이던스를 기반으로 이벤트를 관리하고 이러한 이벤트를 일괄적으로 효율적으로 처리할 수 있습니다. 다음을 참조하십시오. [저널링](https://developer.adobe.com/events/docs/guides/journaling_intro/) 유지 기간, 페이지 매김 등과 같은 필수 고려 사항을 포함하여 심층적인 통찰력을 제공합니다.

Adobe Developer 콘솔 프로젝트 내에서 모든 이벤트 등록은 저널링을 위해 자동으로 활성화되므로 원활한 통합이 가능합니다.

이 예에서 Adobe 제공 사용 _호스팅된 웹 애플리케이션_ 애플리케이션을 설정하지 않고도 분개에서 AEM 이벤트의 첫 번째 배치를 가져올 수 있습니다. 이 Adobe 제공 웹 애플리케이션은에서 호스팅됩니다. [결함](https://glitch.com/): 웹 애플리케이션을 구축하고 배포하는 데 도움이 되는 웹 기반 환경을 제공하는 것으로 알려진 플랫폼입니다. 그러나 원하는 경우 자체 애플리케이션을 사용하는 옵션도 사용할 수 있습니다.

## 사전 요구 사항

이 자습서를 완료하려면 다음이 필요합니다.

- AEM as a Cloud Service 환경 [AEM 이벤트 활성화됨](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [AEM 이벤트에 대해 구성된 Adobe Developer 콘솔 프로젝트](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## 웹 애플리케이션 액세스

Adobe이 제공한 웹 애플리케이션에 액세스하려면 다음 단계를 따르십시오.

- 다음에 액세스할 수 있는지 확인합니다. [결함 - 호스팅된 웹 애플리케이션](https://indigo-speckle-antler.glitch.me/) 새 브라우저 탭에서.

  ![결함 - 호스팅된 웹 애플리케이션](../assets/examples/journaling/glitch-hosted-web-application.png)

## Adobe Developer 콘솔 프로젝트 세부 정보 수집

저널에서 AEM 이벤트를 가져오려면 과 같은 자격 증명 _IMS 조직 ID_, _클라이언트 ID_, 및 _액세스 토큰_ 필수 항목입니다. 이러한 자격 증명을 수집하려면 다음 단계를 수행합니다.

- 다음에서 [Adobe Developer 콘솔](https://developer.adobe.com)로 이동한 다음 를 클릭하여 프로젝트를 엽니다.

- 아래 **자격 증명** 섹션에서 **OAuth 서버 간** 를 여는 링크 **자격 증명 세부 정보** 탭.

- 다음을 클릭합니다. **액세스 토큰 생성** 액세스 토큰을 생성하는 단추입니다.

  ![Adobe Developer 콘솔 프로젝트 액세스 토큰 생성](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- 다음을 복사합니다. **액세스 토큰 생성됨**, **클라이언트 ID**, 및 **조직 ID**. 나중에 이 자습서에서 해당 항목이 필요합니다.

  ![Adobe Developer 콘솔 프로젝트 복사 자격 증명](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- 모든 이벤트 등록은 저널링에 대해 자동으로 활성화됩니다. 을(를) 가져오려면 _고유한 저널링 API 엔드포인트_ 이벤트 등록에서 AEM 이벤트를 구독한 이벤트 카드를 클릭합니다. 다음에서 **등록 세부 정보** 탭을 복사하고 **저널링 고유 API 끝점**.

  ![Adobe Developer 콘솔 프로젝트 이벤트 카드](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## AEM 이벤트 저널 로드

단순성을 유지하기 위해 호스팅된 이 웹 애플리케이션은 저널에서 첫 번째 AEM 이벤트 배치만 가져옵니다. 저널에서 사용할 수 있는 가장 오래된 이벤트입니다. 자세한 내용은 [첫 번째 이벤트 배치](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- 다음에서 [결함 - 호스팅된 웹 애플리케이션](https://indigo-speckle-antler.glitch.me/)을(를) 입력한 후 **IMS 조직 ID**, **클라이언트 ID**, 및 **액세스 토큰** Adobe Developer 콘솔 프로젝트에서 이전에 복사한 다음 **제출**.

- 성공하면 테이블 구성 요소에 AEM 이벤트 분개 데이터가 표시됩니다.

  ![AEM 이벤트 분개 데이터](../assets/examples/journaling/load-journal.png)

- 전체 이벤트 페이로드를 보려면 행을 두 번 클릭합니다. AEM 이벤트 세부 사항에 웹후크에서 이벤트를 처리하는 데 필요한 모든 정보가 포함되어 있음을 알 수 있습니다. 예: 이벤트 유형(`type`), 이벤트 소스(`source`), 이벤트 id(`event_id`), 이벤트 시간(`time`) 및 이벤트 데이터(`data`).

  ![전체 AEM 이벤트 페이로드](../assets/examples/journaling/complete-journal-data.png)

## 추가 리소스

- [결함 웹후크 소스 코드](https://glitch.com/edit/#!/인디고스펙클앤틀러) 을(를) 참조할 수 있습니다. 를 사용하는 간단한 React 애플리케이션입니다 [Adobe 반응 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/index.html) UI를 렌더링할 구성 요소입니다.

- [Adobe I/O 이벤트 저널링 API](https://developer.adobe.com/events/docs/guides/api/journaling_api/) 는 이벤트의 첫 번째, 다음 및 마지막 배치, 페이지 매김 등과 같은 API에 대한 자세한 정보를 제공합니다.
