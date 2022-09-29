---
title: AEM for SPA 편집기 및 Remote SPA 구성
description: AEM SPA Editor에서 Remote SPA을 작성할 수 있도록 하려면 AEM 프로젝트 이 구성 및 컨텐츠 요구 사항을 설정하는 데 필요합니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1215'
ht-degree: 1%

---

# AEM for SPA 편집기 구성

SPA 코드 베이스는 AEM 외부에서 관리되지만, 지원 구성 및 컨텐츠 요구 사항을 설정하려면 AEM 프로젝트가 필요합니다. 이 장에서는 필요한 구성을 포함하는 AEM 프로젝트 생성 과정을 설명합니다.

+ AEM WCM 코어 구성 요소 프록시
+ AEM 원격 SPA 페이지 프록시
+ AEM 원격 SPA 페이지 템플릿
+ 기준선 원격 SPA AEM 페이지
+ SPA-AEM URL 매핑을 정의하는 하위 프로젝트
+ OSGi 구성 폴더

## AEM 프로젝트 만들기

구성 및 기준 컨텐츠를 관리하는 AEM 프로젝트를 만듭니다.

_항상 최신 버전의 를 사용하십시오 [AEM Archetype](https://github.com/adobe/aem-project-archetype)._


```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=27 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/wknd-app/wknd-app ~/Code/wknd-app/com.adobe.aem.guides.wknd-app
```

_마지막 명령은 AEM 프로젝트 폴더의 이름을 변경하기만 하면 됩니다. 따라서 AEM 프로젝트이고 원격 SPA과 혼동하지 않도록 합니다__

While `frontendModule="react"` 이 지정되면 `ui.frontend` 프로젝트는 원격 SPA 사용 사례에 사용되지 않습니다. SPA은 외부에서 AEM으로 개발 및 관리되며 AEM을 컨텐츠 API로만 사용합니다. 다음 `frontendModule="react"` 플래그는 프로젝트에  `spa-project` AEM Java™ 종속성 및 원격 SPA 페이지 템플릿을 설정합니다.

AEM 프로젝트 원형 은 SPA과의 통합을 위해 AEM을 구성하는 데 사용되는 다음 요소를 생성합니다.

+ __AEM WCM 코어 구성 요소 프록시__ at `ui.apps/src/.../apps/wknd-app/components`
+ __AEM SPA 원격 페이지 프록시__ at `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __AEM 페이지 템플릿__ at `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __컨텐츠 매핑을 정의하는 하위 프로젝트__ at `ui.content/src/...`
+ __기준선 원격 SPA AEM 페이지__ at `ui.content/src/.../content/wknd-app`
+ __OSGi 구성 폴더__ at `ui.config/src/.../apps/wknd-app/osgiconfig`

기본 AEM 프로젝트가 생성되면 몇 가지 조정을 통해 SPA 편집기와 Remote SPA이 호환되도록 할 수 있습니다.

## ui.frontend 프로젝트 제거

SPA은 원격 SPA이므로 AEM 프로젝트 외부에서 개발 및 관리된다고 가정합니다. 충돌을 방지하려면 다음을 제거합니다 `ui.frontend` 프로젝트를 배포에서 가져옵니다. 만약 `ui.frontend` 프로젝트가 제거되지 않고 두 개의 SPA이 있으며 이 두 개는 `ui.frontend` 프로젝트 및 원격 SPA이 AEM SPA 편집기에서 동시에 로드됩니다.

1. AEM 프로젝트를 엽니다(`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`) 내의 IDE에 있어야 합니다
1. 루트 열기 `pom.xml`
1. 댓글 달기 `<module>ui.frontend</module` 밖으로 `<modules>` list

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   다음 `pom.xml` 파일 형식은 다음과 같습니다.

   ![반응기 pom에서 ui.frontend 모듈 제거](./assets/aem-project/uifrontend-reactor-pom.png)

1. 를 엽니다. `ui.apps/pom.xml`
1. 댓글 출력 `<dependency>` on `<artifactId>wknd-app.ui.frontend</artifactId>`

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   다음 `ui.apps/pom.xml` 파일 형식은 다음과 같습니다.

   ![ui.apps에서 ui.frontend 종속성 제거](./assets/aem-project/uifrontend-uiapps-pom.png)

AEM 프로젝트가 이러한 변경 사항 전에 빌드된 경우 수동으로 `ui.frontend` 에서 생성된 클라이언트 라이브러리 `ui.apps` 프로젝트 위치 `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## AEM 콘텐츠 매핑

AEM이 SPA 편집기에서 Remote SPA을 로드하려면 SPA 경로와 컨텐츠 열기 및 작성에 사용된 AEM 페이지 간의 매핑이 설정되어야 합니다.

이 구성의 중요성은 나중에 알아봅니다.

매핑은 [Sling 매핑](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) 정의된 `/etc/map`.

1. IDE에서 `ui.content` 하위 프로젝트
1. 다음으로 이동  `src/main/content/jcr_root`
1. 폴더를 만듭니다 `etc`
1. in `etc`, 폴더를 만듭니다. `map`
1. in `map`, 폴더를 만듭니다. `http`
1. in `http`로 지정하는 경우 파일 만들기 `.content.xml` 내용 사용:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. in `http` , 폴더를 만듭니다. `localhost_any`
1. in `localhost_any`로 지정하는 경우 파일 만들기 `.content.xml` 내용 사용:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. in `localhost_any` , 폴더를 만듭니다. `wknd-app-routes-adventure`
1. in `wknd-app-routes-adventure`로 지정하는 경우 파일 만들기 `.content.xml` 내용 사용:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. 매핑 노드를 `ui.content/src/main/content/META-INF/vault/filter.xml` AEM 패키지에 포함된 단계에 대해 자세히 알아보십시오.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

폴더 구조 및 `.context.xml` 파일은 다음과 같습니다.

![Sling 매핑](./assets/aem-project/sling-mapping.png)

다음 `filter.xml` 파일 형식은 다음과 같습니다.

![Sling 매핑](./assets/aem-project/sling-mapping-filter.png)

이제 AEM 프로젝트가 배포되면 이러한 구성이 자동으로 포함됩니다.

Sling 매핑은 AEM에서 실행되는 결과를 생성합니다. `http` 및 `localhost`따라서 로컬 개발만 지원합니다. AEM as a Cloud Service에 배포할 때 해당 대상에 유사한 Sling 매핑을 추가해야 합니다 `https` 및 해당 AEM as a Cloud Service 도메인/s. 자세한 내용은 [Sling 매핑 설명서](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Cross-Origin Resource Sharing 보안 정책

다음으로, 이 SPA만 AEM 컨텐츠에 액세스할 수 있도록 컨텐츠를 보호하도록 AEM을 구성합니다. 구성 [AEM의 Cross-Origin Resource Sharing](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. IDE에서 `ui.config` Maven 하위 프로젝트
1. 탐색하고 `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. 이름이 인 파일 만들기 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. 파일에 다음 내용을 추가합니다.

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

다음 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` 파일 형식은 다음과 같습니다.

![SPA Editor CORS 구성](./assets/aem-project/cors-configuration.png)

주요 구성 요소는 다음과 같습니다.

+ `alloworigin` AEM에서 컨텐츠를 검색할 수 있는 호스트를 지정합니다.
   + `localhost:3000` 로컬에서 실행되는 SPA을 지원하기 위해 가 추가되었습니다.
   + `https://external-hosted-app` 원격 SPA이 호스팅되는 도메인으로 대체될 자리 표시자 역할을 합니다.
+ `allowedpaths` AEM에서 이 CORS 구성으로 적용되는 경로를 지정합니다. 기본값은 AEM의 모든 컨텐츠에 액세스할 수 있도록 하지만 SPA에서 액세스할 수 있는 특정 경로에만 범위를 지정할 수 있습니다. 예: `/content/wknd-app`.

## AEM 페이지를 원격 SPA 페이지 템플릿으로 설정

AEM Project Archetype 은 Remote SPA과 AEM 통합에 대한 사전 설정된 프로젝트를 생성하지만 자동 생성된 AEM 페이지 구조에 대한 작지만 중요한 조정이 필요합니다. 자동 생성된 AEM 페이지의 유형이 __원격 SPA 페이지__, 대신 __SPA 페이지__.

1. IDE에서 `ui.content` 하위 프로젝트
1. 열기 대상 `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. 업데이트 `.content.xml` 다음 파일 포함:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

주요 변경 사항은 `jcr:content` 노드:

+ `cq:template` 끝 `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` 끝 `wknd-app/components/remotepage`

다음 `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` 파일 형식은 다음과 같습니다.

![홈 페이지 .content.xml 업데이트](./assets/aem-project/home-content-xml.png)

이러한 변경 사항으로 AEM의 SPA 루트인 이 페이지에서 SPA 편집기에서 Remote SPA을 로드할 수 있습니다.

>[!NOTE]
>
>이 프로젝트가 이전에 AEM에 배포된 경우 AEM 페이지를 __사이트 > WKND 앱 > us > en > WKND 앱 홈 페이지__, 로서의 `ui.content`  프로젝트가 __병합__ 노드 대신 __업데이트__.

AEM 자체에서 원격 SPA 페이지로 이 페이지를 제거하고 다시 만들 수도 있지만, 이 페이지는 `ui.content` project it는 코드 베이스에서 업데이트하는 것이 가장 좋습니다.

## AEM SDK에 AEM 프로젝트 배포

1. AEM 작성자 서비스가 포트 4502에서 실행 중인지 확인합니다
1. 명령줄에서 AEM Maven 프로젝트의 루트로 이동합니다
1. Maven을 사용하여 프로젝트를 로컬 AEM SDK 작성자 서비스에 배포합니다.

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn 정리 설치 - PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## 루트 AEM 페이지 구성

AEM 프로젝트가 배포되면 Remote SPA을 로드하기 위해 SPA Editor를 준비하는 마지막 단계가 있습니다. AEM에서 SPA 루트에 해당하는 AEM 페이지를 표시합니다.`/content/wknd-app/us/en/home`AEM 프로젝트 원형에 의해 생성된 .

1. AEM 작성자에 로그인
1. 다음으로 이동 __사이트 > WKND 앱 > us > en__
1. 을(를) 선택합니다 __WKND 앱 홈 페이지__, 탭 __속성__

   ![WKND 앱 홈 페이지 - 속성](./assets/aem-content/edit-home-properties.png)

1. 로 이동합니다 __SPA__ 탭
1. 을(를) 입력합니다. __원격 SPA 구성__
   + __SPA 호스트 URL__: `http://localhost:3000`
      + 원격 SPA 루트의 URL

   ![WKND 앱 홈 페이지 - 원격 SPA 구성](./assets/aem-content/remote-spa-configuration.png)

1. 탭 __저장 및 닫기__

이 페이지의 유형을 __원격 SPA 페이지__&#x200B;이렇게 하면 __SPA__ 탭 __페이지 속성__.

이 구성은 SPA의 루트에 해당하는 AEM 페이지에서만 설정해야 합니다. 이 페이지 아래의 모든 AEM 페이지는 값을 상속합니다.

## 축하합니다

이제 AEM 구성을 준비하여 로컬 AEM 작성자에게 배포했습니다! 이제 방법을 알 수 있습니다.

+ 의 종속성을 주석 처리하여 AEM Project Archetype 생성 SPA을 제거합니다. `ui.frontend`
+ SPA 경로를 AEM의 리소스에 매핑하는 AEM에 Sling 매핑을 추가합니다
+ Remote SPA에서 AEM의 컨텐츠를 사용할 수 있도록 하는 AEM Cross-Origin Resource Sharing 보안 정책을 설정합니다
+ 로컬 AEM SDK 작성자 서비스에 AEM 프로젝트 배포
+ SPA 호스트 URL 페이지 속성을 사용하여 AEM 페이지를 원격 SPA 루트로 표시합니다

## 다음 단계

AEM이 구성되면 다음 사항에 집중할 수 있습니다 [원격 SPA 부트스트래핑](./spa-bootstrap.md) AEM SPA 편집기를 사용한 편집 가능한 영역 지원!
