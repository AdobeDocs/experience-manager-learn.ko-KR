---
title: 요청 표준화
description: AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 요청을 변환하여 요청을 정규화하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 8%

---

# 요청 표준화

AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 요청을 변환하여 요청을 정규화하는 방법을 알아봅니다.

## 요청을 변형하는 이유 및 시기

요청 변환은 들어오는 트래픽을 정규화하고 불필요한 쿼리 매개 변수 또는 헤더로 인한 불필요한 분산을 줄이려는 경우에 유용합니다. 이 기법은 일반적으로 다음 작업을 수행하는 데 사용됩니다.

- AEM 애플리케이션과 관련이 없는 캐시 무효화 매개 변수를 제거하여 캐싱 효율성을 개선합니다.
- 요청 순열을 최소화하고 불필요한 처리를 완화하여 원점을 남용으로부터 보호합니다.
- 요청을 AEM으로 전달하기 전에 삭제하거나 단순화합니다.

이러한 변환은 일반적으로 CDN 계층에서 적용되며, 특히 공용 트래픽을 제공하는 AEM Publish 계층에 더 적합합니다.

## 사전 요구 사항

계속하기 전에 [트래픽 필터 및 WAF 규칙을 설정하는 방법](../setup.md) 자습서에 설명된 대로 필요한 설정을 완료했는지 확인하십시오. 또한 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)를 AEM 환경에 복제하고 배포했습니다.

## 예: 애플리케이션에서 쿼리 매개 변수 설정 해제가 필요하지 않음

이 예제에서는 캐시 단편화를 줄이기 위해 **모든 쿼리 매개 변수** `search` 및 `campaignId`을(를) 제거하는 규칙을 구성합니다.

- WKND 프로젝트의 `/config/cdn.yaml` 파일에 다음 규칙을 추가합니다.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- 변경 사항을 Cloud Manager Git 저장소에 커밋하고 푸시합니다.

- Cloud Manager 구성 파이프라인 [이전에 만든](../setup.md#deploy-rules-using-adobe-cloud-manager)을(를) 사용하여 AEM 환경에 변경 사항을 배포합니다.

- WKND 사이트의 페이지에 액세스하여 규칙을 테스트합니다(예: `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`).

- AEM 로그(`aemrequest.log`)에서 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`이(가) 제거된 상태로 요청이 `otherParam`(으)로 변환되었는지 확인해야 합니다.

  ![WKND 요청 변환](../assets/how-to/aemrequest-log-transformation.png)

