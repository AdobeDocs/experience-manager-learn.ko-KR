---
title: AEM Assets 및 InDesign Server을 사용하여 에셋 템플릿 설정
description: 자산 템플릿을 사용하면 마케터가 디지털 및 인쇄용 디지털 자산을 만들고, 관리하고, 제공할 수 있습니다. InDesign 서버와 통합할 때 에셋 템플릿을 사용하면 마케팅 브로셔, 명함, 전단지, 광고 및 엽서를 훨씬 쉽게 만들 수 있습니다. AEM을 사용한 InDesign 서버 구성에 대해서는 이 섹션에서 다룹니다.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# AEM Assets 및 InDesign Server을 사용하여 에셋 템플릿 설정{#set-up-asset-templates-with-aem-assets-and-indesign-server}

자산 템플릿을 사용하면 마케터가 디지털 및 인쇄용 디지털 자산을 만들고, 관리하고, 제공할 수 있습니다. InDesign 서버와 통합할 때 에셋 템플릿을 사용하면 마케팅 브로셔, 명함, 전단지, 광고 및 엽서를 훨씬 쉽게 만들 수 있습니다. AEM을 사용한 InDesign 서버 구성에 대해서는 이 섹션에서 다룹니다.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **필수** indd 템플릿을 업로드할 때 실행 중인 InDesign 서버에 연결해야 합니다. INDD 파일의 초기 처리 중 일부에는 InDesign 서버가 필요합니다.

## InDesign Server 체험판 다운로드 {#download-indesign-server-trial}

다운로드 [InDesign Server 평가판 다운로드 웹 사이트](https://www.adobeprerelease.com/)

## 시작 InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
