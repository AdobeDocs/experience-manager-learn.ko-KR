---
title: 스마트 이미징
description: Dynamic Media Classic의 스마트 이미징은 클라이언트 브라우저 기능을 기반으로 이미지 형식과 품질을 자동으로 최적화하여 이미지 전달 성능을 향상시킵니다. Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정을 사용하여 작업할 수 있습니다. 스마트 이미징에 대한 자세한 내용과 신속한 페이지 로드를 통해 고객 경험을 향상시키는 방법을 살펴볼 수 있습니다.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 3%

---


# 스마트 이미징 {#smart-imaging}

웹 사이트나 모바일 사이트 또는 앱에서 고객 경험의 가장 중요한 측면 중 하나는 페이지 로드 시간입니다. 페이지를 로드하는 데 시간이 너무 오래 걸리면 고객이 사이트 또는 앱을 포기하는 경우가 많습니다. 이미지는 페이지 로드 시간의 대부분을 구성합니다. Dynamic Media Classic의 스마트 이미징은 클라이언트 브라우저 기능을 기반으로 이미지 형식과 품질을 자동으로 최적화하여 이미지 전달 성능을 향상시킵니다. Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정을 사용하여 작업할 수 있습니다. 스마트 이미징은 이미지 크기를 30% 이상 줄임으로써 페이지 로드 시간을 단축하고 고객 경험을 향상시킬 수 있습니다.

스마트 이미징은 또한 Adobe의 최고급 프리미엄 서비스와 완벽하게 통합되는 향상된 성능을 제공합니다. 이 서비스는 서버의 기본 경로보다 지연 시간이 가장 짧은 서버, 네트워크 및 피어링 지점 간의 최적의 인터넷 경로를 찾아냅니다.

[스마트 이미징](https://docs.adobe.com/content/help/ko-KR/experience-manager-64/assets/dynamic/imaging-faq.html)에 대해 자세히 알아보십시오.

## 스마트 이미징 이점

이미지는 페이지 로드 시간의 대부분을 구성하므로 Smart Imaging의 성능 향상은 높은 전환, 사이트에서 보낸 시간, 낮은 사이트 이탈률 등 비즈니스 KPI에 큰 영향을 줄 수 있습니다.

![이미지](assets/smart-imaging/smart-imaging-1.png)

## 스마트 이미징 작동 방식

앞서 언급했듯이 스마트 이미징은 Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정과 함께 이미지를 시각적 품질을 그대로 유지하면서 WebP와 같은 최적의 차세대 이미지 포맷으로 자동으로 변환할 수 있습니다.

지원되는 이미지 형식(및 해당 형식을 사용하지 않는 경우 어떻게 발생합니까?) 및 사용 중인 기존 이미지 사전 설정에 미치는 영향을 비롯하여 [스마트 이미징 작동 방식](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)에 대해 자세히 알아보십시오.

## 스마트 이미징의 영향

스마트 이미징을 활용하려면 사이트의 URL, 이미지 사전 설정 및 코드를 변경해야 할 수 있습니다. 스마트 이미징 사용을 위한 사전 요구 사항을 충족하고 지원되는 JPEG 및 PNG 이미지 형식의 이미지만 사용하는 경우 변경할 필요가 없습니다.

스마트 이미징은 HTTP, HTTPS 및 HTTP/2를 통해 제공되는 이미지와 연동됩니다.

>[!NOTE]
>
>스마트 이미징으로 이동하면 CDN의 캐시를 지웁니다. CDN의 캐시는 일반적으로 1 또는 2일 이내에 다시 빌드됩니다.

스마트 이미징은 기존 Dynamic Media Classic 라이선스에 포함되어 있습니다. 이 기능에 대한 추가 비용은 없습니다. 이를 활용하려면 다음 두 가지 요구 사항을 충족해야 합니다.Adobe 번들로 묶은 CDN과 전용 도메인을 보유합니다. 그러면 계정이 자동으로 활성화되지 않으므로 계정에 대해 활성화해야 합니다.

스마트 이미징 활성화는 기술 지원 요청을 보낼 때 |지원 사례 만들기| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). Smart Imaging과 연결할 사용자 정의 도메인을 설정하는 기능이 지원됩니다. 캐싱(Time To Live) 또는 TTL(TTL)과 관련된 하나의 매개 변수를 변경하고 지원을 통해 캐시를 지웁니다. 프로덕션으로 푸시하기 전에 원할 경우 선택적 스테이징 단계를 수행할 수도 있습니다. 스마트 이미징이 켜져 있으면 고객이 요청한 동일한 품질로 더 작은 크기의 이미지를 제공할 수 있습니다. 즉, Adobe Sensei에서 가장 효율적인 크기를 선택할 수 있으므로 페이지 로드 시간이 단축되고 이 모든 작업이 자동으로 수행됩니다.

스마트 이미징을 활성화하면 예상대로 작동하는지 확인합니다.

스마트 이미징에 대한 추가 질문이 있을 수 있습니다. 질문과 답변(FAQ) 목록을 정리했습니다. [FAQ](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)을 참조하십시오.

## 추가 리소스

스마트 이미징에 대한 자세한 내용을 보려면 [Dynamic Media Classic Optimizing Page Performance Skily Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) on-demand 웨비나를 참조하십시오.
