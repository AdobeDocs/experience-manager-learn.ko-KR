---
title: AEM Forms로 첫 번째 OSGi 번들 만들기
description: maven 및 eclipse를 사용하여 첫 번째 OSGi 번들 구축
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '820'
ht-degree: 2%

---


# 첫 번째 OSGi 번들 만들기

OSGi 번들은 Java 코드, 리소스 및 번들과 해당 종속성을 설명하는 매니페스트를 포함하는 Java™ 아카이브 파일입니다. 번들은 응용 프로그램의 배포 단위입니다. 이 문서는 AEM Forms 6.4 또는 6.5를 사용하여 OSGi 서비스 또는 서블릿을 만들려는 개발자를 위한 것입니다. 첫 번째 OSGi 번들을 빌드하려면 다음 단계를 수행하십시오.


## JDK 설치

지원되는 버전의 JDK를 설치합니다. JDK1.8을 사용했습니다. **JAVA_HOME** 환경 변수에서 및 가 JDK 설치의 루트 폴더를 가리킵니다.
경로에 %JAVA_HOME%/bin 추가

![데이터 소스](assets/java-home.JPG)

>[!NOTE]
> JDK 15를 사용하지 마십시오. AEM에서는 지원되지 않습니다.

### JDK 버전 테스트

새 명령 프롬프트 창을 열고 다음을 입력합니다. `java -version`. 에서 식별한 JDK 버전을 다시 가져와야 합니다. `JAVA_HOME` 변수

![데이터 소스](assets/java-version.JPG)

## Maven 설치

Maven은 주로 Java 프로젝트에 사용되는 빌드 자동화 도구입니다. 다음 단계에 따라 로컬 시스템에 maven을 설치하십시오.

* 라는 폴더를 만듭니다. `maven` C 드라이브에서
* 다운로드 [이진 zip 아카이브](http://maven.apache.org/download.cgi)
* zip 아카이브의 컨텐츠를 다음 위치에 추출합니다. `c:\maven`
* 라는 환경 변수를 만듭니다. `M2_HOME` 값 `C:\maven\apache-maven-3.6.0`. 내 경우에는 **mvn** 버전은 3.6.0입니다. 이 문서를 작성하는 동안 최신 maven 버전은 3.6.3입니다.
* 추가 `%M2_HOME%\bin` 경로 지정
* 변경 내용을 저장합니다
* 새 명령 프롬프트를 열고 을 입력합니다. `mvn -version`. 다음을 볼 수 있습니다. **mvn** 아래 스크린샷에 표시된 대로 버전이 나열됩니다.

![데이터 소스](assets/mvn-version.JPG)

## Settings.xml

A Maven `settings.xml` 파일은 다양한 방법으로 Maven 실행을 구성하는 값을 정의합니다. 일반적으로 로컬 저장소 위치, 대체 원격 저장소 서버 및 개인 리포지토리의 인증 정보를 정의하는 데 사용됩니다.

다음으로 이동 `C:\Users\<username>\.m2 folder`
의 내용을 추출합니다. [settings.zip](assets/settings.zip) 파일을 만든 후 `.m2` 폴더를 입력합니다.

## Eclipse 설치

최신 버전 설치 [일식](https://www.eclipse.org/downloads/)

## 첫 번째 프로젝트 만들기

Archetype은 Maven 프로젝트 템플릿 툴킷입니다. 원형은 같은 종류의 다른 모든 것들이 만들어지는 원래의 패턴 또는 모델로 정의됩니다. 이 이름은 Maven 프로젝트를 일관되게 생성하는 방법을 제공하는 시스템을 제공하려고 할 때 적절합니다. Archetype은 작성자가 사용자를 위한 Maven 프로젝트 템플릿을 만드는 데 도움이 되며, 사용자에게 이러한 프로젝트 템플릿의 매개 변수가 있는 버전을 생성하는 방법을 제공합니다.
첫 번째 maven 프로젝트를 만들려면 다음 단계를 수행하십시오.

* 라는 새 폴더를 만듭니다. `aemformsbundles` C 드라이브에서
* 명령 프롬프트를 열고 다음 위치로 이동합니다. `c:\aemformsbundles`
* 명령 프롬프트에서 다음 명령을 실행합니다.
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Maven 프로젝트가 대화식으로 생성되며 다음과 같은 여러 속성에 값을 제공하라는 메시지가 표시됩니다

| 속성 이름 | 유의 | 값 |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId는 모든 프로젝트에서 프로젝트를 고유하게 식별합니다 | com.learningaemforms.adobe |
| appsFolderName | 프로젝트 구조를 포함하는 폴더의 이름입니다 | 학습 양식 |
| artifactId | artifactId는 버전이 없는 jar의 이름입니다. 생성한 경우 소문자 및 이상한 기호로 원하는 이름을 선택할 수 있습니다. | 학습 양식 |
| 버전 | 배포하면 숫자와 점(1.0, 1.1, 1.0.1, ..)이 있는 일반적인 버전을 선택할 수 있습니다. | 1.0 |

Enter 키를 눌러 다른 속성에 대한 기본값을 사용합니다.
모든 것이 제대로 작동하면 명령 창에 빌드 성공 메시지가 표시됩니다

## 전문 프로젝트에서 eclipse 프로젝트 만들기

작업 디렉토리를 다음으로 변경 `learningaemforms`.
실행 중 `mvn eclipse:eclipse` 명령줄에서 위의 명령은 pom 파일을 읽고 올바른 메타데이터로 Eclipse 프로젝트를 만들어 Eclipse가 프로젝트 유형, 관계, 클래스 경로 등을 이해하도록 합니다.

## 프로젝트를 eclipse로 가져오기

Launch **Eclipse**

이동 **파일 -> 가져오기** 을(를) 선택합니다. **기존 Maven 프로젝트** 여기에 표시된 대로

![데이터 소스](assets/import-mvn-project.JPG)

다음을 클릭합니다

을(를) 선택합니다 `c:\aemformsbundles\learningaemform`s( **찾아보기** 버튼

![데이터 소스](assets/select-mvn-project.JPG)

>[!NOTE]
>필요에 따라 적절한 모듈을 가져오도록 선택할 수 있습니다. 프로젝트에서 Java 코드만 만들려는 경우에만 코어 모듈을 선택하고 가져옵니다.

클릭 **완료** 가져오기 프로세스를 시작하려면

프로젝트는 Eclipse로 가져오며, 많은 항목이 표시됩니다 `learningaemforms.xxxx` 폴더

를 확장합니다. `src/main/java` 아래에 `learningaemforms.core` 폴더를 입력합니다. 대부분의 코드를 쓰는 폴더입니다.

![데이터 소스](assets/learning-core.JPG)

## 프로젝트 빌드

OSGi 서비스 또는 서블릿을 작성했으면 Felix 웹 콘솔을 사용하여 배포할 수 있는 OSGi 번들을 생성하기 위한 프로젝트를 빌드해야 합니다. 자세한 내용은 [AEMFD 클라이언트 SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) maven 프로젝트에 적절한 클라이언트 SDK를 포함하도록 업데이트되고 수정되었습니다. AEM FD 클라이언트 SDK를 의 종속성 섹션에 포함해야 합니다 `pom.xml` 아래와 같이 핵심 프로젝트의 목록을 표시합니다.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

프로젝트를 빌드하려면 다음 단계를 수행하십시오.

* 열기 **명령 프롬프트 창**
* 다음으로 이동 `c:\aemformsbundles\learningaemforms\core`
* 명령 실행 `mvn clean install`
모든 것이 제대로 작동하면 다음 위치에 번들이 표시됩니다 `C:\AEMFormsBundles\learningaemforms\core\target`. 이제 Felix 웹 콘솔을 사용하여 AEM에 이 번들을 배포할 준비가 되었습니다.
