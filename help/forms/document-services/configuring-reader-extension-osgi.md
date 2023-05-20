---
title: AEM Forms OSGi에서 Reader 확장 구성
description: AEM Forms OSGi의 신뢰 저장소에 Reader 확장 자격 증명 추가
feature: Reader Extensions
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Reader 확장 자격 증명 추가{#configuring-reader-extension-osgi}

DocAssurance 서비스는 PDF 문서에 사용 권한을 적용할 수 있습니다. PDF 문서에 사용 권한을 적용하려면 인증서를 구성합니다.

## fd-service 사용자를 위한 키 저장소 만들기

Reader 확장 자격 증명은 fd-service 사용자와 연결됩니다. fd-service 사용자에게 자격 증명을 추가하려면 다음 단계를 따르십시오. fd-service 사용자에 대한 키 저장소를 이미 생성한 경우 이 섹션을 건너뜁니다.

* 관리자로 AEM 작성자 인스턴스에 로그인합니다
* 도구-보안-사용자로 이동
* fd-service 사용자 계정을 찾을 때까지 사용자 목록을 아래로 스크롤합니다
* fd-service 사용자를 클릭합니다.
* 키 저장소 탭을 클릭합니다.
* Create KeyStore 를 클릭합니다.
* KeyStore 액세스 암호를 설정하고 설정을 저장하여 KeyStore 암호를 만드십시오.

### fd-service 사용자 키 저장소에 자격 증명 추가

비디오의 지침에 따라 fd-service 사용자에게 자격 증명을 추가하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


pfx 파일의 세부 사항을 나열하는 명령은 입니다. 다음 명령은 사용자가 pfx 파일 과 동일한 디렉터리에 있다고 가정합니다.

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

예: keytool -v -list -storetype pkcs12 -keystore 1005566.pfx 여기서 1005566.pfx는 내 pfx 파일의 이름입니다.
