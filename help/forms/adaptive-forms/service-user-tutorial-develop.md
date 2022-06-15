---
title: AEM Forms에서 서비스 사용자를 사용한 개발
description: 이 문서에서는 AEM Forms에서 서비스 사용자를 만드는 프로세스를 안내합니다
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
source-git-commit: c462d48d26c9a7aa0e4cfc4f24005b41e8e82cb8
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 1%

---

# AEM Forms에서 서비스 사용자를 사용한 개발

이 문서에서는 AEM Forms에서 서비스 사용자를 만드는 프로세스를 안내합니다

이전 버전의 Adobe Experience Manager(AEM)에서는 저장소에 액세스해야 하는 백엔드 처리에 관리 리소스 확인자가 사용되었습니다. 관리 리소스 확인자 사용은 AEM 6.3에서 더 이상 사용되지 않습니다. 대신 저장소에서 특정 권한이 있는 시스템 사용자가 사용됩니다.

세부 사항에 대해 자세히 알아보기 [AEM에서 서비스 사용자 생성 및 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html).

이 문서에서는 시스템 사용자 생성 및 사용자 매퍼 속성 구성에 대해 설명합니다.

1. 다음으로 이동 [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. &#39; 관리자 &#39;(으)로 로그인합니다.
1. &#39; 사용자 관리 &#39; 를 클릭합니다.
1. &#39; 시스템 사용자 만들기 &#39; 를 클릭합니다.
1. userid 유형을 &#39; data &#39; 로 설정하고 녹색 아이콘을 클릭하여 시스템 사용자 생성 프로세스를 완료합니다
1. [configMgr 열기](http://localhost:4502/system/console/configMgr)
1. 검색 대상 _Apache Sling Service User Mapper Service_ 를 클릭하여 속성을 엽니다
1. 을(를) 클릭합니다. *+* 아이콘(더하기)을 클릭하여 다음 서비스 매핑을 추가합니다.

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. &#39; Save &#39; 를 클릭합니다.

위의 구성 설정에서 DevelopingWithServiceUser.core는 번들의 기호 이름입니다. getresourceresolver는 하위 서비스 이름입니다. data는 이전 단계에서 만든 시스템 사용자입니다.

fd 서비스 사용자를 대신하여 리소스 확인자를 가져올 수도 있습니다. 이 서비스 사용자는 문서 서비스에 사용됩니다. 예를 들어 사용 권한 등을 인증/적용하려면 fd 서비스 사용자의 리소스 확인자를 사용하여 작업을 수행합니다

1. [이 문서와 연결된 zip 파일을 다운로드하여 압축을 해제합니다.](assets/developingwithserviceuser.zip)
1. 다음으로 이동 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. OSGi 번들을 업로드하고 시작합니다
1. 번들이 활성 상태인지 확인합니다.
1. 이제 을(를) 생성했습니다. *시스템 사용자* 또한 *서비스 사용자 번들*.

   /content에 대한 액세스 권한을 제공하려면 시스템 사용자(&#39; 데이터 &#39;)에게 컨텐츠 노드에 대한 읽기 권한을 제공합니다.

   1. 다음으로 이동 [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. &#39; 데이터 &#39; 사용자를 검색합니다. 이전 단계에서 만든 시스템 사용자와 동일합니다.
   1. 사용자를 두 번 클릭한 다음 &#39; 권한 &#39; 탭을 클릭합니다
   1. &#39;content&#39; 폴더에 대한 &#39; 읽기 &#39; 액세스 권한을 제공합니다.
   1. 서비스 사용자를 사용하여 /content 폴더에 대한 액세스 권한을 얻으려면 다음 코드를 사용하십시오



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

번들에서 /content/dam/data.json 파일에 액세스하려면 다음 코드를 사용합니다. 이 코드에서는 /content/dam/ 노드의 &quot;data&quot; 사용자에게 읽기 권한을 부여했다고 가정합니다

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
