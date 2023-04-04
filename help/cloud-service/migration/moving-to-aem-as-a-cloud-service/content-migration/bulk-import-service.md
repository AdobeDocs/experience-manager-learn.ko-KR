---
title: 대량 가져오기 서비스를 사용한 컨텐츠 마이그레이션
description: AEM as a Cloud Services Bulk Import Service를 사용하여 비AEM 소스에서 자산을 가져오는 방법을 알아봅니다.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8918
thumbnail: 336969.jpeg
exl-id: 4944d3d9-52a0-4255-9e6c-eb119160e400
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# 대량 가져오기 서비스

AEM as a Cloud Services Bulk Import Service를 사용하여 비AEM 소스에서 자산을 가져오는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/336969?quality=12&learn=on)

## 대량 가져오기 서비스 사용

![대량 가져오기 서비스 수명 주기](../assets/bulk-import-service.png)

벌크 가져오기 서비스는 Azure Blob 저장 공간 또는 Amazon S3 저장 공간에 저장된 파일을 AEM as a Cloud Service으로 전송하는 데 사용됩니다.

## 주요 활동

+ 파일을 업로드하여 클라우드 저장소 공급자(Azure Blob 저장소 또는 Amazon S3)에 가져옵니다.
+ AEM as a Cloud Service 작성자 서비스에서 대량 가져오기 서비스를 구성하고 실행합니다.
+ 벌크 서비스 임포터를 일회성 임포터로 실행하거나 주기적 임포트를 예약합니다.

## 기타 리소스

+ [자산 수집의 Adobe Developers Live 세션](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/feb2021/asset-bulk-ingestion.html?lang=en)

