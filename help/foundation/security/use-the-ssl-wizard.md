---
title: AEM에서 SSL 마법사 사용
description: Adobe Experience Manager의 SSL 설정 마법사를 사용하여 AEM 인스턴스를 설정하여 HTTPS를 통해 쉽게 실행할 수 있습니다.
seo-description: Adobe Experience Manager의 SSL 설정 마법사를 사용하여 AEM 인스턴스를 설정하여 HTTPS를 통해 쉽게 실행할 수 있습니다.
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: 보안
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# AEM에서 SSL 마법사 사용

Adobe Experience Manager의 SSL 설정 마법사를 사용하여 AEM 인스턴스를 설정하여 HTTPS를 통해 쉽게 실행할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>관리 환경에서는 IT 부서에서 CA 보안 인증서 및 키를 제공하는 것이 가장 좋습니다.
>
>자체 서명된 인증서는 개발 용도로만 사용됩니다.

## 개인 키 및 자체 서명된 인증서 다운로드

다음 zip 파일에는 로컬 호스트에 AEM SSL을 설정하는 데 필요한 [!DNL DER] 및 [!DNL CRT] 파일이 들어 있으며 로컬 개발 용도로만 사용할 수 있습니다.

[!DNL DER] 및 [!DNL CERT] 파일은 편의를 위해 제공되며 아래의 개인 키 및 자체 서명된 인증서 생성 섹션에 설명된 단계에 따라 생성됩니다.

필요한 경우 인증서 전달 구문은 **admin**&#x200B;입니다.

localhost - 개인 키 및 자체 서명된 인증서.zip(2028년 7월 만료)

[인증서 파일 다운로드](assets/use-the-ssl-wizard/certificate.zip)

## 개인 키 및 자체 서명된 인증서 생성

위의 비디오에서는 자체 서명된 인증서를 사용하여 AEM 작성자 인스턴스에서 SSL의 설정 및 구성을 설명합니다. [[!DNL OpenSSL]](https://www.openssl.org/)을 사용하는 아래 명령은 마법사의 2단계에서 사용할 개인 키와 인증서를 생성할 수 있습니다.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
