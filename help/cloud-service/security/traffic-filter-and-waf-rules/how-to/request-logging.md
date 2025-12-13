---
title: 민감한 요청 모니터링
description: AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 민감한 요청을 로깅하여 모니터링하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18311
thumbnail: null
exl-id: 8fa0488f-b901-49bf-afa5-5ed29242355f
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 100%

---

# 민감한 요청 모니터링

AEM as a Cloud Service에서 트래픽 필터 규칙을 사용하여 민감한 요청을 로깅하여 모니터링하는 방법에 대해 알아봅니다.

로깅을 통해 최종 사용자나 서비스에 영향을 주지 않고 트래픽 패턴을 관찰할 수 있으며, 이는 차단 규칙을 구현하기 전에 중요한 첫 단계입니다.

이 튜토리얼에서는 AEM 게시 서비스에 대한 **WKND 로그인 및 로그아웃 경로의 요청을 로그**&#x200B;하는 방법을 보여 줍니다.

## 요청을 로그하는 이유 및 시기

특정 요청을 로그하는 것은 사용자 및 잠재적으로 악의적인 행위자가 AEM 애플리케이션과 상호 작용하는 방식을 이해하는 데 있어 위험이 낮고 가치가 높은 방법입니다. 특히 차단 규칙을 적용하기 전에 유용하며, 합법적인 트래픽을 방해하지 않고 보안 태세를 개선할 수 있는 확신을 줍니다.

로깅에 대한 일반적인 시나리오는 다음과 같습니다.

- 규칙을 `block` 모드로 승격하기 전에 규칙의 영향과 도달 범위를 확인합니다.
- 로그인/로그아웃 경로 및 인증 엔드포인트에서 비정상적인 패턴 또는 무차별 대입 시도를 모니터링합니다.
- 잠재적인 남용 또는 DoS 활동에 대해 API 엔드포인트에 대한 빈도가 높은 액세스를 추적합니다.
- 더 엄격한 통제를 적용하기 전에 봇 동작에 대한 기준선을 설정합니다.
- 보안 사고 발생 시 공격의 특성 및 영향을 받는 리소스를 파악하기 위한 포렌식 데이터를 제공합니다.

## 사전 요구 사항

진행하기 전에 트래픽 [필터 및 WAF 규칙 설정 방법](../setup.md) 튜토리얼에 설명된 필수 설정을 완료했는지 확인하시기 바랍니다. 또한 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)를 AEM 환경에 복제하고 배포했는지 확인하십시오.

## 예: WKND 로그인 및 로그아웃 요청 로그

이 예시에서는 AEM 게시 서비스의 WKND 로그인 및 로그아웃 경로에 대한 요청을 로그하는 트래픽 필터 규칙을 생성합니다. 이렇게 하면 인증 시도를 모니터링하고 잠재적인 보안 문제를 식별하는 데 도움이 됩니다.

- WKND 프로젝트의 `/config/cdn.yaml` 파일에 다음 규칙을 추가합니다.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
    - name: publish-auth-requests
      when:
        allOf:
          - reqProperty: tier
            matches: publish
          - reqProperty: path
            in:
              - /system/sling/login/j_security_check
              - /system/sling/logout
      action: log   
```

- 변경 사항을 Cloud Manager Git 저장소에 커밋하고 푸시합니다.

- [앞서 만든](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 구성 파이프라인을 사용하여 AEM 환경에 변경 사항을 배포합니다.

- 프로그램의 WKND 사이트(예: `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`)에 로그인 및 로그아웃하여 규칙을 테스트합니다. `asmith/asmith`를 사용자 이름과 암호로 사용할 수 있습니다.

  ![WKND 로그인](../assets/how-to/wknd-login.png)

## 분석

Cloud Manager에서 AEMCS CDN 로그를 가져오고 [AEMCS CDN Log Analysis Tooling](../setup.md#setup-the-elastic-dashboard-tool)을 사용하여 `publish-auth-requests` 규칙의 결과를 분석해 보겠습니다.

- [Cloud Manager](https://my.cloudmanager.adobe.com/)의 **환경** 카드에서 AEMCS **게시** 서비스의 CDN 로그를 다운로드합니다.

  ![Cloud Manager CDN 로그 다운로드](../assets/how-to/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 새로운 요청이 CDN 로그에 나타나기까지 최대 5분 정도 소요될 수 있습니다.

- 다운로드한 로그 파일(예: 아래 스크린샷의 `publish_cdn_2023-10-24.log`)을 Elastic 대시보드 도구 프로젝트의 `logs/dev` 폴더에 복사합니다.

  ![ELK 도구 로그 폴더](../assets/how-to/elk-tool-logs-folder.png)

- Elastic 대시보드 도구 페이지를 새로 고칩니다.
   - 상단의 **전역 필터** 섹션에서 `aem_env_name.keyword` 필터를 편집하고 `dev` 환경 값을 선택합니다.

     ![ELK 도구 전역 필터](../assets/how-to/elk-tool-global-filter.png)

   - 시간 간격을 변경하려면 오른쪽 상단에 있는 캘린더 아이콘을 클릭하고 원하는 시간 간격을 선택합니다.

     ![ELK 도구 시간 간격](../assets/how-to/elk-tool-time-interval.png)

- 업데이트된 대시보드의 **분석된 요청**, **플래그가 지정된 요청** 및 **플래그가 지정된 요청 세부 정보** 패널을 검토합니다. CDN 로그 항목을 일치시키려면 각 항목의 클라이언트 IP(cli_ip), 호스트, URL, 작업(waf_action) 및 규칙 이름(waf_match) 값을 표시해야 합니다.

  ![ELK 도구 대시보드](../assets/how-to/elk-tool-dashboard.png)
