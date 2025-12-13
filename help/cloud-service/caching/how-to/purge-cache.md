---
title: CDN 캐시를 제거하는 방법
description: AEM as a Cloud Service CDN에서 캐시된 HTTP 응답을 제거하거나 제거하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-13T00:00:00Z
jira: KT-15963
thumbnail: KT-15963.jpeg
exl-id: 5d81f6ee-a7df-470f-84b9-12374c878a1b
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 0%

---

# CDN 캐시를 제거하는 방법

AEM as a Cloud Service CDN에서 캐시된 HTTP 응답을 제거하거나 제거하는 방법을 알아봅니다. **API 토큰 제거**&#x200B;라는 셀프서비스 기능을 사용하여 특정 리소스, 리소스 그룹 및 전체 캐시에 대한 캐시를 제거할 수 있습니다.

이 자습서에서는 셀프 서비스 기능을 사용하여 샘플 [AEM WKND](https://github.com/adobe/aem-guides-wknd) 사이트의 CDN 캐시를 제거하기 위해 API 토큰 제거를 설정하고 사용하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3436936?captions=kor&quality=12&learn=on)

## 캐시 무효화와 명시적 제거 비교

CDN에서 캐시된 리소스를 제거하는 방법에는 두 가지가 있습니다.

1. **캐시 무효화:** `Cache-Control`, `Surrogate-Control` 또는 `Expires`과(와) 같은 캐시 헤더를 기반으로 CDN에서 캐시된 리소스를 제거하는 프로세스입니다. 캐시 헤더의 `max-age` 특성 값을 사용하여 리소스의 캐시 수명을 결정합니다. 캐시 TTL(Time To Live)이라고도 합니다. 캐시 수명이 만료되면 캐시된 리소스가 CDN 캐시에서 자동으로 제거됩니다.

1. **명시적 제거:** TTL이 만료되기 전에 CDN 캐시에서 캐시된 리소스를 수동으로 제거하는 프로세스입니다. 명시적 제거는 캐시된 리소스를 즉시 제거하려는 경우 유용합니다. 그러나 원본 서버에 대한 트래픽이 증가합니다.

캐시된 리소스가 CDN 캐시에서 제거되면 동일한 리소스에 대한 다음 요청은 원본 서버에서 최신 버전을 가져옵니다.

## API 토큰 제거 설정

CDN 캐시를 제거하기 위해 API 토큰 제거를 설정하는 방법에 대해 알아보겠습니다.

### CDN 규칙 구성

Purge API 토큰은 AEM 프로젝트 코드에 CDN 규칙을 구성하여 만듭니다.

1. AEM 프로젝트의 기본 `cdn.yaml` 폴더에서 `config` 파일을 엽니다. 예를 들어 [WKND 프로젝트의 cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) 파일입니다.

1. `cdn.yaml` 파일에 다음 CDN 규칙을 추가합니다.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:  
  authentication: # The main authentication configuration
    authenticators: # The list of authenticators
       - name: purge-auth # The name of the authenticator
         type: purge  # The type of the authenticator, must be purge
         purgeKey1: ${{CDN_PURGEKEY_081324}} # The first purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_073124}}
         purgeKey2: ${{CDN_PURGEKEY_111324}} # The second purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_111324}}. It is used for the rotation of secrets without any interruptions.
    rules: # The list of authentication rules
       - name: purge-auth-rule # The name of the rule
         when: { reqProperty: tier, equals: "publish" } # The condition when the rule should be applied
         action: # The action to be taken when the rule is applied
           type: authenticate # The type of the action, must be authenticate
           authenticator: purge-auth # The name of the authenticator to be used, must match the name from the above authenticators list               
```

위의 규칙에서 `purgeKey1`과(와) `purgeKey2`은(는) 중단 없이 암호 회전을 지원하기 위해 처음부터 추가됩니다. 그러나 `purgeKey1`로만 시작하여 나중에 암호를 회전할 때 `purgeKey2`을(를) 추가할 수 있습니다.

1. 변경 사항을 저장하고, 커밋하고, Adobe 업스트림 저장소에 푸시합니다.

### Cloud Manager 환경 변수 만들기

그런 다음 Cloud Manager 환경 변수를 만들어 제거 API 토큰 값을 저장합니다.

1. [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/)에서 Cloud Manager에 로그인한 다음 조직과 프로그램을 선택합니다.

1. __환경__ 섹션에서 원하는 환경 옆의 **생략 부호**(...)를 클릭하고 **세부 정보 보기**&#x200B;를 선택합니다.

   ![세부 정보 보기](../assets/how-to/view-env-details.png)

1. 그런 다음 **구성** 탭을 선택하고 **구성 추가** 단추를 클릭합니다.

1. **환경 구성** 대화 상자에 다음 세부 정보를 입력하십시오.
   - **이름**: 환경 변수의 이름을 입력하십시오. `purgeKey1` 파일의 `purgeKey2` 또는 `cdn.yaml` 값과 일치해야 합니다.
   - **값**: 제거 API 토큰 값을 입력하십시오.
   - **서비스 적용됨**: **모두** 옵션을 선택하십시오.
   - **유형**: **암호** 옵션을 선택하십시오.
   - **추가** 단추를 클릭합니다.

   ![변수 추가](../assets/how-to/add-cloud-manager-secrete-variable.png)

1. `purgeKey2` 값에 대한 두 번째 환경 변수를 만들려면 위의 단계를 반복합니다.

1. 변경 내용을 저장하고 적용하려면 **저장**&#x200B;을 클릭하세요.

### CDN 규칙 배포

마지막으로 Cloud Manager 파이프라인을 사용하여 구성된 CDN 규칙을 AEM as a Cloud Service 환경에 배포합니다.

1. Cloud Manager에서 **파이프라인** 섹션으로 이동합니다.

1. 새 파이프라인을 만들거나 **Config** 파일만 배포하는 기존 파이프라인을 선택하십시오. 자세한 단계는 [구성 파이프라인 만들기](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)를 참조하십시오.

1. CDN 규칙을 배포하려면 **실행** 단추를 클릭하십시오.

   ![파이프라인 실행](../assets/how-to/run-config-pipeline.png)

## 제거 API 토큰 사용

CDN 캐시를 제거하려면 제거 API 토큰으로 AEM 서비스별 도메인 URL을 호출합니다. 캐시를 제거하는 구문은 다음과 같습니다.

```
PURGE <URL> HTTP/1.1
Host: <AEM_SERVICE_SPECIFIC_DOMAIN>
X-AEM-Purge-Key: <PURGE_API_TOKEN>
X-AEM-Purge: <PURGE_TYPE>
Surrogate-Key: <SURROGATE_KEY>
```

위치:

- **PURGE`<URL>`**: `PURGE` 메서드 다음에 제거할 리소스의 URL 경로가 옵니다.
- **호스트:`<AEM_SERVICE_SPECIFIC_DOMAIN>`**: AEM 서비스의 도메인을 지정합니다.
- **X-AEM-Purge-Key:`<PURGE_API_TOKEN>`**: 제거 API 토큰 값이 포함된 사용자 지정 헤더입니다.
- **X-AEM-제거:`<PURGE_TYPE>`**: 제거 작업의 유형을 지정하는 사용자 지정 헤더입니다. 값은 `hard`, `soft` 또는 `all`일 수 있습니다. 다음 표에서는 각 삭제 유형에 대해 설명합니다.

  | 삭제 유형 | 설명 |
  |:------------:|:-------------:|
  | 하드(기본값) | 캐시된 리소스를 즉시 제거합니다. 원본 서버로의 트래픽이 증가하므로 이를 피하십시오. |
  | 소프트 | 캐시된 리소스를 오래된 것으로 표시하고 원본 서버에서 최신 버전을 가져옵니다. |
  | 모두 | CDN 캐시에서 캐시된 모든 리소스를 제거합니다. |

- **대리 키:`<SURROGATE_KEY>`**: (선택 사항) 제거할 리소스 그룹의 대리 키(공백으로 구분)를 지정하는 사용자 지정 헤더입니다. 서로게이트 키는 리소스를 함께 그룹화하는 데 사용되며 리소스의 응답 헤더에 설정해야 합니다.

>[!TIP]
>
>아래 예제에서 `X-AEM-Purge: hard`은 데모용으로 사용됩니다. 요구 사항에 따라 `soft` 또는 `all`(으)로 바꿀 수 있습니다. `hard` 제거 형식을 사용하면 원본 서버에 대한 트래픽이 증가하므로 주의하십시오.

### 특정 리소스에 대한 캐시 제거

이 예에서 `curl` 명령은 AEM as a Cloud Service 환경에 배포된 WKND 사이트의 `/us/en.html` 리소스에 대한 캐시를 삭제합니다.

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/us/en.html" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

제거에 성공하면 `200 OK` 응답이 JSON 콘텐츠와 함께 반환됩니다.

```json
{ "status": "ok", "id": "1000098-1722961031-13237063" }
```

### 리소스 그룹에 대한 캐시 제거

이 예제에서 `curl` 명령은 서로게이트 키가 `wknd-assets`인 리소스 그룹의 캐시를 삭제합니다. `Surrogate-Key` 응답 헤더가 [`wknd.vhost`](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L176)에 설정되어 있습니다. 예:

```http
<VirtualHost *:80>
    ...

    # Core Component Image Component: long-term caching (30 days) for immutable URLs, background refresh to avoid MISS
    <LocationMatch "^/content/.*\.coreimg.*\.(?i:jpe?g|png|gif|svg)$">
        Header set Cache-Control "max-age=2592000,stale-while-revalidate=43200,stale-if-error=43200,public,immutable" "expr=%{REQUEST_STATUS} < 400"
        # Set Surrogate-Key header to group the cache of WKND assets, thus it can be flushed independtly
        Header set Surrogate-Key "wknd-assets"
        Header set Age 0
    </LocationMatch>

    ...
</VirtualHost>
```

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com" \
-H "Surrogate-Key: wknd-assets" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

제거에 성공하면 `200 OK` 응답이 JSON 콘텐츠와 함께 반환됩니다.

```json
{ "wknd-assets": "10027-1723478994-2597809-1" }
```

### 전체 캐시 제거

이 예에서 `curl` 명령을 사용하면 AEM as a Cloud Service 환경에 배포된 샘플 WKND 사이트에서 전체 캐시가 제거됩니다.

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: all"
```

제거에 성공하면 `200 OK` 응답이 JSON 콘텐츠와 함께 반환됩니다.

```json
{"status":"ok"}
```

### 캐시 제거 확인

캐시 제거를 확인하려면 웹 브라우저에서 리소스 URL에 액세스하고 응답 헤더를 검토합니다. `X-Cache` 헤더 값은 `MISS`이어야 합니다.

![X-캐시 헤더](../assets/how-to/x-cache-miss.png)
