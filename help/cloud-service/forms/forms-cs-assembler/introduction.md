---
title: DDX 호출 작업을 사용한 Forms CS의 PDF 조작
description: DDX 호출을 사용하여 PDF 파일을 어셈블합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# 소개

이 과정에서는 Forms CS를 사용한 PDF 조작 및 PDF 문서 보관을 사용합니다. 외부 애플리케이션에서 이러한 마이크로서비스를 사용하려면 다음 단계를 수행해야 합니다.

1. AEM 기술 계정에 대한 서비스 자격 증명 생성
1. 서비스 자격 증명에서 JSON 웹 토큰(JWT)을 만들고 액세스 토큰으로 교환합니다
1. AEM에서 기술 계정에 대한 액세스 구성
1. 액세스 토큰을 사용하여 HTTP 호출을 수행하여 PDF 파일 조작/PDF/A 파일 생성 및 확인
