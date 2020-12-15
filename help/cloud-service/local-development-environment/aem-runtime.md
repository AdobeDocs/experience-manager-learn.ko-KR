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

AEM(Adobe Experience Manager)은 AEM을 Cloud Service SDK의 Quickstart Jar로 사용하여 로컬로 실행할 수 있습니다. 이를 통해 개발자는 사용자 정의 코드, 구성 및 컨텐츠를 소스 제어에 커밋하기 전에 배포 및 테스트하고 이를 Cloud Service 환경으로 AEM에 배포할 수 있습니다.

`~`은(는) 사용자 디렉토리의 축약형으로 사용됩니다. Windows에서는 `%HOMEPATH%`과 같습니다.

## Java 설치

Experience Manager은 Java 응용 프로그램이므로 Java SDK가 개발 도구를 지원해야 합니다.

1. [최신 Java SDK 11 다운로드 및 설치](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%4040jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 다음 명령을 실행하여 Java 11 SDK가 설치되었는지 확인합니다.
   + Windows:`java -version`
   + macOS / Linux:`java --version`

![Java](./assets/aem-runtime/java.png)

## AEM을 Cloud Service SDK로 다운로드

Cloud Service SDK 또는 AEM SDK로 AEM에는 AEM 작성자 및 개발을 위해 로컬에서 게시를 실행하는 데 사용되는 빠른 시작 JAR와 호환되는 Dispatcher 도구 버전이 포함되어 있습니다.

1. Adobe ID으로 [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads)에 로그인합니다.
   + AEM을 Cloud Service SDK로 다운로드하려면 Adobe 조직 __이(가) Cloud Service으로__&#x200B;되어야 합니다.
1. Cloud Service __탭으로__ AEM으로 이동합니다.
1. __내림차순__ 순서로 __게시된 날짜__&#x200B;별로 정렬
1. 최신 __AEM SDK__ 결과 행을 클릭합니다.
1. EULA를 검토하고 승인한 후 __다운로드__ 단추를 누릅니다.

## AEM SDK zip에서 빠른 시작 Jar 추출

1. 다운로드한 `aem-sdk-XXX.zip` 파일의 압축을 해제합니다.

## 로컬 AEM 작성자 서비스{#set-up-local-aem-author-service} 설정

로컬 AEM 작성자 서비스는 컨텐츠 작성자가 컨텐츠를 만들고 관리하기 위해 공유할 로컬 경험 디지털 마케터/컨텐츠 작성자를 개발자에게 제공합니다.  AEM 작성자 서비스는 작성 및 미리 보기 환경으로 설계되었으므로, 기능 개발에 대한 대부분의 유효성 검사가 수행될 수 있으므로 로컬 개발 프로세스의 중요한 요소가 됩니다.

1. `~/aem-sdk/author` 폴더 만들기
1. __Quickstart JAR__ 파일을 `~/aem-sdk/author`에 복사하고 이름을 `aem-author-p4502.jar`(으)로 변경합니다.
1. 명령줄에서 다음을 실행하여 로컬 AEM 작성자 서비스를 시작합니다.
   + `java -jar aem-author-p4502.jar`
      + 관리자 암호를 `admin`으로 입력합니다. 모든 관리자 암호는 사용할 수 있지만 다시 구성할 필요가 없도록 로컬 개발에 기본값을 사용하는 것이 좋습니다.

   *AEM을 Cloud Service Quickstart Jar [로 두 번 클릭하여 시작할 수 없습니다.](#troubleshooting-double-click).*
1. 웹 브라우저에서 [http://localhost:4502](http://localhost:4502)의 로컬 AEM 작성자 서비스에 액세스합니다.

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## 로컬 AEM 게시 서비스 설정

로컬 AEM 게시 서비스는 AEM에서 호스팅하는 웹 사이트를 검색하는 것과 같이 AEM의 최종 사용자가 가질 로컬 경험을 개발자에게 제공합니다. 로컬 AEM 게시 서비스는 AEM SDK의 [Dispatcher 도구](./dispatcher-tools.md)와 통합되어 있으므로 개발자가 최종 사용자 대면 경험을 연기하고 세밀하게 조정할 수 있습니다.

1. `~/aem-sdk/publish` 폴더 만들기
1. __Quickstart JAR__ 파일을 `~/aem-sdk/publish`에 복사하고 이름을 `aem-publish-p4503.jar`(으)로 변경합니다.
1. 명령줄에서 다음을 실행하여 로컬 AEM 게시 서비스를 시작합니다.
   + `java -jar aem-publish-p4503.jar`
      + 관리자 암호를 `admin`으로 입력합니다. 모든 관리자 암호는 사용할 수 있지만 다시 구성할 필요가 없도록 로컬 개발에 기본값을 사용하는 것이 좋습니다.

   *AEM을 Cloud Service Quickstart Jar [로 두 번 클릭하여 시작할 수 없습니다.](#troubleshooting-double-click).*
1. 웹 브라우저에서 [http://localhost:4503](http://localhost:4503)의 로컬 AEM 게시 서비스에 액세스합니다.

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## 빠른 시작 Jar 시작 모드

빠른 시작 JAR 이름 `aem-<tier>_<environment>-p<port number>.jar`은 시작할 방법을 지정합니다. 특정 계층, 작성자 또는 게시에서 AEM을 시작한 후에는 대체 계층으로 변경할 수 없습니다. 이렇게 하려면 첫 번째 실행 중에 생성된 `crx-Quickstart` 폴더를 삭제해야 하며 빠른 시작 Jar를 다시 실행해야 합니다. 환경 및 포트는 변경할 수 있지만 로컬 AEM 인스턴스의 중지/시작이 필요합니다.

환경 변경, `dev`, `stage` 및 `prod` 등은 개발자가 환경별 구성을 AEM에서 올바르게 정의하고 해결할 수 있도록 하는 데 유용합니다. 로컬 개발은 기본적으로 기본 `dev` 환경 실행 모드에 대해 수행하는 것이 좋습니다.

사용 가능한 순열은 다음과 같습니다.

+ `aem-author-p4502.jar`
   + 포트 4502의 개발 실행 모드에서 작성자로
+ `aem-author_dev-p4502.jar`
   + 포트 4502의 개발 실행 모드에서 작성자로(`aem-author-p4502.jar`과 동일)
+ `aem-author_stage-p4502.jar`
   + 포트 4502의 스테이징 실행 모드에서 작성자로
+ `aem-author_prod-p4502.jar`
   + 포트 4502의 프로덕션 실행 모드에서 작성자로
+ `aem-publish-p4503.jar`
   + 포트 4503의 개발 실행 모드에서 작성자로
+ `aem-publish_dev-p4503.jar`
   + 포트 4503의 개발 실행 모드에서 작성자로(`aem-publish-p4503.jar`과 동일)
+ `aem-publish_stage-p4503.jar`
   + 포트 4503의 스테이징 실행 모드에서 작성자로
+ `aem-publish_prod-p4503.jar`
   + 포트 4503의 프로덕션 실행 모드에서 작성자로

포트 번호는 로컬 개발 시스템에서 사용할 수 있는 포트이지만 다음 규칙에 따라 사용할 수 있습니다.

+ 포트 __4502__&#x200B;은(는) __로컬 AEM 작성자 서비스__&#x200B;에 사용됩니다.
+ __4503__ 포트는 __로컬 AEM 게시 서비스__&#x200B;에 사용됩니다.

이러한 사항을 변경하려면 AEM SDK 구성을 조정해야 할 수 있습니다.

## 로컬 AEM 런타임 중지

로컬 AEM 런타임을 중지하려면 AEM 작성자 또는 게시 서비스에서 AEM 런타임을 시작하는 데 사용된 명령줄 창을 열고 `Ctrl-C`를 누릅니다. AEM이 종료될 때까지 기다립니다. 종료 프로세스가 완료되면 명령줄 프롬프트를 사용할 수 있습니다.

## 빠른 시작 JAR를 업데이트하는 경우

AEM SDK를 매월 업데이트(매달 마지막 목요일 이후)하여 업데이트할 수 있습니다. 이는 AEM의 릴리스 카드로서 Cloud Service &quot;기능 릴리스&quot;입니다.

>[!WARNING]
>
> Quickstart Jar를 새 버전으로 업데이트하려면 전체 로컬 개발 환경을 교체하여 로컬 AEM 저장소의 모든 코드, 구성 및 컨텐츠를 손실해야 합니다. 파기하지 않아야 하는 모든 코드, 구성 또는 컨텐츠가 Git에 안전하게 커밋되거나 로컬 AEM 인스턴스에서 AEM 패키지로 내보내졌는지 확인합니다.

### AEM SDK를 업그레이드할 때 컨텐츠 손실을 방지하는 방법

AEM SDK를 업그레이드하면 새 저장소를 비롯한 새로운 AEM 런타임을 효과적으로 생성할 수 있습니다. 즉, 이전 AEM SDK의 저장소에 대한 변경 사항이 손실됩니다. 다음은 AEM SDK 업그레이드 간에 컨텐츠를 지속하기 위한 실용적인 전략을 말하며, 재정확하거나 함께 사용할 수 있습니다.

1. 개발에 도움이 되고 Git에서 유지 관리할 수 있도록 &quot;샘플&quot; 컨텐츠를 포함하는 전용 컨텐츠 패키지를 만듭니다. AEM SDK 업그레이드를 통해 지속되어야 하는 모든 컨텐츠는 이 패키지로 유지되며 AEM SDK를 업그레이드한 후 다시 배포됩니다.
1. `includepaths` 지시문과 함께 [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html)을 사용하여 이전 AEM SDK 저장소의 컨텐츠를 새 AEM SDK 저장소로 복사합니다.
1. 이전 AEM SDK에서 AEM Package Manager와 컨텐츠 패키지를 사용하여 모든 컨텐츠를 백업하고 새로운 AEM SDK에 다시 설치합니다.

AEM SDK 업그레이드 간에 코드를 유지 관리하기 위해 위의 접근 방식을 사용하는 것은 개발 앤티 패턴을 나타냅니다. 일회용 코드는 개발 IDE에서 시작되어야 하며 배포를 통해 AEM SDK로 전환해야 합니다.

## 문제 해결

## 빠른 시작 Jar 파일을 두 번 클릭하면 오류{#troubleshooting-double-click} 발생

빠른 시작 jar를 두 번 클릭하여 시작하면 AEM이 로컬에서 시작하지 않도록 오류 양식이 표시됩니다.

![문제 해결 - 빠른 시작 Jar 파일을 두 번 클릭합니다.](./assets/aem-runtime/troubleshooting__double-click.png)

Cloud Service Quickstart Jar로서 AEM에서 AEM을 로컬로 시작할 때 빠른 시작 JAR을 두 번 클릭하는 것을 지원하지 않기 때문입니다. 대신 해당 명령줄에서 Jar 파일을 실행해야 합니다.

AEM 작성자 서비스를 시작하려면 `cd`을(를) 빠른 시작 Jar가 포함된 디렉토리로 입력하고 명령을 실행합니다.

`$ java -jar aem-author-p4502.jar`

또는 AEM 게시 서비스를 시작하려면 빠른 시작 JAR이 포함된 디렉토리로 `cd`의 명령을 실행합니다.

`$ java -jar aem-author-p4503.jar`

## 명령줄에서 빠른 시작 JAR를 시작하면 {#troubleshooting-java-8}이 즉시 중단됩니다.

명령줄에서 빠른 시작 JAR를 시작할 때 프로세스가 즉시 중단되고 AEM 서비스가 시작되지 않고 다음 오류가 발생합니다.

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

Cloud Service의 AEM에는 Java SDK 11이 필요하며 다른 버전을 실행 중이기 때문입니다(대부분의 경우 Java 8). 이 문제를 해결하려면 [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%4040jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)을(를) 다운로드하고 설치하십시오.
Java SDK 11이 설치되면 명령줄에서 다음을 실행하여 활성 버전인지 확인합니다.

Java 11 SDK가 설치되면 명령줄에서 명령을 실행하여 활성 버전인지 확인합니다.

+ Windows: `java -version`
+ macOS / Linux:`java --version`

## 추가 리소스

+ [AEM SDK 다운로드](https://experience.adobe.com/#/downloads)
+ [Adobe 클라우드 관리자](https://my.cloudmanager.adobe.com/)
+ [문서 다운로드](https://www.docker.com/)
+ [Experience Manager 발송자 설명서](https://docs.adobe.com/content/help/ko-KR/experience-manager-dispatcher/using/dispatcher.html)
