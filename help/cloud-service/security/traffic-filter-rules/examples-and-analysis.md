---
title: WAF 규칙을 포함한 트래픽 필터 규칙의 예 및 결과 분석
description: WAF 규칙 예를 포함하여 다양한 트래픽 필터 규칙에 대해 알아봅니다. 또한 AEMCS(AEM as a Cloud Service) CDN 로그를 사용하여 결과를 분석하는 방법도 설명합니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
duration: 532
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1472'
ht-degree: 0%

---

# WAF 규칙을 포함한 트래픽 필터 규칙의 예 및 결과 분석

Adobe Experience Manager as a Cloud Service(AEMCS) CDN 로그 및 대시보드 도구를 사용하여 다양한 유형의 트래픽 필터 규칙을 선언하고 결과를 분석하는 방법에 대해 알아봅니다.

이 섹션에서는 WAF 규칙을 포함하여 트래픽 필터 규칙의 실제 예를 살펴봅니다. [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)를 사용하여 URI(또는 경로), IP 주소, 요청 수 및 다양한 공격 유형을 기반으로 요청을 기록, 허용 및 차단하는 방법에 대해 알아봅니다.

또한 Adobe에서 제공하는 샘플 대시보드를 통해 필수 지표를 시각화하기 위해 AEMCS CDN 로그를 수집하는 대시보드 도구를 사용하는 방법에 대해 알아봅니다.

특정 요구 사항에 맞게 사용자 정의 대시보드를 개선하고 만들어 보다 심층적인 통찰력을 얻고 AEM 사이트에 대한 규칙 구성을 최적화할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## 예

WAF 규칙을 포함하여 트래픽 필터 규칙의 다양한 예를 살펴보겠습니다. 이전 [설정 방법](./how-to-setup.md) 장에 설명된 대로 필요한 설정 프로세스를 완료했으며 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)를 복제했는지 확인하십시오.

### 요청 로깅

AEM Publish 서비스에 대해 **WKND 로그인 및 로그아웃 경로의 요청 로깅**&#x200B;부터 시작합니다.

- WKND 프로젝트의 `/config/cdn.yaml` 파일에 다음 규칙을 추가합니다.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
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

- 변경 사항을 커밋하고 Cloud Manager Git 저장소에 푸시합니다.

- Cloud Manager `Dev-Config` 구성 파이프라인 [이전에 만든](how-to-setup.md#deploy-rules-through-cloud-manager)을(를) 사용하여 AEM 개발 환경에 변경 사항을 배포합니다.

  ![Cloud Manager 구성 파이프라인](./assets/cloud-manager-config-pipeline.png)

- 게시 서비스에서 프로그램의 WKND 사이트에 로그인하고 로그아웃하여 규칙을 테스트합니다(예: `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). `asmith/asmith`을(를) 사용자 이름과 암호로 사용할 수 있습니다.

  ![WKND 로그인](./assets/wknd-login.png)

#### 분석 중{#analyzing}

Cloud Manager에서 AEMCS CDN 로그를 다운로드하고 이전 장에서 설정한 [대시보드 도구](how-to-setup.md#analyze-results-using-elk-dashboard-tool)를 사용하여 `publish-auth-requests` 규칙의 결과를 분석해 보겠습니다.

- [Cloud Manager](https://my.cloudmanager.adobe.com/)의 **환경** 카드에서 AEMCS **Publish** 서비스의 CDN 로그를 다운로드합니다.

  ![Cloud Manager CDN 로그 다운로드](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    새 요청이 CDN 로그에 표시되는 데 최대 5분이 걸릴 수 있습니다.

- 다운로드한 로그 파일(예: 아래 스크린샷의 `publish_cdn_2023-10-24.log`)을 Elastic Dashboard Tool 프로젝트의 `logs/dev` 폴더에 복사합니다.

  ![ELK 도구 로그 폴더](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- 탄력적인 대시보드 도구 페이지를 새로 고칩니다.
   - 위쪽 **전역 필터** 섹션에서 `aem_env_name.keyword` 필터를 편집하고 `dev` 환경 값을 선택합니다.

     ![ELK 도구 전역 필터](./assets/elk-tool-global-filter.png)

   - 시간 간격을 변경하려면 오른쪽 상단 모서리의 달력 아이콘을 클릭하고 원하는 시간 간격을 선택합니다.

     ![ELK 도구 시간 간격](./assets/elk-tool-time-interval.png)

- 업데이트된 대시보드의 **분석된 요청**, **플래그가 지정된 요청** 및 **플래그가 지정된 요청 세부 정보** 패널을 검토하십시오. 일치하는 CDN 로그 항목의 경우 각 항목의 클라이언트 IP(cli_ip), 호스트, url, 작업(waf_action) 및 규칙 이름(waf_match) 값이 표시되어야 합니다.

  ![ELK 도구 대시보드](./assets/elk-tool-dashboard.png)


### 요청 차단

이 예제에서는 배포된 WKND 프로젝트의 경로 `/content/wknd/internal`에 있는 _internal_ 폴더에 페이지를 추가해 보겠습니다. 그런 다음 조직에 일치하는 지정된 IP 주소가 아닌 다른 곳(예: 회사 VPN)에서 하위 페이지로 이동하는 트래픽을 **차단**&#x200B;하는 트래픽 필터 규칙을 선언합니다.

고유한 내부 페이지(예: `demo-page.html`)를 만들거나 [첨부된 패키지](./assets/demo-internal-pages-package.zip)를 사용할 수 있습니다.

- WKND 프로젝트의 `/config/cdn.yaml` 파일에 다음 규칙을 추가합니다.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...

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

- 변경 사항을 커밋하고 Cloud Manager Git 저장소에 푸시합니다.

- Cloud Manager에서 [이전에 만든](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` 구성 파이프라인을 사용하여 AEM 개발 환경에 변경 내용을 배포합니다.

- WKND 사이트의 내부 페이지(예: `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`)에 액세스하거나 아래 CURL 명령을 사용하여 규칙을 테스트합니다.

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 규칙에 사용한 IP 주소와 다른 IP 주소(예: 휴대폰 사용)에서 위의 단계를 반복합니다.

#### 분석 중

`block-internal-paths` 규칙의 결과를 분석하려면 [이전 예제](#analyzing)에 설명된 것과 동일한 단계를 수행합니다.

그러나 이번에는 클라이언트 IP(cli_ip), 호스트, URL, 작업(waf_action) 및 규칙 이름(waf_match) 열에 **차단된 요청**&#x200B;과 해당 값이 표시됩니다.

![ELK 도구 대시보드 차단된 요청](./assets/elk-tool-dashboard-blocked.png)


### DoS 공격 방지

IP 주소의 요청을 차단하여 **DoS 공격을 방지**&#x200B;합니다. 초당 100개의 요청을 수행하여 5분 동안 차단됩니다.

- WKND 프로젝트의 `/config/cdn.yaml` 파일에 다음 [속도 제한 트래픽 필터 규칙](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=ko#ratelimit-structure)을(를) 추가합니다.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
```

>[!WARNING]
>
>프로덕션 환경의 경우 웹 보안 팀과 협력하여 `rateLimit`에 대한 적절한 값을 결정하십시오.

- [이전 예제](#logging-requests)에서 설명한 대로 변경 내용을 커밋하고, 푸시하고, 배포합니다.

- DoS 공격을 시뮬레이션하려면 다음 [Vegeta](https://github.com/tsenart/vegeta) 명령을 사용합니다.

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=60s | vegeta report
  ```

  이 명령은 5초 동안 120개의 요청을 만들고 보고서를 출력합니다. 보시는 것처럼 성공률은 32.5%입니다. 나머지 항목에 대해서는 406 HTTP 응답 코드가 수신되므로 트래픽이 차단되었음을 알 수 있습니다.

  ![Vegeta DoS 공격](./assets/vegeta-dos-attack.png)

#### 분석 중

`prevent-dos-attacks` 규칙의 결과를 분석하려면 [이전 예제](#analyzing)에 설명된 것과 동일한 단계를 수행합니다.

이번에는 클라이언트 IP(cli_ip), 호스트, url, 작업(waf_action) 및 규칙 이름(waf_match) 열에 **차단된 요청**&#x200B;과 해당 값이 많이 표시됩니다.

![ELK 도구 대시보드 DoS 요청](./assets/elk-tool-dashboard-dos.png)

또한 클라이언트 IP, 국가 및 사용자 에이전트별 **상위 100개 공격** 패널에는 규칙 구성을 최적화하는 데 사용할 수 있는 추가 세부 정보가 표시됩니다.

![ELK 도구 대시보드 DoS 상위 100개 요청](./assets/elk-tool-dashboard-dos-top-100.png)

DoS 및 DDoS 공격을 방지하는 방법에 대한 자세한 내용은 [트래픽 필터 규칙을 사용하여 DoS 및 DDoS 공격 차단](../blocking-dos-attack-using-traffic-filter-rules.md) 자습서를 검토하십시오.

### WAF 규칙

지금까지의 트래픽 필터 규칙 예는 모든 Sites 및 Forms 고객이 구성할 수 있습니다.

다음으로, 향상된 보안 또는 WAF-DDoS 보호 라이센스를 구입한 고객이 고급 규칙을 구성하여 보다 정교한 공격으로부터 웹 사이트를 보호할 수 있는 경험에 대해 알아보겠습니다.

계속하기 전에 트래픽 필터 규칙 설명서 [설정 단계](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=ko#setup)에 설명된 대로 프로그램에 대해 WAF-DDoS 보호를 사용하도록 설정하십시오.

#### WAFFlags가 없는 경우

WAF 규칙이 선언되기 전에도 경험을 먼저 살펴보겠습니다. 프로그램에서 WAF-DDoS가 활성화되면 CDN은 기본적으로 악성 트래픽의 일치를 기록하므로 적절한 규칙을 마련할 수 있는 올바른 정보를 보유하고 있습니다.

먼저 WAF 규칙을 추가하거나 `wafFlags` 속성을 사용하지 않고 WKND 사이트를 공격하고 결과를 분석해 보겠습니다.

- 공격을 시뮬레이션하려면 아래의 [Nikto](https://github.com/sullo/nikto) 명령을 사용합니다. 이 명령은 6분 동안 약 700개의 악성 요청을 보냅니다.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Nikto 공격 시뮬레이션](./assets/nikto-attack.png)

  공격 시뮬레이션에 대해 알아보려면 포함 또는 제외할 테스트 공격 유형을 지정하는 방법을 알려주는 [Nikto - Scan Tuning](https://github.com/sullo/nikto/wiki/Scan-Tuning) 설명서를 검토하십시오.

##### 분석 중

공격 시뮬레이션의 결과를 분석하려면 [이전 예제](#analyzing)에 설명된 것과 동일한 단계를 수행합니다.

그러나 이번에는 클라이언트 IP(cli_ip), 호스트, url, 작업(waf_action) 및 규칙 이름(waf_match) 열에 **플래그가 지정된 요청**&#x200B;과 해당 값이 표시됩니다. 이 정보를 사용하면 결과를 분석하고 규칙 구성을 최적화할 수 있습니다.

![ELK 도구 대시보드 WAF 플래그 지정된 요청](./assets/elk-tool-dashboard-waf-flagged.png)

**WAF 플래그 배포** 및 **상위 공격** 패널에 추가 세부 정보가 표시되는 방법을 참고하십시오. 이러한 추가 세부 정보는 규칙 구성을 최적화하는 데 사용할 수 있습니다.

![ELK 도구 대시보드 WAF 플래그 공격 요청](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ELK 도구 대시보드 WAF 상위 공격 요청](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### WAFFlags 포함

이제 `wafFlags` 속성을 `action` 속성의 일부로 포함하고 **시뮬레이션된 공격 요청을 차단**&#x200B;하는 WAF 규칙을 추가하겠습니다.

구문 측면에서 볼 때 WAF 규칙은 앞에서 본 규칙과 유사하지만 `action` 속성이 하나 이상의 `wafFlags` 값을 참조합니다. `wafFlags`에 대해 자세히 알아보려면 [WAF 플래그 목록](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=ko#waf-flags-list) 섹션을 검토하세요.

- WKND 프로젝트의 `/config/cdn.yaml` 파일에 다음 규칙을 추가합니다. `block-waf-flags` 규칙에 시뮬레이션된 악성 트래픽으로 공격받을 때 대시보드 도구에 표시된 일부 wafFlags가 어떻게 포함되어 있는지 확인합니다. 실제로, 위협 환경이 발전함에 따라, 로그를 분석하여 선언할 새로운 규칙을 결정하는 것은 시간이 지남에 따라 좋은 관례입니다.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
```

- [이전 예제](#logging-requests)에서 설명한 대로 변경 내용을 커밋하고, 푸시하고, 배포합니다.

- 공격을 시뮬레이션하려면 이전과 동일한 [Nikto](https://github.com/sullo/nikto) 명령을 사용합니다.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### 분석 중

[이전 예제](#analyzing)에 설명된 것과 동일한 단계를 반복합니다.

이번에는 **차단된 요청** 아래에 항목이 표시되고 클라이언트 IP(cli_ip), 호스트, url, 작업(waf_action) 및 규칙 이름(waf_match) 열에 해당 값이 표시됩니다.

![ELK 도구 대시보드 WAF 차단 요청](./assets/elk-tool-dashboard-waf-blocked.png)

또한 **WAF 플래그 배포** 및 **상위 공격** 패널에 추가 세부 정보가 표시됩니다.

![ELK 도구 대시보드 WAF 플래그 공격 요청](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ELK 도구 대시보드 WAF 상위 공격 요청](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### 포괄적인 분석

위의 _분석_ 섹션에서 대시보드 도구를 사용하여 특정 규칙의 결과를 분석하는 방법을 배웠습니다. 다음을 포함한 다른 대시보드 패널을 사용하여 분석 결과를 더 탐색할 수 있습니다.


- 분석, 플래그 지정 및 차단된 요청
- 시간 경과에 따른 WAF 플래그 분포
- 시간 경과에 따라 트리거된 트래픽 필터 규칙
- WAF 플래그 ID별 상위 공격
- 상위 트리거된 트래픽 필터
- 클라이언트 IP, 국가 및 사용자 에이전트별 상위 100명의 공격자

![ELK 도구 대시보드의 포괄적인 분석](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![ELK 도구 대시보드의 포괄적인 분석](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## 다음 단계

보안 위반 위험을 줄이기 위해 권장되는 [모범 사례](./best-practices.md)를 숙지하십시오.

## 추가 리소스

[트래픽 필터 규칙 구문](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=ko#rules-syntax)

[CDN 로그 형식](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=ko#cdn-log-format)

