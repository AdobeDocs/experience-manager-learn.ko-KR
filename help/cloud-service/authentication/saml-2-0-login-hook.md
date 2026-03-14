---
title: SAML 2.0 사용자 정의 로그인 후크
description: AEM용 사용자 지정 SAML 2.0 로그인 후크를 개발하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# SAML 2.0 로그인 후크

AEM용 사용자 지정 SAML 2.0 로그인 후크를 개발하는 방법에 대해 알아봅니다. 이 자습서에서는 사용자가 SAML 자격 증명을 사용하여 인증할 수 있도록 SAML 2.0 ID 공급자와 통합하는 사용자 지정 로그인 후크를 만드는 단계별 지침을 제공합니다.

IDP가 SAML 어설션의 사용자 프로필 데이터 및 사용자 그룹 멤버십을 전송할 수 없거나 AEM에 동기화하기 전에 데이터를 변환해야 하는 경우 SAML 인증 프로세스를 확장하기 위해 사용자 지정 SAML 후크를 구현할 수 있습니다. SAML 후크를 사용하면 그룹 멤버십 할당을 사용자 지정하고, 사용자 프로필 속성을 수정하고, 인증 흐름 동안 사용자 지정 비즈니스 논리를 추가할 수 있습니다.

>[!NOTE]
>사용자 지정 SAML 후크는 **AEM as a Cloud Service** 및 **AEM LTS**&#x200B;에서 지원됩니다. 이전 AEM 버전에서는 이 기능을 사용할 수 없습니다.

## 일반적인 사용 사례

사용자 정의 SAML 후크는 다음과 같은 경우에 유용합니다.

+ SAML 어설션에 제공된 것 이상의 사용자 정의 비즈니스 논리에 따라 그룹 멤버십을 동적으로 할당
+ AEM에 동기화되기 전에 사용자 프로필 데이터 변형 또는 보강
+ 복잡한 SAML 속성 구조를 AEM 사용자 속성에 매핑
+ 사용자 지정 권한 부여 규칙 또는 조건부 그룹 할당 구현
+ SAML 인증 중 사용자 정의 로깅 또는 감사 추가
+ 인증 프로세스 중 외부 시스템과 통합

## `SamlHook` OSGi 서비스 인터페이스

`com.adobe.granite.auth.saml.spi.SamlHook` 인터페이스는 SAML 인증 프로세스의 여러 단계에서 호출되는 두 개의 후크 메서드를 제공합니다.

### `postSamlValidationProcess()` 메서드

이 메서드는 SAML 응답의 유효성을 검사했지만 사용자 동기화 프로세스가 시작되는 **이전**&#x200B;에 **이후**&#x200B;라고 합니다. 속성 추가 또는 변형과 같이 SAML 어설션 데이터를 수정하기에 이상적인 위치입니다.

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### 사용 사례

+ 어설션에 추가 그룹 멤버십 추가
+ 특성 값을 동기화하기 전에 변형합니다.
+ 외부 소스의 데이터로 어설션 보강
+ 사용자 정의 비즈니스 규칙 유효성 검사

### `postSyncUserProcess()` 메서드

이 메서드는 사용자 동기화 프로세스가 완료된 후 **after**&#x200B;에 호출됩니다. 이 후크는 AEM 사용자가 생성되거나 업데이트된 후 추가 작업을 수행하는 데 사용할 수 있습니다.

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### 사용 사례

+ 표준 동기화에서 다루지 않은 추가 사용자 프로필 속성 업데이트
+ AEM에서 사용자 정의 사용자 관련 리소스 만들기 또는 업데이트
+ 사용자 인증 후 워크플로우 또는 알림 트리거
+ 사용자 지정 인증 이벤트 기록

**중요:** 리포지토리에서 사용자 속성을 수정하려면 후크를 구현해야 합니다.

+ `SlingRepository`을(를) 통해 `@Reference` 참조가 삽입되었습니다.
+ 적절한 권한을 가진 구성된 [서비스 사용자](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)&#x200B;(&quot;Apache Sling Service User Mapper Service Advisory&quot;에서 구성됨)
+ try-catch-finally 블록을 사용한 적절한 세션 관리

## 사용자 정의 SAML 후크 구현

다음 단계는 사용자 정의 SAML 후크를 만들고 배포하는 방법에 대해 설명합니다.

### SAML 후크 구현 만들기

`com.adobe.granite.auth.saml.spi.SamlHook` 인터페이스를 구현하는 AEM 프로젝트에서 새 Java 클래스를 만듭니다.

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### SAML 후크 구성

SAML 후크는 OSGi 구성을 사용하여 적용해야 하는 IDP를 지정합니다. 다음 위치의 프로젝트에 OSGi 구성 파일을 생성합니다.

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

`idpIdentifier`은(는) 해당 SAML 인증 처리기 OSGi 팩터리 구성(PID: `idpIdentifier`)에 구성된 `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json` 값과 일치해야 합니다. 이 일치는 중요합니다. SAML 후크는 동일한 `idpIdentifier` 값을 가진 SAML 인증 처리기 인스턴스에 대해서만 호출됩니다. SAML 인증 처리기는 팩터리 구성입니다. 즉, 여러 인스턴스(예: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`)를 가질 수 있으며 각 후크는 `idpIdentifier`을(를) 통해 특정 처리기에 연결됩니다. `service.ranking` 속성은 여러 후크를 구성할 때 실행 순서를 제어합니다(높은 값이 먼저 실행됨).

### Maven 종속성 추가

AEM Maven 핵심 프로젝트의 `pom.xml`에 필요한 SAML SPI 종속성을 추가합니다.

**클라우드 서비스 프로젝트로 AEM**&#x200B;의 경우 SAML 인터페이스를 포함하는 AEM SDK API 종속성을 사용합니다.

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

`aem-sdk-api` 아티팩트에 `com.adobe.granite.auth.saml.spi.SamlHook`을(를) 포함하여 필요한 모든 Adobe 화강암 SAML 인터페이스가 포함되어 있습니다.

### 서비스 사용자 구성(선택 사항)

SAML 후크가 사용자 속성과 같은 AEM JCR 저장소의 콘텐츠를 수정해야 하는 경우(`postSyncUserProcess` 예제와 같이) [서비스 사용자](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)를 구성해야 합니다.

1. `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json`의 프로젝트에서 서비스 사용자 매핑을 만듭니다.

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json`에서 서비스 사용자 및 권한을 정의하기 위한 재지정 스크립트를 만듭니다.

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

이렇게 하면 저장소에서 사용자 등록 정보를 읽고 수정할 수 있는 서비스 사용자 권한이 부여됩니다.

### AEM에 배포

사용자 지정 SAML 후크를 AEM as a Cloud Service에 배포합니다.

1. AEM 프로젝트 빌드
1. Cloud Manager Git 리포지토리에 코드 커밋
1. 전체 스택 배포 파이프라인을 사용하여 배포
1. 사용자가 SAML을 통해 인증하면 SAML 후크가 자동으로 활성화됩니다


### 중요한 고려 사항

+ **IDP 식별자 일치**: SAML 후크에 구성된 `idpIdentifier`은(는) SAML 인증 처리기 팩터리 구성(`idpIdentifier`)의 `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`과(와) 정확히 일치해야 합니다.
+ **특성 이름**: 후크에서 참조된 특성 이름(예: `groupMembership`)이 SAML 인증 처리기에 구성된 특성과 일치하는지 확인하십시오.
+ **성능**: 모든 SAML 인증 중에 실행될 때 후크 구현을 가볍게 유지하십시오.
+ **오류 처리**: 인증에 실패하는 심각한 오류가 발생하면 SAML 후크 구현에서 `com.adobe.granite.auth.saml.spi.SamlHookException`을(를) throw해야 합니다. SAML 인증 처리기가 이러한 예외를 catch하고 `AuthenticationInfo.FAIL_AUTH`을(를) 반환합니다. 저장소 작업의 경우 항상 `RepositoryException`을(를) catch하고 오류를 적절하게 기록합니다. try-catch-finally 블록을 사용하여 리소스를 올바르게 정리합니다.
+ **테스트**: 프로덕션에 배포하기 전에 낮은 환경에서 사용자 지정 후크를 철저히 테스트합니다.
+ **여러 후크**: 여러 SAML 후크 구현을 구성할 수 있습니다. 일치하는 모든 후크가 실행됩니다. OSGi 구성 요소에서 `service.ranking` 속성을 사용하여 실행 순서를 제어합니다. 순위가 높은 값이 먼저 실행됩니다. 여러 SAML 인증 처리기 팩터리 구성(`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)에서 SAML 후크를 다시 사용하려면 각 SAML 인증 처리기와 일치하는 다른 `idpIdentifier`을(를) 사용하여 여러 후크 구성(OSGi 팩터리 구성)을 만듭니다
+ **보안**: 비즈니스 논리에 사용하기 전에 SAML 어설션의 모든 데이터를 확인하고 정리합니다.
+ **저장소 액세스**: `postSyncUserProcess`에서 사용자 속성을 수정할 때는 관리 세션이 아닌 적절한 권한이 있는 [서비스 사용자](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)를 항상 사용하십시오
+ **서비스 사용자 권한**: [서비스 사용자](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)에게 최소한의 필수 권한을 부여합니다(예: `jcr:read`의 `rep:write` 및 `/home/users`만, 전체 관리자 권한 아님).
+ **세션 관리**: 예외가 발생하더라도 항상 try-catch-finally 블록을 사용하여 리포지토리 세션이 제대로 닫히도록 하십시오
+ **사용자 동기화 타이밍**: `postSyncUserProcess` 후크는 사용자가 OAK에 동기화한 후에 실행되므로 해당 시점에 사용자 개체가 저장소에 존재할 수 있습니다
