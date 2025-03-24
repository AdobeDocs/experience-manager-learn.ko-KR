---
title: Asset Compute 확장성을 위한 계정 및 서비스 설정
description: Asset Compute 작업자를 개발하려면 Microsoft 또는 Amazon에서 제공하는 AEM as a Cloud Service, App Builder 및 클라우드 스토리지를 포함한 계정 및 서비스에 액세스해야 합니다.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 2%

---

# 계정 및 서비스 설정

이 자습서에서는 다음 서비스를 프로비저닝하고 학습자의 Adobe ID을 통해 액세스할 수 있어야 합니다.

모든 Adobe 서비스는 Adobe ID을 사용하여 동일한 Adobe 조직을 통해 액세스할 수 있어야 합니다.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + 프로비저닝은 2~10일 정도 소요될 수 있습니다.
+ 클라우드 스토리지
   + [Azure Blob 저장소](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 또는 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>이 자습서를 계속 진행하기 전에 앞서 설명한 모든 서비스에 액세스할 수 있는지 확인하십시오.
> 
> 필요한 서비스를 설정하고 프로비저닝하는 방법에 대한 아래 섹션을 검토하십시오.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

사용자 지정 AEM as a Cloud Service 작업자를 호출하도록 AEM Assets 처리 프로필을 구성하려면 Asset Compute 환경에 액세스해야 합니다.

샌드박스 프로그램 또는 비샌드박스 개발 환경을 사용하는 것이 좋습니다.

로컬 AEM SDK이 Asset Compute 마이크로서비스와 통신할 수 없으므로, 대신 진정한 AEM as a Cloud Service 환경이 필요하므로 로컬 AEM SDK만으로는 이 자습서를 완료할 수 없습니다.

## App Builder{#app-builder}

[App Builder](https://developer.adobe.com/app-builder/) 프레임워크는 Adobe의 서버리스 플랫폼인 Adobe I/O Runtime에 사용자 지정 작업을 빌드하고 배포하는 데 사용됩니다. AEM Asset Compute 프로젝트는 특별히 제작된 App Builder 프로젝트로, 처리 프로필 을 통해 AEM Assets과 통합되며 에셋 바이너리에 액세스하고 처리하는 기능을 제공합니다.

App Builder에 액세스하려면 미리보기에 등록하십시오.

1. [App Builder 평가판에 등록](https://developer.adobe.com/app-builder/trial/).
1. 튜토리얼을 계속하기 전에 이메일을 통해 프로비저닝되었다는 알림이 표시될 때까지 약 2~10일 기다립니다.
   + 프로비저닝되었는지 확실하지 않은 경우 다음 단계를 계속 진행하고 [App Builder](https://developer.adobe.com/console/)에서 __Adobe Developer Console__ 프로젝트를 만들 수 없는 경우 아직 프로비저닝되지 않았습니다.

## 클라우드 스토리지

Asset Compute 프로젝트의 로컬 개발을 위해 클라우드 스토리지가 필요합니다.

Asset Compute 작업자가 AEM as a Cloud Service에서 직접 사용하기 위해 Adobe I/O Runtime에 배포되는 경우, AEM이 자산을 읽고 렌디션을 쓸 클라우드 스토리지를 제공하므로 이 클라우드 스토리지는 엄격히 필요하지 않습니다.

### Microsoft Azure Blob 저장소{#azure-blob-storage}

Microsoft Azure Blob Storage에 대한 액세스 권한이 없는 경우 [무료 12개월 계정](https://azure.microsoft.com/en-us/free/)에 등록하세요.

이 자습서에서는 Azure Blob 저장소를 사용하지만 [Amazon S3](#amazon-s3)은(는) 자습서에 대한 작은 변형만 사용할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Azure Blob 저장소를 프로비전하는 클릭스루(오디오 없음)_

1. [Microsoft Azure 계정](https://azure.microsoft.com/en-us/account/)에 로그인합니다.
1. __저장소 계정__ Azure 서비스 섹션으로 이동합니다.
1. 새 Blob 저장소 계정을 만들려면 __+ 추가__&#x200B;를 탭하세요.
1. 필요에 따라 새 __리소스 그룹__&#x200B;을(를) 만듭니다. 예: `aem-as-a-cloud-service`
1. __저장소 계정 이름__&#x200B;을(를) 지정하십시오. 예: `aemguideswkndassetcomput`
   + 로컬 Asset Compute 개발 도구에서 [클라우드 저장소 구성](../develop/environment-variables.md)에 사용되는 __저장소 계정 이름__
   + [클라우드 저장소를 구성](../develop/environment-variables.md)할 때도 저장소 계정과 연결된 __액세스 키__&#x200B;가 필요합니다.
1. 다른 모든 항목은 기본값으로 두고 __검토 + 만들기__ 단추를 탭합니다.
   + 필요한 경우 가까운 __위치__&#x200B;를 선택하십시오.
1. 프로비전 요청을 검토하여 정확성을 확인하고, 만족하면 __만들기__ 단추를 탭합니다.

### Amazon{#amazon-s3}

이 자습서를 완료하려면 [Microsoft Azure Blob 저장소](#azure-blob-storage)를 사용하는 것이 좋지만 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)도 사용할 수 있습니다.

Amazon S3 저장소를 사용하는 경우 [프로젝트의 환경 변수를 구성](../develop/environment-variables.md#amazon-s3)할 때 Amazon S3 클라우드 저장소 자격 증명을 지정하십시오.

이 자습서를 위해 특별히 클라우드 저장소를 프로비저닝해야 하는 경우 [Azure Blob 저장소](#azure-blob-storage)를 사용하는 것이 좋습니다.
