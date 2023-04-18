---
title: AEM Forms 및 Acrobat Sign과 반응형 앱
description: Acrobat Sign과 AEM Forms을 사용하면 복잡한 트랜잭션을 자동화할 수 있고, 원활한 디지털 경험의 일부로 법적 전자 서명을 포함시킬 수 있습니다.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# Acrobat Sign 웹 양식을 사용한 AEM Forms


이 자습서에서는 [React](https://react.dev/) Acrobat Sign 웹 양식을 사용하여 서명을 위해 생성된 문서를 앱 및 제공합니다.

다음은 사용 사례의 흐름입니다

* 사용자가 React 앱으로 양식을 채웁니다.
* 양식 데이터가 AEM Forms 종단점에 제출되어 대화형 통신 문서를 생성합니다.
* 생성된 문서를 사용하여 Acrobat Sign 위젯 url을 만듭니다.
* 사용자가 문서에 서명할 수 있도록 호출 애플리케이션에 위젯 url을 표시합니다.

## 사전 요구 사항

사용 사례가 작동하려면 다음 항목이 필요합니다.

* Forms이 패키지에 추가된 AEM 서버
* An [Acrobat Sign 애플리케이션용 통합 키](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

