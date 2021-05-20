---
title: asset compute 확장성을 위한 계정 및 서비스 설정
description: 개발 Asset compute 작업자는 AEM as a Cloud Service, Project Firefly Adobe, Microsoft 또는 Amazon에서 제공하는 클라우드 스토리지 등의 서비스 및 계정에 액세스해야 합니다.
feature: asset compute 마이크로서비스
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: 통합, 개발
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 1%

---


# 계정 및 서비스 설정

이 자습서를 사용하려면 학습자의 Adobe ID을 통해 다음 서비스를 제공하고 액세스할 수 있어야 합니다.

모든 Adobe 서비스는 Adobe ID을 사용하여 동일한 Adobe 조직을 통해 액세스할 수 있어야 합니다.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe 프로젝트 FireFly](#adobe-project-firefly)
   + 프로비저닝은 2~10일 정도 걸릴 수 있습니다
+ 클라우드 스토리지
   + [Azure Blob 저장소](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 또는 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>이 자습서를 계속 진행하기 전에 위의 모든 서비스에 액세스할 수 있는지 확인하십시오.
> 
> 필요한 서비스를 설정하고 프로비저닝하는 방법에 대한 아래 섹션을 검토하십시오.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

사용자 지정 Asset compute 작업자를 호출하도록 AEM Assets 처리 프로필을 구성하려면 Cloud Service 환경으로 AEM에 액세스해야 합니다.

샌드박스 프로그램 또는 샌드박스 개발 환경이 아닌 환경을 사용할 수 있는 것이 좋습니다.

로컬 AEM SDK는 Asset compute 마이크로 서비스와 통신할 수 없으므로 로컬 AEM SDK가 이 자습서를 완료하기에 충분하지 않으며 Cloud Service 환경으로서 진정한 AEM이 필요합니다.

## Adobe 프로젝트 Firefly{#adobe-project-firefly}

[Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) 프레임워크는 Adobe의 서버리스 플랫폼인 Adobe I/O Runtime에 사용자 지정 작업을 작성하고 배포하는 데 사용됩니다. AEM Asset compute 프로젝트는 처리 프로필을 통해 AEM Assets과 통합되고 자산 바이너리에 액세스 및 처리할 수 있는 기능을 제공하는 특별히 빌드된 Firefly 프로젝트입니다.

Project Firefly에 액세스하려면 미리 보기에 등록하십시오.

1. [Project Firefly 미리 보기에 등록](https://adobeio.typeform.com/to/obqgRm).
1. 자습서를 계속하기 전에 제공된다는 전자 메일을 통해 알림을 받을 때까지 약 2~10일을 기다립니다.
   + 프로비저닝되었는지 확실하지 않은 경우 다음 단계를 계속 진행하십시오. 그리고 [Analysis Developer Console](https://console.adobe.io)에서 __Project Firefly__ 프로젝트를 만들 수 없다면 아직 프로비저닝되지 않았습니다.

## 클라우드 스토리지

asset compute 프로젝트의 로컬 개발에 클라우드 스토리지가 필요합니다.

AEM에서 Cloud Service으로 직접 사용하기 위해 Asset compute 작업자를 Adobe I/O Runtime에 배포하면, AEM에서 자산을 읽고 렌디션이 작성된 클라우드 저장소를 제공하므로 이 클라우드 저장소가 엄격하게 필요하지 않습니다.

### Microsoft Azure Blob 저장소{#azure-blob-storage}

Microsoft Azure Blob 저장 공간에 아직 액세스할 수 없는 경우 [무료 12개월 계정](https://azure.microsoft.com/en-us/free/)에 등록하세요.

이 자습서에서는 Azure Blob 저장소를 사용하지만 [Amazon S3](#amazon-s3) 은 자습서에 작은 변형만 사용할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Azure Blob 저장소 프로비저닝의 클릭스루(오디오 없음)_


1. [Microsoft Azure 계정](https://azure.microsoft.com/en-us/account/)에 로그인합니다.
1. __저장소 계정__ Azure 서비스 섹션으로 이동합니다.
1. __+ 추가__&#x200B;를 탭하여 새 Blob 저장소 계정을 만듭니다
1. 필요에 따라 새 __리소스 그룹__&#x200B;을 만듭니다. 예를 들면 다음과 같습니다.`aem-as-a-cloud-service`
1. __저장소 계정 이름__&#x200B;을 입력합니다. 예를 들면 다음과 같습니다.`aemguideswkndassetcomput`
   + __저장소 계정 이름__&#x200B;은 로컬 Asset compute 개발 도구에 대한 클라우드 저장소 구성](../develop/environment-variables.md)에 사용됩니다[
   + [클라우드 저장소](../develop/environment-variables.md)를 구성할 때 저장소 계정과 연결된 __액세스 키__&#x200B;도 필요합니다.
1. 다른 모든 항목을 기본값으로 두고 __검토 + 만들기__ 단추를 누릅니다
   + 선택 사항으로 사용자에게 가까운 __위치__&#x200B;를 선택합니다.
1. 프로비전 요청을 올바르게 검토하고 만족하면 __만들기__ 단추를 누릅니다

### Amazon S3{#amazon-s3}

이 자습서를 완료하려면 [Microsoft Azure Blob 저장 공간](#azure-blob-storage)을 사용하는 것이 좋지만 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)도 사용할 수 있습니다.

Amazon S3 저장소를 사용하는 경우 [프로젝트의 환경 변수를 구성할 때 Amazon S3 클라우드 스토리지 자격 증명을 지정합니다](../develop/environment-variables.md#amazon-s3).

이 자습서에서 클라우드 저장소를 특별히 프로비저닝해야 하는 경우 [Azure Blob 저장 공간](#azure-blob-storage)을 사용하는 것이 좋습니다.
