---
title: 요청 표준화
description: AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 요청을 변환하여 정규화하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
exl-id: eee81cd6-9090-45d6-b77f-a266de1d9826
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 100%

---

# 요청 표준화

AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 요청을 변환하여 정규화하는 방법에 대해 알아봅니다.

## 요청을 변환하는 이유와 시기

요청 변환은 불필요한 쿼리 매개변수나 헤더로 인해 발생하는 불필요한 분산을 줄이고 들어오는 트래픽을 정규화하려는 경우에 유용합니다. 이 기술은 일반적으로 다음과 같은 경우에 사용됩니다.

- AEM 애플리케이션과 관련 없는 캐시 무효화 매개변수를 제거하여 캐싱 효율성을 향상시킵니다.
- 요청 순열을 최소화하고 불필요한 처리를 완화하여 남용으로부터 원본을 보호합니다.
- AEM에 전달되기 전에 요청을 정리하거나 단순화합니다.

이러한 변환은 일반적으로 CDN 계층, 특히 공용 트래픽을 처리하는 AEM 게시 계층에 적용됩니다.

## 사전 요구 사항

진행하기 전에 트래픽 [필터 및 WAF 규칙 설정 방법](../setup.md) 튜토리얼에 설명된 필수 설정을 완료했는지 확인하시기 바랍니다. 또한 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)를 AEM 환경에 복제하고 배포했는지 확인하십시오.

## 예: 애플리케이션에 필요하지 않은 쿼리 매개변수 설정 해제

이 예에서는 캐시 조각화를 줄이기 위해 `search` 및 `campaignId`를 제외한 **모든 쿼리 매개변수를 제거**&#x200B;하는 규칙을 구성합니다.

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

- [앞서 만든](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 구성 파이프라인을 사용하여 AEM 환경에 변경 사항을 배포합니다.

- WKND 사이트의 페이지(예: `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`)에 액세스하여 규칙을 테스트합니다.

- AEM 로그(`aemrequest.log`)에서 요청이 `otherParam`이 제거된 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`로 변환된 것을 확인할 수 있습니다.

  ![WKND 요청 변환](../assets/how-to/aemrequest-log-transformation.png)
