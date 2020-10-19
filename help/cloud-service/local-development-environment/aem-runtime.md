---
title: AEM용 로컬 AEM 런타임을 Cloud Service 개발으로 설정
description: AEM을 Cloud Service SDK의 Quickstart Jar로 사용하여 로컬 AEM 런타임을 설정합니다.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: 4cfbf975919eb38413be8446b70b107bbfebb845
workflow-type: tm+mt
source-wordcount: '1406'
ht-degree: 1%

---


# 로컬 AEM 런타임 설정

Adobe Experience Manager(AEM)은 AEM을 Cloud Service SDK의 Quickstart Jar로 사용하여 로컬로 실행할 수 있습니다. 이를 통해 개발자는 사용자 정의 코드, 구성 및 컨텐츠를 소스 제어에 커밋하기 전에 배포 및 테스트하고 이를 AEM에 Cloud Service 환경으로 배포할 수 있습니다.

사용자 디렉토리 `~` 의 약칭으로 사용됩니다. Windows의 경우 이 값은 `%HOMEPATH%`

## Java 설치

Experience Manager은 Java 응용 프로그램이므로 개발 도구 지원을 위해 Java SDK가 필요합니다.

1. [최신 Java SDK 11 다운로드 및 설치](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AssoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atfoling&amp;fulltext=Oracle%7E+JDK%7E&amp;orderby%444 0jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 다음 명령을 실행하여 Java 11 SDK가 설치되어 있는지 확인합니다.
   + Windows:`java -version`
   + macOS/Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## AEM을 Cloud Service SDK로 다운로드

Cloud Service SDK 또는 AEM SDK로 AEM에는 개발을 위해 AEM 작성자 및 로컬로 게시 실행을 위해 사용되는 빠른 시작 JAR와 호환되는 Dispatcher 도구 버전이 포함되어 있습니다.

1. https://experience.adobe.com/#/downloads [으로](https://experience.adobe.com/#/downloads) 로그인
   + AEM을 Cloud Service SDK로 다운로드하려면 Adobe 조직이 Cloud Service으로 프로비저닝되어야 ____ 합니다.
1. Cloud Service 탭으로 __AEM으로__ 이동
1. 게시된 __날짜별로 내림차순__ __으로__ 정렬
1. 최신 __AEM SDK__ 결과 행을 클릭합니다.
1. EULA를 검토하고 승인한 후 __다운로드__ 단추를 누릅니다

## AEM SDK zip에서 빠른 시작 Jar 추출

1. 다운로드한 파일의 압축을 `aem-sdk-XXX.zip` 해제합니다.

## 로컬 AEM 작성자 서비스 설정{#set-up-local-aem-author-service}

로컬 AEM 작성자 서비스는 개발자에게 컨텐츠를 만들고 관리하기 위해 공유할 디지털 마케터/컨텐츠 작성자가 로컬 경험을 제공합니다.  AEM 작성자 서비스는 저작 및 미리 보기 환경으로 디자인되어 대부분의 기능 개발 유효성을 검사할 수 있으므로 로컬 개발 프로세스의 중요한 요소가 됩니다.

1. 폴더 만들기 `~/aem-sdk/author`
1. 빠른 __시작 JAR__ 파일을 복사하여 `~/aem-sdk/author` `aem-author-p4502.jar`
1. 명령줄에서 다음을 실행하여 로컬 AEM 작성자 서비스를 시작합니다.
   + `java -jar aem-author-p4502.jar`
      + 관리자 암호를 입력합니다 `admin`. 모든 관리자 암호는 사용할 수 있지만 로컬 개발에 기본값을 사용하여 다시 구성할 필요가 없습니다.

   두 번 클릭하여 Cloud Service 빠른 시작 JAR *로 AEM을 시작할 수 없습니다* [](#troubleshooting-double-click).
1. 웹 브라우저에서 로컬 AEM 작성자 서비스 [http://localhost:4502](http://localhost:4502) 액세스

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## 로컬 AEM 게시 서비스 설정

로컬 AEM 게시 서비스는 개발자에게 AEM에서 호스팅하는 웹 사이트를 검색하는 것과 같이 AEM 최종 사용자가 가질 로컬 경험을 제공합니다. 로컬 AEM 게시 서비스는 AEM SDK의 [디스패처 도구와](./dispatcher-tools.md) 통합되어 개발자가 최종 사용자 대면 경험을 연기하고 세밀하게 조정할 수 있으므로 중요합니다.

1. 폴더 만들기 `~/aem-sdk/publish`
1. 빠른 __시작 JAR__ 파일을 복사하여 `~/aem-sdk/publish` `aem-publish-p4503.jar`
1. 명령줄에서 다음을 실행하여 로컬 AEM 게시 서비스를 시작합니다.
   + `java -jar aem-publish-p4503.jar`
      + 관리자 암호를 입력합니다 `admin`. 모든 관리자 암호는 사용할 수 있지만 로컬 개발에 기본값을 사용하여 다시 구성할 필요가 없습니다.

   두 번 클릭하여 Cloud Service 빠른 시작 JAR *로 AEM을 시작할 수 없습니다* [](#troubleshooting-double-click).
1. 웹 브라우저에서 로컬 AEM 게시 서비스 [에](http://localhost:4503) 액세스

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## 빠른 시작 Jar 시작 모드

빠른 시작 JAR의 이름 지정 `aem-<tier>_<environment>-p<port number>.jar` 은 시작할 방법을 지정합니다. AEM이 특정 계층, 작성자 또는 게시에서 시작되면 대체 계층으로 변경할 수 없습니다. 이렇게 하려면 첫 번째 실행 동안 생성된 `crx-Quickstart` 폴더를 삭제해야 하며 빠른 시작 Jar를 다시 실행해야 합니다. 환경 및 포트는 변경할 수 있지만 로컬 AEM 인스턴스의 중지/시작이 필요합니다.

환경 `dev`과 `stage` `prod`를 변경하는 것은 개발자가 환경별 구성을 AEM에서 올바르게 정의하고 해결하도록 하는 데 유용할 수 있습니다. 로컬 개발은 기본 `dev` 환경 실행 모드에 대해 수행하는 것이 좋습니다.

사용 가능한 순열은 다음과 같습니다.

+ `aem-author-p4502.jar`
   + 포트 4502에서 개발 실행 모드에서 작성자로
+ `aem-author_dev-p4502.jar`
   + 포트 4502에서 개발 실행 모드에서 작성자로(과 동일 `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + 포트 4502의 스테이징 실행 모드에서 작성자로
+ `aem-author_prod-p4502.jar`
   + 포트 4502의 프로덕션 실행 모드에서 작성자로
+ `aem-publish-p4503.jar`
   + 포트 4503에서 개발 실행 모드에서 작성자로
+ `aem-publish_dev-p4503.jar`
   + 포트 4503에서 개발 실행 모드에서 작성자로(과 동일 `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + 포트 4503의 스테이징 실행 모드에서 작성자로
+ `aem-publish_prod-p4503.jar`
   + 포트 4503의 프로덕션 실행 모드에서 작성자로

포트 번호는 로컬 개발 시스템에서 사용 가능한 포트일 수 있지만, 다음 규칙으로 가능합니다.

+ 포트 __4502__ 는 __로컬 AEM 작성자 서비스에 사용됩니다.__
+ 포트 __4503__ 은 __로컬 AEM 게시 서비스에 사용됩니다.__

이러한 사항을 변경하려면 AEM SDK 구성을 조정해야 할 수 있습니다.

## 로컬 AEM 런타임 중지

로컬 AEM 런타임을 중지하려면 AEM 작성자 또는 게시 서비스에서 AEM 런타임을 시작하는 데 사용한 명령줄 창을 열고 을 누릅니다 `Ctrl-C`. AEM이 종료될 때까지 기다립니다. 종료 프로세스가 완료되면 명령줄 프롬프트를 사용할 수 있습니다.

## 빠른 시작 JAR를 업데이트하는 경우

AEM SDK를 매달 또는 그 다음 주 목요일(AEM의 릴리스 cadence)에 Cloud Service &quot;기능 릴리스&quot;로 업데이트할 수 있습니다.

>[!WARNING]
>
> Quickstart Jar를 새 버전으로 업데이트하려면 전체 로컬 개발 환경을 교체하여 로컬 AEM 저장소의 모든 코드, 구성 및 컨텐츠를 손실해야 합니다. 훼손되지 않아야 하는 모든 코드, 구성 또는 컨텐츠는 Git에 안전하게 커밋되거나 로컬 AEM 인스턴스에서 AEM 패키지로 내보내집니다.

### AEM SDK를 업그레이드할 때 컨텐츠 손실을 방지하는 방법

AEM SDK를 업그레이드하면 새 저장소를 비롯한 새로운 AEM 런타임을 효과적으로 제작할 수 있습니다. 즉, 이전 AEM SDK의 저장소에 대한 변경 사항이 손실됩니다. 다음은 AEM SDK 업그레이드 간에 컨텐츠를 지속하는 데 도움이 되는 실용적인 전략이며, 재정확하거나 공동으로 사용할 수 있습니다.

1. 개발에 도움이 되는 &quot;샘플&quot; 컨텐츠를 포함하는 전용 컨텐츠 패키지를 만들어 Git에서 유지 관리할 수 있습니다. AEM SDK 업그레이드를 통해 유지되어야 하는 모든 컨텐츠는 이 패키지로 유지되고 AEM SDK를 업그레이드한 후 다시 배포됩니다.
1. 디렉티브와 함께 [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) `includepaths` 를 사용하여 이전 AEM SDK 저장소의 컨텐츠를 새 AEM SDK 저장소로 복사합니다.
1. 이전 AEM SDK에서 AEM Package Manager와 컨텐츠 패키지를 사용하여 모든 컨텐츠를 백업하고 새로운 AEM SDK에 다시 설치합니다.

AEM SDK 업그레이드 간에 코드를 유지 관리하기 위해 상기 접근 방식을 사용하는 것은 개발 앤티 패턴을 나타냅니다. 일회용 코드는 개발 IDE에서 비롯되며 배포를 통해 AEM SDK로 전달됩니다.

## 문제 해결

## 빠른 시작 jar 파일을 두 번 클릭하면 오류가 발생합니다.{#troubleshooting-double-click}

빠른 시작 jar를 두 번 클릭하여 시작하면 오류 모달이 표시되므로 AEM이 로컬에서 시작되지 않습니다.

![문제 해결 - 빠른 시작 Jar 파일을 두 번 클릭합니다.](./assets/aem-runtime/troubleshooting__double-click.png)

Cloud Service Quickstart Jar로서 AEM에서 AEM을 로컬로 시작하는 빠른 시작 jar의 두 번 클릭을 지원하지 않기 때문입니다. 대신 해당 명령줄에서 Jar 파일을 실행해야 합니다.

AEM 작성자 서비스를 시작하려면 빠른 시작 JAR가 포함된 디렉토리 `cd` 로 이동하여 명령을 실행합니다.

`$ java -jar aem-author-p4502.jar`

또는 AEM 게시 서비스를 시작하려면 빠른 시작 JAR가 포함된 디렉토리 `cd` 로 이동하여 명령을 실행합니다.

`$ java -jar aem-author-p4503.jar`

## 명령줄에서 빠른 시작 JAR를 시작하면 즉시 중단됩니다.{#troubleshooting-java-8}

명령줄에서 빠른 시작 JAR를 시작하면 프로세스가 즉시 중단되고 AEM 서비스가 시작되지 않으며 다음 오류가 발생합니다.

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

이는 AEM Cloud Service의 경우 Java SDK 11이 필요하며 다른 버전을 실행 중이기 때문입니다. Java 8이 될 가능성이 높습니다. 이 문제를 해결하려면 [Oracle Java SDK 11을 다운로드하여 설치하십시오](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AssoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atfoling&amp;fulltext=Oracle%7E+JDK%7E&amp;orderby%444 0jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Java SDK 11이 설치되면 명령줄에서 다음을 실행하여 활성 버전인지 확인합니다.

Java 11 SDK가 설치되면 명령줄에서 명령을 실행하여 활성 버전인지 확인합니다.

+ Windows: `java -version`
+ macOS/Linux: `java --version`

## 추가 리소스

+ [AEM SDK 다운로드](https://experience.adobe.com/#/downloads)
+ [Adobe 클라우드 관리자](https://my.cloudmanager.adobe.com/)
+ [Docker 다운로드](https://www.docker.com/)
+ [Experience Manager 발송자 설명서](https://docs.adobe.com/content/help/ko-KR/experience-manager-dispatcher/using/dispatcher.html)
