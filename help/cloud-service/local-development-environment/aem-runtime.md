---
title: AEM as a Cloud Service 개발을 위한 로컬 AEM SDK 설정
description: AEM as a Cloud Service SDK의 Quickstart Jar를 사용하여 로컬 AEM SDK 런타임을 설정합니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 411
source-git-commit: 99e3cadc71ca4e26f9e4034085788dfc5407d1bb
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 7%

---

# 로컬 AEM SDK 설정 {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="로컬 AEM 런타임"
>abstract="AEM(Adobe Experience Manager)은 AEM as a Cloud Service SDK의 QuickStart Jar로 사용하여 로컬에서 실행할 수 있습니다. 이 경우 개발자는 소스 제어에 커밋하기 전에 사용자 정의 코드, 구성 및 콘텐츠를 배포하고 테스트하고, AEM as a Cloud Service 환경으로 배포할 수 있습니다."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=ko" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEM as a Cloud Service SDK 다운로드"

AEM(Adobe Experience Manager)은 AEM as a Cloud Service SDK의 QuickStart Jar로 사용하여 로컬에서 실행할 수 있습니다. 이 경우 개발자는 소스 제어에 커밋하기 전에 사용자 정의 코드, 구성 및 콘텐츠를 배포하고 테스트하고, AEM as a Cloud Service 환경으로 배포할 수 있습니다.

`~`은(는) 사용자 디렉터리의 약어로 사용됩니다. Windows에서는 `%HOMEPATH%`에 해당합니다.

## Java™ 설치

Experience Manager은 Java™ 애플리케이션이므로 개발 도구를 지원하려면 Oracle Java™ SDK이 필요합니다.

1. [최신 Java™ SDK 11 다운로드 및 설치](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 다음 명령을 실행하여 Oracle Java™ 11 SDK이 설치되어 있는지 확인합니다.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## AEM as a Cloud Service SDK 다운로드

AEM as a Cloud Service SDK 또는 AEM SDK에는 개발을 위해 로컬에서 AEM 작성자 및 게시를 실행하는 데 사용되는 Quickstart Jar와 호환되는 버전의 Dispatcher 도구가 포함되어 있습니다.

1. Adobe ID으로 [https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)에 로그인합니다.
   + AEM as a Cloud Service SDK을 다운로드하려면 Adobe 조직 __을(를) AEM as a Cloud Service에 대해 프로비저닝해야__&#x200B;합니다.
1. __AEM as a Cloud Service__ 탭으로 이동
1. __내림차순__ 순서로 __게시 날짜__&#x200B;별로 정렬
1. 최신 __AEM SDK__ 결과 행 클릭
1. EULA를 검토하고 수락한 다음 __다운로드__ 단추를 탭합니다.

## AEM SDK zip에서 Quickstart Jar 추출

1. 다운로드한 `aem-sdk-XXX.zip` 파일 압축 풀기

## 로컬 AEM 작성자 서비스 설정{#set-up-local-aem-author-service}

로컬 AEM 작성자 서비스는 개발자가 콘텐츠를 만들고 관리하기 위해 공유할 로컬 경험 디지털 마케터/콘텐츠 작성자를 제공합니다.  AEM Author Service는 작성 및 미리보기 환경으로 설계되어 기능 개발에 대한 대부분의 유효성 검사를 수행할 수 있으므로 로컬 개발 프로세스의 중요한 요소입니다.

1. `~/aem-sdk/author` 폴더 만들기
1. __Quickstart JAR__ 파일을 `~/aem-sdk/author`에 복사하고 이름을 `aem-author-p4502.jar`(으)로 변경합니다.
1. 명령줄에서 다음을 실행하여 로컬 AEM 작성자 서비스를 시작합니다.
   + `java -jar aem-author-p4502.jar`
      + 관리자 암호를 `admin`(으)로 제공하십시오. 모든 관리자 암호는 사용할 수 있지만, 로컬 개발에 기본값을 사용하여 재구성할 필요가 없도록 하는 것이 좋습니다.

   *두 번 클릭[&#128279;](#troubleshooting-double-click)하여 AEM을 Cloud Service Quickstart Jar 로 시작할 수*&#x200B;없습니다.
1. 웹 브라우저의 [http://localhost:4502](http://localhost:4502)에서 로컬 AEM 작성자 서비스에 액세스합니다

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## 로컬 AEM 게시 서비스 설정

로컬 AEM Publish 서비스는 개발자에게 AEM이 사용할 로컬 경험(예: AEM에 있는 웹 사이트 검색)을 제공합니다. 로컬 AEM 게시 서비스는 AEM SDK의 [Dispatcher 도구](./dispatcher-tools.md)와 통합되며 개발자가 최종 사용자 대면 경험을 연기 테스트하고 세부 조정할 수 있으므로 중요합니다.

1. `~/aem-sdk/publish` 폴더 만들기
1. __Quickstart JAR__ 파일을 `~/aem-sdk/publish`에 복사하고 이름을 `aem-publish-p4503.jar`(으)로 변경합니다.
1. 명령줄에서 다음을 실행하여 로컬 AEM 게시 서비스를 시작합니다.
   + `java -jar aem-publish-p4503.jar`
      + 관리자 암호를 `admin`(으)로 제공하십시오. 모든 관리자 암호는 사용할 수 있지만, 로컬 개발에 기본값을 사용하여 재구성할 필요가 없도록 하는 것이 좋습니다.

   *두 번 클릭[&#128279;](#troubleshooting-double-click)하여 AEM을 Cloud Service Quickstart Jar 로 시작할 수*&#x200B;없습니다.
1. 웹 브라우저의 [http://localhost:4503](http://localhost:4503)에서 로컬 AEM 게시 서비스에 액세스합니다

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## 프리릴리스 모드에서 로컬 AEM 서비스 설정

로컬 AEM 런타임을 [프리릴리스 모드](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=ko)에서 시작하여 개발자가 AEM as a Cloud Service의 다음 릴리스 기능에 맞게 빌드할 수 있습니다. 로컬 AEM 런타임의 첫 번째 시작 시 `-r prerelease` 인수를 전달하여 프리릴리스를 사용할 수 있습니다. 로컬 AEM Author 및 AEM Publish 서비스에서 모두 사용할 수 있습니다.


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux®]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## 콘텐츠 배포 시뮬레이션 {#content-distribution}

실제 Cloud Service 환경에서는 [Sling 콘텐츠 배포](https://sling.apache.org/documentation/bundles/content-distribution.html) 및 Adobe 파이프라인을 사용하여 Author 서비스에서 Publish 서비스로 콘텐츠가 배포됩니다. [Adobe 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=ko#content-distribution)은(는) 클라우드 환경에서만 사용할 수 있는 격리된 마이크로서비스입니다.

개발 중에 로컬 Author 및 Publish 서비스를 사용하여 콘텐츠 배포를 시뮬레이션하는 것이 바람직할 수 있습니다. 이 작업은 기존 복제 에이전트를 활성화하여 수행할 수 있습니다.

>[!NOTE]
>
> 복제 에이전트는 로컬 Quickstart JAR에서만 사용할 수 있으며 컨텐츠 배포 시뮬레이션만 제공합니다.

1. **작성자** 서비스에 로그인하여 [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html)&#x200B;(으)로 이동합니다.
1. 기본 복제 에이전트를 열려면 **기본 에이전트(게시)**&#x200B;를 클릭하십시오.
1. 에이전트의 구성을 열려면 **편집**&#x200B;을 클릭하십시오.
1. **설정** 탭에서 다음 필드를 업데이트합니다.

   + **사용** - true 확인
   + **에이전트 사용자 ID** - 이 필드를 비워 둡니다.

   ![복제 에이전트 구성 - 설정](assets/aem-runtime/settings-config.png)

1. **전송** 탭에서 다음 필드를 업데이트합니다.

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **사용자** - `admin`
   + **암호** - `admin`

   ![복제 에이전트 구성 - 전송](assets/aem-runtime/transport-config.png)

1. 구성을 저장하고 **기본** 복제 에이전트를 사용하려면 **확인**&#x200B;을 클릭하십시오.
1. 이제 Author 서비스의 콘텐츠를 변경하고 Publish 서비스에 게시할 수 있습니다.

![페이지 게시](assets/aem-runtime/publish-page-changes.png)

## Jar 시작 모드 빠른 시작

Quickstart Jar 이름 `aem-<tier>_<environment>-p<port number>.jar`은(는) 시작 방법을 지정합니다. AEM이 특정 계층, 작성자 또는 게시에서 시작되면 대체 계층으로 변경할 수 없습니다. 이렇게 하려면 첫 번째 실행 중에 생성된 `crx-Quickstart` 폴더를 삭제하고 Quickstart Jar를 다시 실행해야 합니다. 환경 및 포트는 변경할 수 있지만 로컬 AEM 인스턴스의 중지/시작이 필요합니다.

환경 `dev`, `stage` 및 `prod`을(를) 변경하면 개발자가 AEM에서 환경별 구성을 올바르게 정의하고 확인할 수 있습니다. 로컬 개발은 주로 기본 `dev` 환경 실행 모드에 대해 수행하는 것이 좋습니다.

사용 가능한 순열은 다음과 같습니다.

| Jar 파일 이름 빠른 시작 | 모드 설명 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | 포트 4502에서 개발 실행 모드의 작성자 |
| `aem-author_dev-p4502.jar` | 포트 4502(`aem-author-p4502.jar`과(와) 동일)에서 개발 실행 모드의 작성자로 |
| `aem-author_stage-p4502.jar` | 포트 4502에서 스테이징 실행 모드의 작성자로 |
| `aem-author_prod-p4502.jar` | 포트 4502에서 프로덕션 실행 모드의 작성자로 |
| `aem-publish-p4503.jar` | 포트 4503의 개발 실행 모드에서 게시로 |
| `aem-publish_dev-p4503.jar` | 포트 4503의 개발 실행 모드에서 게시로 사용(`aem-publish-p4503.jar`과 동일) |
| `aem-publish_stage-p4503.jar` | 포트 4503의 스테이징 실행 모드에서 게시로 |
| `aem-publish_prod-p4503.jar` | 포트 4503에서 프로덕션 실행 모드로 게시로 |

포트 번호는 로컬 개발 시스템에서 사용 가능한 모든 포트일 수 있지만, 규칙에 따라 다릅니다.

+ __4502__ 포트는 __로컬 AEM 작성자 서비스__&#x200B;에 사용됩니다.
+ __4503__ 포트는 __로컬 AEM 게시 서비스__&#x200B;에 사용됩니다.

이를 변경하려면 AEM SDK 구성을 조정해야 할 수 있습니다

## 로컬 AEM 런타임 중지

로컬 AEM 런타임을 중지하려면 AEM Author 또는 Publish 서비스를 사용하고 AEM 런타임을 시작하는 데 사용된 명령줄 창을 열고 `Ctrl-C`을(를) 탭합니다. AEM이 종료될 때까지 기다립니다. 종료 프로세스가 완료되면 명령줄 프롬프트를 사용할 수 있습니다.

## 선택적 로컬 AEM 런타임 설정 작업

+ __OSGi 구성 환경 변수 및 비밀 변수__&#x200B;은(는) aio CLI를 사용하여 관리하는 대신 [AEM 로컬 런타임에 특별히 설정된](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ko#local-development)입니다.

## Quickstart Jar 업데이트 시기

AEM as a Cloud Service &quot;기능 릴리스&quot;에 대한 릴리스 케이던스인 매월 마지막 목요일에 AEM SDK을 적어도 매월 또는 그 직후에 업데이트합니다.

>[!WARNING]
>
> Quickstart Jar를 새 버전으로 업데이트하려면 전체 로컬 개발 환경을 교체해야 하므로 로컬 AEM 저장소의 모든 코드, 구성 및 콘텐츠가 손실됩니다. 폐기해서는 안 되는 코드, 구성 또는 콘텐츠가 Git에 안전하게 커밋되거나 로컬 AEM 인스턴스에서 AEM 패키지로 내보내지는지 확인합니다.

### AEM SDK 업그레이드 시 컨텐츠 손실을 방지하는 방법

AEM SDK을 업그레이드하면 새 저장소를 포함하여 완전히 새로운 AEM 런타임이 생성됩니다. 즉, 이전 AEM SDK 저장소에 대한 변경 사항이 모두 손실됩니다. 다음은 AEM SDK 업그레이드 사이에서 콘텐츠를 유지하는 데 도움이 되는 실행 가능한 전략이며, 개별적으로 또는 함께 사용할 수 있습니다.

1. 개발에 도움이 되는 &quot;샘플&quot; 콘텐츠를 포함하는 전용 콘텐츠 패키지를 만들고 Git에서 유지 관리합니다. AEM SDK 업그레이드를 통해 유지해야 하는 모든 콘텐츠는 이 패키지로 유지되며 AEM SDK을 업그레이드한 후 다시 배포됩니다.
1. `includepaths` 지시문과 함께 [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html)를 사용하여 이전 AEM SDK 저장소의 콘텐츠를 새 AEM SDK 저장소로 복사합니다.
1. 이전 AEM SDK의 AEM 패키지 관리자 및 컨텐츠 패키지를 사용하여 모든 컨텐츠를 백업하고 새 AEM SDK에 다시 설치합니다.

위의 접근 방식을 사용하여 AEM SDK 업그레이드 간에 코드를 유지 관리하면 개발 방지 패턴이 있음을 나타냅니다. 일회용이 아닌 코드는 개발 IDE에서 가져와서 배포를 통해 AEM SDK으로 이동해야 합니다.

## 문제 해결

### Quickstart Jar 파일을 두 번 클릭하면 오류가 발생합니다{#troubleshooting-double-click}

Quickstart Jar를 두 번 클릭하여 시작하면 오류 모달이 표시되어 AEM이 로컬로 시작되지 않도록 합니다.

![문제 해결 - Quickstart Jar 파일을 두 번 클릭합니다](./assets/aem-runtime/troubleshooting__double-click.png)

이는 AEM as a Cloud Service Quickstart Jar가 AEM을 로컬에서 시작하기 위해 Quickstart Jar를 두 번 클릭하는 것을 지원하지 않기 때문입니다. 대신 해당 명령줄에서 Jar 파일을 실행해야 합니다.

AEM 작성자 서비스를 시작하려면 Quickstart Jar가 포함된 디렉터리로 `cd`하고 명령을 실행합니다.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

또는 AEM Publish 서비스를 시작하려면 Quickstart Jar가 포함된 디렉터리에 `cd`을(를) 삽입하고 다음 명령을 실행합니다.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]

### 명령줄에서 Quickstart Jar를 시작하면 즉시 중단됩니다{#troubleshooting-java-8}

명령줄에서 Quickstart Jar를 시작하면 프로세스가 즉시 중단되고 AEM 서비스가 시작되지 않으며 다음 오류가 발생합니다.

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

이는 AEM as a Cloud Service에 Java™ SDK 11이 필요하며 다른 버전(Java™ 8일 가능성이 높음)을 실행 중이기 때문입니다. 이 문제를 해결하려면 [Oracle Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)을(를) 다운로드하여 설치하십시오.

Oracle Java™ 11 SDK이 설치되면 명령줄에서 명령을 실행하여 활성 버전인지 확인합니다.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

## 추가 리소스

+ [AEM SDK 다운로드](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [도커 다운로드](https://www.docker.com/)
+ [Experience Manager Dispatcher 설명서](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ko-KR)
