---
title: 파이프라인 없는 URL 리디렉션 구현
description: AEM as a Cloud Service에서 파이프라인 없는 URL 리디렉션을 구현하여 개발자가 필요 없이 마케팅 팀이 리디렉션을 관리할 수 있도록 하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Operations, Dispatcher
topic: Development, Content Management, Administration
role: Architect, Developer, User
level: Beginner, Intermediate
doc-type: Article
duration: 0
last-substantial-update: 2025-02-05T00:00:00Z
jira: KT-15739
thumbnail: KT-15739.jpeg
source-git-commit: bc2f4655631f28323a39ed5b4c7878613296a0ba
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 0%

---


# 파이프라인 없는 URL 리디렉션 구현

AEM as a Cloud Service에서 [파이프라인 없는 URL 리디렉션](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)을 구현하여 개발자가 필요 없이 마케팅 팀이 리디렉션을 관리할 수 있도록 하는 방법에 대해 알아봅니다.

AEM에는 URL 리디렉션을 관리하는 여러 옵션이 있습니다. 자세한 내용은 [URL 리디렉션](url-redirection.md)을 참조하십시오.

이 자습서에서는 [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html)과 같은 텍스트 파일에서 URL 리디렉션을 키-값 쌍으로 만드는 데 중점을 두고 AEM as a Cloud Service 특정 구성을 사용하여 Apache/Dispatcher 모듈에 로드합니다.

## 사전 요구 사항

이 자습서를 완료하려면 다음이 필요합니다.

- 버전 **18311 이상의 AEM as a Cloud Service 환경**.

- 샘플 [WKND Sites](https://github.com/adobe/aem-guides-wknd) 프로젝트를 여기에 배포해야 합니다.

## 튜토리얼 사용 사례

데모 목적으로 WKND 마케팅 팀이 새로운 스키 캠페인을 시작한다고 가정해 보겠습니다. 스키 어드벤처 페이지에 대한 짧은 URL을 만들고 콘텐츠 관리 방법과 마찬가지로 직접 관리하려고 합니다. [파이프라인 없는 URL 리디렉션](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) 접근 방식을 사용하여 URL 리디렉션을 관리하기로 결정했습니다.

마케팅 팀의 요구 사항에 따라 만들어야 하는 URL 리디렉션은 다음과 같습니다.

| SOURCE URL | 대상 URL |
|------------|------------|
| /ski | /us/en/adventures.html |
| /ski/northamerica | /us/en/adventures/downhill-skiing-wyoming.html |
| /ski/westcoast | /us/en/adventures/tahoe-skiing.html |
| /스키/유럽 | /us/en/adventures/ski-touring-mont-blanc.html |

이제 AEM as a Cloud Service 환경에서 이러한 URL 리디렉션 및 필수 일회용 Dispatcher 구성을 관리하는 방법을 살펴보겠습니다.

## URL 리디렉션을 관리하는 방법{#manage-redirects}

URL 리디렉션을 관리하기 위해 사용 가능한 옵션이 여러 개 있습니다. 해당 옵션을 살펴보겠습니다.

### DAM의 텍스트 파일

URL 리디렉션은 텍스트 파일에서 키-값 쌍으로 관리되고 AEM DAM(디지털 에셋 관리)에 업로드될 수 있습니다.

예를 들어 위의 URL 리디렉션을 `skicampaign.txt` 텍스트 파일에 저장하고 DAM @ `/content/dam/wknd/redirects` 폴더에 업로드할 수 있습니다. 검토 및 승인 시 마케팅 팀은 텍스트 파일을 게시할 수 있습니다.

```
# Ski Campaign Redirects separated by the TAB character
/ski      /us/en/adventures.html
/ski/northamerica  /us/en/adventures/downhill-skiing-wyoming.html
/ski/westcoast   /us/en/adventures/tahoe-skiing.html
/ski/europe          /us/en/adventures/ski-touring-mont-blanc.html
```

![DAM의 텍스트 파일](./assets/pipeline-free-redirects/text-file-in-dam.png)

### ACS Commons - 맵 관리자 리디렉션

[ACS Commons - 리디렉션 맵 관리자](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html)는 URL 리디렉션을 관리할 수 있는 사용자 친화적 인터페이스를 제공합니다.

예를 들어 마케팅 팀은 `SkiCampaign`(이)라는 이름의 새 *리디렉션 맵* 페이지를 만들고 **항목 편집** 탭을 사용하여 위의 URL 리디렉션을 추가할 수 있습니다. `/etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt`에서 URL 리디렉션을 사용할 수 있습니다.

![맵 관리자 리디렉션](./assets/pipeline-free-redirects/redirect-map-manager.png)

>[!IMPORTANT]
>
>리디렉션 맵 관리자를 사용하려면 ACS Commons 버전 **6.7.0 이상**&#x200B;이(가) 필요합니다. 자세한 내용은 [ACS Commons - Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html)를 참조하십시오.

### ACS Commons - 리디렉션 관리자

또는 [ACS Commons - Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html)에서 URL 리디렉션을 관리할 수 있는 사용자 친화적 인터페이스도 제공합니다.

예를 들어 마케팅 팀은 `/conf/wknd`(이)라는 새 구성을 만들고 **+ 리디렉션 구성** 단추를 사용하여 위의 URL 리디렉션을 추가할 수 있습니다. `/conf/wknd/settings/redirects.txt`에서 URL 리디렉션을 사용할 수 있습니다.

![리디렉션 관리자](./assets/pipeline-free-redirects/redirect-manager.png)

>[!IMPORTANT]
>
>리디렉션 관리자를 사용하려면 ACS Commons 버전 **6.10.0 이상**&#x200B;이(가) 필요합니다. 자세한 내용은 [ACS Commons - Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html)를 참조하십시오.

## Dispatcher 구성 방법

URL 리디렉션을 RewriteMap으로 로드하여 수신 요청에 적용하려면 다음 Dispatcher 구성이 필요합니다.

### 유연한 모드를 위한 Dispatcher 모듈 활성화

먼저 Dispatcher 모듈이 _유연한 모드_&#x200B;에 대해 활성화되어 있는지 확인합니다. `dispatcher/src/opt-in` 폴더에 `USE_SOURCES_DIRECTLY` 파일이 있으면 Dispatcher이 유연한 모드에 있음을 나타냅니다.

### URL 리디렉션을 RewriteMap으로 로드

다음 구조로 `dispatcher/src/opt-in` 폴더에 새 구성 파일 `managed-rewrite-maps.yaml`을(를) 만듭니다.

```yaml
maps:
- name: <MAPNAME>.map # e.g. skicampaign.map
    path: <ABSOLUTE_PATH_TO_URL_REDIRECTS_FILE> # e.g. /content/dam/wknd/redirects/skicampaign.txt, /etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt, /conf/wknd/settings/redirects.txt
    wait: false # Optional, default is false, when true, the Apache waits for the map to be loaded before starting
    ttl: 300 # Optional, default is 300 seconds, the reload interval for the map
```

배포하는 동안 Dispatcher은 `/tmp/rewrites` 폴더에 `<MAPNAME>.map` 파일을 만듭니다.

>[!IMPORTANT]
>
> 파일 이름(`managed-rewrite-maps.yaml`)과 위치(`dispatcher/src/opt-in`)는 위에서 설명한 대로 정확히 지정해야 합니다. 따라야 할 규칙이라고 생각하십시오.

### 수신 요청에 URL 리디렉션 적용

마지막으로 위의 맵(`<MAPNAME>.map`)을 사용하도록 Apache 재작성 구성 파일을 만들거나 업데이트합니다. 예를 들어 `dispatcher/src/conf.d/rewrites` 폴더의 `rewrite.rules` 파일을 사용하여 URL 리디렉션을 적용해 보겠습니다.

```
...
# Use the RewriteMap to define the URL redirects
RewriteMap <MAPALIAS> dbm=sdbm:/tmp/rewrites/<MAPNAME>.map

RewriteCond ${<MAPALIAS>:$1} !=""
RewriteRule ^(.*)$ ${<MAPALIAS>:$1|/} [L,R=301]    
...
```

### 예제 구성

[위](#manage-redirects)에서 언급한 각 URL 리디렉션 관리 옵션에 대한 Dispatcher 구성을 검토해 보겠습니다.

>[!BEGINTABS]

>[!TAB DAM의 텍스트 파일]

URL 리디렉션이 텍스트 파일에서 키-값 쌍으로 관리되고 DAM에 업로드되는 경우 구성은 다음과 같습니다.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```yaml
maps:
- name: skicampaign.map
  path: /content/dam/wknd/redirects/skicampaign.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```
...

# The DAM-managed skicampaign.txt file as skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!TAB ACS Commons - 맵 관리자 리디렉션]

ACS Commons - Redirect Map Manager를 사용하여 URL 리디렉션을 관리하는 경우, 구성은 다음과 같습니다.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```yaml
maps:
- name: skicampaign.map
  path: /etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```
...

# The Redirect Map Manager-managed skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!TAB ACS Commons - 리디렉션 관리자]

ACS Commons - Redirect Manager를 사용하여 URL 리디렉션을 관리하는 경우, 구성은 다음과 같습니다.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```yaml
maps:
- name: skicampaign.map
  path: /conf/wknd/settings/redirects.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```
...

# The Redirect Manager-managed skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!ENDTABS]

## 구성을 배포하는 방법

>[!IMPORTANT]
>
>*파이프라인 없음* 용어를 사용하여 구성이 *한 번만 배포*&#x200B;되고 마케팅 팀이 텍스트 파일을 업데이트하여 URL 리디렉션을 관리할 수 있음을 강조합니다.

구성을 배포하려면 [Cloud Manager](https://my.cloudmanager.adobe.com/)에서 [전체 스택](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline) 또는 [웹 계층 구성](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#web-tier-config-pipelines) 파이프라인을 사용하십시오.

![전체 스택 파이프라인을 통해 배포](./assets/pipeline-free-redirects/deploy-full-stack-pipeline.png)


배포가 성공하면 URL 리디렉션이 활성화되고 마케팅 팀이 개발자가 필요 없이 관리할 수 있습니다.

## URL 리디렉션을 테스트하는 방법

브라우저 또는 `curl` 명령을 사용하여 URL 리디렉션을 테스트해 보겠습니다. `/ski/westcoast` URL에 액세스하여 `/us/en/adventures/tahoe-skiing.html`(으)로 리디렉션되는지 확인하십시오.

## 요약

이 자습서에서는 AEM as a Cloud Service 환경에서 파이프라인 없는 구성을 사용하여 URL 리디렉션을 관리하는 방법을 배웠습니다.

마케팅 팀은 URL 리디렉션을 텍스트 파일의 키-값 쌍으로 관리하고 DAM에 업로드하거나 ACS Commons - Redirect Map Manager 또는 Redirect Manager를 사용할 수 있습니다. Dispatcher 구성이 업데이트되어 URL 리디렉션을 RewriteMap으로 로드하고 수신 요청에 적용합니다.

## 추가 리소스

- [파이프라인 없는 URL 리디렉션](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)
- [URL 리디렉션](url-redirection.md)

