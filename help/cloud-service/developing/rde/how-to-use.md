---
title: 신속한 개발 환경 사용 방법
description: Rapid Development Environment를 사용하여 로컬 컴퓨터에서 코드 및 콘텐츠를 배포하는 방법에 대해 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 815
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 0%

---

# 신속한 개발 환경 사용 방법

학습 **사용 방법** AEMas a Cloud Service 의 RDE(신속한 개발 환경) 즐겨찾는 IDE(통합 개발 환경)에서 최종 코드에 대한 개발 주기를 단축하기 위해 코드 및 콘텐츠를 배포합니다.

사용 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM-RDE를 실행하여 다양한 AEM 아티팩트를 RDE에 배포하는 방법에 대해 알아봅니다. `install` 즐겨 사용하는 IDE에서 명령.

- AEM 코드 및 콘텐츠 패키지(모두, ui.apps) 배포
- OSGi 번들 및 구성 파일 배포
- Apache 및 Dispatcher 구성을 zip 파일로 배포
- HTL 같은 개별 파일 `.content.xml` (대화 상자 XML) 배포
- 다음과 같은 기타 RDE 명령 검토 `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## 전제 조건

복제 [WKND 사이트](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 를 프로젝트에 추가하고 즐겨 찾는 IDE에서 열어 AEM 아티팩트를 RDE에 배포합니다.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

그런 다음 다음 다음 maven 명령을 실행하여 로컬 AEM-SDK에 빌드하고 배포합니다.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## AEM-RDE 플러그인을 사용하여 AEM 아티팩트 배포

사용 `aem:rde:install` command, 다양한 AEM 아티팩트를 배포하겠습니다.

### 배포 `all` 및 `dispatcher` 패키지

일반적인 시작점은 먼저 를 배포하는 것입니다. `all` 및 `dispatcher` 다음 명령을 실행하여 패키지화합니다.

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

배포가 성공하면 작성 및 게시 서비스 모두에서 WKND 사이트를 확인하십시오. WKND 사이트 페이지에서 콘텐츠를 추가 및 편집하고 게시할 수 있어야 합니다.

### 구성 요소 개선 및 배포

을(를) 강화합니다. `Hello World Component` RDE에 배포합니다.

1. 대화 상자 XML(`.content.xml`) 파일 출처: `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` 폴더
1. 추가 `Description` 기존 뒤에 텍스트 필드 `Text` 대화 상자 필드

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. 를 엽니다. `helloworld.html` 파일 출처: `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` 폴더
1. 렌더링 `Description` 기존 항목 이후의 속성 `<div>` 의 요소 `Text` 속성.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Maven 빌드를 수행하거나 개별 파일을 동기화하여 로컬 AEM-SDK에서 변경 사항을 확인합니다.

1. 를 통해 변경 사항을 RDE에 배포 `ui.apps` 개별 대화 상자 및 HTL 파일을 패키징하거나 배포합니다.

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

1. 를 추가하거나 편집하여 RDE에서 변경 사항 확인 `Hello World Component` WKND 사이트 페이지에서 확인할 수 있습니다.

### 리뷰 `install` 명령 옵션

위의 개별 파일 배포 명령 예제에서 `-t` 및 `-p` 플래그는 JCR 경로의 유형과 대상을 각각 나타내는 데 사용됩니다. 사용 가능한 을(를) 검토해 보겠습니다. `install` 다음 명령을 실행하여 표시되는 명령 옵션입니다.

```shell
$ aio aem:rde:install --help
```

깃발은 자기 설명력이에요 `-s` 플래그는 작성자 또는 게시 서비스에만 배포를 타겟팅하는 데 유용합니다. 사용 `-t` 배포 시 플래그 지정 **content-file 또는 content-xml** 와 함께 있는 파일 `-p` AEM RDE 환경에서 대상 JCR 경로를 지정하는 플래그입니다.

### OSGi 번들 배포

OSGi 번들을 배포하는 방법에 대해 알아보려면 다음을 향상하겠습니다. `HelloWorldModel` Java™ 클래스 및 RDE에 배포

1. 를 엽니다. `HelloWorldModel.java` 파일 출처: `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 폴더
1. 업데이트 `init()` 메서드를 다음과 같이 바꿉니다.

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. 를 배포하여 로컬 AEM-SDK에서 변경 사항을 확인합니다. `core` maven 명령을 통한 번들
1. 다음 명령을 실행하여 변경 사항을 RDE에 배포합니다

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. 를 추가하거나 편집하여 RDE에서 변경 사항 확인 `Hello World Component` WKND 사이트 페이지에서 확인할 수 있습니다.

### OSGi 구성 배포

다음과 같은 개별 구성 파일 또는 전체 구성 패키지를 배포할 수 있습니다.

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

Apache 또는 Dispatcher 구성 파일 **개별적으로 배포할 수 없음**, 그러나 전체 Dispatcher 폴더 구조는 ZIP 파일 형태로 배포해야 합니다.

1. 의 구성 파일에서 원하는 대로 변경합니다. `dispatcher` 모듈, 데모 목적으로 `dispatcher/src/conf.d/available_vhosts/wknd.vhost` 캐시하기 `html` 파일은 60초 동안만 사용할 수 있습니다.

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

1. 로컬에서 변경 사항을 확인합니다. [로컬에서 Dispatcher 실행](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) 을 참조하십시오.
1. 다음 명령을 실행하여 변경 사항을 RDE에 배포합니다.

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. RDE에서 변경 사항 확인

## 추가 AEM RDE 플러그인 명령

로컬 컴퓨터에서 RDE를 관리하고 상호 작용하기 위한 추가 AEM RDE 플러그인 명령을 검토해 보겠습니다.

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

위의 명령을 사용하면 즐겨 찾는 IDE에서 RDE를 관리하여 개발/배포 수명 주기를 단축할 수 있습니다.

## 다음 단계

에 대해 알아보기 [RDE를 사용한 개발/배포 수명 주기](./development-life-cycle.md) 신속하게 기능을 제공할 수 있습니다.


## 추가 리소스

[RDE 명령 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[AEM Rapid Development 환경과의 상호 작용을 위한 Adobe I/O Runtime CLI 플러그인](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM 프로젝트 설정](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
