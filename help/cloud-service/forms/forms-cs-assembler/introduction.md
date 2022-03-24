---
title: DDX 호출 작업을 사용하여 Forms CS에서 PDF 조작
description: DDX 호출을 사용하여 PDF 파일을 어셈블합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# 소개

이 과정에서는 Forms CS를 사용하여 PDF 문서의 PDF 조작 및 보관을 사용하게 됩니다. 외부 응용 프로그램에서 이러한 마이크로 서비스를 사용하려면 다음 단계를 수행합니다.

1. AEM 기술 계정에 대한 서비스 자격 증명 생성
1. 서비스 자격 증명에서 JWT(JSON 웹 토큰)를 만들고 액세스 토큰에 대해 교환합니다
1. AEM에서 기술 계정에 대한 액세스 구성
1. 액세스 토큰을 사용하여 HTTP 호출을 수행하여 PDF 파일을 조작하거나 PDF/A 파일을 생성하고 유효성을 검사합니다
