---
title: IC 문서에 대한 양식 데이터 모델 만들기
description: 대화형 통신 문서에 대한 데이터를 동적으로 검색하기 위해 AEM Forms에서 양식 데이터 모델을 만드는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Interactive Communication
role: Developer
level: Intermediate
doc-type: Feature Video
duration: 170
last-substantial-update: 2026-02-20T00:00:00Z
jira: KT-20353
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---


# IC 문서에 대한 양식 데이터 모델 만들기

Forms 데이터 모델을 만들어 Adobe AEM의 대화형 통신과 외부 데이터 소스를 통합합니다. 이 프로세스에는 RESTful 서비스를 설정하고, Swagger 파일을 업로드하고, 데이터를 동적으로 검색하고 바인딩하도록 서비스 끝점을 구성하는 작업이 포함됩니다. 성공적인 데이터 검색을 위해 외부 서비스에 안전하게 연결하고 모델을 테스트하는 방법에 대해 알아봅니다.

개발 및 테스트 목적으로 주문 서비스를 시뮬레이션하는 모의 API 서버가 구현되었습니다. 끝점을 노출하여 지정된 사용자(예: 사용자 ID별)에 대한 주문을 가져오고 프로덕션 API와 동일한 스키마에서 사전 정의되거나 동적으로 생성된 주문 데이터를 반환합니다.

양식 데이터 모델을 만드는 데 사용되는 Swagger 파일은 [여기에서 다운로드](assets/UsersAndOrders.json)할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3480024/?captions=kor&learn=on&enablevpops)

## 다음 단계

[템플릿 만들기](./create-template.md)
