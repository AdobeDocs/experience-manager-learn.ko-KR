---
title: AEM Assets 및 InDesign Server을 사용하여 자산 템플릿 설정
description: 자산 템플릿을 사용하면 마케터가 디지털 및 인쇄용 디지털 자산을 만들고, 관리하고, 제공할 수 있습니다. InDesign 서버와 통합할 때 에셋 템플릿을 사용하면 마케팅 브로셔, 명함, 전단지, 광고 및 엽서를 훨씬 쉽게 만들 수 있습니다. 이 섹션에서는 AEM을 사용한 InDesign 서버 구성에 대해 설명합니다.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# AEM Assets 및 InDesign Server을 사용하여 자산 템플릿 설정{#set-up-asset-templates-with-aem-assets-and-indesign-server}

자산 템플릿을 사용하면 마케터가 디지털 및 인쇄용 디지털 자산을 만들고, 관리하고, 제공할 수 있습니다. InDesign 서버와 통합할 때 에셋 템플릿을 사용하면 마케팅 브로셔, 명함, 전단지, 광고 및 엽서를 훨씬 쉽게 만들 수 있습니다. 이 섹션에서는 AEM을 사용한 InDesign 서버 구성에 대해 설명합니다.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>INDD 템플릿을 업로드할 때 AEM **must**&#x200B;이(가) 실행 중인 InDesign 서버에 연결되어 있어야 합니다. INDD 파일의 초기 처리 중 일부에는 InDesign 서버가 필요합니다.

## InDesign Server 체험판 다운로드 {#download-indesign-server-trial}

[InDesign Server 평가판 다운로드 웹 사이트](https://www.adobeprerelease.com/) 다운로드

## InDesign Server 시작 {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
