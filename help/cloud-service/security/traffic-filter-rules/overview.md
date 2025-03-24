---
title: 트래픽 필터 규칙(WAF 규칙 포함)으로 웹 사이트 보호
description: 웹 애플리케이션 방화벽(WAF) 규칙의 하위 범주를 포함하여 트래픽 필터 규칙에 대해 알아봅니다. 규칙을 만들고, 배포하고, 테스트하는 방법. 또한 결과를 분석하여 AEM 사이트를 보호하십시오.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 165
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 13%

---

# 트래픽 필터 규칙(WAF 규칙 포함)으로 웹 사이트 보호

AEMCS(AEM as a Cloud Service)의 **웹 응용 프로그램 방화벽(WAF) 규칙**&#x200B;의 하위 범주를 포함하여 **트래픽 필터 규칙**&#x200B;에 대해 알아봅니다. 규칙을 만들고, 배포하고, 테스트하는 방법에 대해 알아보십시오. 또한 결과를 분석하여 AEM 사이트를 보호하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## 개요

보안 위반 위험을 줄이는 것이 모든 조직의 최우선 과제입니다. AEMCS는 웹 사이트 및 애플리케이션을 보호하기 위해 WAF 규칙을 포함한 트래픽 필터 규칙 기능을 제공합니다.

트래픽 필터 규칙은 [기본 제공 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)에 배포되며 요청이 AEM 인프라에 도달하기 전에 평가됩니다. 이 기능을 사용하면 웹 사이트의 보안을 크게 강화하여 합법적인 요청만 AEM 인프라에 액세스할 수 있도록 할 수 있습니다.

이 튜토리얼에서는 WAF 규칙을 포함한 트래픽 필터 규칙의 생성, 배포, 테스트 및 결과 분석 과정을 안내합니다.

[이 문서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en)에서 트래픽 필터 규칙에 대해 자세히 알아볼 수 있습니다.

>[!IMPORTANT]
>
> &quot;WAF 규칙&quot;이라는 트래픽 필터 규칙의 하위 카테고리는 WAF-DDoS Protection 또는 Enhanced Security 라이센스가 필요합니다.

**aemcs-waf-adopter@adobe.com**&#x200B;으로 메일을 보내어 트래픽 필터 규칙에 대한 피드백을 제공하거나 질문을 보내 주시기 바랍니다.

## 다음 단계

트래픽 필터 규칙을 만들고 배포하고 테스트할 수 있도록 기능을 [설정하는 방법](./how-to-setup.md)을 알아봅니다. AEMCS CDN 로그 결과를 분석하기 위해 **Elasticsearch, Logstash 및 ELK(Kibana)** 스택 대시보드 도구 설정에 대해 읽어 보십시오.


