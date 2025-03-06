---
title: 샘플 프로젝트
description: 사용자 환경에서 가져오고 실행할 수 있는 샘플 프로젝트
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---


# 로컬 환경에서 테스트합니다.

* 프로젝트 가져오기

   * [샘플 프로젝트](./assets/formsdocumentservices.zip) 다운로드 및 추출
   * 선호하는 **Java 개발 환경**(IntelliJ IDEA,Eclipse 또는 VS 코드)을(를) 열고 프로젝트를 Maven 프로젝트로 가져옵니다.
* 자격 증명 구성

   * `resources/credentials/server_credentials.json` 파일을 찾습니다.
   * 여정을 열고 **사용자 환경에 맞는 자격 증명을 업데이트**&#x200B;합니다.
   * 다음에 대한 유효한 값이 포함되어 있는지 확인합니다.
     `clientId`, `clientSecret`,`adobeIMSV3TokenEndpointURL` 및
     `scopes`

* Main 클래스 실행

   * `src/main/java/Main.java`(으)로 이동하여 기본 메서드 실행

* 실행 확인
   * 터미널 창에서 출력 확인

