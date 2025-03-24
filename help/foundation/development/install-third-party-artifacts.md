---
title: 타사 아티팩트 설치 - 공개 Maven 저장소에서 사용할 수 없음
description: AEM 프로젝트를 빌드하고 배포할 때 공개 Maven 저장소에서 *사용할 수 없는 타사 아티팩트를 설치하는 방법을 알아봅니다.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: OSGI
topic: Development
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-13T00:00:00Z
jira: KT-16207
thumbnail: KT-16207.jpeg
exl-id: 0cec14b3-4be5-4666-a36c-968ea2fc634f
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1569'
ht-degree: 0%

---

# 타사 아티팩트 설치 - 공개 Maven 저장소에서 사용할 수 없음

AEM 프로젝트를 빌드하고 배포할 때 공용 Maven 저장소에서 *사용할 수 없는 타사 아티팩트를 설치하는 방법*&#x200B;을 알아봅니다.

**타사 아티팩트**&#x200B;는 다음과 같을 수 있습니다.

- [OSGi 번들](https://www.osgi.org/resources/architecture/): OSGi 번들은 Java 클래스, 리소스 및 번들과 해당 종속성을 설명하는 매니페스트를 포함하는 Java™ 보관 파일입니다.
- [Java jar](https://docs.oracle.com/javase/tutorial/deployment/jar/basicsindex.html): Java 클래스와 리소스가 포함된 Java™ 아카이브 파일입니다.
- [패키지](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/administering/contentmanagement/package-manager#what-are-packages): 패키지는 파일 시스템 직렬화 양식의 저장소 콘텐츠가 들어 있는 zip 파일입니다.

## 표준 시나리오

일반적으로 AEM 프로젝트의 `pom.xml` 파일에 종속되어 있는 공개 Maven 저장소에 *사용할 수 있는*&#x200B;서드파티 번들을 설치합니다.

예:

- [AEM WCM 핵심 구성 요소](https://github.com/adobe/aem-core-wcm-components) **번들**&#x200B;이(가) [WKND 프로젝트의 ](https://github.com/adobe/aem-guides-wknd/blob/main/pom.xml#L747-L753) `pom.xml` 파일에 종속성으로 추가되었습니다. 여기서 `provided` 범위는 AEM 런타임에서 AEM WCM 핵심 구성 요소 번들을 제공할 때 사용됩니다. AEM 런타임에서 번들을 제공하지 않으면 `compile` 범위를 사용하게 되며 기본 범위입니다.

- [WKND 공유](https://github.com/adobe/aem-guides-wknd-shared) **패키지**&#x200B;가 [WKND 프로젝트의 ](https://github.com/adobe/aem-guides-wknd/blob/main/pom.xml#L767-L773) `pom.xml` 파일에 종속성으로 추가되었습니다.



## 드문 시나리오

경우에 따라 AEM 프로젝트를 빌드하고 배포할 때 [Maven 중앙 저장소](https://mvnrepository.com/) 또는 [Adobe 공용 저장소](https://repo.adobe.com/index.html)에 사용할 수 없는 **서드파티 번들 또는 jar 또는 패키지를 설치해야 할 수 있습니다.**

이유는 다음과 같습니다.

- 번들 또는 패키지는 내부 팀이나 타사 공급업체에서 제공하며 _은(는) 공용 Maven 저장소에서 사용할 수 없습니다_.

- Java™ jar 파일 _은(는) OSGi 번들_&#x200B;이(가) 아니므로 공용 Maven 저장소에서 사용할 수 있거나 사용할 수 없을 수 있습니다.

- 공개 Maven 저장소에서 사용할 수 있는 최신 버전의 타사 패키지에 아직 릴리스되지 않은 기능이 필요합니다. 로컬로 빌드된 RELEASE 또는 SNAPSHOT 버전을 설치하기로 결정했습니다.

## 사전 요구 사항

이 자습서를 수행하려면 다음이 필요합니다.

- [로컬 AEM 개발 환경](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview) 또는 [신속한 개발 환경(RDE)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/rde/overview) 설정.

- [AEM WKND 프로젝트](https://github.com/adobe/aem-guides-wknd) _타사 번들 또는 jar 또는 패키지를 추가_&#x200B;하고 변경 사항을 확인합니다.

## 설정

- AEM 6.X 또는 AEM as a Cloud Service(AEMCS) 로컬 개발 환경 또는 RDE 환경을 설정합니다.

- AEM WKND 프로젝트를 복제하고 배포합니다.

  ```
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallPackage 
  ```

  WKND 사이트 페이지가 올바르게 렌더링되는지 확인합니다.

## AEM 프로젝트에 서드파티 번들 설치{#install-third-party-bundle}

공개 Maven 저장소에서 _사용할 수 없는 데모 OSGi [my-example-bundle](./assets/install-third-party-articafcts/my-example-bundle.zip)을(를) AEM WKND 프로젝트에 설치 및 사용하겠습니다_.

**my-example-bundle**&#x200B;이(가) `HelloWorldService` OSGi 서비스를 내보냅니다. 해당 `sayHello()` 메서드가 `Hello Earth!` 메시지를 반환합니다.

자세한 내용은 [my-example-bundle.zip](./assets/install-third-party-articafcts/my-example-bundle.zip) 파일의 README.md 파일을 참조하십시오.

### `all` 모듈에 번들 추가

첫 번째 단계는 AEM WKND 프로젝트의 `all` 모듈에 `my-example-bundle`을(를) 추가하는 것입니다.

- [my-example-bundle.zip](./assets/install-third-party-articafcts/my-example-bundle.zip) 파일을 다운로드하여 추출하십시오.

- AEM WKND 프로젝트의 `all` 모듈에서 `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` 디렉터리 구조를 만듭니다. `/all/src/main/content` 디렉터리가 있습니다. `jcr_root/apps/wknd-vendor-packages/container/install` 디렉터리만 만들면 됩니다.

- 추출된 `target` 디렉터리에서 위의 `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` 디렉터리로 `my-example-bundle-1.0-SNAPSHOT.jar` 파일을 복사합니다.

  모든 모듈의 ![타사 번들](./assets/install-third-party-articafcts/3rd-party-bundle-all-module.png)

### 번들의 서비스 사용

AEM WKND 프로젝트의 `my-example-bundle`에서 `HelloWorldService` OSGi 서비스를 사용해 보겠습니다.

- AEM WKND 프로젝트의 `core` 모듈에서 `SayHello.java` Sling 서블릿 @ `core/src/main/java/com/adobe/aem/guides/wknd/core/servlet`을(를) 만듭니다.

  ```java
  package com.adobe.aem.guides.wknd.core.servlet;
  
  import java.io.IOException;
  
  import javax.servlet.Servlet;
  import javax.servlet.ServletException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.api.servlets.HttpConstants;
  import org.apache.sling.api.servlets.ServletResolverConstants;
  import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
  import org.osgi.service.component.annotations.Component;
  import org.osgi.service.component.annotations.Reference;
  import com.example.services.HelloWorldService;
  
  @Component(service = Servlet.class, property = {
      ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/sayhello",
      ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
  })
  public class SayHello extends SlingSafeMethodsServlet {
  
          private static final long serialVersionUID = 1L;
  
          // Injecting the HelloWorldService from the `my-example-bundle` bundle
          @Reference
          private HelloWorldService helloWorldService;
  
          @Override
          protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException, IOException {
              // Invoking the HelloWorldService's `sayHello` method
              response.getWriter().write("My-Example-Bundle service says: " + helloWorldService.sayHello());
          }
  }
  ```

- AEM WKND 프로젝트의 루트 `pom.xml` 파일에서 `my-example-bundle`을(를) 종속성으로 추가합니다.

  ```xml
  ...
  <!-- My Example Bundle -->
  <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-example-bundle</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${maven.multiModuleProjectDirectory}/all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install/my-example-bundle-1.0-SNAPSHOT.jar</systemPath>
  </dependency>
  ...
  ```

  여기:
   - `system` 범위는 공용 Maven 저장소에서 종속성을 조회해서는 안 됨을 나타냅니다.
   - `systemPath`은(는) AEM WKND 프로젝트의 `all` 모듈에 있는 `my-example-bundle` 파일의 경로입니다.
   - `${maven.multiModuleProjectDirectory}`은(는) 다중 모듈 프로젝트의 루트 디렉터리를 가리키는 Maven 속성입니다.

- AEM WKND 프로젝트 `core` 모듈의 `core/pom.xml` 파일에서 `my-example-bundle`을(를) 종속성으로 추가합니다.

  ```xml
  ...
  <!-- My Example Bundle -->
  <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-example-bundle</artifactId>
  </dependency>
  ...
  ```

- 다음 명령을 사용하여 AEM WKND 프로젝트를 빌드하고 배포합니다.

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- 브라우저의 URL `http://localhost:4502/bin/sayhello`에 액세스하여 `SayHello` 서블릿이 예상대로 작동하는지 확인하십시오.

- 위의 변경 사항을 AEM WKND 프로젝트의 저장소에 커밋합니다. 그런 다음 Cloud Manager 파이프라인을 실행하여 RDE 또는 AEM 환경의 변경 사항을 확인합니다.

  ![SayHello 서블릿 확인 - 번들 서비스](./assets/install-third-party-articafcts/verify-sayhello-servlet-bundle-service.png)

AEM WKND 프로젝트의 [tutorial/install-third-party-bundle](https://github.com/adobe/aem-guides-wknd/compare/main...tutorial/install-3rd-party-bundle) 분기에 위와 같은 참조 변경 사항이 있습니다.

### 주요 학습{#key-learnings-bundle}

공개 Maven 저장소에서 사용할 수 없는 OSGi 번들은 다음 단계에 따라 AEM 프로젝트에 설치할 수 있습니다.

- OSGi 번들을 `all` 모듈의 `jcr_root/apps/<PROJECT-NAME>-vendor-packages/container/install` 디렉터리에 복사합니다. 이 단계는 번들을 패키징하고 AEM 인스턴스에 배포하는 데 필요합니다.

- 루트 및 코어 모듈의 `pom.xml` 파일을 업데이트하여 `system` 범위 및 `systemPath`이(가) 번들 파일을 가리키는 종속성으로 OSGi 번들을 추가합니다. 이 단계는 프로젝트를 컴파일하는 데 필요합니다.

## AEM 프로젝트에 서드파티 jar 설치

이 예에서 `my-example-jar`은(는) OSGi 번들이 아니라 Java jar 파일입니다.

공개 Maven 저장소에서 _사용할 수 없는 데모 [my-example-jar](./assets/install-third-party-articafcts/my-example-jar.zip)을(를) AEM WKND 프로젝트에 설치 및 사용하겠습니다_.

**my-example-jar**&#x200B;은(는) `Hello World!` 메시지를 반환하는 `sayHello()` 메서드를 사용하는 `MyHelloWorldService` 클래스를 포함하는 Java jar 파일입니다.

자세한 내용은 [my-example-jar.zip](./assets/install-third-party-articafcts/my-example-jar.zip) 파일의 README.md 파일을 참조하십시오.

### `all` 모듈에 jar 추가

첫 번째 단계는 AEM WKND 프로젝트의 `all` 모듈에 `my-example-jar`을(를) 추가하는 것입니다.

- [my-example-jar.zip](./assets/install-third-party-articafcts/my-example-jar.zip) 파일을 다운로드하여 추출하십시오.

- AEM WKND 프로젝트의 `all` 모듈에서 `all/resource/jar` 디렉터리 구조를 만듭니다.

- 추출된 `target` 디렉터리에서 위의 `all/resource/jar` 디렉터리로 `my-example-jar-1.0-SNAPSHOT.jar` 파일을 복사합니다.

  모든 모듈의 ![타사-jar](./assets/install-third-party-articafcts/3rd-party-JAR-all-module.png)

### jar의 서비스 사용

AEM WKND 프로젝트에서 `my-example-jar`의 `MyHelloWorldService`을(를) 사용해 보겠습니다.

- AEM WKND 프로젝트의 `core` 모듈에서 `SayHello.java` Sling 서블릿 @ `core/src/main/java/com/adobe/aem/guides/wknd/core/servlet`을(를) 만듭니다.

  ```java
  package com.adobe.aem.guides.wknd.core.servlet;
  
  import java.io.IOException;
  
  import javax.servlet.Servlet;
  import javax.servlet.ServletException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.api.servlets.HttpConstants;
  import org.apache.sling.api.servlets.ServletResolverConstants;
  import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
  import org.osgi.service.component.annotations.Component;
  
  import com.my.example.MyHelloWorldService;
  
  @Component(service = Servlet.class, property = {
          ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/sayhello",
          ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
  })
  public class SayHello extends SlingSafeMethodsServlet {
  
      private static final long serialVersionUID = 1L;
  
      @Override
      protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
              throws ServletException, IOException {
  
          // Creating an instance of MyHelloWorldService
          MyHelloWorldService myHelloWorldService = new MyHelloWorldService();
  
          // Invoking the MyHelloWorldService's `sayHello` method
          response.getWriter().write("My-Example-JAR service says: " + myHelloWorldService.sayHello());
      }
  }    
  ```

- AEM WKND 프로젝트의 루트 `pom.xml` 파일에서 `my-example-jar`을(를) 종속성으로 추가합니다.

  ```xml
  ...
  <!-- My Example JAR -->
  <dependency>
      <groupId>com.my.example</groupId>
      <artifactId>my-example-jar</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${maven.multiModuleProjectDirectory}/all/resource/jar/my-example-jar-1.0-SNAPSHOT.jar</systemPath>
  </dependency>            
  ...
  ```

  여기:
   - `system` 범위는 공용 Maven 저장소에서 종속성을 조회해서는 안 됨을 나타냅니다.
   - `systemPath`은(는) AEM WKND 프로젝트의 `all` 모듈에 있는 `my-example-jar` 파일의 경로입니다.
   - `${maven.multiModuleProjectDirectory}`은(는) 다중 모듈 프로젝트의 루트 디렉터리를 가리키는 Maven 속성입니다.

- AEM WKND 프로젝트 `core` 모듈의 `core/pom.xml` 파일에서 다음 두 가지 사항을 변경합니다.

   - `my-example-jar`을(를) 종속성으로 추가합니다.

     ```xml
     ...
     <!-- My Example JAR -->
     <dependency>
         <groupId>com.my.example</groupId>
         <artifactId>my-example-jar</artifactId>
     </dependency>
     ...
     ```

   - 빌드 중인 OSGi 번들(aem-guides-wknd.core)에 `my-example-jar`을(를) 포함하도록 `bnd-maven-plugin` 구성을 업데이트합니다.

     ```xml
     ...
     <plugin>
         <groupId>biz.aQute.bnd</groupId>
         <artifactId>bnd-maven-plugin</artifactId>
         <executions>
             <execution>
                 <id>bnd-process</id>
                 <goals>
                     <goal>bnd-process</goal>
                 </goals>
                 <configuration>
                     <bnd><![CDATA[
                 Import-Package: javax.annotation;version=0.0.0,*
                 <!-- Include the 3rd party jar as inline resource-->
                 -includeresource: \
                 lib/my-example-jar.jar=my-example-jar-1.0-SNAPSHOT.jar;lib:=true
                         ]]></bnd>
                 </configuration>
             </execution>
         </executions>
     </plugin>        
     ...
     ```

- 다음 명령을 사용하여 AEM WKND 프로젝트를 빌드하고 배포합니다.

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- 브라우저의 URL `http://localhost:4502/bin/sayhello`에 액세스하여 `SayHello` 서블릿이 예상대로 작동하는지 확인하십시오.

- 위의 변경 사항을 AEM WKND 프로젝트의 저장소에 커밋합니다. 그런 다음 Cloud Manager 파이프라인을 실행하여 RDE 또는 AEM 환경의 변경 사항을 확인합니다.

  ![SayHello 서블릿 확인 - JAR 서비스](./assets/install-third-party-articafcts/verify-sayhello-servlet-jar-service.png)

AEM WKND 프로젝트의 [tutorial/install-third-party-jar](https://github.com/adobe/aem-guides-wknd/compare/main...tutorial/install-3rd-party-jar) 분기에 위와 같은 참조 변경 사항이 있습니다.

Java jar 파일 _을(를) 공용 Maven 저장소에서 사용할 수 있지만 OSGi 번들이 아닌 경우_&#x200B;에서는 `<dependency>`의 `system` 범위 및 `systemPath` 요소가 필요하지 않은 경우를 제외하고 위의 단계를 따를 수 있습니다.

### 주요 학습{#key-learnings-jar}

OSGi 번들이 아니며 공용 Maven 저장소에서 사용할 수 있거나 사용할 수 없는 Java jar는 다음 단계에 따라 AEM 프로젝트에 설치할 수 있습니다.

- 작성 중인 OSGi 번들에 Java jar를 인라인 리소스로 포함하도록 코어 모듈의 `pom.xml` 파일에서 `bnd-maven-plugin` 구성을 업데이트합니다.

공개 Maven 저장소에서 Java jar를 사용할 수 없는 경우에만 다음 단계가 필요합니다.

- Java jar를 `all` 모듈의 `resource/jar` 디렉터리에 복사합니다.

- 루트 및 코어 모듈의 `pom.xml` 파일을 업데이트하여 `system` 범위 및 jar 파일을 가리키는 `systemPath`에 종속성으로 Java jar를 추가합니다.

## AEM 프로젝트에 서드파티 패키지 설치

주 분기에서 로컬로 빌드된 [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) _SNAPSHOT_ 버전을 설치하겠습니다.

이 작업은 공용 Maven 저장소에서 사용할 수 없는 AEM 패키지를 설치하는 단계를 보여 주기 위해 수행됩니다.

ACS AEM Commons 패키지는 공개 Maven 저장소에서 사용할 수 있습니다. [AEM Maven 프로젝트에 ACS AEM Commons 추가](https://adobe-consulting-services.github.io/acs-aem-commons/pages/maven.html)를 참조하여 AEM 프로젝트에 추가하십시오.

### `all` 모듈에 패키지 추가

첫 번째 단계는 AEM WKND 프로젝트의 `all` 모듈에 패키지를 추가하는 것입니다.

- POM 파일에서 ACS AEM Commons 릴리스 종속성을 주석 처리하거나 제거합니다. 종속성을 확인하려면 [AEM Maven 프로젝트에 ACS AEM Commons 추가](https://adobe-consulting-services.github.io/acs-aem-commons/pages/maven.html)를 참조하십시오.

- [ACS AEM Commons 저장소](https://github.com/Adobe-Consulting-Services/acs-aem-commons)의 `master` 분기를 로컬 컴퓨터에 복제합니다.

- 다음 명령을 사용하여 ACS AEM Commons SNAPSHOT 버전을 작성합니다.

  ```
  $mvn clean install
  ```

- 로컬로 빌드된 패키지는 @ `all/target`에 있으며 두 개의 .zip 파일이 있습니다. 하나는 AEM as a Cloud Service을 위한 것이고 다른 하나는 AEM 6.X를 위한 것입니다.`-cloud`

- AEM WKND 프로젝트의 `all` 모듈에서 `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` 디렉터리 구조를 만듭니다. `/all/src/main/content` 디렉터리가 있습니다. `jcr_root/apps/wknd-vendor-packages/container/install` 디렉터리만 만들면 됩니다.

- 로컬로 빌드된 패키지(.zip) 파일을 `/all/src/main/content/jcr_root/apps/mysite-vendor-packages/container/install` 디렉터리에 복사합니다.

- 다음 명령을 사용하여 AEM WKND 프로젝트를 빌드하고 배포합니다.

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- 설치된 ACS AEM Commons 패키지 확인:

   - CRX 패키지 관리자 @ `http://localhost:4502/crx/packmgr/index.jsp`

     ![ACS AEM Commons 스냅숏 버전 패키지](./assets/install-third-party-articafcts/acs-aem-commons-snapshot-package.png)

   - OSGi 콘솔 @ `http://localhost:4502/system/console/bundles`

     ![ACS AEM Commons 스냅숏 버전 번들](./assets/install-third-party-articafcts/acs-aem-commons-snapshot-bundle.png)

- 위의 변경 사항을 AEM WKND 프로젝트의 저장소에 커밋합니다. 그런 다음 Cloud Manager 파이프라인을 실행하여 RDE 또는 AEM 환경의 변경 사항을 확인합니다.

### 주요 학습{#key-learnings-package}

공개 Maven 저장소에서 사용할 수 없는 AEM 패키지는 다음 단계에 따라 AEM 프로젝트에 설치할 수 있습니다.

- `all` 모듈의 `jcr_root/apps/<PROJECT-NAME>-vendor-packages/container/install` 디렉터리에 패키지를 복사합니다. 이 단계는 패키지를 패키징하고 AEM 인스턴스에 배포하는 데 필요합니다.


## 요약

이 자습서에서는 AEM 프로젝트를 빌드하고 배포할 때 공개 Maven 저장소에서 사용할 수 없는 서드파티 아티팩트(번들, Java Jar 및 패키지)를 설치하는 방법을 배웠습니다.
