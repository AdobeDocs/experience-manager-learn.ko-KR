---
title: AEM Assets 및 InDesign Server을 사용하여 자산 템플릿 설정
description: 자산 템플릿을 사용하면 마케터가 디지털 및 인쇄용 디지털 자산을 생성, 관리 및 전달할 수 있습니다. InDesign 서버와 통합하여 자산 템플릿을 사용하면 마케팅 브로셔, 명함, 전단, 광고 및 게시물 카드를 보다 손쉽게 만들 수 있습니다. AEM을 사용한 InDesign 서버 구성은 이 섹션에서 다룹니다.
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 1%

---


# AEM Assets 및 InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}을 사용하여 자산 템플릿 설정

자산 템플릿을 사용하면 마케터가 디지털 및 인쇄용 디지털 자산을 생성, 관리 및 전달할 수 있습니다. InDesign 서버와 통합하여 자산 템플릿을 사용하면 마케팅 브로셔, 명함, 전단, 광고 및 게시물 카드를 보다 손쉽게 만들 수 있습니다. AEM을 사용한 InDesign 서버 구성은 이 섹션에서 다룹니다.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **은(는) INDD 템플릿을 업로드할 때 실행 중인 InDesign 서버에 연결되어 있어야 합니다.** INDD 파일에서 초기 처리 과정의 일부에는 InDesign 서버가 필요합니다.

## InDesign Server 체험판 다운로드 {#download-indesign-server-trial}

[InDesign Server 평가판 다운로드 웹 사이트](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html) 다운로드

## InDesign Server {#starting-indesign-server} 시작

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
