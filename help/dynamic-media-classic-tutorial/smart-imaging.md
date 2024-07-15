---
title: 스마트 이미징
description: Dynamic Media Classic의 스마트 이미징은 클라이언트 브라우저 기능을 기반으로 이미지 형식 및 품질을 자동으로 최적화하여 이미지 전달 성능을 향상시킵니다. 이는 Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정으로 작업을 수행함으로써 수행됩니다. 스마트 이미징과 이를 사용하여 더 빠른 페이지 로드를 통해 더 나은 고객 경험을 제공하는 방법에 대해 자세히 알아보십시오.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 1%

---

# 스마트 이미징 {#smart-imaging}

웹 사이트나 모바일 사이트 또는 앱에서 고객 경험의 가장 중요한 측면 중 하나는 페이지 로드 시간입니다. 페이지를 로드하는 데 너무 오래 걸리는 경우 고객은 종종 사이트 또는 앱을 포기합니다. 이미지는 페이지 로드 시간의 대부분을 차지합니다. Dynamic Media Classic의 스마트 이미징은 클라이언트 브라우저 기능을 기반으로 이미지 형식 및 품질을 자동으로 최적화하여 이미지 전달 성능을 향상시킵니다. 이는 Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정으로 작업을 수행함으로써 수행됩니다. 스마트 이미징은 이미지 크기를 30% 이상 줄여 페이지 로드 속도를 높이고 고객 경험을 향상시킵니다.

스마트 이미징은 또한 Adobe의 동급 최고의 프리미엄 서비스와 완전히 통합됨으로써 더욱 향상된 성능을 제공합니다. 이 서비스는 인터넷의 기본 경로보다 지연 시간 및/또는 패킷 손실률이 가장 낮은 서버, 네트워크 및 피어링 포인트 사이의 최적 인터넷 경로를 찾습니다.

[스마트 이미징](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html)에 대해 자세히 알아보세요.

## 스마트 이미징의 이점

이미지가 페이지 로드 시간의 대부분을 차지하기 때문에 스마트 이미징의 성능 향상은 전환 증가, 사이트에서 보낸 시간 및 사이트 바운스 비율 감소와 같은 비즈니스 KPI에 지대한 영향을 줄 수 있습니다.

![이미지](assets/smart-imaging/smart-imaging-1.png)

## 스마트 이미징 작동 방식

앞에서 언급했듯이 스마트 이미징은 Adobe Sensei AI 기능을 활용하고 기존 이미지 사전 설정과 함께 작동하여 시각적 충실도를 유지하면서 이미지를 WebP와 같은 최적의 차세대 이미지 형식으로 자동으로 변환합니다.

지원되는 이미지 형식(및 해당 형식을 사용하지 않을 경우 발생하는 일)과 같은 세부 정보와 사용 중인 기존 이미지 사전 설정에 미치는 영향을 포함하여 [스마트 이미징 작동 방식](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)에 대해 자세히 알아보십시오.

## 스마트 이미징의 영향

스마트 이미징을 사용하려면 사이트의 URL, 이미지 사전 설정 및 코드를 변경해야 할 수도 있습니다. 스마트 이미징을 사용하기 위한 사전 요구 사항을 충족하고 지원되는 JPEG 및 PNG 이미지 형식의 이미지만 작업하는 경우에는 변경할 필요가 없습니다.

스마트 이미징은 HTTP, HTTPS 및 HTTP/2를 통해 제공되는 이미지와 함께 작동합니다.

>[!NOTE]
>
>스마트 이미징으로 이동하면 CDN의 캐시가 지워집니다. CDN의 캐시는 일반적으로 1~2일 내에 다시 빌드됩니다.

스마트 이미징은 Dynamic Media Classic의 기존 라이선스에 포함되어 있습니다. 이 기능에 대한 추가 비용은 없습니다. 이를 활용하려면 Adobe 번들 CDN과 전용 도메인의 두 가지 요구 사항을 충족해야 합니다. 그러면 자동으로 활성화되지 않으므로 계정에 대해 활성화해야 합니다.

스마트 이미징 활성화는에서 기술 지원부에 요청을 보내면 시작됩니다. |지원 사례 만들기| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/kr/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). 지원 기능은 스마트 이미징과 연결할 사용자 지정 도메인을 설정하는 데 사용할 수 있습니다. 캐싱과 관련된 한 개의 매개 변수(Time To Live 또는 TTL)를 변경하고 지원을 통해 캐시를 지웁니다. 프로덕션으로 푸시하기 전에 원하는 경우 선택적 스테이징 단계를 수행할 수도 있습니다. 그런 다음 스마트 이미징이 켜지면 고객에게 더 작은 크기의 이미지를 제공하지만 고객이 요청한 품질은 동일합니다. 즉, 페이지 로드 시간이 빨라집니다. Adobe Sensei은 가장 효율적인 크기를 선택할 수 있도록 도와주기 때문에 이 모든 작업이 자동으로 수행됩니다.

스마트 이미징을 활성화하면 스마트 이미징이 예상대로 작동하는지 확인해야 합니다.

스마트 이미징에 대한 추가 질문이 있을 수 있습니다. 답변이 포함된 자주 묻는 질문(FAQ) 목록을 정리했습니다. [FAQ](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html)를 읽어 보세요.

## 추가 리소스

[Dynamic Media Classic 최적화 페이지 성능 스킬 빌더](https://seminars.adobeconnect.com/pzc1gw0cihpv) 온디맨드 웨비나를 시청하여 스마트 이미징에 대해 자세히 알아보십시오.
