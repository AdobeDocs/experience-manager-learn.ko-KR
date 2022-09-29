---
title: AEM Forms CS에서 배치 API를 사용하여 문서 생성
description: 일괄 처리 작업을 구성하여 문서를 생성합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# 소개

배치 요청은 한 번에 수만, 수백 또는 수천 개의 유사한 문서가 생성되는 경우입니다. 예: 금융 회사는 모든 고객에게 보낼 신용 카드 명세서를 생성할 수 있습니다.
배치 API(비동기 API)는 예약된 높은 처리량 다중 문서 생성 사용 사례에 적합합니다. 이러한 API는 문서를 일괄로 생성합니다. 예를 들어, 매월 생성된 전화 요금 청구서, 신용 카드 명세서 및 혜택 명세서 등이 있습니다.

AEM Forms CS 배치 작업 API를 사용하려면 다음 구성이 필요합니다

1. Azure 저장소 계정 구성
1. Azure 저장 공간 지원 클라우드 구성 만들기
1. 배치 데이터 저장소 구성 만들기
1. 배치 API 실행

을 잘 알고 있는 것이 좋습니다 [API 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) 계속 이 자습서를 사용하기 전에
