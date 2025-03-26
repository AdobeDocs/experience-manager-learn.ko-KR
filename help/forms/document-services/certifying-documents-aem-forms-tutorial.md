---
title: AEM Forms의 문서 인증
description: Docassurance 서비스를 사용하여 AEM Forms에서 PDF 문서 인증
feature: Document Security
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# AEM Forms의 문서 인증

인증된 문서는 PDF 문서 및 양식 수신자에게 신뢰성 및 무결성을 추가로 보장합니다.

문서를 인증하려면 데스크탑의 Acrobat DC 또는 AEM Forms Document Services를 서버의 자동화된 프로세스의 일부로 사용할 수 있습니다.

이 문서에서는 AEM Forms Document Services를 사용하여 pdf 문서를 인증하는 샘플 OSGI 번들을 제공합니다. 샘플에 사용된 코드는 [여기에서 사용 가능](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)합니다.

AEM Forms을 사용하여 문서를 인증하려면 다음 단계를 수행해야 합니다

## 인증서를 신뢰 저장소에 추가하는 중 {#adding-certificate-to-trust-store}

AEM의 키 저장소에 인증서를 추가하려면 아래 단계를 따르십시오.

* [글로벌 신뢰 저장소 초기화](http://localhost:4502/libs/granite/security/content/truststore.html)
* [fd-service 검색](http://localhost:4502/security/users.html) 사용자
* **모든 사용자를 로드하여 fd-service 사용자를 찾으려면 결과 페이지를 스크롤해야 합니다**
* fd-service 사용자를 두 번 클릭하여 사용자 설정 창을 엽니다
* &quot;키 저장소 파일에서 개인 키 추가&quot;를 클릭합니다.인증서와 관련된 별칭 및 암호를 지정합니다
  ![인증서 추가](assets/adding-certificate-keystore.PNG)
* 변경 사항 저장

## OSGI 서비스 만들기

고유한 OSGi 번들을 작성하고 AEM Forms 클라이언트 SDK 를 사용하여 PDF 문서를 인증하는 서비스를 구현할 수 있습니다. 다음 링크는 자체 OSGi 번들을 작성하는 데 유용합니다

* [첫 번째 OSGi 번들 만들기](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [문서 서비스 API 사용](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

또는 이 튜토리얼 에셋의 일부로 포함된 샘플 번들을 사용할 수 있습니다.

>[!NOTE]
>
>샘플 번들은 &quot;ares&quot;라는 별칭을 사용하여 문서를 인증합니다. 따라서 이 번들을 사용할 때 별칭을 &quot;ares&quot;라고 하는지 확인하십시오.

## 로컬 시스템에서 샘플 테스트

* [사용자 지정 문서 서비스 번들](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) 다운로드 및 설치
* [서비스 사용자 번들로 개발](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)을 다운로드하여 설치하십시오.
* [Apache Sling 서비스 사용자 매퍼 서비스에 다음 항목을 추가했는지 확인하십시오](http://localhost:4502/system/console/configMgr)
  아래 스크린샷에 표시된 대로 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
  ![사용자 매퍼](assets/user-mapper-service.PNG)
* [샘플 적응형 양식 가져오기](assets/certify-pdf-af.zip)
* [사용자 정의 제출 가져오기 및 설치](assets/custom-submit-certify.zip)
* [적응형 양식 열기](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 인증해야 하는 PDF 문서 업로드
  **선택 사항** - 문서 인증에 사용할 서명 필드를 지정합니다.
* 제출을 누릅니다.
* 인증된 PDF은 귀하에게 반환되어야 합니다.
