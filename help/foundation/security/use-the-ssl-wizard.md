---
title: AEM에서 SSL 마법사 사용
description: HTTPS를 통해 실행할 AEM 인스턴스를 더 쉽게 설정할 수 있는 Adobe Experience Manager의 SSL 설정 마법사.
version: 6.5, Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 595
source-git-commit: 153ce0ff2d433bdbce05ef7fba4856356c0f6d4c
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# AEM에서 SSL 마법사 사용

기본 제공 SSL 마법사를 사용하여 HTTPS를 통해 실행하도록 Adobe Experience Manager에서 SSL을 설정하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>관리되는 환경의 경우 IT 부서에서 CA 보안 인증서 및 키를 제공하는 것이 가장 좋습니다.
>
>자체 서명된 인증서는 개발 목적으로만 사용됩니다.

## SSL 구성 마법사 사용

다음으로 이동 __AEM Author > 도구 > 보안 > SSL 구성__&#x200B;를 클릭하고 __SSL 구성 마법사__.

![SSL 구성 마법사](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### 저장소 자격 증명 만들기

을(를) 만들려면 _키 저장소_ 과(와) 연계됨 `ssl-service` 시스템 사용자 및 전역 _Trust Store_, 사용 __자격 증명 저장__ 마법사 단계입니다.

1. 암호 입력 및 암호 확인 __키 저장소__ 과(와) 연계됨 `ssl-service` 시스템 사용자입니다.
1. 암호를 입력하고 글로벌 암호 확인 __Trust Store__. 시스템 전체의 Trust Store이며 이미 만들어져 있으면 입력한 암호가 무시됩니다.

   ![SSL 설정 - 자격 증명 저장](assets/use-the-ssl-wizard/store-credentials.png)

### 개인 키 및 인증서 업로드

업로드 _개인 키_ 및 _SSL 인증서_, 사용 __키 및 인증서__ 마법사 단계입니다.

일반적으로 IT 부서에서는 CA 트러스트된 인증서와 키를 제공하지만 자체 서명된 인증서는 __개발__ 및 __테스트__ 목적.

자체 서명된 인증서를 만들거나 다운로드하려면 [자체 서명된 개인 키 및 인증서](#self-signed-private-key-and-certificate).

1. 업로드 __개인 키__ DER(Distinguished Encoding Rules) 형식에서 참조할 수 있습니다. PEM과 달리 DER로 인코딩된 파일에는 다음과 같은 일반 텍스트 문이 포함되지 않습니다. `-----BEGIN CERTIFICATE-----`
1. 관련 항목 업로드 __SSL 인증서__ 다음에서 `.crt` 포맷.

   ![SSL 설정 - 개인 키 및 인증서](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### SSL 커넥터 세부 정보 업데이트

를 업데이트하려면 _호스트 이름_ 및 _포트_ 사용 __SSL 커넥터__ 마법사 단계입니다.

1. 업데이트 또는 확인 __HTTPS 호스트 이름__ 값과 일치해야 합니다. `Common Name (CN)` 인증서에서 가져옵니다.
1. 업데이트 또는 확인 __HTTPS 포트__ 값.

   ![SSL 설정 - SSL 커넥터 세부 정보](assets/use-the-ssl-wizard/ssl-connector-details.png)

### SSL 설정 확인

1. SSL을 확인하려면 __HTTPS URL로 이동__ 단추를 클릭합니다.
1. 자체 서명된 인증서를 사용하는 경우 다음과 같이 표시됩니다. `Your connection is not private` 오류.

   ![SSL 설정 - HTTPS에서 AEM 확인](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## 자체 서명된 개인 키 및 인증서

다음 zip에는 [!DNL DER] 및 [!DNL CRT] 로컬에서 AEM SSL을 설정하는 데 필요한 파일이며 로컬 개발 목적으로만 사용됩니다.

다음 [!DNL DER] 및 [!DNL CERT] 파일은 편의상 제공되며, 아래의 개인 키 및 자체 서명된 인증서 생성 섹션에 설명된 단계를 따라 생성됩니다.

필요한 경우 인증서 암호는 입니다. **admin**.

이 localhost - 개인 키 및 자체 서명된 certificate.zip(2028년 7월 만료)

[인증서 파일 다운로드](assets/use-the-ssl-wizard/certificate.zip)

### 개인 키 및 자체 서명된 인증서 생성

위의 비디오는 자체 서명된 인증서를 사용하는 AEM 작성자 인스턴스에 대한 SSL의 설정 및 구성을 보여 줍니다. 아래 명령은 [[!DNL OpenSSL]](https://www.openssl.org/) 은 마법사의 2단계에서 사용할 개인 키 및 인증서를 생성할 수 있습니다.

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
