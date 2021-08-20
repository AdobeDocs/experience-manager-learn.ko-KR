---
title: AEM Forms 6.3 및 6.4에서 Salesforce로 DataSource 구성
description: 양식 데이터 모델을 사용하여 AEM Forms과 Salesforce 통합
feature: 적응형 Forms, 양식 데이터 모델
topics: integrations
version: 6.3,6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---


# AEM Forms 6.3 및 6.4에서 Salesforce로 DataSource 구성{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 전제 조건 {#prerequisites}

이 문서에서는 Salesforce를 사용하여 데이터 소스를 만드는 프로세스를 살펴봅니다

이 자습서를 위한 사전 요구 사항:

* 이 페이지 하단으로 스크롤하여 swagger 파일을 다운로드하고 하드 드라이브에 저장합니다.
* SSL을 사용하도록 설정된 AEM Forms

   * [AEM 6.3에서 SSL을 사용하기 위한 공식 설명서](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [AEM 6.4에서 SSL을 사용하기 위한 공식 설명서](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Salesforce 계정이 있어야 합니다.
* 연결된 앱을 만들어야 합니다. 앱을 만들기 위한 Salesforce의 공식 설명서가 [여기](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0)에 나열되어 있습니다.
* 앱에 적절한 OAuth 범위를 제공합니다(테스트 목적으로 사용 가능한 모든 OAuth 범위를 선택함)
* 콜백 URL을 제공합니다. 내 경우의 콜백 URL은

   * **AEM Forms 6.3**&#x200B;을 사용하는 경우 콜백 URL은 https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html입니다. 이 URL에서 createload는 내 양식 데이터 모델의 이름입니다.

   * ** AEM Forms 6.4**를 사용하는 경우 콜백 URL은 [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)입니다.

이 예에서 gbedekar -w7-1:6443은 AEM이 실행 중인 서버 및 포트의 이름입니다.

연결된 앱을 만들었으면 **소비자 키 및 비밀 키**&#x200B;를 메모하십시오. AEM Forms에서 데이터 소스를 만들 때 이러한 권한이 필요합니다.

연결된 앱을 만들었으므로 이제 salesforce에서 수행해야 하는 작업을 위해 Swagger 파일을 만들어야 합니다. 다운로드 가능한 자산의 일부로 샘플 스웨거 파일이 포함됩니다. 이 Swagger 파일을 사용하면 적응형 양식 제출 시 &quot;Lead&quot; 개체를 만들 수 있습니다. 이 swagger 파일을 살펴보십시오.

다음 단계는 AEM Forms에서 데이터 소스를 만드는 것입니다. AEM Forms 버전에 따라 다음 단계를 수행하십시오

## AEM Forms 6.3 {#aem-forms}

* https 프로토콜을 사용하여 AEM Forms에 로그인합니다
* https://&lt;servername>:&lt;serverport> /etc/cloudservices.html에 입력하여 클라우드 서비스로 이동합니다(예: https://gbedekar-w7-1:6443/etc/cloudservices.html).
* 아래로 스크롤하여 &quot;양식 데이터 모델&quot;로 이동합니다.
* &quot;구성 표시&quot;를 클릭합니다.
* 새 구성을 추가하려면 &quot;+&quot;를 클릭합니다
* &quot;Rest Full Service&quot;를 선택합니다. 구성에 의미 있는 제목과 이름을 제공합니다. 예,

   * 이름: CreateLeadInSalesForce
   * 제목: CreateLeadInSalesForce

* &quot;만들기&quot;를 클릭합니다

**다음 화면에서 **

* Swagger 소스 파일의 옵션으로 &quot;파일&quot;을 선택합니다. 이전에 다운로드한 파일을 찾습니다
* 인증 유형을 OAuth2.0으로 선택합니다.
* ClientID 및 Client Secret 값을 제공합니다.
* OAuth Url은 - **https://login.salesforce.com/services/oauth2/authorize**
* 새로 고침 토큰 Url - **https://na5.salesforce.com/services/oauth2/token**
* **액세스 토큰 Url - https://na5.salesforce.com/services/oauth2/token**
* 권한 부여 범위: ** api   chatter_api 전체 id   openid   refresh_token visualforce web**
* 인증 처리기: 권한 부여 베어러
* &quot;Connect To OAUTH&quot;를 클릭합니다.모든 것이 제대로 작동하면 오류가 표시되지 않습니다

Salesforce를 사용하여 양식 데이터 모델을 만들었으면 방금 만든 데이터 소스를 사용하여 양식 데이터 통합을 만들 수 있습니다. 양식 데이터 통합을 만들기 위한 공식 설명서는 [여기](https://helpx.adobe.com/aem-forms/6-3/data-integration.html)입니다.

SFDC에서 리드 개체를 만들기 위해 POST 서비스를 포함하도록 양식 데이터 모델을 구성해야 합니다.

또한 리드 개체에 대해 읽기 및 쓰기 서비스를 구성해야 합니다. 이 페이지 하단에 있는 스크린샷을 참조하십시오.

양식 데이터 모델을 만든 후 이 모델을 기반으로 적응형 Forms을 만들고 양식 데이터 모델 제출 메서드를 사용하여 SFDC에서 리드를 만들 수 있습니다.

## AEM Forms 6.4 {#aem-forms-1}

* 데이터 소스 만들기

   * [Data Sources로 이동](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * &quot;만들기&quot; 단추를 클릭합니다
   * 몇 가지 의미 있는 값 제공

      * 이름: CreateLeadInSalesForce
      * 제목: CreateLeadInSalesForce
      * 서비스 유형: RESTful 서비스
   * 다음을 클릭합니다
   * Swagger 소스: 파일
   * 이전 단계에서 다운로드한 swagger 파일을 찾아 선택합니다
   * 인증 유형: OAuth 2.0. 다음 값을 지정합니다
   * ClientID 및 Client Secret 값을 제공합니다.
   * OAuth Url은 - **https://login.salesforce.com/services/oauth2/authorize**
   * 새로 고침 토큰 Url - **https://na5.salesforce.com/services/oauth2/token**
   * 액세스 토큰 Url **l - https://na5.salesforce.com/services/oauth2/token**
   * 권한 부여 범위: ** api chatter_api 전체 id openid refresh_token visualforce web**
   * 인증 처리기: 권한 부여 베어러
   * &quot;OAuth에 연결&quot; 단추를 클릭합니다. 오류가 발생하면 이전 단계를 검토하여 모든 정보가 정확하게 입력되었는지 확인하십시오.


SalesForce를 사용하여 데이터 소스를 만들고 나면 방금 만든 데이터 소스를 사용하여 양식 데이터 통합을 만들 수 있습니다. 에 대한 설명서 링크는 [여기](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)입니다.

SFDC에서 리드 개체를 만들기 위해 POST 서비스를 포함하도록 양식 데이터 모델을 구성해야 합니다.

또한 리드 개체에 대해 읽기 및 쓰기 서비스를 구성해야 합니다. 이 페이지 하단에 있는 스크린샷을 참조하십시오.

양식 데이터 모델을 만든 후 이 모델을 기반으로 적응형 Forms을 만들고 양식 데이터 모델 제출 메서드를 사용하여 SFDC에서 리드를 만들 수 있습니다.

>[!NOTE]
>
>swagger 파일의 URL이 영역에 해당하는지 확인합니다. 예를 들어, 샘플 swagger 파일의 url은 북미 지역에 계정을 만들 때 &quot;na46.salesforce.com&quot;입니다. 가장 쉬운 방법은 Salesforce 계정에 로그인하고 url 을 확인하는 것입니다.

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
