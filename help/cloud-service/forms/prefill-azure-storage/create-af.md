---
title: Azure Storage에 적응형 양식 데이터 저장
description: Azure Storage에 데이터를 저장하도록 적응형 양식 만들기 및 구성
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
exl-id: 0b543c6b-9cfd-4fac-b8d0-33153c036f4b
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 2%

---

# 결합하기

이제 사용 사례에 필요한 모든 필수 구성/통합이 있습니다. 마지막 단계는 Azure Storage에서 지원하는 양식 데이터 모델을 기반으로 적응형 양식을 만드는 것입니다.

적응형 양식을 만들고 올바른 적응형 양식 템플릿을 기반으로 하는지 확인하십시오. 이렇게 하면 적응형 양식이 렌더링될 때마다 템플릿과 연결된 사용자 지정 코드가 실행됩니다.

## 샘플 적응형 양식

양식에서 2개의 숨겨진 필드를 추가했습니다

* Blob ID - 이 필드는 필드가 초기화될 때 GUID로 채워집니다. 이 필드의 값은 양식 데이터의 Blob 저장소를 고유하게 식별하는 블롭으로 사용됩니다. 이 블롭은 양식 데이터를 식별하는 데 사용됩니다.
* Blob ID 반환됨 - 이 필드는 Azure Storage에 데이터를 저장하기 위한 서비스 호출 결과로 채워집니다. 이 값은 이전 단계의 Blob ID와 동일합니다.

양식에는 다음과 같은 비즈니스 규칙이 있습니다

* 사용자가 전자 메일 주소를 입력하면 양식 저장 버튼이 표시됩니다. 양식 저장 단추를 클릭하면 양식 데이터 모델의 호출 서비스 작업을 사용하여 양식 데이터가 Azure Storage에 저장됩니다.
* 호출 서비스에서 반환된 BlobID가 Blob ID 필드에 저장됩니다. 이 값이 변경되면 SendGrid를 사용하여 지원자에게 전자 메일이 전송됩니다. 이메일에는 Blob ID로 식별되는 부분적으로 완료된 양식을 여는 링크가 포함됩니다.
* 데이터가 Azure 스토리지에 성공적으로 저장되면 확인 텍스트가 사용자에게 표시됩니다

### 다음 단계

[샘플 자산 배포](./deploy-sample-assets.md)
