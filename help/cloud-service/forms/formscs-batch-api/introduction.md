---
title: AEM Forms CS에서 배치 API를 사용하여 문서 생성
description: 문서를 생성하도록 배치 작업을 구성하고 트리거합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 14%

---

# 소개

일괄 처리 요청은 수십, 수백 또는 수천 개의 유사한 문서가 한 번에 생성되는 곳입니다. 예: 금융 회사는 모든 고객에게 보낼 신용 카드 명세서를 생성할 수 있습니다.
배치 API(비동기 API)는 예약된 높은 처리량의 여러 문서 생성 사용 사례에 적합합니다. 해당 API는 배치 문서를 생성합니다. 예: 매월 생성되는 전화요금 청구서, 신용카드 명세서와 혜택 명세서.

AEM Forms CS 일괄 처리 작업 API를 사용하려면 다음 구성이 필요합니다

1. Azure 스토리지 계정 구성
1. Azure 스토리지 지원 클라우드 구성 만들기
1. 일괄 처리 데이터 저장소 구성 만들기
1. 일괄 처리 API 실행

이 자습서를 사용하기 전에 [API 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en)를 숙지하는 것이 좋습니다.
