---
title: Acrobat Sign 클라우드 구성 만들기 Cloud Service
description: 클라우드 서비스 구성을 사용하여 AEM Forms 및 Acrobat Sign 통합을 만듭니다.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 7428
thumbnail: 332437.jpg
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# Acrobat Sign 클라우드 구성 만들기

AEM의 클라우드 서비스 구성을 사용하면 AEM과 다른 클라우드 애플리케이션 간의 통합을 만들 수 있습니다.

다음 비디오에서는 AEM을 Acrobat Sign과 통합하기 위한 클라우드 서비스 구성을 만드는 데 필요한 단계를 설명합니다

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## 문제 해결

Abobe Sign 클라우드 구성 구성에 오류가 발생하는 경우 다음 단계를 수행하여 문제를 해결할 수 있습니다
* Acrobat Sign API 애플리케이션에 지정된 리디렉션 URL이 다음 형식인지 확인합니다
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
예: https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS는 클라우드 구성을 보유할 컨테이너의 이름입니다
* oAuth URL이 올바른지 확인하십시오
* 클라이언트 Id 및 클라이언트 암호 확인
* 시크릿 창 모드 시도

