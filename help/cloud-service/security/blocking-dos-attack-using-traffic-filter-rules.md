---
title: 트래픽 필터 규칙을 사용하여 DoS 및 DDoS 공격 차단
description: AEM as a Cloud Service 제공 CDN에서 트래픽 필터 규칙을 사용하여 DoS 및 DDoS 공격을 차단하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1924'
ht-degree: 100%

---

# 트래픽 필터 규칙을 사용하여 DoS 및 DDoS 공격 차단

AEM as a Cloud Service(AEMCS) 관리 CDN에서 **속도 제한 트래픽 필터** 규칙 및 기타 전략을 사용하여 DoS(서비스 거부) 및 DDoS(분산 서비스 거부) 공격을 차단하는 방법을 알아봅니다. 이러한 공격은 CDN과 잠재적으로 AEM 게시 서비스(Origin이라고도 함)에서 트래픽 스파이크를 유발하여 사이트 응답성과 가용성에 영향을 미칠 수 있습니다.

이 튜토리얼은 _트래픽 패턴을 분석하고 속도 제한 [트래픽 필터 규칙](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)을 구성_&#x200B;하여 이러한 공격을 완화하는 방법에 대한 안내서입니다. 또한 이 튜토리얼에서는 의심되는 공격이 있을 때 알림을 받을 수 있도록 [알림을 구성](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts)하는 방법도 설명합니다.

## 보호 이해

AEM 웹 사이트를 위한 기본 DDoS 보호에 대해 알아보겠습니다.

- **캐싱:** 적절한 캐싱 정책을 사용하면 DDoS 공격의 영향이 제한됩니다. CDN이 대부분의 요청이 Origin으로 전달되는 것을 방지하여 성능 저하를 막기 때문입니다.
- **자동 크기 조정:** AEM 작성자 및 게시 서비스는 트래픽 스파이크에 대응하여 자동으로 확장되지만 갑작스럽고 대규모의 트래픽 스파이크에는 여전히 영향을 받을 수 있습니다.
- **차단:** Adobe CDN은 특정 IP 주소에서 각 CDN PoP(Point of Presence) 기준으로 Adobe에서 정의한 속도를 초과하는 트래픽이 Origin으로 이동하는 경우 해당 트래픽을 차단합니다.
- **경고:** 트래픽이 일정 수준을 초과하면 액션 센터에서 오리진 트래픽 스파이크 경고가 전송됩니다. 이 경고는 특정 CDN PoP에 대한 트래픽이 IP 주소당 _Adobe에서 정의한_ 요청 속도를 초과할 때 실행됩니다. 자세한 내용은 [트래픽 필터 규칙 경고](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts)를 참조하십시오.

이러한 기본 제공 보호 기능은 DDoS 공격으로 인한 성능 영향을 최소화하기 위한 조직의 기준선으로 간주되어야 합니다. 웹 사이트마다 성능 특성이 다르기 때문에 Adobe에서 정의한 속도 제한에 도달하기 전에 성능 저하가 발생할 수 있습니다. 따라서 _고객 구성_&#x200B;을 통해 기본 보호 기능을 확장하는 것이 좋습니다.

고객이 DDoS 공격으로부터 웹 사이트를 보호하기 위해 추가로 수행할 수 있는 권장 조치를 살펴보겠습니다.

- 단일 IP 주소에서 PoP 단위로 특정 속도를 초과하면 트래픽을 차단하도록 **속도 제한 트래픽 필터 규칙**&#x200B;을 선언합니다. 이 임계값은 일반적으로 Adobe가 정의한 속도 제한보다 낮게 설정됩니다.
- “경고 액션”을 통해 속도 제한 트래픽 필터 규칙에 대한 **경고**&#x200B;를 구성하여, 규칙이 트리거되면 액션 센터 알림이 전송되도록 합니다.
- 쿼리 매개변수를 무시하도록 **요청 변환**&#x200B;을 선언하여 캐시 적용 범위를 확대합니다.

### 속도 제한 트래픽 규칙 변형 {#rate-limit-variations}

속도 제한 트래픽 규칙에는 두 가지 변형이 있습니다.

1. Edge - 특정 IP가 각 PoP에서 발생시키는 전체 트래픽의 속도(CDN 캐시에서 제공될 수 있는 요청 포함)에 따라 요청을 차단합니다.
1. 원본 - 특정 IP가 각 PoP에서 Origin으로 향하는 트래픽의 속도에 따라 요청을 차단합니다.

## 고객 여정

아래 단계는 고객이 웹 사이트를 보호하기 위해 거쳐야 할 일반적인 프로세스를 보여 줍니다.

1. 속도 제한 트래픽 필터 규칙의 필요성을 인식합니다. 이는 Adobe에서 원래 경고한 트래픽 스파이크에 대한 결과일 수도 있고, 성공적인 DDoS 공격의 위험을 줄이기 위한 선제적 조치일 수 있습니다.
1. 사이트가 이미 활성화되어 있는 경우 대시보드를 사용하여 트래픽 패턴을 분석하여 속도 제한 트래픽 필터 규칙에 대한 최적 임계값을 결정합니다. 사이트가 아직 공개되지 않았다면 예상 트래픽을 기반으로 값을 설정합니다.
1. 이전 단계의 값을 사용하여 속도 제한 트래픽 필터 규칙을 구성합니다. 임계값에 도달할 때마다 알림을 받을 수 있도록 해당 경고를 활성화하십시오.
1. 트래픽 스파이크가 발생할 때마다 트래픽 필터 규칙 경고를 수신하면 조직이 악의적인 행위자의 표적이 되고 있는지 여부에 대한 귀중한 인사이트를 얻을 수 있습니다.
1. 필요한 경우 경고에 따라 조치를 취하십시오. 트래픽을 분석하여 스파이크가 정상적인 요청인지 공격인지를 분석하여 판단합니다. 트래픽이 정상적이면 임계값을 높이고, 그렇지 않으면 임계값을 낮춥니다.

이 튜토리얼의 나머지 부분에서는 이 절차를 단계별로 안내합니다.

## 규칙 구성의 필요성 인식 {#recognize-the-need}

앞서 언급했듯이 Adobe는 기본적으로 특정 속도를 초과하는 트래픽을 CDN 수준에서 차단하지만 일부 웹 사이트는 그 임계값에 도달하기 전에 성능이 저하될 수 있습니다. 따라서 속도 제한 트래픽 필터 규칙을 구성하는 것이 중요합니다.

이상적으로는 프로덕션에 들어가기 전에 규칙을 구성하는 것이 좋습니다. 그러나 실제로는 많은 조직에서 트래픽 스파이크로 인해 공격이 의심된다는 경고를 받은 후에야 규칙을 선언합니다.

Adobe는 단일 IP 주소로부터 특정 PoP에서 기본 임계값을 초과하는 트래픽이 감지될 경우, [액션 센터 알림](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/operations/actions-center)을 통해 Origin 트래픽 스파이크 경고를 전송합니다. 이러한 경고를 수신한 경우 속도 제한 트래픽 필터 규칙을 구성하는 것이 좋습니다. 이 기본 알림은 트래픽 필터 규칙을 정의할 때 고객이 명시적으로 활성화해야 하는 경고와 다릅니다. 이에 대해서는 이후 섹션에서 알아보도록 하겠습니다.

## 트래픽 패턴 분석 {#analyze-traffic}

사이트가 이미 운영 중이라면 CDN 로그와 Adobe 제공 대시보드를 사용하여 트래픽 패턴을 분석할 수 있습니다.

- **CDN 트래픽 대시보드**: CDN 및 Origin 요청 속도, 4xx 및 5xx 오류율 및 캐시되지 않은 요청을 통해 트래픽에 대한 인사이트를 제공합니다. 또한 클라이언트 IP 주소당 초당 최대 CND 및 Origin 요청 수와 CDN 구성을 최적화하기 위한 추가 인사이트도 제공합니다.

- **CDN 캐시 적중률**: 적중, 통과 및 실패 상태별로 총 캐시 적중률과 총 요청 수에 대한 인사이트를 제공합니다. 상위 적중, 통과 및 실패 URL도 제공합니다.

_다음 옵션 중 하나_&#x200B;를 사용하여 대시보드 도구를 구성하십시오.

### ELK - 대시보드 도구 구성

Adobe에서 제공하는 **Elasticsearch, Logstash 및 Kibana(ELK)** 대시보드 도구를 사용하면 CDN 로그를 분석할 수 있습니다. 이 도구에는 트래픽 패턴을 시각화하는 대시보드가 포함되어 있어 속도 제한 트래픽 필터 규칙에 대한 최적 임계값을 쉽게 결정할 수 있습니다.

- [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) GitHub 저장소를 복제합니다.
- [ELK Docker 컨테이너 설정 방법](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container) 단계에 따라 도구를 설정합니다.
- 설정의 일부로 `traffic-filter-rules-analysis-dashboard.ndjson` 파일을 가져와 데이터를 시각화합니다. _CDN 트래픽_ 대시보드에는 CDN Edge 및 Origin에서 IP/POP당 최대 요청 수를 보여 주는 시각화가 포함되어 있습니다.
- [Cloud Manager](https://my.cloudmanager.adobe.com/)의 _환경_ 카드에서 AEMCS 게시 서비스의 CDN 로그를 다운로드합니다.

  ![Cloud Manager CDN 로그 다운로드](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 새로운 요청이 CDN 로그에 나타나기까지 최대 5분 정도 소요될 수 있습니다.

### Splunk - 대시보드 도구 구성

[활성화된 Splunk 로그 전달](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs)이 있는 고객은 새 대시보드를 만들어 트래픽 패턴을 분석할 수 있습니다.

Splunk에서 대시보드를 만들려면 [AEMCS CDN 로그 분석을 위한 Splunk 대시보드](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis) 단계를 따르십시오.

### 데이터 살펴보기

다음은 ELK 및 Splunk 대시보드에서 사용할 수 있는 시각화 기능입니다.

- **클라이언트 IP 및 POP당 Edge RPS**: 이 시각화는 **CDN Edge에서** IP/POP당 최대 요청 수를 보여 줍니다. 시각화에서 피크는 최대 요청 수를 나타냅니다.

  **ELK 대시보드**:
  ![ELK 대시보드 - IP/POP당 최대 요청 수](./assets/elk-edge-max-per-ip-pop.png)

  **Splunk 대시보드**:
  ![Splunk 대시보드 - IP/POP당 최대 요청 수](./assets/splunk-edge-max-per-ip-pop.png)

- **클라이언트 IP 및 POP당 Origin RPS**: 이 시각화는 **CDN Origin**&#x200B;에서 IP/POP당 최대 요청 수를 보여 줍니다. 시각화에서 피크는 최대 요청 수를 나타냅니다.

  **ELK 대시보드**:
  ![ELK 대시보드 - IP/POP당 최대 Origin 요청 수](./assets/elk-origin-max-per-ip-pop.png)

  **Splunk 대시보드**:
  ![Splunk 대시보드 - IP/POP당 최대 Origin 요청 수](./assets/splunk-origin-max-per-ip-pop.png)

## 임계값 선택

속도 제한 트래픽 필터 규칙에 대한 임계값은 위의 분석을 기반으로 해야 하며, 정상적인 트래픽이 차단되지 않도록 해야 합니다. 아래 표에서는 임계값을 설정하는 데 도움이 되는 지침을 제공합니다.

| 변형 | 값 |
| :--------- | :------- |
| Origin | **일반적인** 트래픽 조건(즉, DDoS 발생 시 속도가 아님)에서의 IP/POP당 Origin 최대 요청 수를 기준으로 하여, 여기에 배수를 곱해 증가시킵니다. |
| Edge | **일반적인** 트래픽 조건(즉, DDoS 발생 시 속도가 아님)에서의 IP/POP당 Edge 최대 요청 수를 기준으로 하여, 여기에 배수를 곱해 증가시킵니다. |

사용할 배수는 유기적 트래픽, 캠페인 및 기타 이벤트로 인한 정상적인 트래픽 스파이크를 고려하여 설정해야 합니다. 5~10배 정도가 적당할 수 있습니다.

사이트가 아직 공개되지 않았다면 분석할 데이터가 없으므로, 경험적 추정을 바탕으로 속도 제한 트래픽 필터 규칙에 적절한 값을 설정해야 합니다. 예:

| 변형 | 값 |
|------------------------------ |:-----------:|
| Edge | 500 |
| Origin | 100 |

## 규칙 구성 {#configure-rules}

위에서 논의한 내용을 기반으로 값을 지정하여 AEM 프로젝트의 `/config/cdn.yaml` 파일에서 **속도 제한 트래픽 필터** 규칙을 구성합니다. 필요한 경우 웹 보안 팀에 문의하여 속도 제한 값이 적절한지, 정상적인 트래픽을 차단하지 않는지 확인하십시오.

자세한 내용은 [AEM 프로젝트에서 규칙 만들기](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project)를 참조하십시오.

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
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
```

Origin 및 Edge 규칙이 모두 선언되어 있으며, 경고 속성이 `true`로 설정되어 있어 임계값에 도달할 경우(공격일 가능성이 높음) 경고를 수신할 수 있습니다.

처음에는 액션 유형을 기록으로 설정하는 것이 좋습니다. 이렇게 하면 몇 시간 또는 며칠 동안 트래픽을 모니터링하여 정상적인 트래픽이 이러한 속도를 초과하지 않는지 확인할 수 있습니다. 며칠 후에는 차단 모드로 전환하십시오.

AEMCS 환경에 변경 사항을 배포하려면 아래 단계를 따릅니다.

- 위의 변경 사항을 Cloud Manager Git 저장소에 커밋하고 푸시합니다.
- Cloud Manager의 구성 파이프라인을 사용하여 AEMCS 환경에 변경 사항을 배포합니다. 자세한 내용은 [Cloud Manager를 통해 규칙 배포](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)를 참조하십시오.
- **속도 제한 트래픽 필터 규칙**&#x200B;이 예상대로 작동하는지 확인하려면 [공격 시뮬레이션](#attack-simulation) 섹션에 설명된 대로 공격을 시뮬레이션할 수 있습니다. 규칙에 설정된 속도 제한 값보다 높은 값으로 요청 수를 제한하십시오.

### 요청 변환 규칙 구성 {#configure-request-transform-rules}

속도 제한 트래픽 필터 규칙 외에도 [요청 변환](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations)을 사용하여 애플리케이션에 필요하지 않은 쿼리 매개변수를 설정 해제하여 캐시 무효화 기술을 통해 캐시를 우회하는 방법을 최소화하는 것이 좋습니다. 예를 들어 `search` 및 `campaignId` 쿼리 매개변수만 허용하려는 경우 다음 규칙을 선언할 수 있습니다.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  requestTransformations:
    rules:
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## 트래픽 필터 규칙 경고 수신 {#receiving-alerts}

위에서 언급한 대로, 트래픽 필터 규칙에 *alert: true*&#x200B;가 포함되어 있으면 규칙이 일치할 때 경고를 받게 됩니다.

## 경고에 대응하기 {#acting-on-alerts}

때때로 경고는 단순 정보 제공용으로, 공격 빈도를 파악하는 데 도움을 줍니다. 위에서 설명한 대시보드를 활용해 CDN 데이터를 분석하여 트래픽 스파이크가 정상적인 트래픽 양의 증가인지, 아니면 공격으로 인한 것인지 확인하는 것이 좋습니다. 후자에 해당하는 경우 임계값을 높이는 것을 고려하십시오.

## 공격 시뮬레이션{#attack-simulation}

이 섹션에서는 DoS 공격을 시뮬레이션하는 방법을 설명합니다. 이 방법을 사용하면 이 튜토리얼에서 사용되는 대시보드에 대한 데이터를 생성하고 구성된 모든 규칙이 공격을 성공적으로 차단하는지 검증할 수 있습니다.

>[!CAUTION]
>
> 프로덕션 환경에서는 이 단계를 수행하지 마십시오. 다음 단계는 시뮬레이션 목적으로만 사용해야 합니다.
>
>트래픽 스파이크 경고를 수신한 경우 [트래픽 패턴 분석](#analyzing-traffic-patterns) 섹션으로 진행하십시오.

공격을 시뮬레이션하려면 [Apache Benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html), [Apache JMeter](https://jmeter.apache.org/), [Vegeta](https://github.com/tsenart/vegeta) 등의 도구를 사용할 수 있습니다.

### Edge 요청

다음 [Vegeta](https://github.com/tsenart/vegeta) 명령을 사용하면 웹 사이트에 여러 요청을 보낼 수 있습니다.

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=60s | vegeta report
```

위 명령은 5초 동안 120개의 요청을 보내고 보고서를 출력합니다. 웹 사이트에 속도 제한이 없다면 트래픽 스파이크가 발생할 수 있습니다.

### Origin 요청

CDN 캐시를 우회하고 Origin(AEM 게시 서비스)에 요청을 보내려면 URL에 고유한 쿼리 매개변수를 추가할 수 있습니다. [JMeter 스크립트를 사용하여 DoS 공격 시뮬레이션](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)에서 샘플 Apache JMeter 스크립트를 참조하십시오.

