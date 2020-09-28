---
title: AEM Forms의 문서 인증
seo-title: AEM Forms의 문서 인증
description: Docassurance 서비스를 사용하여 AEM Forms에서 PDF 문서 인증
seo-description: Docassurance 서비스를 사용하여 AEM Forms에서 PDF 문서 인증
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: document-security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---


# AEM Forms의 문서 인증

인증된 문서는 PDF 문서와 양식 수신자에게 문서의 신뢰성 및 무결성에 대한 추가 보장을 제공합니다.

문서를 인증하기 위해 데스크탑의 Acrobat DC 또는 서버의 자동화된 프로세스의 일부로 AEM Forms 다큐멘트 서비스를 사용할 수 있습니다.

이 문서에서는 AEM Forms 다큐멘트 서비스를 사용하여 pdf 문서를 인증하는 샘플 OSGI 번들을 제공합니다.샘플에 사용된 코드는 여기에서 [확인할 수 있습니다](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

AEM Forms을 사용하여 문서를 인증하려면 다음 단계를 수행해야 합니다

## 트러스트 저장소에 인증서 추가 {#adding-certificate-to-trust-store}

AEM의 키 저장소에 인증서를 추가하려면 아래 단계를 따르십시오

* [전역 트러스트 저장소 초기화](http://localhost:4502/libs/granite/security/content/truststore.html)
* [fd-service](http://localhost:4502/security/users.html) 사용자 검색
* **모든 사용자를 로드하여 fd-서비스 사용자를 찾으려면 결과 페이지를 스크롤해야 합니다**
* fd-service 사용자를 두 번 클릭하여 사용자 설정 창을 엽니다
* &quot;키 저장소 파일에서 개인 키 추가&quot;를 클릭합니다.인증서에 대한 별칭 및 암호를 지정합니다
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* 변경 내용 저장

## OSGI 서비스 만들기

직접 OSGi 번들을 작성하고 AEM Forms 클라이언트 SDK를 사용하여 PDF 문서를 인증하는 서비스를 구현할 수 있습니다. 다음 링크를 사용하면 OSGi 번들을 직접 작성할 수 있습니다

* [간단한 OSGi 번들 만들기](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Document Service API 사용](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

또는 이 자습서 에셋의 일부로 포함된 샘플 번들을 사용할 수 있습니다.
>[!NOTE]
샘플 번들은 &quot;ares&quot;라는 별칭을 사용하여 문서를 인증합니다. 따라서 이 번들을 사용할 때 별칭이 &quot;ares&quot;인지 확인하십시오.

## 로컬 시스템에서 샘플 테스트

* 맞춤형 문서 서비스 [번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 서비스 사용자 번들을 [사용한 개발 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Apache Sling Service User Mapper 서비스에 다음 항목을 추가했는지 확인하십시오](http://localhost:4502/system/console/configMgr)

   **아래 스크린샷에 나와 있는 것처럼 DevelopingWithServiceUser.core:getformsresourisresolver=fd-service**
   ![User-Mapper](assets/user-mapper-service.PNG)
* [샘플 적응형 양식 가져오기](assets/certify-pdf-af.zip)
* [사용자 지정 제출 가져오기 및 설치](assets/custom-submit-certify.zip)
* [응용 양식 열기](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 인증을 받아야 하는 PDF 문서 업로드
   **선택** 사항 - 문서 인증에 사용할 서명 필드를 지정합니다.
* 제출을 클릭합니다.
* 인증된 PDF를 반환해야 합니다.


