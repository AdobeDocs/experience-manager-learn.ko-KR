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
duration: 564
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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

__AEM 작성자 > 도구 > 보안 > SSL 구성__(으)로 이동하여 __SSL 구성 마법사__&#x200B;를 엽니다.

![SSL 구성 마법사](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### 저장소 자격 증명 만들기

`ssl-service` 시스템 사용자 및 글로벌 _신뢰 저장소_&#x200B;와(과) 연결된 _키 저장소_&#x200B;를 만들려면 __자격 증명 저장__ 마법사 단계를 사용하십시오.

1. `ssl-service` 시스템 사용자와 연결된 __키 저장소__&#x200B;의 암호를 입력하고 확인하십시오.
1. 글로벌 __Trust Store__&#x200B;에 대한 암호를 입력하고 암호를 확인하십시오. 시스템 전체의 Trust Store이며 이미 만들어져 있으면 입력한 암호가 무시됩니다.

   ![SSL 설정 - 자격 증명 저장](assets/use-the-ssl-wizard/store-credentials.png)

### 개인 키 및 인증서 업로드

_개인 키_ 및 _SSL 인증서_&#x200B;를 업로드하려면 __키 및 인증서__ 마법사 단계를 사용하십시오.

일반적으로 IT 부서에서 CA 트러스트된 인증서와 키를 제공하지만 자체 서명된 인증서는 __개발__ 및 __테스트__ 목적으로 사용할 수 있습니다.

자체 서명된 인증서를 만들거나 다운로드하려면 [자체 서명된 개인 키 및 인증서](#self-signed-private-key-and-certificate)를 참조하십시오.

1. DER(Distinguished Encoding Rules) 형식으로 __개인 키__&#x200B;를 업로드합니다. PEM과 달리 DER로 인코딩된 파일에는 `-----BEGIN CERTIFICATE-----`과(와) 같은 일반 텍스트 문이 포함되어 있지 않습니다.
1. 연결된 __SSL 인증서__&#x200B;을(를) `.crt` 형식으로 업로드합니다.

   ![SSL 설정 - 개인 키 및 인증서](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### SSL 커넥터 세부 정보 업데이트

_호스트 이름_ 및 _포트_&#x200B;을(를) 업데이트하려면 __SSL 커넥터__ 마법사 단계를 사용하십시오.

1. __HTTPS 호스트 이름__ 값을 업데이트하거나 확인하십시오. 인증서의 `Common Name (CN)`과(와) 일치해야 합니다.
1. __HTTPS 포트__ 값을 업데이트하거나 확인하십시오.

   ![SSL 설정 - SSL 커넥터 세부 정보](assets/use-the-ssl-wizard/ssl-connector-details.png)

### SSL 설정 확인

1. SSL을 확인하려면 __HTTPS URL로 이동__ 단추를 클릭합니다.
1. 자체 서명된 인증서를 사용하는 경우 `Your connection is not private` 오류가 표시됩니다.

   ![SSL 설정 - HTTPS에서 AEM 확인](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## 자체 서명된 개인 키 및 인증서

다음 zip에는 로컬로 AEM SSL을 설정하는 데 필요한 [!DNL DER] 및 [!DNL CRT] 파일이 포함되어 있으며 로컬 개발 목적으로만 사용됩니다.

편의를 위해 [!DNL DER] 및 [!DNL CERT] 파일이 제공되며, 아래의 개인 키 및 자체 서명된 인증서 생성 섹션에 설명된 단계를 사용하여 생성됩니다.

필요한 경우 인증서 암호는 **admin**&#x200B;입니다.

이 localhost - 개인 키 및 자체 서명된 certificate.zip(2028년 7월 만료)

[인증서 파일 다운로드](assets/use-the-ssl-wizard/certificate.zip)

### 개인 키 및 자체 서명된 인증서 생성

위의 비디오는 자체 서명된 인증서를 사용하는 AEM 작성자 인스턴스에 대한 SSL의 설정 및 구성을 보여 줍니다. [[!DNL OpenSSL]](https://www.openssl.org/)을(를) 사용하는 아래 명령은 마법사의 2단계에서 사용할 개인 키와 인증서를 생성할 수 있습니다.

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
