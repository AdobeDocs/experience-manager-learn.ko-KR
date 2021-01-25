---
title: 여러 문서 솔루션 서명 문제 해결
description: 솔루션 테스트 및 문제 해결
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# 테스트 및 문제 해결


## 재재정 양식 미리 보기

사용 사례는 고객 서비스 에이전트가 [재무구조 양식](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled)을 채우고 제출하면 트리거됩니다.

여러 Forms 서명 작업 과정은 이 양식 제출 시 트리거되며 고객은 양식 채우기 및 서명 프로세스를 시작하는 링크가 포함된 이메일 알림을 받습니다.

## 패키지에 양식 채우기

고객이 패키지의 첫 번째 양식을 채우고 서명하도록 합니다. 양식 서명에 성공하면 고객이 패키지의 다음 양식으로 이동할 수 있습니다. 모든 양식이 채워지고 서명되면 고객에게 &quot;**AllDone**&quot; 양식이 표시됩니다.

## 문제 해결

### 전자 메일 알림이 생성되지 않습니다.

전자 메일 알림은 여러 양식 서명 워크플로우에서 전자 메일 보내기 구성 요소를 통해 전송됩니다. 이 워크플로우의 단계가 실패할 경우 이메일 알림이 전송됩니다. 워크플로우의 사용자 지정 프로세스 단계가 MySQL 데이터베이스에 행을 만들고 있는지 확인합니다. 행을 만드는 경우 CQ일 메일 서비스 구성 설정을 확인하십시오

### 전자 메일 알림의 링크가 작동하지 않습니다.

이메일 알림의 링크가 동적으로 생성됩니다. AEM 서버가 localhost:4502에서 실행되고 있지 않은 경우 여러 Forms Sign 작업 과정의 [서명할 Forms 저장] 단계의 인수에 올바른 서버 이름과 포트를 제공하십시오.

### 양식에 서명할 수 없음

이 문제는 데이터 소스에서 데이터를 가져와 양식이 올바르게 채워지지 않은 경우에 발생할 수 있습니다. 서버의 상태 기록 로그를 확인합니다. fetchformdata.jsp는 stdout에 유용한 메시지를 기록합니다.

### 패키지에서 다음 양식으로 이동할 수 없음

패키지에서 양식이 성공적으로 서명되면 서명 상태 업데이트 워크플로우가 트리거됩니다. 워크플로우의 첫 번째 단계는 데이터베이스의 양식 서명 상태를 업데이트합니다. 양식의 상태가 0에서 1까지 업데이트되었는지 확인하십시오.

### AllDone 양식 보기 안 함

패키지에 로그인할 양식이 더 이상 없으면 AllDone 양식이 사용자에게 표시됩니다.AllDone 양식이 표시되지 않으면 **getnextform** client lib의 일부인 GetNextFormToSign.js 파일의 33행에 사용된 URL을 확인하십시오.











