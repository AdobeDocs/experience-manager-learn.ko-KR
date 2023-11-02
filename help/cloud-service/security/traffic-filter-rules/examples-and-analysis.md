---
title: WAF 규칙을 포함한 트래픽 필터 규칙의 예 및 결과 분석
description: WAF 규칙 예를 포함하여 다양한 트래픽 필터 규칙에 대해 알아봅니다. 또한 AEMCS(AEM as a Cloud Service) CDN 로그를 사용하여 결과를 분석하는 방법도 설명합니다.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
source-git-commit: ceb498f751ffc50d0022a16b63f9f52594bc507e
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 0%

---

# WAF 규칙을 포함한 트래픽 필터 규칙의 예 및 결과 분석

Adobe Experience Manager as a Cloud Service(AEMCS) CDN 로그 및 대시보드 도구를 사용하여 다양한 유형의 트래픽 필터 규칙을 선언하고 결과를 분석하는 방법에 대해 알아봅니다.

이 섹션에서는 WAF 규칙을 비롯한 트래픽 필터 규칙의 실제 예를 살펴봅니다. 다음을 사용하여 URI(또는 경로), IP 주소, 요청 수 및 다양한 공격 유형을 기반으로 요청을 기록, 허용 및 차단하는 방법을 배웁니다. [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

또한 AEM CS CDN 로그를 수집하는 대시보드 도구를 사용하여 Adobe 제공 샘플 대시보드를 통해 필수 지표를 시각화하는 방법에 대해 알아봅니다.

특정 요구 사항에 맞게 사용자 정의 대시보드를 개선하고 만들어 더 자세한 통찰력을 얻고 AEM 사이트에 대한 규칙 구성을 최적화할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## 예

WAF 규칙을 비롯한 트래픽 필터 규칙의 다양한 예를 살펴보겠습니다. 앞에서 설명한 대로 필요한 설정 프로세스를 완료했는지 확인합니다 [설정 방법](./how-to-setup.md) 챕터를 복제했습니다. [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### 요청 로깅

시작 일자 **WKND 로그인 및 로그아웃 경로의 요청 로깅** AEM Publish 서비스에 대해 설명합니다.

- WKND 프로젝트의 `/config/cdn.yaml` 파일.

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

- Cloud Manager를 사용하여 AEM 개발 환경에 변경 사항 배포 `Dev-Config` 구성 파이프라인 [이전에 생성됨](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Cloud Manager 구성 파이프라인](./assets/cloud-manager-config-pipeline.png)

- Publish 서비스에서 프로그램의 WKND 사이트에 로그인하고 로그아웃하여 규칙을 테스트합니다(예: `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). 다음을 사용할 수 있습니다. `asmith/asmith` 을 사용자 이름과 암호로 사용하십시오.

  ![WKND 로그인](./assets/wknd-login.png)

#### 분석 중{#analyzing}

의 결과를 분석해 보겠습니다. `publish-auth-requests` Cloud Manager에서 AEMCS CDN 로그를 다운로드하고 [대시보드 도구](how-to-setup.md#analyze-results-using-elk-dashboard-tool): 이전 장에서 설정한 것입니다.

- 출처: [Cloud Manager](https://my.cloudmanager.adobe.com/)의 **환경** 카드, AEM cs 다운로드 **게시** 서비스의 CDN 로그.

  ![Cloud Manager CDN 로그 다운로드](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    새 요청이 CDN 로그에 표시되는 데 최대 5분이 걸릴 수 있습니다.

- 다운로드한 로그 파일을 복사합니다(예: `publish_cdn_2023-10-24.log` 아래 스크린샷에서) `logs/dev` 탄력적인 대시보드 도구 프로젝트의 폴더입니다.

  ![ELK 도구 로그 폴더](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- 탄력적인 대시보드 도구 페이지를 새로 고칩니다.
   - 맨 위에 **전역 필터** 섹션, 편집 `aem_env_name.keyword` 필터링 및 선택 `dev` 환경 값입니다.

     ![ELK 도구 글로벌 필터](./assets/elk-tool-global-filter.png)

   - 시간 간격을 변경하려면 오른쪽 상단 모서리의 달력 아이콘을 클릭하고 원하는 시간 간격을 선택합니다.

     ![ELK 도구 시간 간격](./assets/elk-tool-time-interval.png)

- 업데이트된 대시보드 검토  **분석된 요청**, **플래그가 지정된 요청**, 및 **플래그가 지정된 요청 세부 정보** 패널. 일치하는 CDN 로그 항목의 경우 각 항목의 클라이언트 IP(cli_ip), 호스트, url, 작업(waf_action) 및 규칙 이름(waf_match) 값이 표시되어야 합니다.

  ![ELK 도구 대시보드](./assets/elk-tool-dashboard.png)


### 요청 차단

이 예에서는 의 페이지를 추가하겠습니다. _내부_ 경로의 폴더 `/content/wknd/internal` 배포된 WKND 프로젝트에서 그런 다음 다음을 포함하는 트래픽 필터 규칙을 선언합니다. **트래픽 차단** 조직(예: 회사 VPN)에 일치하는 지정된 IP 주소 이외의 위치에서 하위 페이지를 만드는 경우

고유한 내부 페이지를 만들 수 있습니다(예: `demo-page.html`) 또는 [첨부된 패키지](./assets/demo-internal-pages-package.zip).

- WKND 프로젝트의 다음 규칙을 추가합니다 `/config/cdn.yaml` 파일:

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

- 를 사용하여 AEM 개발 환경에 변경 사항을 배포합니다. [이전에 생성됨](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` cloud Manager의 구성 파이프라인.

- 예를 들어 WKND 사이트의 내부 페이지에 액세스하여 규칙을 테스트합니다 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` 또는 아래의 CURL 명령을 사용합니다.

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 규칙에 사용한 IP 주소와 다른 IP 주소(예: 휴대폰 사용)에서 위의 단계를 반복합니다.

#### 분석 중

의 결과를 분석하려면 `block-internal-paths` 규칙에 설명된 것과 동일한 단계를 따릅니다. [이전 예](#analyzing).

하지만 이번에는 다음을 확인해야 합니다. **차단된 요청** 및 클라이언트 IP(cli_ip), 호스트, URL, 작업(waf_action) 및 규칙 이름(waf_match) 열의 해당 값.

![ELK 도구 대시보드 차단된 요청](./assets/elk-tool-dashboard-blocked.png)


### DoS 공격 방지

예: **doS 공격 방지** ip 주소에서 초당 100개의 요청을 수행하여 5분 동안 요청을 차단합니다.

- 다음 추가 [비율 제한 트래픽 필터 규칙](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) WKND 프로젝트의 `/config/cdn.yaml` 파일.

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
>프로덕션 환경의 경우 웹 보안 팀과 협력하여 다음에 대한 적절한 값을 결정합니다. `rateLimit`,

- 에 언급된 대로 변경 사항 커밋, 푸시 및 배포 [이전 예](#logging-requests).

- DoS 공격을 시뮬레이션하려면 다음을 사용합니다 [Vegeta](https://github.com/tsenart/vegeta) 명령입니다.

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  이 명령은 5초 동안 120개의 요청을 만들고 보고서를 출력합니다. 보시는 것처럼 성공률은 32.5%입니다. 나머지 항목에 대해서는 406 HTTP 응답 코드가 수신되므로 트래픽이 차단되었음을 알 수 있습니다.

  ![베게타 도스 공격](./assets/vegeta-dos-attack.png)

#### 분석 중

의 결과를 분석하려면 `prevent-dos-attacks` 규칙에 설명된 것과 동일한 단계를 따릅니다. [이전 예](#analyzing).

이번에는 많은 항목을 볼 수 있습니다. **차단된 요청** 및 클라이언트 IP(cli_ip), 호스트, url, 작업(waf_action) 및 규칙 이름(waf_match) 열의 해당 값.

![ELK Tool Dashboard DoS 요청](./assets/elk-tool-dashboard-dos.png)

또한 **클라이언트 IP, 국가 및 사용자 에이전트별 상위 100개 공격** 패널에는 규칙 구성을 추가로 최적화하는 데 사용할 수 있는 추가 세부 정보가 표시됩니다.

![ELK Tool Dashboard DoS 상위 100개 요청](./assets/elk-tool-dashboard-dos-top-100.png)

### WAF 규칙

지금까지의 트래픽 필터 규칙 예는 모든 Sites 및 Forms 고객이 구성할 수 있습니다.

다음으로, 향상된 보안 또는 WAF-DDoS 보호 라이센스를 구입한 고객이 더 정교한 공격으로부터 웹 사이트를 보호하기 위해 고급 규칙을 구성할 수 있는 경험에 대해 알아보겠습니다.

계속하기 전에 트래픽 필터 규칙 설명서에 설명된 대로 프로그램에 대해 WAF-DDoS 보호를 활성화합니다 [설정 단계](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup).

#### WAFFlags가 없는 경우

WAF 규칙이 선언되기 전이라도 먼저 경험을 살펴보겠습니다. 프로그램에서 WAF-DDoS가 활성화되면 CDN은 기본적으로 악성 트래픽의 일치를 기록하므로 적절한 규칙을 마련할 수 있는 올바른 정보를 보유하고 있습니다.

먼저 WAF 규칙을 추가하지 않고(또는 를 사용하여) WKND 사이트를 공격합니다. `wafFlags` 속성)을 클릭하여 결과를 분석합니다.

- 공격을 시뮬레이션하려면 [니토](https://github.com/sullo/nikto) 아래 명령입니다. 6분 동안 약 700개의 악의적인 요청을 보냅니다.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Nikto 공격 시뮬레이션](./assets/nikto-attack.png)

  공격 시뮬레이션에 대해 알아보려면 [Nikto - 스캔 조정](https://github.com/sullo/nikto/wiki/Scan-Tuning) 설명서는 포함 또는 제외할 테스트 공격의 유형을 지정하는 방법을 알려줍니다.

##### 분석 중

공격 시뮬레이션의 결과를 분석하려면 다음 단계에 설명된 것과 동일한 단계를 수행합니다. [이전 예](#analyzing).

하지만 이번에는 다음을 확인해야 합니다. **플래그가 지정된 요청** 및 클라이언트 IP(cli_ip), 호스트, url, 작업(waf_action) 및 규칙 이름(waf_match) 열의 해당 값. 이 정보를 사용하면 결과를 분석하고 규칙 구성을 최적화할 수 있습니다.

![ELK 도구 대시보드 WAF 플래그가 지정된 요청](./assets/elk-tool-dashboard-waf-flagged.png)

이(가) **WAF 플래그 분포** 및 **상위 공격** 패널에는 규칙 구성을 추가로 최적화하는 데 사용할 수 있는 추가 세부 정보가 표시됩니다.

![ELK 도구 대시보드 WAF 플래그 공격 요청](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ELK 도구 대시보드 WAF 상위 공격 요청](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### WAFFlags 포함

이제 다음을 포함하는 WAF 규칙을 추가하겠습니다. `wafFlags` 의 일부인 속성 `action` 속성 및 **시뮬레이션된 공격 요청 차단**.

구문 관점에서 WAF 규칙은 이전에 본 규칙과 유사하지만 `action` 속성이 하나 이상을 참조합니다. `wafFlags` 값. 에 대해 자세히 알아보려면 `wafFlags`, 검토 [WAF 플래그 목록](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list) 섹션.

- WKND 프로젝트의 다음 규칙을 추가합니다 `/config/cdn.yaml` 파일. 이(가) `block-waf-flags` 규칙은 시뮬레이션된 악성 트래픽으로 공격받을 때 대시보드 도구에 표시된 일부 wafFlags를 포함합니다. 실제로, 위협 환경이 발전함에 따라, 로그를 분석하여 선언할 새로운 규칙을 결정하는 것은 시간이 지남에 따라 좋은 관례입니다.

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
            - SIGSCI-IP
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

- 에 언급된 대로 변경 사항 커밋, 푸시 및 배포 [이전 예](#logging-requests).

- 공격을 시뮬레이션하려면 동일한 방법을 사용합니다 [니토](https://github.com/sullo/nikto) 이전과 같이 명령합니다.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### 분석 중

에 설명된 것과 동일한 단계를 반복합니다. [이전 예](#analyzing).

이번에는 아래에 항목이 표시됩니다. **차단된 요청** 및 클라이언트 IP(cli_ip), 호스트, url, 작업(waf_action) 및 규칙 이름(waf_match) 열의 해당 값

![ELK 도구 대시보드 WAF 차단된 요청](./assets/elk-tool-dashboard-waf-blocked.png)

또한 **WAF 플래그 분포** 및 **상위 공격** 패널에 추가 세부 정보가 표시됩니다.

![ELK 도구 대시보드 WAF 플래그 공격 요청](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ELK 도구 대시보드 WAF 상위 공격 요청](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### 포괄적인 분석

위의 _분석_ 섹션에서는 대시보드 도구를 사용하여 특정 규칙의 결과를 분석하는 방법에 대해 알아보았습니다. 다음을 포함한 다른 대시보드 패널을 사용하여 분석 결과를 더 탐색할 수 있습니다.


- 분석, 플래그 지정 및 차단된 요청
- 시간 경과에 따른 WAF 플래그 분포
- 시간 경과에 따라 트리거된 트래픽 필터 규칙
- WAF 플래그 ID별 상위 공격
- 상위 트리거된 트래픽 필터
- 클라이언트 IP, 국가 및 사용자 에이전트별 상위 100명의 공격자

![ELK Tool Dashboard 포괄적인 분석](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![ELK Tool Dashboard 포괄적인 분석](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## 다음 단계

권장 사항 숙지 [우수 사례](./best-practices.md) 보안 위반 위험을 줄이려면

## 추가 리소스

[트래픽 필터 규칙 구문](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[CDN 로그 형식](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)
