---
title: 쿼리 인터페이스 작성
description: Azure 포털에 저장된 양식 제출을 쿼리하는 단계를 안내하는 다중 파트 튜토리얼입니다.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 570a8176-ecf8-4a57-ab58-97190214c53f
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 1%

---

# 쿼리 인터페이스 작성

&quot;관리자&quot;가 특정 양식 제출을 검색하기 위해 검색 기준을 입력할 수 있도록 간단한 쿼리 인터페이스를 빌드했습니다. 그러면 결과가 특정 양식 제출을 보는 옵션과 함께 간단한 표 형식으로 표시됩니다.

![쿼리 제출](assets/query-submissions.png)

이 인터페이스의 드롭다운은 계단식 드롭다운입니다. 드롭다운에서 사용할 수 있는 옵션은 이전 드롭다운에서 선택한 사항에 따라 변경됩니다.

드롭다운은 RESTful 데이터 소스를 사용하여 채워집니다.

검색 결과는 &quot;SearchResults&quot;라는 사용자 지정 구성 요소에 표시됩니다. 보기 단추를 클릭하면 양식에 제출된 데이터와 첨부 파일이 미리 채워집니다.

## 다음 단계

[미리 채우기 서비스 작성](./part4.md)
