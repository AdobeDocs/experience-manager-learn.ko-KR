---
title: WAF 규칙을 포함한 트래픽 필터 규칙에 대한 모범 사례
description: 보안을 강화하고 위험을 완화하기 위해 AEM as a Cloud Service에서 WAF 규칙을 포함한 트래픽 필터 규칙을 구성하기 위한 권장 모범 사례에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# WAF 규칙을 포함한 트래픽 필터 규칙에 대한 모범 사례

보안을 강화하고 위험을 완화하기 위해 AEM as a Cloud Service에서 WAF 규칙을 포함한 트래픽 필터 규칙을 구성하기 위한 권장 모범 사례에 대해 알아봅니다.

>[!IMPORTANT]
>
>이 문서에 설명된 우수 사례는 완벽하지 않으며, 자체 보안 정책 및 절차를 대체하기 위한 것이 아닙니다.

## 일반적인 모범 사례

- Adobe에서 제공하는 표준 트래픽 필터 및 WAF 규칙의 [권장 집합](./overview.md#adobe-recommended-rules)(으)로 시작하여 애플리케이션의 특정 요구 사항 및 위협 환경에 따라 수정하십시오.
- 보안 팀과 협력하여 조직의 보안 태도 및 규정 준수 요구 사항에 부합하는 규칙을 결정합니다.
- 개발 환경에서 새로 추가되거나 업데이트된 규칙을 스테이지 및 프로덕션으로 승격하기 전에 항상 테스트하십시오.
- 규칙을 선언하고 유효성을 검사할 때 올바른 트래픽을 차단하지 않고 동작을 관찰하려면 `action` 형식 `log`(으)로 시작하십시오.
- 충분한 트래픽 데이터를 분석하고 유효한 요청이 영향을 받지 않는지 확인한 후에만 `log`에서 `block`(으)로 이동하십시오.
- 의도하지 않은 부작용을 식별하기 위해 QA, 성능 및 보안 테스트 팀이 관여하는 규칙을 점진적으로 도입합니다.
- [대시보드 도구](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling)를 사용하여 규칙 효과를 정기적으로 검토하고 분석합니다. 리뷰 빈도(일별, 주별, 월별)는 사이트의 트래픽 양 및 위험 프로필과 일치해야 합니다.
- 새로운 위협 인텔리전스, 트래픽 행동 및 감사 결과를 기반으로 규칙을 지속적으로 세분화합니다.

## 트래픽 필터 규칙에 대한 모범 사례

- 에지, 원본 보호 및 OFAC 기반 제한에 대한 규칙이 포함된 Adobe [권장 표준 트래픽 필터 규칙](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)을(를) 기준으로 사용합니다.
- 경고 및 로그를 정기적으로 검토하여 남용 또는 잘못된 구성의 패턴을 식별하십시오.
- 애플리케이션의 트래픽 패턴 및 사용자 동작에 따라 속도 제한에 대한 임계값을 조정합니다.

  아래 표에서는 임계값을 설정하는 데 도움이 되는 지침을 제공합니다.

  | 변형 | 값 |
  | :--------- | :------- |
  | Origin | **일반적인** 트래픽 조건(즉, DDoS 발생 시 속도가 아님)에서의 IP/POP당 Origin 최대 요청 수를 기준으로 하여, 여기에 배수를 곱해 증가시킵니다. |
  | Edge | **일반적인** 트래픽 조건(즉, DDoS 발생 시 속도가 아님)에서의 IP/POP당 Edge 최대 요청 수를 기준으로 하여, 여기에 배수를 곱해 증가시킵니다. |

  자세한 내용은 [임계값 선택](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) 섹션도 참조하십시오.

- `block` 작업이 올바른 트래픽에 영향을 주지 않는지 확인한 후에만 `log` 작업으로 이동하십시오.

## WAF 규칙에 대한 모범 사례

- 알려진 잘못된 IP를 차단하고, DDoS 공격을 탐지하고, 봇 남용을 완화하는 규칙을 포함하는 Adobe [권장 WAF 규칙](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)으로 시작합니다.
- `ATTACK` WAF 플래그는 잠재적인 위협을 경고해야 합니다. `block`(으)로 이동하기 전에 긍정 오류(false positive)가 없는지 확인하십시오.
- 권장 WAF 규칙이 특정 위협을 다루지 않는 경우 애플리케이션의 고유 요구 사항을 기반으로 사용자 지정 규칙을 만드는 것이 좋습니다. 설명서에서 [WAF 플래그](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)의 전체 목록을 확인하십시오.

## 규칙 구현

AEM as a Cloud Service에서 트래픽 필터 규칙 및 WAF 규칙을 구현하는 방법을 알아봅니다.

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="표준 트래픽 필터 규칙을 사용하여 AEM 웹 사이트 보호" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="표준 트래픽 필터 규칙을 사용하여 AEM 웹 사이트 보호"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="표준 트래픽 필터 규칙을 사용하여 AEM 웹 사이트 보호">표준 트래픽 필터 규칙을 사용하여 AEM 웹 사이트 보호</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 Adobe 권장 표준 트래픽 필터 규칙을 사용하여 DoS, DDoS 및 봇 남용으로부터 AEM 웹 사이트를 보호하는 방법을 알아봅니다.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">규칙 적용</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="WAF 규칙을 사용하여 AEM 웹 사이트 보호" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="WAF 규칙을 사용하여 AEM 웹 사이트 보호"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="WAF 규칙을 사용하여 AEM 웹 사이트 보호">WAF 규칙을 사용하여 AEM 웹 사이트 보호</a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service에서 Adobe 권장 웹 애플리케이션 방화벽(WAF) 규칙을 사용하여 DoS, DDoS 및 봇 남용을 비롯한 정교한 위협으로부터 AEM 웹 사이트를 보호하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF 활성화</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 추가 리소스

- [WAF 규칙을 포함하는 트래픽 필터 규칙](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [AEM의 DoS/DDoS 방지 이해](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [트래픽 필터 규칙을 사용하여 DoS 및 DDoS 공격 차단](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

