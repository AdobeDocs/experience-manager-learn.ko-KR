---
title: AEM Forms 6.3 및 6.4에서 Salesforce를 사용하여 DataSource 구성
description: 양식 데이터 모델을 사용하여 AEM Forms과 Salesforce 통합
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
duration: 202
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# AEM Forms 6.3 및 6.4에서 Salesforce를 사용하여 DataSource 구성{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 사전 요구 사항 {#prerequisites}

이 문서에서는 Salesforce를 사용하여 데이터 소스를 만드는 프로세스를 살펴봅니다

이 자습서의 사전 요구 사항:

* 이 페이지 하단으로 스크롤하여 Swagger 파일을 다운로드하고 하드 드라이브에 저장하십시오.
* SSL이 활성화된 AEM Forms

   * [AEM 6.3에서 SSL을 활성화하는 공식 설명서](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [AEM 6.4에서 SSL을 활성화하는 공식 설명서](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Salesforce 계정이 있어야 합니다.
* 연결된 앱을 만들어야 합니다. Salesforce의 앱 만들기에 대한 공식 설명서 양식이 나열됩니다 [여기](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* 앱에 적절한 OAuth 범위를 제공합니다(테스트 목적으로 사용 가능한 모든 OAuth 범위를 선택함).
* 콜백 URL을 제공합니다. 내 경우 콜백 URL은

   * 을 사용하는 경우 **AEM Forms 6.3**, 콜백 URL은 https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html입니다. 이 URL에서 createlead 는 내 양식 데이터 모델의 이름입니다.

   * AEM Forms 6**4**를 사용하는 경우 콜백 URL은 https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html입니다.

이 예에서 gbedekar -w7-1:6443은 AEM이 실행 중인 내 서버 및 포트의 이름입니다.

연결된 앱을 만들었으면 다음을 참고하십시오. **소비자 키 및 암호 키**. AEM Forms에서 데이터 소스를 만들 때 필요합니다.

연결된 앱을 만들었으므로 이제 Salesforce에서 수행해야 하는 작업에 대한 Swagger 파일을 만들어야 합니다. 샘플 swagger 파일이 다운로드 가능한 에셋의 일부로 포함됩니다. 이 Swagger 파일을 사용하면 적응형 양식 제출에 대한 &quot;잠재 고객&quot; 개체를 만들 수 있습니다. 이 Swagger 파일을 살펴보십시오.

다음 단계는 AEM Forms에서 데이터 소스를 만드는 것입니다. AEM Forms 버전에 따라 다음 단계를 따르십시오

## AEM Forms 6.3 {#aem-forms}

* https 프로토콜을 사용하여 AEM Forms에 로그인
* https://을 입력하여 클라우드 서비스로 이동합니다.&lt;servername>:&lt;serverport> /etc/cloudservices.html (예: https://gbedekar-w7-1:6443/etc/cloudservices.html)
* &quot;양식 데이터 모델&quot;로 스크롤합니다.
* &quot;구성 표시&quot;를 클릭합니다.
* 새 구성을 추가하려면 &quot;+&quot;를 클릭하십시오.
* &quot;Rest Full Service&quot;를 선택합니다. 구성에 의미 있는 제목 및 이름 을 입력합니다. 예,

   * 이름: CreateLeadInSalesForce
   * 제목: CreateLeadInSalesForce

* &quot;만들기&quot; 클릭

**다음 화면 **

* Swagger 소스 파일에 대한 옵션으로 &quot;파일&quot;을 선택합니다. 이전에 다운로드한 파일을 찾습니다.
* 인증 유형을 OAuth2.0으로 선택
* ClientID 및 클라이언트 암호 값 제공
* OAuth Url은 - **https://login.salesforce.com/services/oauth2/authorize**
* 새로 고침 토큰 URL - **https://na5.salesforce.com/services/oauth2/token**
* **액세스 토큰 URL - https://na5.salesforce.com/services/oauth2/token**
* 인증 범위: ** api chatter_api 전체 id openid refresh_token visualforce 웹**
* 인증 핸들러: 권한 부여 전달자
* &quot;OAUTH에 연결 &quot;.모든 것이 잘 작동하면 오류가 표시되지 않습니다

Salesforce를 사용하여 양식 데이터 모델을 만들면 방금 만든 데이터 소스를 사용하여 양식 데이터 통합을 만들 수 있습니다. 양식 데이터 통합 작성을 위한 공식 설명서는 다음과 같습니다 [여기](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

SFDC에서 리드 개체를 만들려면 POST 서비스를 포함하도록 양식 데이터 모델을 구성해야 합니다.

리드 개체에 대해 읽기 및 쓰기 서비스를 구성해야 합니다. 이 페이지 하단에 있는 스크린샷을 참조하십시오.

양식 데이터 모델을 만든 후 이 모델을 기반으로 적응형 Forms을 만들고 양식 데이터 모델 제출 방법을 사용하여 SFDC에서 잠재 고객을 만들 수 있습니다.

## AEM Forms 6.4 {#aem-forms-1}

* 데이터 소스 만들기

   * [데이터 소스로 이동](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * &quot;만들기&quot; 단추 클릭
   * 의미 있는 값 제공

      * 이름: CreateLeadInSalesForce
      * 제목: CreateLeadInSalesForce
      * 서비스 유형: RESTful 서비스

   * 다음 을 클릭합니다
   * Swagger 소스: 파일
   * 이전 단계에서 다운로드한 Swagger 파일을 찾아서 선택합니다
   * 인증 유형: OAuth 2.0. 다음 값을 지정하십시오.
   * ClientID 및 클라이언트 암호 값 제공
   * OAuth Url은 - **https://login.salesforce.com/services/oauth2/authorize**
   * 새로 고침 토큰 URL - **https://na5.salesforce.com/services/oauth2/token**
   * 액세스 토큰 Url **l - https://na5.salesforce.com/services/oauth2/token**
   * 인증 범위: ** api chatter_api 전체 id openid refresh_token visualforce 웹**
   * 인증 핸들러: 권한 부여 전달자
   * &quot;OAuth에 연결&quot; 버튼을 클릭합니다. 오류가 표시되면 이전 단계를 검토하여 모든 정보가 정확하게 입력되었는지 확인하십시오.

SalesForce를 사용하여 데이터 소스를 만든 다음 방금 만든 데이터 소스를 사용하여 양식 데이터 통합을 만들 수 있습니다. 에 대한 설명서 링크는 다음과 같습니다 [여기](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

SFDC에서 리드 개체를 만들려면 POST 서비스를 포함하도록 양식 데이터 모델을 구성해야 합니다.

리드 개체에 대해 읽기 및 쓰기 서비스를 구성해야 합니다. 이 페이지 하단에 있는 스크린샷을 참조하십시오.

양식 데이터 모델을 만든 후 이 모델을 기반으로 적응형 Forms을 만들고 양식 데이터 모델 제출 방법을 사용하여 SFDC에서 잠재 고객을 만들 수 있습니다.

>[!NOTE]
>
>Swagger 파일의 URL이 지역에 해당하는지 확인합니다. 예를 들어 샘플 swagger 파일의 URL은 계정이 북미에서 만들어졌으므로 &quot;na46.salesforce.com&quot;입니다. 가장 쉬운 방법은 Salesforce 계정에 로그인하고 url 을 확인하는 것입니다.

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[샘플 Swagger 파일](assets/swagger-sales-force-lead.json)
