---
title: SendGrid를 사용하여 전자 메일 보내기
description: 저장된 양식에 대한 링크가 있는 이메일 트리거
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
duration: 37
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 1%

---

# SendGrid와 통합

AEM Forms 데이터 통합을 통해 다양한 데이터 소스를 구성하고 AEM Forms과 연결할 수 있습니다. 연결된 데이터 소스 전반에 걸쳐 비즈니스 엔티티 및 서비스의 통합 데이터 표현 스키마를 생성할 수 있는 직관적인 사용자 인터페이스를 제공합니다.

SendGrid API를 사용하여 동적 템플릿을 사용하여 이메일을 전송했습니다. 동적 템플릿을 사용하여 전자 메일을 전송하는 SendGrid API에 익숙하다고 가정합니다. API를 설명하는 Swagger 파일이 이 자습서의 일부로 제공되었습니다.

## 통합 만들기

AEM Forms과 SendGrid 간의 통합을 만들려면 다음 단계를 따르십시오

* [swagger 파일](./assets/SendGridWithDynamicTemplate.yaml)을 사용하여 RESTful 데이터 원본을 만드십시오. AEM Forms에서 데이터 원본을 만드는 방법에 대한 자세한 지침은 [이 비디오를 참조하세요](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ko)
* 이전 단계에서 만든 데이터 소스를 기반으로 양식 데이터 모델을 만듭니다.[자세한 설명서를 따라](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html?lang=ko) 양식 데이터 모델을 만듭니다.

이 자습서를 위해 만든 양식 데이터 모델은 문서 에셋의 일부로 포함됩니다.

### 다음 단계

[Azure 스토리지 통합 만들기](./create-fdm.md)
