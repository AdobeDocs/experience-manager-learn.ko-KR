---
title: HTML5 양식 제출 소개에서 AEM 워크플로우 트리거
description: 모바일 양식 제출 시 AEM 워크플로우 트리거
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: c6ffa8f7a398b01fc12e1e2efe4382c941900496
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# 모바일 양식 제출 시 AEM 워크플로우 트리거

일반적인 사용 사례는 XDP를 데이터 캡처 활동에 대한 HTML으로 렌더링하는 것입니다. 이 양식을 제출하면 AEM 워크플로우를 트리거해야 할 수 있습니다. AEM 워크플로에서는 데이터를 XDP 템플릿과 병합하고 검토 및 승인을 위해 생성된 PDF을 제공할 수 있습니다. 양식이 게시 인스턴스에서 렌더링되고 워크플로가 AEM 처리 인스턴스에서 트리거됩니다.

사용 사례에는 다음 단계가 포함됩니다

* 사용자가 HTML5 양식(XDP의 HTML5 렌디션)을 작성하여 제출합니다.
* 제출은 게시 인스턴스의 서블릿에 의해 처리됩니다.
* 서블릿은 AEM 처리 인스턴스의 저장소에 사전 정의된 폴더에 데이터를 저장합니다.
* 워크플로우 런처는 새 파일이 특정 폴더에 추가될 때마다 AEM 워크플로우를 트리거하도록 구성됩니다.

이 튜토리얼에서는 위의 사용 사례를 완수하는 데 필요한 단계를 안내합니다. 이 자습서와 관련된 샘플 코드 및 자산은 [여기에서 사용할 수 있습니다.](./deploy-assets.md)


## 다음 단계

[양식 제출 처리](./handle-form-submission.md)
