---
title: asset compute 확장성을 위한 계정 및 서비스 설정
description: asset compute 개발 작업자는 AEM을 비롯한 계정 및 서비스에 Cloud Service, Microsoft 또는 Amazon에서 제공하는 Adobe, Project Firefly 및 클라우드 스토리지를 이용해야 합니다.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---


# 계정 및 서비스 설정

이 자습서에서는 학습자의 Adobe ID을 통해 다음 서비스를 제공하고 액세스할 수 있어야 합니다.

모든 Adobe 서비스는 Adobe ID을 사용하여 동일한 Adobe 조직을 통해 액세스할 수 있어야 합니다.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe 프로젝트 FireFly](#adobe-project-firefly)
   + 프로비저닝은 2~10일 걸릴 수 있습니다.
+ 클라우드 스토리지
   + [Azure Blob 저장소](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 또는 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>이 튜토리얼을 계속 진행하기 전에 앞에서 언급한 모든 서비스를 이용할 수 있는지 확인하십시오.
> 
> 필요한 서비스를 설정하고 제공하는 방법에 대한 아래 섹션을 검토하십시오.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

사용자 지정 Asset compute 작업자를 호출하도록 AEM Assets 처리 프로필을 구성하려면 Cloud Service 환경으로 AEM에 액세스해야 합니다.

샌드박스 프로그램 또는 비샌드박스 개발 환경을 사용하는 것이 좋습니다.

로컬 AEM SDK가 Asset compute 마이크로서비스와 통신할 수 없으므로 로컬 AEM SDK가 이 자습서를 완료하는 데 불충분하며 Cloud Service 환경으로서 진정한 AEM이 필요합니다.

## Adobe 프로젝트 Firefly{#adobe-project-firefly}

[Adobe 프로젝트 Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) 프레임워크는 Adobe 서버를 사용하지 않는 플랫폼 Adobe I/O Runtime에 사용자 지정 동작을 빌드하고 배포하는 데 사용됩니다. AEM Asset compute 프로젝트는 처리 프로필을 통해 AEM Assets과 통합되고 자산 바이너리에 액세스하고 처리할 수 있는 기능을 제공하는 특별히 빌드된 Firefox 프로젝트입니다.

Project Firefox에 액세스하려면 미리 보기에 등록하십시오.

1. [Project Firefox 미리 보기에 등록하십시오](https://adobeio.typeform.com/to/obqgRm).
1. 이 튜토리얼을 계속 진행하기 전에 제공된 전자 메일을 통해 통보될 때까지 약 2-10일 동안 기다립니다.
   + 프로비저닝되었는지 확신할 수 없는 경우 다음 단계를 계속 진행하십시오. 그리고 [Adobe 개발자 콘솔](https://console.adobe.io)에서 __Project Firefox__ 프로젝트를 만들 수 없는 경우 아직 제공되지 않았습니다.

## 클라우드 스토리지

asset compute 프로젝트의 로컬 개발을 위해서는 클라우드 스토리지가 필요합니다.

asset compute 작업자가 AEM에서 Cloud Service으로 직접 사용하기 위해 Adobe I/O Runtime에 배포되는 경우, AEM에서 에셋을 읽고 표현하여 쓰는 클라우드 스토리지를 제공하므로 이 클라우드 스토리지가 엄격하게 필요하지 않습니다.

### Microsoft Azure Blob 저장소{#azure-blob-storage}

Microsoft Azure Blob 저장소에 대한 액세스 권한이 없는 경우 [무료 12개월 계정](https://azure.microsoft.com/en-us/free/)에 등록하십시오.

이 자습서는 Azure Blob 저장소를 사용하지만 [Amazon S3](#amazon-s3)은 자습서에 약간의 변형만 사용할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Azure Blob 저장소 프로비저닝 클릭스루(오디오 없음)_


1. [Microsoft Azure 계정](https://azure.microsoft.com/en-us/account/)에 로그인합니다.
1. __저장소 계정__ Azure 서비스 섹션으로 이동합니다.
1. __+ 추가__&#x200B;를 눌러 새 Blob 저장소 계정을 만듭니다.
1. 필요에 따라 새 __리소스 그룹__&#x200B;을 만듭니다. 예:`aem-as-a-cloud-service`
1. __저장소 계정 이름__&#x200B;을 입력합니다. 예:`aemguideswkndassetcomput`
   + __저장소 계정 이름__&#x200B;은 로컬 Asset compute 개발 도구에 대해 클라우드 저장소](../develop/environment-variables.md)를 구성하는 데 사용됩니다.[
   + [클라우드 스토리지](../develop/environment-variables.md)을 구성하는 경우에도 스토리지 계정과 연결된 __액세스 키__&#x200B;가 필요합니다.
1. 다른 모든 것을 기본값으로 유지하고 __검토 + 만들기__ 단추를 누릅니다
   + 원할 경우, __위치__&#x200B;를 선택합니다.
1. 프로비전 요청에 대한 정확성을 검토하고 만족한 경우 __만들기__ 단추를 누릅니다.

### Amazon S3{#amazon-s3}

이 자습서를 완료하려면 [Microsoft Azure Blob 저장소](#azure-blob-storage)를 사용하는 것이 좋지만 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)도 사용할 수 있습니다.

Amazon S3 저장소를 사용하는 경우 [프로젝트의 환경 변수](../develop/environment-variables.md#amazon-s3)를 구성할 때 Amazon S3 클라우드 스토리지 자격 증명을 지정합니다.

이 튜토리얼을 위해 클라우드 스토리지를 특별히 프로비저닝해야 하는 경우 [Azure Blob 저장소](#azure-blob-storage)를 사용하는 것이 좋습니다.
