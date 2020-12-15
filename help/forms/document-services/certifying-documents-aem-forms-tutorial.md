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
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 1%

---


# AEM Forms의 문서 인증

인증된 문서는 PDF 문서 및 양식 수신자에게 문서의 신뢰성 및 무결성에 대한 추가 보장을 제공합니다.

문서를 인증하기 위해 데스크탑의 Acrobat DC 또는 AEM Forms Document Services를 서버의 자동화된 프로세스의 일부로 사용할 수 있습니다.

이 문서에서는 AEM Forms Document Services를 사용하여 pdf 문서를 인증하기 위해 샘플 OSGI 번들을 제공합니다.샘플에 사용된 코드는 여기에서 [사용할 수 있습니다](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

AEM Forms을 사용하여 문서를 인증하려면 다음 단계를 수행해야 합니다

## 트러스트 저장소 {#adding-certificate-to-trust-store}에 인증서를 추가하는 중

아래 절차에 따라 AEM의 키 저장소에 인증서를 추가하십시오.

* [전역 신뢰 저장소 초기화](http://localhost:4502/libs/granite/security/content/truststore.html)
* [fd-service 사용자 ](http://localhost:4502/security/users.html) 검색
* **fd-서비스 사용자를 찾기 위해 모든 사용자를 로드하려면 결과 페이지를 스크롤해야 합니다**
* fd 서비스 사용자를 두 번 클릭하여 사용자 설정 창을 엽니다.
* &quot;Add Private Key from the keystore file&quot;을 클릭합니다.인증서에 대한 별칭 및 암호를 지정합니다
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* 변경 내용 저장

## OSGI 서비스 만들기

직접 OSGi 번들을 작성하고 AEM Forms Client SDK를 사용하여 PDF 문서를 인증하는 서비스를 구현할 수 있습니다. 다음 링크는 OSGi 번들을 직접 작성하는 데 유용합니다

* [간단한 OSGi 번들 만들기](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [문서 서비스 API 사용](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

또는 이 자습서 에셋의 일부로 포함된 샘플 번들을 사용할 수 있습니다.

>[!NOTE]
>
>샘플 번들은 &quot;ares&quot;라는 별칭을 사용하여 문서를 인증합니다. 따라서 이 번들을 사용할 때 별칭이 &quot;ares&quot;인지 확인하십시오.

## 로컬 시스템에서 샘플 테스트

* [사용자 정의 문서 서비스 번들 다운로드 및 설치](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [서비스 사용자 번들로 개발](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) 다운로드 및 설치
* [Apache Sling Service User Mapper 서비스에 다음 항목을 추가해야 합니다.](http://localhost:4502/system/console/configMgr)

   **아래 스크린샷에 나와 있는 DevelopingWithServiceUser.core:getformsresourisresolver=fd-** services
   ![사용자 매퍼](assets/user-mapper-service.PNG)
* [샘플 적응형 양식 가져오기](assets/certify-pdf-af.zip)
* [사용자 정의 제출 가져오기 및 설치](assets/custom-submit-certify.zip)
* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 인증을 받아야 하는 PDF 문서 업로드
   **선택**  사항 - 문서 인증에 사용할 서명 필드를 지정합니다.
* 제출을 클릭합니다.
* 인증된 PDF를 반환해야 합니다.


