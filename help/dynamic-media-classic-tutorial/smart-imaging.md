---
title: 스마트 이미징
description: Dynamic Media Classic의 스마트 이미징은 클라이언트 브라우저 기능을 기반으로 이미지 형식과 품질을 자동으로 최적화하여 이미지 전달 성능을 향상시킵니다. Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정을 사용하여 작업하여 이렇게 합니다. 스마트 이미징에 대해 자세히 알아보고 더 빠른 페이지 로드를 통해 더 나은 고객 경험을 제공하는 데 스마트 이미징을 사용하는 방법을 알아봅니다.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: 컨텐츠 관리
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 3%

---


# 스마트 이미징 {#smart-imaging}

웹 사이트나 모바일 사이트 또는 앱에서 고객 경험의 가장 중요한 측면 중 하나는 페이지 로드 시간입니다. 페이지를 로드하는 데 시간이 너무 오래 걸리는 경우 고객이 종종 사이트 또는 앱을 포기하는 경우가 있습니다. 이미지가 페이지 로드 시간의 대부분을 구성합니다. Dynamic Media Classic의 스마트 이미징은 클라이언트 브라우저 기능을 기반으로 이미지 형식과 품질을 자동으로 최적화하여 이미지 전달 성능을 향상시킵니다. Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정을 사용하여 작업하여 이렇게 합니다. 스마트 이미징은 이미지 크기를 30% 이상 줄임으로써 페이지 로드 속도가 빨라지고 고객 경험이 향상됩니다.

또한 스마트 이미징은 Adobe의 동급 최고의 프리미엄 서비스와 완전히 통합될 수 있는 향상된 성능 증대의 이점을 제공합니다. 이 서비스는 지연 시간이 가장 짧은 서버, 네트워크 및 피어링 지점 간의 최적의 인터넷 경로를 찾아 인터넷 상의 기본 경로보다 패킷 손실률을 찾습니다.

[스마트 이미징](https://docs.adobe.com/content/help/ko-KR/experience-manager-64/assets/dynamic/imaging-faq.html)에 대해 자세히 알아보십시오.

## 스마트 이미징의 이점

이미지가 페이지 로드 시간의 대부분을 차지하므로 Smart Imaging의 성능 향상은 높은 전환, 사이트에서 보낸 시간 및 낮은 사이트 바운스 비율 등 비즈니스 KPI에 큰 영향을 줄 수 있습니다.

![이미지](assets/smart-imaging/smart-imaging-1.png)

## 스마트 이미징 작동 방식

앞에서 설명한 바와 같이, 스마트 이미징은 Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정과 함께 시각적 품질을 유지하면서 이미지를 WebP와 같은 최적의 차세대 이미지 형식으로 자동으로 변환합니다.

지원되는 이미지 형식(및 이러한 형식을 사용하지 않을 경우 발생하는 내용)과 사용 중인 기존 이미지 사전 설정에 미치는 영향을 포함하여 [스마트 이미징 작동 방식](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)에 대해 자세히 알아보십시오.

## 스마트 이미징의 영향

스마트 이미징을 활용하려면 사이트에서 URL, 이미지 사전 설정 및 코드를 변경해야 할 것 같습니다. 스마트 이미징을 사용하기 위한 사전 요구 사항을 충족하고 지원되는 JPEG 및 PNG 이미지 형식의 이미지만 작업하는 경우 변경할 필요가 없습니다.

스마트 이미징은 HTTP, HTTPS 및 HTTP/2를 통해 전달된 이미지에서 작동합니다.

>[!NOTE]
>
>Smart Imaging으로 전환하면 CDN에서 캐시가 지워집니다. CDN의 캐시는 일반적으로 1~2일 내에 다시 빌드됩니다.

스마트 이미징은 Dynamic Media Classic의 기존 라이선스에 포함되어 있습니다. 이 기능에 대한 추가 비용은 없습니다. 이를 활용하려면 다음 두 가지 요구 사항을 충족해야 합니다.는 Adobe 번들 CDN 및 전용 도메인을 갖습니다. 그러면 자동으로 활성화되지 않으므로 계정에 대해 활성화해야 합니다.

스마트 이미징을 활성화하는 것은 기술 지원 요청을 보내는 것부터 시작합니다 |지원 사례 만들기| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 스마트 이미징과 연결할 사용자 지정 도메인을 설정하는 지원이 사용자와 함께 작동합니다. 캐싱과 관련된 하나의 매개 변수(Time To Live 또는 TTL)를 변경하고 지원이 캐시를 지웁니다. 프로덕션에 푸시하기 전에 원하는 경우 선택적 스테이징 단계를 수행할 수도 있습니다. 그런 다음 스마트 이미징을 켜면 고객이 요청한 것과 동일한 품질로 더 작은 크기의 이미지를 고객에게 제공할 수 있습니다. 즉, 페이지 로드 시간이 빨라지고 Adobe Sensei이 가장 효율적인 크기를 선택하는 데 도움이 되므로 이 모든 작업이 자동으로 수행됩니다.

스마트 이미징을 활성화하면 예상대로 작동하는지 확인할 수 있습니다.

스마트 이미징에 대한 추가 질문이 있을 수 있습니다. 질문과 대답(FAQ) 목록을 정리했습니다. [FAQ](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)를 참조하십시오.

## 추가 리소스

스마트 이미징에 대한 자세한 내용은 [Dynamic Media Classic 페이지 성능 최적화 Skill Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) 온디맨드 웨비나를 참조하십시오.
