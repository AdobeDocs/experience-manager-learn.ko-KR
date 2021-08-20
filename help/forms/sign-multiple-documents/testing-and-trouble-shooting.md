---
title: 여러 문서 솔루션 서명 문제 해결
description: 해결 방법을 테스트하고 문제 발생
feature: 적응형 양식
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: 개발
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 1%

---


# 테스트 및 문제 해결


## 재재무 양식 미리 보기

사용 사례는 고객 서비스 에이전트가 [재재무 양식](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled)을 작성하고 제출하면 트리거됩니다.

여러 Forms 서명 워크플로우는 이 양식 제출에서 트리거되고 고객은 양식 작성 및 서명 프로세스를 시작할 수 있는 링크가 포함된 이메일 알림을 받게 됩니다.

## 패키지에 양식 작성

고객이 패키지의 첫 양식을 작성하고 서명하도록 표시됩니다. 양식 서명에 성공하면 고객이 패키지의 다음 양식으로 이동할 수 있습니다. 모든 양식을 작성하고 서명하면 고객에게 &quot;**AllDone**&quot; 양식이 표시됩니다.

## 문제 해결

### 전자 메일 알림이 생성되지 않습니다.

이메일 알림은 여러 양식 서명 워크플로우에서 이메일 전송 구성 요소에 의해 전송됩니다. 이 워크플로우의 단계가 실패하면 이메일 알림이 전송됩니다. 워크플로우의 사용자 지정 프로세스 단계가 MySQL 데이터베이스에 행을 만들고 있는지 확인합니다. 행을 만드는 경우 Day CQ Mail Service 구성 설정을 확인합니다

### 전자 메일 알림의 링크가 작동하지 않습니다

전자 메일 알림의 링크는 동적으로 생성됩니다. AEM 서버가 localhost:4502에서 실행되고 있지 않은 경우 Sign Multiple Forms 워크플로우의 Store Sign to Sign 단계의 인수에 올바른 서버 이름과 포트를 제공하십시오

### 양식에 서명할 수 없음

이 문제는 데이터 소스에서 데이터를 가져와 양식이 올바르게 채워지지 않은 경우에 발생할 수 있습니다. 서버의 체크아웃 로그를 확인합니다. Fetchformdata.jsp는 stdout에 유용한 메시지를 기록합니다.

### 패키지에서 다음 양식으로 이동할 수 없음

패키지에서 양식에 성공적으로 서명하면 서명 상태 업데이트 워크플로우가 트리거됩니다. 워크플로우의 첫 번째 단계는 데이터베이스의 양식의 서명 상태를 업데이트합니다. 양식의 상태가 0에서 1로 업데이트되었는지 확인하십시오.

### AllDone 양식이 표시되지 않음

패키지에 로그인할 양식이 더 없으면 AllDone 양식이 사용자에게 표시됩니다.AllDone 양식이 표시되지 않으면 **getnextform** 클라이언트 라이브러리의 일부인 GetNextFormToSign.js 파일의 33행에 사용된 URL을 확인하십시오.











