---
title: 빌드 및 배포
description: Adobe Cloud Manager을 사용하면 코드를 쉽게 빌드하고 AEM as a Cloud Service에 배포할 수 있습니다. 빌드 프로세스의 단계 중에 오류가 발생하여 이를 해결하기 위한 작업이 필요할 수 있습니다. 이 안내서에서는 배포의 일반적인 오류를 이해하고 이러한 오류에 가장 잘 접근하는 방법을 안내합니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
duration: 534
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2476'
ht-degree: 0%

---

# AEM as a Cloud Service 빌드 및 배포 디버깅

Adobe Cloud Manager을 사용하면 코드를 쉽게 빌드하고 AEM as a Cloud Service에 배포할 수 있습니다. 빌드 프로세스의 단계 중에 오류가 발생하여 이를 해결하기 위한 작업이 필요할 수 있습니다. 이 안내서에서는 배포의 일반적인 오류를 이해하고 이러한 오류에 가장 잘 접근하는 방법을 안내합니다.

![클라우드 빌드 파이프라인 관리](./assets/build-and-deployment/build-pipeline.png)

## 유효성 검사

유효성 검사 단계에서는 기본 Cloud Manager 구성이 유효한지 확인하기만 하면 됩니다. 일반적인 유효성 검사 실패는 다음과 같습니다.

### 환경이 잘못된 상태입니다.

+ __오류 메시지:__ 환경이 잘못된 상태입니다.
  ![환경이 잘못된 상태입니다.](./assets/build-and-deployment/validation__invalid-state.png)
+ __원인:__ 파이프라인의 대상 환경이 전환 상태이며, 이 상태에서는 새 빌드를 수락할 수 없습니다.
+ __해결 방법:__ 상태가 실행 중(또는 업데이트 사용 가능) 상태로 확인될 때까지 기다립니다. 환경을 삭제하는 경우 환경을 다시 만들거나 빌드할 다른 환경을 선택하십시오.

### 파이프라인과 연계된 환경을 찾을 수 없습니다.

+ __오류 메시지:__ 환경이 삭제된 것으로 표시됩니다.
  ![환경이 삭제된 것으로 표시됨](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __원인:__ 파이프라인이 사용하도록 구성된 환경이 삭제되었습니다.
동일한 이름의 새 환경이 다시 만들어지더라도 Cloud Manager은 파이프라인을 동일한 이름의 환경에 자동으로 다시 연결하지 않습니다.
+ __해결 방법:__ 파이프라인 구성을 편집하고 배포할 환경을 다시 선택하십시오.

### 파이프라인과 연결된 Git 분기를 찾을 수 없습니다

+ __오류 메시지:__ 잘못된 파이프라인: XXXXXX. Reason=Branch=xxxx를 저장소에서 찾을 수 없습니다.
  ![잘못된 파이프라인: XXXXXX. Reason=Branch=xxxx를 저장소에서 찾을 수 없음](./assets/build-and-deployment/validation__branch-not-found.png)
+ __원인:__ 파이프라인이 사용하도록 구성된 Git 분기가 삭제되었습니다.
+ __해결 방법:__ 정확한 동일한 이름을 사용하여 누락된 Git 분기를 다시 만들거나 다른 기존 분기에서 빌드하도록 파이프라인을 다시 구성하십시오.

## 빌드 및 단위 테스트

![빌드 및 단위 테스트](./assets/build-and-deployment/build-and-unit-testing.png)

빌드 및 단위 테스트 단계는 파이프라인의 구성된 Git 분기에서 체크아웃된 프로젝트의 Maven 빌드(`mvn clean package`)를 수행합니다.

이 단계에서 식별된 오류는 다음 예외를 제외하고 로컬에서 프로젝트를 빌드하여 다시 생산할 수 있어야 합니다.

+ [Maven Central](https://search.maven.org/)에서 사용할 수 없는 Maven 종속성이 사용되었으며 종속성을 포함하는 Maven 저장소는 다음 중 하나입니다.
   + 비공개 내부 Maven 저장소 또는 Maven 저장소와 같이 Cloud Manager에서 연결할 수 없는 경우 인증이 필요하며 잘못된 자격 증명이 제공되었습니다.
   + 프로젝트의 `pom.xml`에 명시적으로 등록되지 않았습니다. Maven 리포지토리를 포함하는 것은 작성 시간을 늘리기 때문에 권장되지 않습니다.
+ 타이밍 문제로 인해 단위 테스트에 실패했습니다. 단위 테스트가 시간에 민감한 경우 이러한 문제가 발생할 수 있습니다. 강력한 표시기가 테스트 코드의 `.sleep(..)`에 의존하고 있습니다.
+ 지원되지 않는 Maven 플러그인 사용

## 코드 검색

![코드 검색](./assets/build-and-deployment/code-scanning.png)

코드 스캔은 Java 및 AEM 관련 모범 사례를 혼합하여 정적 코드 분석을 수행합니다.

코드에 심각한 보안 취약점이 존재하는 경우 코드 스캔으로 인해 빌드 오류가 발생합니다. 더 적은 위반은 재정의할 수 있지만 수정되는 것이 좋습니다. 코드 검사가 불완전하여 [긍정 오류](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html?lang=ko#dealing-with-false-positives)가 발생할 수 있습니다.

코드 스캔 문제를 해결하려면 **세부 정보 다운로드** 단추를 통해 Cloud Manager에서 제공한 CSV 형식의 보고서를 다운로드하고 항목을 검토하십시오.

자세한 내용은 AEM 관련 규칙을 참조하십시오. Cloud Manager 설명서의 [사용자 지정 AEM 관련 코드 스캔 규칙](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html?lang=ko)을 참조하십시오.

## 이미지 빌드

![이미지 빌드](./assets/build-and-deployment/build-images.png)

빌드 이미지는 빌드 및 단위 테스트 단계에서 생성된 빌드된 코드 아티팩트를 AEM 릴리스와 결합하여 하나의 배포 가능한 아티팩트를 형성합니다.

빌드 및 단위 테스트 중에 코드 빌드 및 컴파일 문제가 발견되지만 사용자 지정 빌드 아티팩트를 AEM 릴리스와 결합하려고 할 때 구성 또는 구조적 문제가 확인될 수 있습니다.

### 중복 OSGi 구성

여러 OSGi 구성이 대상 AEM 환경에 대한 실행 모드를 통해 확인되는 경우 이미지 작성 단계가 실패하고 다음과 같은 오류가 발생합니다.

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### 원인 1

+ __원인:__ AEM 프로젝트의 모든 패키지에 여러 코드 패키지가 포함되어 있으며, 두 개 이상의 코드 패키지에서 동일한 OSGi 구성을 제공하므로 충돌이 발생하여 이미지 빌드 단계에서 사용할 항목을 결정할 수 없으므로 빌드가 실패합니다. 고유한 이름이 있는 한 OSGi 팩토리 구성에는 적용되지 않습니다.
+ __해결 방법:__ AEM 응용 프로그램의 일부로 배포되는 모든 코드 패키지(포함된 타사 코드 패키지 포함)를 검토하여 실행 모드를 통해 대상 환경으로 확인되는 중복 OSGi 구성을 찾습니다. AEM as a Cloud Service에서는 &quot;mergeConfigurations 플래그를 true로 설정&quot;이라는 오류 메시지의 지침을 사용할 수 없으므로 이 지침을 무시해야 합니다.

#### 원인 2

+ __원인:__ AEM 프로젝트에 동일한 코드 패키지가 두 번 잘못 포함되어 해당 패키지에 포함된 모든 OSGi 구성이 중복됩니다.
+ __해결 방법:__ 모든 프로젝트에 포함된 패키지의 모든 pom.xml을 검토하고 `filevault-package-maven-plugin` [구성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=ko#cloud-manager-target)이 `<cloudManagerTarget>none</cloudManagerTarget>`(으)로 설정되어 있는지 확인하십시오.

### 잘못된 repoinit 스크립트

Repoinit 스크립트는 기준 컨텐츠, 사용자, ACL 등을 정의합니다. AEM as a Cloud Service에서 repoinit 스크립트는 빌드 이미지 중에 적용되지만 AEM SDK의 로컬 빠른 시작에서는 OSGi repoinit 팩토리 구성이 활성화될 때 적용됩니다. 이러한 이유로 Repoinit 스크립트는 AEM SDK의 로컬 빠른 시작에서(로깅과 함께) 조용히 실패하고 이미지 작성 단계가 실패하여 배포가 중지될 수 있습니다.

+ __원인:__ Repoinit 스크립트의 형식이 잘못되었습니다. 이로 인해 실패한 스크립트가 저장소에 대해 실행되지 않은 후 모든 Repoinit 스크립트로 저장소가 불완전한 상태로 남아 있을 수 있습니다.
+ __해결 방법:__ repoinit 스크립트 OSGi 구성이 배포될 때 AEM SDK의 로컬 빠른 시작을 검토하여 오류의 여부와 원인을 파악합니다.

### 충족되지 않은 리포인트 콘텐츠 종속성

Repoinit 스크립트는 기준 컨텐츠, 사용자, ACL 등을 정의합니다. AEM SDK의 로컬 빠른 시작에서 repoinit 스크립트는 repoinit OSGi 팩토리 구성이 활성화될 때 또는 다시 말해 저장소가 활성화된 후 콘텐츠 변경이 직접 또는 콘텐츠 패키지를 통해 발생했을 수 있습니다. AEM as a Cloud Service에서 repoinit 스크립트는 repoinit 스크립트가 의존하는 콘텐츠를 포함하지 않을 수 있는 저장소에 대해 이미지 작성 중에 적용됩니다.

+ __원인:__ Repoinit 스크립트는 존재하지 않는 콘텐츠에 따라 다릅니다.
+ __해결 방법:__ repoinit 스크립트가 사용하는 콘텐츠가 있는지 확인합니다. 종종 이는 콘텐츠 구조가 누락되었지만 필요한 디렉티브가 누락된 부적절하게 정의된 Repoinit 스크립트를 나타냅니다. AEM을 삭제하고 Jar의 압축을 푼 다음 Repoinit 스크립트가 포함된 Repoinit OSGi 구성을 설치 폴더에 추가하고 AEM을 시작하여 로컬에서 재현할 수 있습니다. 이 오류는 AEM SDK 로컬 빠른 시작의 error.log에 표시됩니다.


### 응용 프로그램의 핵심 구성 요소 버전이 배포된 버전보다 큽니다.

_이 문제는 최신 AEM 릴리스로 자동 업데이트되지 않는 비프로덕션 환경에만 영향을 줍니다._

AEM as a Cloud Service은 모든 AEM 릴리스에 최신 핵심 구성 요소 버전을 자동으로 포함합니다. 즉, AEM as a Cloud Service 환경을 자동 또는 수동으로 업데이트하고 최신 버전의 핵심 구성 요소를 배포한 후 이를 의미합니다.

다음과 같은 경우 이미지 작성 단계가 실패할 수 있습니다.

+ 배포 응용 프로그램이 `core`(OSGi 번들) 프로젝트에서 핵심 구성 요소 Maven 종속성 버전을 업데이트합니다.
+ 그런 다음 배포 애플리케이션이 새로운 핵심 구성 요소 버전이 포함된 AEM 릴리스를 사용하도록 업데이트되지 않은 샌드박스(비프로덕션) AEM as a Cloud Service 환경에 배포됩니다.

이 실패를 방지하려면 AEM as a Cloud Service 환경 업데이트를 사용할 수 있을 때마다 다음 빌드/배포의 일부로 업데이트를 포함하고 애플리케이션 코드 베이스에서 핵심 구성 요소 버전을 증가시킨 후 항상 업데이트가 포함되도록 해야 합니다.

+ __증상:__
`core` 프로젝트에서 특정 버전 범위의 `com.adobe.cq.wcm.core.components...` 패키지를 가져올 수 없다는 오류 보고와 함께 이미지 빌드 단계가 실패합니다.

  ```
  [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
  [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD FAILURE
  [INFO] ------------------------------------------------------------------------
  ```

+ __원인:__ 응용 프로그램의 OSGi 번들(`core` 프로젝트에 정의됨)은 AEM as a Cloud Service에 배포된 버전과 다른 버전 수준에서 핵심 구성 요소 핵심 종속성으로 Java 클래스를 가져옵니다.
+ __해상도:__
   + Git을 사용하여 핵심 구성 요소 버전 증가 전에 존재하는 작업 커밋으로 되돌립니다. 이 커밋을 Cloud Manager Git 분기에 푸시하고 이 분기에서 환경 업데이트를 수행합니다. 이렇게 하면 AEM as a Cloud Service이 최신 AEM 릴리스로 업그레이드되고 최신 핵심 구성 요소 버전이 포함됩니다. AEM as a Cloud Service이 최신 핵심 구성 요소 버전이 제공되는 최신 AEM 릴리스로 업데이트되면 원래 실패한 코드를 다시 배포합니다.
   + 이 문제를 로컬에서 재현하려면 AEM SDK 버전이 AEM as a Cloud Service 환경에서 사용 중인 AEM 릴리스 버전과 동일한지 확인하십시오.


### Adobe 지원 사례 만들기

위의 문제 해결 방법으로 문제가 해결되지 않으면 다음을 통해 Adobe 지원 사례를 만드십시오.

+ [Adobe Admin Console](https://adminconsole.adobe.com) > 지원 탭 > 사례 만들기

  _여러 Adobe 조직의 구성원인 경우 사례를 만들기 전에 실패한 파이프라인이 있는 Adobe 조직이 Adobe 조직 전환기에서 선택되었는지 확인하십시오._

## 배포 대상

배포처 단계는 빌드 이미지에서 생성된 코드 아티팩트를 가져오고, 이를 사용하여 새 AEM Author 및 Publish 서비스를 시작하고, 성공하면 이전 AEM Author 및 Publish 서비스를 제거하는 역할을 합니다. 이 단계에서도 변경 가능한 콘텐츠 패키지 및 색인이 설치되고 업데이트됩니다.

Deploy to 단계를 디버깅하기 전에 [AEM as a Cloud Service 로그](./logs.md)를 숙지하십시오. `aemerror` 로그에는 Pod의 시작 및 종료와 관련된 정보가 포함되어 있으며 이는 문제에 배포하는 것과 관련이 있을 수 있습니다. Cloud Manager의 배포 대상 단계에서 로그 다운로드 단추를 통해 사용할 수 있는 로그는 `aemerror` 로그가 아니며 응용 프로그램 시작과 관련된 자세한 정보를 포함하지 않습니다.

![배포 대상](./assets/build-and-deployment/deploy-to.png)

배포 대상 단계가 실패할 수 있는 세 가지 주요 이유는 다음과 같습니다.

### Cloud Manager 파이프라인에는 이전 AEM 버전이 있습니다

+ __원인:__ Cloud Manager 파이프라인에는 대상 환경에 배포된 버전보다 이전 버전의 AEM이 있습니다. 파이프라인을 다시 사용하여 최신 버전의 AEM을 실행하는 새 환경을 가리키면 이러한 문제가 발생할 수 있습니다. 환경의 AEM 버전이 파이프라인의 AEM 버전보다 큰지 확인하여 식별할 수 있습니다.
  ![Cloud Manager 파이프라인에 이전 AEM 버전이 있습니다](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __해상도:__
   + 대상 환경에 업데이트 사용 가능 이 있는 경우 환경의 작업에서 업데이트 를 선택한 다음 빌드를 다시 실행합니다.
   + 대상 환경에 업데이트 사용 가능 이 없는 경우 최신 버전의 AEM이 실행 중임을 의미합니다. 이 문제를 해결하려면 파이프라인을 삭제하고 다시 생성합니다.


### Cloud Manager 시간 초과

새로 배포된 AEM 서비스를 시작하는 동안 코드가 실행되므로 배포가 완료되기 전에 Cloud Manager 시간이 초과됩니다. 이러한 경우 Cloud Manager 상태가 실패로 보고되었더라도 배포가 결국 성공할 수 있습니다.

+ __원인:__ 사용자 지정 코드는 OSGi 번들 또는 구성 요소 수명 주기에서 초기에 트리거된 대규모 쿼리 또는 콘텐츠 트래버스 등의 작업을 실행하여 AEM 시작 시간을 상당히 지연시킬 수 있습니다.
+ __해결 방법:__ OSGi 번들의 라이프사이클에서 일찍 실행되는 코드에 대한 구현을 검토하고, Cloud Manager에서 표시하는 대로 오류가 발생한 시간(GMT에서 로그 시간)에 AEM 작성자 및 게시 서비스에 대한 `aemerror` 로그를 검토하고, 사용자 지정 로그 실행 프로세스를 나타내는 로그 메시지를 찾습니다.

### 호환되지 않는 코드 또는 구성

대부분의 코드 및 구성 위반은 빌드의 앞부분에서 발견되지만, 사용자 지정 코드 또는 구성이 AEM as a Cloud Service과 호환되지 않을 수 있으며 컨테이너에서 실행될 때까지 감지되지 않습니다.

+ __원인:__ 사용자 지정 코드는 OSGi 번들 또는 구성 요소 수명 주기에서 초기에 트리거된 대규모 쿼리 또는 콘텐츠 트래버스처럼 긴 작업을 호출하여 AEM의 시작 시간을 상당히 지연시킬 수 있습니다.
+ __해결 방법:__ Cloud Manager에서 표시된 대로 오류가 발생한 시간(GMT로 로그 시간)에 즈음하여 AEM 작성자 및 게시 서비스에 대한 `aemerror` 로그를 검토하십시오.
   1. 사용자 지정 애플리케이션에서 제공하는 Java 클래스에서 발생한 모든 오류에 대한 로그를 검토하십시오. 문제가 발견되면 해결한 후 고정 코드를 푸시하고 파이프라인을 다시 빌드합니다.
   1. 사용자 정의 애플리케이션에서 확장/상호 작용하고 있는 AEM의 측면에서 보고된 오류에 대한 로그를 검토하여 오류를 조사합니다. 이러한 오류는 Java 클래스에 직접 귀속되지 않을 수 있습니다. 문제가 발견되면 해결한 후 고정 코드를 푸시하고 파이프라인을 다시 빌드합니다.

### 컨텐츠 패키지에 /var 포함

다양한 임시 런타임 콘텐츠를 포함하는 `/var`을(를) 변경할 수 있습니다. 콘텐츠 패키지에 `/var` 포함(예: `ui.content`)을(를) Cloud Manager을 통해 배포하면 배포 단계가 실패할 수 있습니다.

이 문제는 초기 배포에 실패하지 않고 후속 배포에만 실패하므로 식별하기가 어렵습니다. 눈에 띄는 증상은 다음과 같습니다.

+ 초기 배포가 성공하지만 배포의 일부인 새로운 변경 가능한 콘텐츠 또는 변경된 변경 가능한 콘텐츠가 AEM Publish 서비스에 없는 것으로 표시됩니다.
+ AEM Author의 콘텐츠 활성화/비활성화가 차단됨
+ 이후 배포는 다음으로 배포 단계에서 실패하고, 다음으로 배포 단계는 약 60분 후 실패합니다.

이 문제의 유효성을 검사하려면 실패 동작의 원인을 확인하십시오.

1. 배포의 일부인 하나 이상의 콘텐츠 패키지가 `/var`에 쓰기되는지 확인하고 있습니다.
1. 기본(굵게) 배포 큐가 다음 위치에서 차단되었는지 확인:
   + AEM 작성자 > 도구 > 배포 > 배포

     ![차단된 배포 큐](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 후속 배포가 실패하면 로그 다운로드 버튼을 사용하여 Cloud Manager의 &quot;배포 대상&quot; 로그를 다운로드합니다.

   ![로그에 배포 다운로드](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... 그리고 log 문 사이에 약 60분이 있는지 확인합니다.

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... 및 ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   이 로그에는 성공적인 것으로 보고하는 초기 배포에 대한 이러한 지표가 포함되지 않고 후속 배포 실패 시에만 포함됩니다.

+ __원인:__ AEM 게시 서비스에 콘텐츠 패키지를 배포하는 데 사용되는 AEM의 복제 서비스 사용자가 AEM 게시의 `/var`에 쓸 수 없습니다. 따라서 콘텐츠 패키지를 AEM Publish 서비스로 배포하지 못합니다.
+ __해결 방법:__ 이 문제를 해결하기 위한 다음 방법은 기본 설정 순서대로 나열되어 있습니다.
   1. `/var` 리소스가 필요하지 않은 경우 응용 프로그램의 일부로 배포된 콘텐츠 패키지에서 `/var`의 리소스를 제거합니다.
   2. `/var` 리소스가 필요한 경우 [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=ko#repoinit)를 사용하여 노드 구조를 정의합니다. Repoinit 스크립트는 OSGi 실행 모드를 통해 AEM 작성자, AEM 게시 또는 둘 다에 타깃팅할 수 있습니다.
   3. `/var` 리소스가 AEM 작성자에만 필요하며 [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=ko#repoinit)을(를) 사용하여 합리적으로 모델링할 수 없는 경우 이 리소스를 [AEM 작성자 실행 모드 폴더(`<target>/apps/example-packages/content/install.author</target>`)의 `all` 패키지에 포함](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=ko#embeddeds)하여 AEM 작성자에만 설치된 개별 콘텐츠 패키지로 이동하십시오.
   4. 이 [Adobe KB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html)에 설명된 대로 `sling-distribution-importer` 서비스 사용자에게 적절한 ACL을 제공하십시오.

### Adobe 지원 사례 만들기

위의 문제 해결 방법으로 문제가 해결되지 않으면 다음을 통해 Adobe 지원 사례를 만드십시오.

+ [Adobe Admin Console](https://adminconsole.adobe.com) > 지원 탭 > 사례 만들기

  _여러 Adobe 조직의 구성원인 경우 사례를 만들기 전에 실패한 파이프라인이 있는 Adobe 조직이 Adobe 조직 전환기에서 선택되었는지 확인하십시오._
