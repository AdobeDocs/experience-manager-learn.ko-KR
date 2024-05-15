---
title: WAF 규칙을 포함한 트래픽 필터 규칙에 대한 우수 사례
description: WAF 규칙을 포함한 트래픽 필터 규칙에 대한 권장 모범 사례를 알아봅니다.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 170
source-git-commit: c7c78ca56c1d72f13d2dc80229a10704ab0f14ab
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# WAF 규칙을 포함한 트래픽 필터 규칙에 대한 우수 사례

WAF 규칙을 포함한 트래픽 필터 규칙에 대한 권장 모범 사례를 알아봅니다. 이 문서에 설명된 우수 사례는 완벽하지 않으며, 자체 보안 정책 및 절차를 대체하기 위한 것이 아닙니다.

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## 일반 모범 사례

- 조직에 적합한 규칙을 결정하려면 보안 팀과 공동 작업하십시오.
- 스테이지 및 프로덕션 환경에 배포하기 전에 항상 개발 환경에서 규칙을 테스트하십시오.
- 규칙을 선언하고 유효성을 검사할 때 항상 다음으로 시작 `action` 유형 `log` 규칙이 합법적인 트래픽을 차단하지 않는지 확인하기 위해.
- 특정 규칙의 경우 다음 항목에서 전환 `log` 끝 `block` 은 충분한 사이트 트래픽에 대한 분석을 기반으로 해야 합니다.
- 규칙을 점진적으로 도입하고 프로세스에 테스트 팀(QA, 성능, 침투 테스트)을 포함시키는 것을 고려합니다.
- 다음을 사용하여 규칙에서 발생하는 영향을 정기적으로 분석합니다. [대시보드 도구](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). 사이트의 트래픽 양에 따라 분석은 매일, 매주 또는 매월 수행할 수 있습니다.
- 분석 후에 알게 될 수 있는 악성 트래픽을 차단하려면 추가 규칙을 추가합니다. 예를 들어 사이트를 공격하는 특정 IP가 있습니다.
- 규칙 작성, 배포 및 분석은 지속적이고 반복적인 프로세스여야 합니다. 일회성 활동이 아닙니다.

## 트래픽 필터 규칙에 대한 우수 사례

AEM 프로젝트에 대해 아래의 트래픽 필터 규칙을 활성화합니다. 그러나 원하는 값 `rateLimit` 및 `clientCountry` 속성은 보안 팀과 협력하여 결정해야 합니다.

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
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
              - reqProperty: clientCountry
                in:
                  - SY
                  - BY
                  - MM
                  - KP
                  - IQ
                  - CD
                  - SD
                  - IR
                  - LR
                  - ZW
                  - CU
                  - CI
```

>[!WARNING]
>
>프로덕션 환경의 경우 웹 보안 팀과 협력하여 다음에 대한 적절한 값을 결정합니다. `rateLimit`

## WAF 규칙 모범 사례

프로그램에 대해 WAF에 라이센스가 부여되고 활성화되면 규칙에 선언하지 않았더라도 일치하는 트래픽 WAF 플래그가 차트 및 요청 로그에 표시됩니다. 따라서 항상 잠재적으로 새로운 악성 트래픽을 알고 있으며 필요에 따라 규칙을 만들 수 있습니다. 선언된 규칙에 반영되지 않은 WAF 플래그를 보고 선언하는 것이 좋습니다.

AEM 프로젝트에 대한 아래 WAF 규칙을 고려하십시오. 그러나 원하는 값 `action` 및 `wafFlags` 속성은 보안 팀과 협력하여 결정해야 합니다.

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

    # Traffic Filter rules shown in above section
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
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## 요약

결론적으로, 이 자습서에서는 Adobe Experience Manager as a Cloud Service(AEMCS)에서 웹 애플리케이션의 보안을 강화하는 데 필요한 지식과 도구를 갖추고 있습니다. 실제 규칙 예제와 결과 분석에 대한 통찰력을 통해 웹 사이트 및 애플리케이션을 효과적으로 보호할 수 있습니다.



