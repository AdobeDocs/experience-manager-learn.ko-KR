---
title: 액세스 제한
description: AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 특정 요청을 차단하여 액세스를 제한하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
exl-id: 53cb8996-4944-4137-a979-6cf86b088d42
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 100%

---

# 액세스 제한

AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 특정 요청을 차단하여 액세스를 제한하는 방법에 대해 알아봅니다.

이 튜토리얼에서는 AEM 게시 서비스에서 **공용 IP로부터 내부 경로에 대한 요청을 차단**&#x200B;하는 방법을 보여 줍니다.

## 요청을 차단해야 하는 이유 및 시기

트래픽 차단은 특정 조건에서 민감한 리소스 또는 URL에 대한 액세스를 방지하여 조직의 보안 정책을 시행하는 데 도움이 됩니다. 로깅에 비해 차단은 더 엄격한 조치이며, 특정 소스에서 오는 트래픽이 승인되지 않았거나 원치 않는 것이라고 확신할 때 사용해야 합니다.

차단이 적절한 일반적인 시나리오는 다음과 같습니다.

- `internal` 또는 `confidential` 페이지에 대한 액세스를 내부 IP 범위(예: 회사 VPN 뒤)로만 제한합니다.
- IP 또는 지리적 위치로 식별된 봇 트래픽, 자동 스캐너 또는 위협 행위자를 차단합니다.
- 단계적 마이그레이션 중 사용 중단되거나 보안되지 않은 엔드포인트에 대한 액세스를 방지합니다.
- 게시 계층에서 작성 도구 또는 관리 경로에 대한 액세스를 제한합니다.

## 사전 요구 사항

진행하기 전에 트래픽 [필터 및 WAF 규칙 설정 방법](../setup.md) 튜토리얼에 설명된 필수 설정을 완료했는지 확인하시기 바랍니다. 또한 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)를 AEM 환경에 복제하고 배포했는지 확인하십시오.

## 예: 공용 IP로부터 내부 경로 차단

이 예시에서는`https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`과 같은 내부 WKND 페이지에 대한 외부 액세스를 공용 IP 주소로부터 차단하는 규칙을 구성합니다. 신뢰할 수 있는 IP 범위(예: 회사 VPN) 내의 사용자만 이 페이지에 액세스할 수 있습니다.

자체 내부 페이지를 만들거나(예: `demo-page.html`) [첨부된 패키지](../assets/how-to/demo-internal-pages-package.zip)를 사용할 수 있습니다.

- WKND 프로젝트의 `/config/cdn.yaml` 파일에 다음 규칙을 추가합니다.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
    - name: block-internal-paths
      when:
        allOf:
          - reqProperty: path
            matches: /content/wknd/internal
          - reqProperty: clientIp
            notIn: [192.150.10.0/24]
      action: block    
```

- 변경 사항을 Cloud Manager Git 저장소에 커밋하고 푸시합니다.

- [앞서 만든](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 구성 파이프라인을 사용하여 AEM 환경에 변경 사항을 배포합니다.

- WKND 사이트의 내부 페이지(예: `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`)에 액세스하거나 아래의 CURL 명령을 사용하여 규칙을 테스트합니다.

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 규칙에서 사용한 IP 주소와 다른 IP 주소(예: 휴대전화 사용)에서 위의 단계를 반복합니다.

## 분석

`block-internal-paths` 규칙의 결과를 분석하려면 [설정 튜토리얼](../setup.md#cdn-logs-ingestion)에서 설명한 것과 동일한 단계를 따릅니다.

클라이언트 IP(cli_ip), 호스트, URL, 작업(waf_action) 및 규칙 이름(waf_match) 열에 **차단된 요청**&#x200B;과 해당 값이 표시되어야 합니다.

![ELK 도구 대시보드 차단된 요청](../assets/how-to/elk-tool-dashboard-blocked.png)
