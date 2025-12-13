---
title: Adobe CDN - 캐싱을 넘어선 고급 기능
description: CDN에서 트래픽 구성, 토큰 및 자격 증명 설정, CDN 오류 페이지 등과 같은 캐싱을 넘어선 Adobe CDN의 고급 기능에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# Adobe CDN - 캐싱을 넘어선 고급 기능

CDN(Adobe Content Delivery Network)에서 트래픽 구성, 토큰 및 자격 증명 설정, CDN 오류 페이지 등과 같은 캐싱 이상의 고급 기능에 대해 알아봅니다.

Adobe CDN은 콘텐츠 캐싱 외에 웹 사이트 성능을 최적화하는 데 도움이 되는 몇 가지 고급 기능을 제공합니다. 이러한 기능에는 다음이 포함됩니다.

- CDN에서 트래픽 구성
- CDN 자격 증명 및 인증 구성
- CDN 오류 페이지

이러한 기능은 **셀프 서비스** 기능입니다. AEM 프로젝트의 `cdn.yaml` 파일에서 구성되고 Cloud Manager 구성 파이프라인을 사용하여 배포됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/3433104?quality=12&learn=on)

## CDN에서 트래픽 구성

_CDN에서 트래픽 구성_&#x200B;과 관련된 주요 기능에 대해 알아보겠습니다.

- **DoS 공격 방지:** Adobe CDN은 네트워크 계층에서 DoS 공격을 흡수하여 원본 서버에 도달하지 못하도록 합니다.
- **속도 제한:** 원본 서버에 너무 많은 요청이 포함되는 것을 방지하기 위해 CDN에서 속도 제한을 구성할 수 있습니다.
- **웹 응용 프로그램 방화벽(WAF):** WAF은 SQL 삽입, 사이트 간 스크립팅 등과 같은 일반적인 웹 응용 프로그램 취약점으로부터 웹 사이트를 보호합니다. 이 기능을 사용하려면 Enhanced Security 라이센스 또는 WAF-DDoS Protection 라이센스가 필요합니다.
- **요청 변환:** 헤더 설정 또는 설정 해제, 쿼리 매개 변수, 쿠키 수정 등과 같은 수신 요청을 수정합니다.
- **응답 변환:** 헤더를 설정하거나 해제하는 등의 보내는 응답을 수정합니다.
- **원본 선택:** 요청 URL을 기반으로 다른 원본 서버(Adobe 및 Adobe 이외)로 트래픽을 라우팅합니다.
- **URL 리디렉션:** 다른 절대 또는 상대 URL로 리디렉션 요청(HTTP 301/302)을 리디렉션합니다.

## CDN 자격 증명 및 인증 구성

_CDN 자격 증명 및 인증 구성_&#x200B;과 관련된 주요 기능에 대해 알아보겠습니다.

- **API 토큰 제거**: 캐시에서 단일 또는 그룹 또는 모든 리소스를 제거하기 위한 고유한 제거 키를 만들 수 있습니다.
- **기본 인증**: 웹 사이트 또는 웹 사이트의 일부에 대한 액세스를 제한하려는 경우 간단한 인증 메커니즘입니다. 주로 라이브로 전환되기 전에 다양한 검토 프로세스의 일부로 필요합니다.
- **HTTP 헤더 유효성 검사**: 고객 관리 CDN이 트래픽을 Adobe CDN으로 라우팅하는 경우 사용됩니다. Adobe CDN이 `X-AEM-Edge-Key` 헤더 값을 기반으로 들어오는 요청의 유효성을 검사합니다. `X-AEM-Edge-Key` 헤더에 대해 고유한 값을 만들 수 있습니다.

## CDN 오류 페이지

_CDN 오류 페이지_&#x200B;와 관련된 주요 기능에 대해 알아보겠습니다.

- **브랜드 오류 페이지**: Adobe CDN이 원본 서버에 연결할 수 없는 경우 _가능성 없는 시나리오_&#x200B;에서 사용자에게 브랜드 오류 페이지를 표시합니다.

## 구현 방법

이러한 고급 기능의 구현에는 다음 두 단계가 포함됩니다.

1. **CDN 구성 파일 업데이트**: 필요한 구성으로 AEM 프로젝트의 `cdn.yaml` 파일을 업데이트합니다. 구성은 규칙으로 추가되며 규칙 구문을 따릅니다. 규칙 3개의 기본 구성 요소: `name`, `when` 및 `action`.

2. **CDN 구성 파일 배포**: Cloud Manager 구성 파이프라인을 사용하여 업데이트된 `cdn.yaml` 파일을 배포합니다. 자세한 내용은 [Cloud Manager을 통해 규칙 배포](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)를 참조하십시오.

### 예

아래 예제에서는 샘플 WKND 사이트가 `/top3` URL을 `/us/en/top3.html`(으)로 리디렉션하도록 구성되었습니다.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## 관련 자습서

[트래픽 필터 규칙으로 웹 사이트 보호](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[HTTP 헤더 유효성 검사 CDN 규칙 구성 및 배포](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[CDN 캐시를 제거하는 방법](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[CDN 오류 페이지 구성](https://experienceleague.adobe.com/ko/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[CDN에서 트래픽 구성](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[CDN 자격 증명 및 인증 구성](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

