---
title: SendGrid와 AEM Forms 통합
description: AEM Forms을 사용하여 SengGrid 클라우드 기반 이메일 게재 플랫폼을 활용합니다.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 118
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# SendGrid와 AEM Forms 통합

AEM Forms에서 SendGrid 동적 템플릿을 사용하여 전자 메일을 보내는 프로세스를 살펴보는 이 기술 안내서를 시작합니다. 이 안내서는 다이내믹 템플릿을 활용하여 이메일 콘텐츠를 효과적으로 개인화하는 방법을 명확하게 이해하는 것을 목적으로 합니다.

동적 템플릿을 사용하면 적응형 양식에 캡처된 데이터를 기반으로 수신자에게 다양한 콘텐츠를 표시할 수 있는 이메일 템플릿을 만들 수 있습니다. 개인화 변수를 활용하여 대상자에게 공감을 주는 타겟팅되고 맞춤화된 이메일 경험을 제공할 수 있습니다.

또한 고객의 이름과 이메일 주소를 포함하여 이메일을 더욱 개인화할 수 있고, 적절한 동적 이메일 템플릿을 선택할 수 있는 Swagger 파일의 사용에 대해 자세히 살펴봅니다.

이 문서의 단계별 지침에 따라 SendGrid 동적 템플릿 및 AEM Forms의 기능을 활용하고 이메일 커뮤니케이션을 새로운 수준의 참여 및 관련성으로 향상하십시오. 시작하자!

## 사전 요구 사항

AEM Forms에서 SendGrid 동적 템플릿을 사용하여 이메일 전송을 진행하기 전에 다음 사전 요구 사항을 충족하는지 확인하십시오.

1. **SendGrid 계정**: [https://sendgrid.com](https://sendgrid.com)에서 SendGrid 계정에 등록하여 이메일 게재 서비스에 액세스합니다. SendGrid를 AEM Forms과 통합하려면 계정 자격 증명이 필요합니다.
1. **데이터 원본 만들기에 익숙함**: AEM Forms에서 데이터 원본을 만드는 데 대한 작업 지식을 가지고 있습니다. 필요한 경우 자세한 지침은 [데이터 원본 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)에 대한 설명서를 참조하세요.
1. **양식 데이터 모델에 익숙함**: AEM Forms의 양식 데이터 모델에 대한 개념을 이해합니다. 필요한 경우 [양식 데이터 모델 만들기](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)에 대한 설명서를 검토하여 필요한 내용을 이해하십시오.

이러한 전제 조건을 충족하면 AEM Forms의 SendGrid 동적 템플릿을 사용하여 이메일을 효과적으로 보낼 수 있는 필수 지식과 리소스를 갖추게 됩니다.

## 샘플 자산

이 문서와 함께 제공되는 샘플 자산은 다음과 같습니다.

* **[Swagger 파일](assets/SendGridWithDynamicTemplate.yaml)**: 이 파일을 사용하면 동적 전자 메일 템플릿을 사용하여 전자 메일을 보낼 수 있습니다. 원활한 이메일 전달을 위해 SendGrid 및 AEM Forms과 통합하는 데 필요한 사양 및 구성을 제공합니다.

제공된 Swagger 파일을 참조 또는 동적 템플릿으로 이메일 기능을 구현하기 위한 시작점으로 자유롭게 활용할 수 있습니다.

## 테스트 지침

이 안내서에 설명된 기능을 테스트하려면 다음 단계를 따르십시오.

1. 자산 폴더에 제공된 [swagger 파일](assets/SendGridWithDynamicTemplate.yaml)을 다운로드합니다.
2. 다운로드한 Swagger 파일 및 SendGrid 자격 증명을 사용하여 Restful 데이터 소스를 만듭니다.
3. Restful 데이터 소스를 기반으로 양식 데이터 모델을 만듭니다.
4. 요구 사항에 따라 양식 데이터 모델의 `mail/send` POST 작업을 호출합니다. 예를 들어 단추 클릭에 전자 메일을 트리거하거나 AEM Forms 워크플로의 일부로 포함할 수 있습니다.

서비스에 대한 샘플 페이로드는 다음과 같습니다. 자리 표시자 값을 자신의 데이터로 바꿉니다.

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

`template_id`이(가) SendGrid 동적 전자 메일 템플릿의 ID에 해당하고 전자 메일 주소가 유효하고 SendGrid에서 확인되었는지 확인하십시오. `personalizations` 섹션의 값을 사용하면 적응형 양식의 사용자 입력 데이터를 사용하여 전자 메일을 개인화할 수 있습니다.

이러한 단계를 수행하고 제공된 페이로드를 맞춤화하면 AEM Forms과 SendGrid 동적 템플릿의 통합을 효과적으로 테스트할 수 있습니다.
