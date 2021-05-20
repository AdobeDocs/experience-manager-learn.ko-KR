---
title: 양식 데이터 모델을 사용하여 Campaign 프로필 만들기
seo-title: 양식 데이터 모델을 사용하여 Campaign 프로필 만들기
description: AEM Forms 양식 데이터 모델을 사용하여 Adobe Campaign Standard 프로필을 만드는 단계입니다
seo-description: AEM Forms 양식 데이터 모델을 사용하여 Adobe Campaign Standard 프로필을 만드는 단계입니다
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: 출력 서비스
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 3%

---


# 양식 데이터 모델 {#create-campaign-profile-using-form-data-model}을 사용하여 Campaign 프로필 만들기

AEM Forms 양식 데이터 모델을 사용하여 Adobe Campaign Standard 프로필을 만드는 단계입니다

## 사용자 지정 인증 만들기 {#create-custom-authentication}

Swagger 파일로 데이터 소스를 만들 때 AEM Forms에서는 다음과 같은 유형의 인증 유형을 지원합니다

* 없음
* OAuth 2.0
* 기본 인증
* API 키
* 사용자 정의 인증

![campaigfdm](assets/campaignfdm.gif)

Adobe Campaign Standard에 REST 호출을 하려면 사용자 지정 인증을 사용해야 합니다.

사용자 지정 인증을 사용하려면 IAuthentication 인터페이스를 구현하는 OSGi 구성 요소를 개발해야 합니다

getAuthDetails 메서드를 구현해야 합니다. 이 메서드는 AuthenticationDetails 개체를 반환합니다. 이 AuthenticationDetails 개체에는 Adobe Campaign에 REST API를 호출하는 데 필요한 필수 HTTP 헤더 세트가 있습니다.

다음은 사용자 지정 인증을 만드는 데 사용된 코드입니다. getAuthDetails 메서드가 모든 작업을 수행합니다. AuthenticationDetails 개체를 만듭니다. 그런 다음 이 개체에 적절한 HttpHeaders를 추가하고 이 개체를 반환합니다.

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## 데이터 소스 만들기 {#create-data-source}

첫 번째 단계는 swagger 파일을 만드는 것입니다. Swagger 파일은 Adobe Campaign Standard에서 프로필을 만드는 데 사용할 REST API를 정의합니다. swagger 파일은 REST API의 입력 매개 변수와 출력 매개 변수를 정의합니다.

swagger 파일을 사용하여 데이터 소스가 생성됩니다. 데이터 소스를 만들 때 인증 유형을 지정할 수 있습니다. 이 경우 사용자 지정 인증을 사용하여 Adobe Campaign에 대해 인증하려고 합니다. 위에 나열된 코드는 Adobe Campaign에 대해 인증하는 데 사용되었습니다.

이 문서와 관련된 자산의 일부로 샘플 Swagger 파일이 제공됩니다.**ACS 인스턴스와 일치하도록 swagger 파일의 호스트 및 basePath를 변경해야 합니다.**

## 솔루션 {#test-the-solution} 테스트

솔루션을 테스트하려면 다음 단계를 수행하십시오.
* [여기에 설명된 대로 단계를 따랐는지 확인합니다](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Swagger 파일을 가져오려면 이 파일을 다운로드하고 압축 해제합니다.](assets/create-acs-profile-swagger-file.zip)
* swagger 파일을 사용하여 데이터 소스 만들기
양식 데이터 모델을 만들고 이전 단계에서 만든 데이터 소스를 기반으로 합니다
* 이전 단계에서 만든 양식 데이터 모델을 기반으로 적응형 양식을 만듭니다.
* 데이터 소스 탭에서 다음 요소를 적응형 양식으로 끌어다 놓습니다

   * 이메일
   * 이름
   * 성
   * 휴대폰

* 제출 작업을 &quot;양식 데이터 모델을 사용하여 제출&quot;으로 구성합니다.
* 적절하게 제출하도록 데이터 모델을 구성합니다.
* 양식을 미리 봅니다. 필드를 작성하고 제출합니다.
* 프로필이 Adobe Campaign Standard에서 만들어졌는지 확인합니다.
