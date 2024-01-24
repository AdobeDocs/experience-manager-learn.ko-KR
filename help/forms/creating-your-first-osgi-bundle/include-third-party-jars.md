---
title: 서드파티 jar 포함
description: AEM 프로젝트에서 타사 jar 파일을 사용하는 방법을 알아봅니다.
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 61
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# AEM 프로젝트에 서드파티 번들 포함

이 문서에서는 AEM 프로젝트에 타사 OSGi 번들을 포함하는 것과 관련된 단계를 설명합니다.이 문서에서는 다음을 포함하게 됩니다. [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) AEM 프로젝트에서.  Maven 저장소에서 OSGi를 사용할 수 있는 경우 프로젝트의 POM.xml 파일에 번들의 종속성이 포함됩니다.

>[!NOTE]
> 타사 jar를 OSGi 번들로 가정합니다

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

OSGi 번들이 파일 시스템에 있는 경우 **localjar** 프로젝트의 기본 디렉터리(C:\aemformsbundles\AEMFormsProcessStep\localjar) 아래에서 종속성은 다음과 같이 표시됩니다

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## 폴더 구조 만들기

이 번들을 AEM 프로젝트에 추가하고 있습니다. **AEMFormsProcessStep** 다음 위치에 있는 경우: **c:\aemformsbundles** 폴더

* 를 엽니다. **filter.xml** 프로젝트의 C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault 폴더에서 필터 요소의 루트 속성을 기록합니다.

* 다음 폴더 구조를 만듭니다 C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* 다음 **apps/AEMFormsProcessStep-vendor-packages** 은 filter.xml의 루트 속성 값입니다.
* 프로젝트 POM.xml의 종속성 섹션을 업데이트합니다.
* 명령 프롬프트를 엽니다. 프로젝트의 폴더(c:\aemformsbundles\AEMFormsProcessStep)로 이동합니다. 다음 명령 실행

```java
mvn clean install -PautoInstallSinglePackage
```

모든 것이 정상적으로 작동하면 패키지가 타사 번들과 함께 AEM 인스턴스에 설치됩니다. 다음을 사용하여 번들을 확인할 수 있습니다. [felix 웹 콘솔](http://localhost:4502/system/console/bundles). 타사 번들은 의 /apps 폴더에서 사용할 수 있습니다. `crx` 아래와 같은 저장소
![서드파티](assets/custom-bundle1.png)
