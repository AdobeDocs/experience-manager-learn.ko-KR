---
title: AEM Forms에서 서비스 사용자를 사용하여 개발
description: 이 문서에서는 AEM Forms에서 서비스 사용자를 만드는 프로세스를 안내합니다
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
duration: 129
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# AEM Forms에서 서비스 사용자를 사용하여 개발

이 문서에서는 AEM Forms에서 서비스 사용자를 만드는 프로세스를 안내합니다

이전 버전의 Adobe Experience Manager(AEM)에서는 저장소에 액세스해야 하는 백엔드 처리에 관리 리소스 확인자가 사용되었습니다. 관리 리소스 확인자는 AEM 6.3에서 더 이상 사용되지 않습니다. 대신 저장소에서 특정 권한이 있는 시스템 사용자가 사용됩니다.

[AEM에서 서비스 사용자를 만들고 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html)하는 방법에 대해 자세히 알아보세요.

이 문서에서는 시스템 사용자를 만들고 사용자 매퍼 속성을 구성하는 과정을 안내합니다.

1. [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)(으)로 이동
1. &#39; admin &#39;(으)로 로그인
1. &#39; 사용자 관리 &#39; 를 클릭합니다.
1. &#39; 시스템 사용자 만들기 &#39; 를 클릭합니다.
1. 사용자 ID 유형을 &#39; data &#39;로 설정하고 녹색 아이콘을 클릭하여 시스템 사용자 생성 프로세스를 완료합니다
1. [configMgr 열기](http://localhost:4502/system/console/configMgr)
1. _Apache Sling Service 사용자 매퍼 서비스_&#x200B;를 검색하고 클릭하여 속성을 엽니다.
1. *+* 아이콘(더하기)을 클릭하여 다음 서비스 매핑을 추가합니다

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. &#39; 저장 &#39; 클릭

위의 구성 설정 DevelopingWithServiceUser.core는 번들의 기호 이름입니다. getresourceresolver 는 하위 서비스 이름입니다. data 는 이전 단계에서 만든 시스템 사용자입니다.

fd-service 사용자를 대신하여 resource resolver를 가져올 수도 있습니다. 이 서비스 사용자는 문서 서비스에 사용됩니다. 예를 들어 사용 권한 등을 인증/적용하려는 경우 fd-service 사용자의 resource resolver를 사용하여 작업을 수행합니다

1. [이 문서와 관련된 zip 파일을 다운로드하고 압축 해제합니다.](assets/developingwithserviceuser.zip)
1. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)(으)로 이동
1. OSGi 번들 업로드 및 시작
1. 번들이 활성 상태인지 확인합니다.
1. 이제 *시스템 사용자*&#x200B;를 만들고 *서비스 사용자 번들*&#x200B;도 배포했습니다.

   /content에 대한 액세스 권한을 제공하려면 시스템 사용자(&#39; data &#39;)에게 콘텐츠 노드에 대한 읽기 권한을 제공합니다.

   1. [http://localhost:4502/useradmin](http://localhost:4502/useradmin)(으)로 이동
   1. 사용자 &#39; 데이터 &#39; 검색 이전 단계에서 만든 시스템 사용자와 동일합니다.
   1. 사용자를 두 번 클릭한 다음 &quot;권한&quot; 탭을 클릭합니다
   1. &#39; 컨텐츠 &#39; 폴더에 &#39; 읽기 &#39; 액세스 권한을 부여합니다.
   1. 서비스 사용자를 사용하여 /content 폴더에 액세스하려면 다음 코드를 사용하십시오



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

번들의 /content/dam/data.json 파일에 액세스하려면 다음 코드를 사용합니다. 이 코드는 사용자가 /content/dam/ 노드에서 &quot;data&quot; 사용자에게 읽기 권한을 부여했다고 가정합니다.

```java
@Reference
GetResolver getResolver;
.
.
.
try {
   ResourceResolver serviceResolver = getResolver.getServiceResolver();

   // To get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();

   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
} catch(LoginException ex) {
   // Unable to get the service user, handle this exception as needed
} finally {
  // Always close all ResourceResolvers you open!
  
  if (serviceResolver != null( { serviceResolver.close(); }
  if (fdserviceResolver != null) { fdserviceResolver.close(); }
}
```

구현의 전체 코드는 아래에 나와 있습니다

```java
package com.mergeandfuse.getserviceuserresolver.impl;
import java.util.HashMap;
import org.apache.sling.api.resource.LoginException;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = GetResolver.class)
public class GetResolverImpl implements GetResolver {
        @Reference
        ResourceResolverFactory resolverFactory;

        @Override
        public ResourceResolver getServiceResolver() {
                System.out.println("#### Trying to get service resource resolver ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {

                        System.out.println("Login Exception " + e.getMessage());
                }
                return resolver;

        }

        @Override
        public ResourceResolver getFormsServiceResolver() {

                System.out.println("#### Trying to get Resource Resolver for forms ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getformsresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {
                        System.out.println("Login Exception ");
                        System.out.println("The error message is " + e.getMessage());
                }
                return resolver;
        }

}
```
