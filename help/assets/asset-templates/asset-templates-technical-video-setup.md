---
title: AEM Assets 및 InDesign Server을 사용하여 자산 템플릿 설정
description: 자산 템플릿을 사용하면 디지털 및 인쇄용 디지털 자산을 제작, 관리 및 전달할 수 있습니다. InDesign 서버와 통합된 에셋 템플릿을 사용하면 마케팅 브로셔, 명함, 전단지, 광고 및 게시물 카드를 보다 손쉽게 제작할 수 있습니다. AEM을 사용하는 InDesign 서버의 구성은 이 섹션에서 다룹니다.
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: 개발자
level: 중간
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 1%

---


# AEM Assets 및 InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}으로 자산 템플릿 설정

자산 템플릿을 사용하면 디지털 및 인쇄용 디지털 자산을 제작, 관리 및 전달할 수 있습니다. InDesign 서버와 통합된 에셋 템플릿을 사용하면 마케팅 브로셔, 명함, 전단지, 광고 및 게시물 카드를 보다 손쉽게 제작할 수 있습니다. AEM을 사용하는 InDesign 서버의 구성은 이 섹션에서 다룹니다.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **은(는) INDD 템플릿이 업로드될 때 실행 중인 InDesign 서버에 연결되어 있어야 합니다.** INDD 파일의 초기 처리 중 일부는 InDesign 서버가 필요합니다.

## InDesign Server 시험버전 다운로드 {#download-indesign-server-trial}

[InDesign Server 시험버전 다운로드 웹 사이트](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html) 다운로드

## 시작 InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
