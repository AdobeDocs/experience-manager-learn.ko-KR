---
title: XDP 템플릿과 데이터 병합
description: 템플릿과 데이터를 병합하여 PDF 문서 만들기
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# XDP 템플릿과 데이터 병합

다음 단계는 XML 데이터를 템플릿과 병합하여 PDF을 생성하는 것입니다. 그런 다음 이 PDF은 Adobe Sign을 사용하여 서명을 위해 전송됩니다.

## OutputService를 사용하여 PDF 생성

다음 [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) PDF 생성에 OutputService 메서드가 사용되었습니다.
그런 다음 생성된 PDF은 Adobe Sign REST API를 사용하여 서명을 위해 전송되었습니다.
