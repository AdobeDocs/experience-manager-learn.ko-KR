---
title: AEM Forms에서 서비스 사용자와 개발
seo-title: AEM Forms에서 서비스 사용자와 개발
description: 이 문서에서는 AEM Forms에서 서비스 사용자를 만드는 프로세스를 안내합니다
seo-description: 이 문서에서는 AEM Forms에서 서비스 사용자를 만드는 프로세스를 안내합니다
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: adaptive-forms
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# AEM Forms에서 서비스 사용자와 개발

이 문서에서는 AEM Forms에서 서비스 사용자를 만드는 프로세스를 안내합니다

이전 버전의 Adobe Experience Manager(AEM)에서 관리 리소스 확인자는 저장소에 액세스해야 하는 백엔드 처리에 사용되었습니다. 관리 리소스 확인자 사용은 AEM 6.3에서 더 이상 사용되지 않습니다. 대신 보관소에 특정 권한이 있는 시스템 사용자가 사용됩니다.

이 문서에서는 시스템 사용자 생성 및 사용자 매퍼 속성 구성에 대해 설명합니다.

1. [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)으로 이동합니다.
1. &#39; 관리자 &#39;(으)로 로그인
1. &#39; 사용자 관리 &#39;
1. &#39; 시스템 사용자 만들기 &#39;
1. 사용자 ID 유형을 &#39; 데이터&#39;로 설정하고 녹색 아이콘을 클릭하여 시스템 사용자를 만드는 프로세스를 완료합니다
1. [configMgr 열기](http://localhost:4502/system/console/configMgr)
1. &#39; Apache Sling Service User Mapper Service &#39; 를 검색하고 클릭하여 속성을 엽니다.
1. 다음 서비스 매핑을 추가하려면 *+* 아이콘(더하기)을 클릭합니다

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. &#39; 저장 &#39;

위 구성 설정에서 DevelopingWithServiceUser.core는 번들의 상징적 이름입니다. getresourceresolver는 하위 서비스 이름입니다.data는 이전 단계에서 만든 시스템 사용자입니다.

fd 서비스 사용자를 대신하여 리소스 확인자를 받을 수도 있습니다. 이 서비스 사용자는 문서 서비스에 사용됩니다. 예를 들어 사용 권한 인증/적용을 원하는 경우 fd-service 사용자의 리소스 확인자를 사용하여 작업을 수행합니다

1. [이 아티클과 관련된 zip 파일을 다운로드하고 압축을 해제합니다.](assets/developingwithserviceuser.zip)
1. [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)으로 이동합니다.
1. OSGi 번들 업로드 및 시작
1. 번들이 활성 상태인지 확인
1. 이제 *시스템 사용자*&#x200B;를 만들고 *서비스 사용자 번들*&#x200B;도 배포했습니다.

   /content에 대한 액세스 권한을 제공하려면 시스템 사용자(&#39; 데이터 &#39;)에게 컨텐츠 노드에 대한 읽기 권한을 부여합니다.

   1. [http://localhost:4502/useradmin](http://localhost:4502/useradmin)으로 이동합니다.
   1. &#39; 데이터 &#39; 사용자를 검색합니다. 이 사용자는 이전 단계에서 만든 것과 동일한 시스템 사용자입니다.
   1. 사용자를 두 번 클릭한 다음 &#39; 권한 &#39; 탭을 클릭합니다.
   1. &#39;content&#39; 폴더에 대한 &#39; 읽기 권한을 부여합니다.
   1. 서비스 사용자를 사용하여 /content 폴더에 대한 액세스 권한을 얻으려면 다음 코드를 사용하십시오.

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   번들의 /content/dam/data.json 파일에 액세스하려면 다음 코드를 사용합니다. 이 코드에서는 /content/dam/ 노드의 &quot;data&quot; 사용자에게 읽기 권한을 부여했다고 가정합니다

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   구현의 전체 코드는 다음과 같습니다

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
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

