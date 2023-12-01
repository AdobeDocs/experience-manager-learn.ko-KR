---
title: 여러 문서 서명 문제 해결 솔루션
description: 테스트 및 문제 해결 솔루션
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# 테스트 및 문제 해결


## 리파이낸스 양식 미리 보기

사용 사례는 고객 서비스 에이전트가 작성하여 제출하면 트리거됩니다 [재융자 형태](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

여러 Forms 서명 워크플로우는 이 양식 제출 시 트리거되며 고객은 양식 작성 및 서명 프로세스를 시작하는 링크가 포함된 이메일 알림을 받습니다.

## 패키지의 양식 작성

고객은 패키지의 첫 번째 양식에 기입하고 서명하도록 제시됩니다. 양식 서명 성공 시 고객은 패키지의 다음 양식으로 이동할 수 있습니다. 모든 양식이 채워지고 서명되면 고객에게 &quot;**AllDone**&quot; 양식입니다.

## 문제 해결

### 이메일 알림이 생성되지 않음

전자 메일 알림은 여러 양식 서명 워크플로의 전자 메일 전송 구성 요소에 의해 전송됩니다. 이 워크플로우의 단계 중 하나라도 실패하면 전자 메일 알림이 전송됩니다. 워크플로우의 사용자 지정 프로세스 단계가 MySQL 데이터베이스에 행을 만들고 있는지 확인합니다. 행을 만드는 경우 일별 CQ 메일 서비스 구성 설정을 확인하십시오

### 이메일 알림의 링크가 작동하지 않음

전자 메일 알림의 링크는 동적으로 생성됩니다. AEM 서버가 localhost:4502에서 실행되고 있지 않은 경우 여러 Forms 서명 워크플로의 Forms To Sign 저장 단계의 인수에 올바른 서버 이름과 포트를 제공하십시오.

### 양식에 서명할 수 없음

이 문제는 데이터 소스에서 데이터를 가져와 양식을 올바르게 채우지 않은 경우 발생할 수 있습니다. 서버의 stdout 로그를 확인합니다. fetchformdata.jsp는 stdout에 몇 가지 유용한 메시지를 기록합니다.

### 패키지의 다음 양식으로 이동할 수 없음

패키지에서 양식에 성공적으로 서명하면 서명 상태 업데이트 워크플로가 트리거됩니다. 워크플로우의 첫 번째 단계에서는 데이터베이스에서 양식의 서명 상태를 업데이트합니다. 양식 상태가 0에서 1로 업데이트되었는지 확인하십시오.

### AllDone 양식이 표시되지 않음

패키지에 서명할 양식이 더 이상 없으면 AllDone 양식이 사용자에게 표시됩니다.AllDone 양식이 표시되지 않으면 의 일부인 GetNextFormToSign.js 파일의 33행에 사용된 URL을 확인하십시오. **getnextform** 클라이언트 라이브러리.
