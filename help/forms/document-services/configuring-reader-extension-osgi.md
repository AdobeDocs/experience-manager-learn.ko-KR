---
title: AEM Forms OSGi에서 Reader 확장 구성
description: AEM Forms OSGi의 신뢰 저장소에 Reader 확장 자격 증명을 추가합니다
feature: Reader 확장
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: 관리
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---


# Reader 확장 자격 증명 추가{#configuring-reader-extension-osgi}

DocAssurance 서비스는 PDF 문서에 사용 권한을 적용할 수 있습니다. 사용 권한을 PDF 문서에 적용하려면 인증서를 구성합니다.

## fd 서비스 사용자를 위한 키 저장소 만들기

판독기 확장 자격 증명이 fd 서비스 사용자와 연결되어 있습니다. fd 서비스 사용자에게 자격 증명을 추가하려면 다음 단계를 따르십시오. fd 서비스 사용자에 대한 키 저장소를 이미 만든 경우 이 섹션을 건너뜁니다

* 관리자로 AEM 작성자 인스턴스에 로그인합니다
* Tools-Security-Users로 이동
* fd-service 사용자 계정을 찾을 때까지 사용자 목록을 아래로 스크롤합니다.
* fd-service 사용자를 클릭합니다.
* 키 저장소 탭을 클릭합니다.
* 키 저장소 만들기를 클릭합니다.
* KeyStore 액세스 암호를 설정하고 설정을 저장하여 KeyStore 암호를 만듭니다

### fd 서비스 사용자 키 저장소에 자격 증명 추가

비디오에 따라 fd 서비스 사용자에게 자격 증명을 추가하십시오

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)











