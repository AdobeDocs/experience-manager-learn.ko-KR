---
title: AEM Forms 및 Acrobat Sign과 앱 반응
description: Acrobat Sign 및 AEM Forms을 사용하면 복잡한 트랜잭션을 자동화하고 원활한 디지털 경험의 일부로서 합법적인 전자 서명을 포함할 수 있습니다.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# Acrobat Sign Web Form이 포함된 AEM Forms


이 튜토리얼에서는 의 제출 데이터로 대화형 통신 문서를 생성하는 사용 사례를 안내합니다. [반응](https://react.dev/) Acrobat Sign 웹 양식을 사용하여 서명할 생성된 문서를 앱 및 표시합니다.

다음은 사용 사례의 흐름입니다

* 사용자가 React 앱에서 양식을 작성합니다.
* 양식 데이터가 대화형 통신 문서를 생성하기 위해 AEM Forms 엔드포인트에 제출됩니다.
* 생성된 문서를 사용하여 Acrobat Sign 위젯 URL을 만듭니다.
* 사용자가 문서에 서명할 수 있도록 호출 애플리케이션에 위젯 URL을 제공합니다.

## 사전 요구 사항

사용 사례가 작동하려면 다음 조건을 충족해야 합니다.

* Forms 추가 기능 패키지가 포함된 AEM 서버
* An [Acrobat Sign 애플리케이션용 통합 키](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 다음 단계

쓰기 [대화형 통신 문서를 생성하기 위한 사용자 지정 OSGi 서비스](./create-ic-document.md) 문서화된 API 사용
