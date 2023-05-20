---
title: 일괄 가져오기 서비스를 사용한 컨텐츠 마이그레이션
description: AEM as a Cloud Services의 일괄 가져오기 서비스 를 사용하여 AEM이 아닌 소스에서 에셋을 가져오는 방법에 대해 알아봅니다.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8918
thumbnail: 336969.jpeg
exl-id: 4944d3d9-52a0-4255-9e6c-eb119160e400
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# 일괄 가져오기 서비스

AEM as a Cloud Services의 일괄 가져오기 서비스 를 사용하여 AEM이 아닌 소스에서 에셋을 가져오는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336969?quality=12&learn=on)

## 일괄 가져오기 서비스 사용

![일괄 가져오기 서비스 라이프사이클](../assets/bulk-import-service.png)

일괄 가져오기 서비스는 Azure Blob 저장소 또는 Amazon S3 저장소에 저장된 파일을 자산으로 AEMas a Cloud Service 에 전송하는 데 사용됩니다.

## 주요 활동

+ 가져올 파일을 클라우드 스토리지 공급자(Azure Blob Storage 또는 Amazon S3)에 업로드합니다.
+ AEM as a Cloud Service Author 서비스에서 일괄 가져오기 서비스를 구성하고 실행합니다.
+ 일괄 서비스 가져오기를 일회성 가져오기로 실행하거나 정기적 가져오기를 예약합니다.

## 기타 리소스

+ [일괄 가져오기 서비스 구성 옵션](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/add-assets.html#configure-bulk-ingestor-tool)
+ [자산 수집에 대한 Adobe Developers Live 세션](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/feb2021/asset-bulk-ingestion.html)

