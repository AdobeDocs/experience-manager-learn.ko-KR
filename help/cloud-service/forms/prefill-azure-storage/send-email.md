---
title: SendGrid를 사용하여 전자 메일 보내기
description: 저장된 양식에 대한 링크가 있는 이메일 트리거
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-8474
duration: 41
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 전자 메일 보내기

양식의 데이터가 Azure Blob Storage에 저장되면 저장된 양식에 대한 링크가 포함된 이메일이 사용자에게 전송됩니다. 이 전자 메일은 SendGrid REST API를 사용하여 전송됩니다.

e-메일을 보내는 데 필요한 Swagger 파일, 양식 데이터 모델 및 클라우드 서비스 구성은 문서 에셋의 일부로 제공됩니다.

적응형 양식에서 캡처된 데이터를 수집할 수 있는 동적 템플릿인 SendGrid 계정을 만들어야 합니다.


다음은 동적 템플릿에 사용되는 html 코드 조각입니다. blobID 매개 변수 값이 양식 데이터 모델 호출 서비스를 사용하여 전달됩니다.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


