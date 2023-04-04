---
title: AEM에서 SSL 마법사 사용
description: Adobe Experience Manager의 SSL 설정 마법사를 사용하여 HTTPS에서 실행할 AEM 인스턴스를 쉽게 설정할 수 있습니다.
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# AEM에서 SSL 마법사 사용

Adobe Experience Manager의 SSL 설정 마법사를 사용하여 HTTPS에서 실행할 AEM 인스턴스를 쉽게 설정할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

를 엽니다. __SSL 구성 마법사__ 로 이동하여 직접 열 수 있습니다. __AEM 작성자 > 도구 > 보안 > SSL 구성__.

>[!NOTE]
>
>관리 환경의 경우 IT 부서에서 CA 보안 인증서 및 키를 제공하는 것이 가장 좋습니다.
>
>자체 서명된 인증서는 개발 목적으로만 사용됩니다.

## 개인 키 및 자체 서명된 인증서 다운로드

다음 zip에는 다음이 포함됩니다 [!DNL DER] 및 [!DNL CRT] localhost에서 AEM SSL을 설정하는 데 필요한 파일이며 로컬 개발용으로만 사용됩니다.

다음 [!DNL DER] 및 [!DNL CERT] 편의를 위해 파일이 제공되며 아래의 개인 키 및 자체 서명된 인증서 생성 섹션에 설명된 단계를 사용하여 생성됩니다.

필요한 경우 인증서 암호문은 **관리**.

localhost - 개인 키 및 자체 서명된 certificate.zip(2028년 7월 만료)

[인증서 파일 다운로드](assets/use-the-ssl-wizard/certificate.zip)

## 개인 키 및 자체 서명된 인증서 생성

위의 비디오에서는 자체 서명된 인증서를 사용하는 AEM 작성자 인스턴스에서 SSL의 설정 및 구성을 설명합니다. 아래 명령은 [[!DNL OpenSSL]](https://www.openssl.org/) 마법사의 2단계에서 사용할 개인 키 및 인증서를 생성할 수 있습니다.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
