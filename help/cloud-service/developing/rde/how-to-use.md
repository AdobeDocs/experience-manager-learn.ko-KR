---
title: 신속한 개발 환경 사용 방법
description: Rapid Development Environment를 사용하여 로컬 시스템에서 코드 및 컨텐츠를 배포하는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---


# 신속한 개발 환경 사용 방법

학습 **사용 방법** AEM as a Cloud Service의 RDE(Rapid Development Environment) 자주 사용하는 IDE(Integrated Development Environment)에서 최종 코드 개발 주기를 RDE에 빠르게 배포할 수 있습니다.

사용 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM-RDE를 실행하여 다양한 AEM 객체를 RDE에 배포하는 방법을 알아봅니다 `install` 명령을 선택합니다.

- AEM 코드 및 컨텐츠 패키지(모두, ui.apps) 배포
- OSGi 번들 및 구성 파일 배포
- Apache 및 Dispatcher가 배포를 zip 파일로 구성합니다
- HTL과 같은 개별 파일, `.content.xml` (대화 상자 XML) 배포
- 와 같은 기타 RDE 명령 검토 `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## 전제 조건

복제 [WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 프로젝트를 생성하고 즐겨찾는 IDE에서 열어 AEM 아티팩트를 RDE에 배포합니다.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

그런 다음 다음 다음 maven 명령을 실행하여 로컬 AEM-SDK에 빌드하고 배포합니다.

```
$ cd aem-guides-wknd/
$ mvn clean install -PautoInstallSinglePackage
```

## AEM-RDE 플러그인을 사용하여 AEM 객체 배포

사용 `aem:rde:install` 명령, 다양한 AEM 아티팩트를 배포하겠습니다.

### 배포 `all` 및 `dispatcher` 패키지

일반적인 시작점은 먼저 `all` 및 `dispatcher` 다음 명령을 실행하여 패키지 생성

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

성공적으로 배포한 경우 작성 및 게시 서비스 모두에서 WKND 사이트를 확인합니다. WKND 사이트 페이지에서 컨텐츠를 추가 및 편집하고 게시할 수 있습니다.

### 구성 요소 개선 및 배포

더 나은 `Hello World Component` RDE에 배포합니다.

1. 대화 상자 XML(`.content.xml`)의 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` 폴더
1. 추가 `Description` 기존 필드 뒤에 있는 텍스트 필드 `Text` 대화 상자 필드

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. 를 엽니다. `helloworld.html` 파일 위치 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` 폴더
1. 렌더링 `Description` 기존 속성 뒤에 속성 추가 `<div>` 요소의 요소 `Text` 속성을 사용합니다.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. maven 빌드를 수행하거나 개별 파일을 동기화하여 로컬 AEM-SDK에서 변경 사항을 확인합니다.

1. 를 통해 RDE에 변경 사항 배포 `ui.apps` 개별 대화 상자 및 HTL 파일을 배포하거나 패키지로 만듭니다.

   ```shell
   # Using 'ui.apps' package
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 를 추가하거나 편집하여 RDE에서 변경 사항을 확인합니다. `Hello World Component` 클릭합니다.

### 를 검토합니다. `install` 명령 옵션

위의 개별 파일 배포 명령 예제에서 `-t` 및 `-p` 플래그는 각각 JCR 경로의 유형과 대상을 나타내는 데 사용됩니다. 사용 가능한 사항을 검토하겠습니다 `install` 다음 명령을 실행하여 명령 옵션을 선택합니다.

```shell
$ aio aem:rde:install --help
```

깃발은 자기 설명이고 `-s` 플래그는 작성자 또는 게시 서비스에만 배포를 타깃팅하는 데 유용합니다. 를 사용하십시오 `-t` 배포할 때 플래그 지정 **content-file 또는 content-xml** 와 함께 파일 `-p` AEM RDE 환경에서 대상 JCR 경로를 지정하는 플래그입니다.

### OSGi 번들 배포

OSGi 번들을 배포하는 방법을 배우려면 다음을 강화하겠습니다 `HelloWorldModel` Java™ 클래스를 RDE에 배포합니다.

1. 를 엽니다. `HelloWorldModel.java` 파일 위치 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 폴더
1. 업데이트 `init()` 방법은 다음과 같습니다.

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. 를 배포하여 로컬 AEM-SDK에서 변경 사항을 확인합니다 `core` maven 명령을 통해 번들
1. 다음 명령을 실행하여 RDE에 변경 사항을 배포합니다

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. 를 추가하거나 편집하여 RDE에서 변경 사항을 확인합니다. `Hello World Component` 클릭합니다.

### OSGi 구성 배포

개별 구성 파일 또는 전체 구성 패키지를 배포할 수 있습니다. 예를 들면 다음과 같습니다.

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>작성자 또는 게시 인스턴스에만 OSGi 구성을 설치하려면 `-s` 플래그.


### Apache 또는 Dispatcher 구성 배포

Apache 또는 Dispatcher 구성 파일 **개별적으로 배포할 수 없음**&#x200B;하지만 전체 Dispatcher 폴더 구조를 ZIP 파일 형태로 배포해야 합니다.

1. 의 구성 파일에서 원하는 대로 변경합니다 `dispatcher` 모듈, 데모 목적으로 업데이트 `dispatcher/src/conf.d/available_vhosts/wknd.vhost` 캐시하려면 `html` 파일은 60초 동안만 적용됩니다.

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. 로컬에서 변경 내용을 확인합니다. 자세한 내용은 [로컬에서 Dispatcher 실행](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) 자세한 내용
1. 다음 명령을 실행하여 RDE에 변경 사항을 배포합니다.

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. RDE에서 변경 사항 확인

## 추가 AEM RDE 플러그인 명령

추가 AEM RDE 플러그인 명령을 검토하여 관리하고 로컬 시스템에서 RDE와 상호 작용하겠습니다.

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

위의 명령을 사용하면 자주 사용하는 IDE에서 RDE를 관리할 수 있으므로 개발/배포 수명 주기가 빨라집니다.

## 다음 단계

에 대해 알아보기 [RDE를 사용한 개발/배포 수명 주기](./development-life-cycle.md) 을 신속하게 기능을 제공할 수 있습니다.


## 추가 리소스

[RDE 명령 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[AEM Rapid Development Environments와의 상호 작용을 위한 Adobe I/O Runtime CLI 플러그인](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM 프로젝트 설정](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
