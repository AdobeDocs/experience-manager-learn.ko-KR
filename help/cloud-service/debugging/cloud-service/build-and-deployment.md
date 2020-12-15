---
title: 구축 및 배포
description: Adobe Cloud Manager는 Cloud Service으로 AEM에 대한 코드 작성 및 배포를 용이하게 합니다. 빌드 프로세스의 단계 중에 오류가 발생할 수 있으므로 오류를 해결하기 위한 작업이 필요합니다. 이 안내서에서는 배포의 일반적인 실패를 파악하고 이러한 오류에 가장 잘 접근하는 방법을 안내합니다.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
translation-type: tm+mt
source-git-commit: a405cf14d3f71bf51e32e50c828c3216d29aa253
workflow-type: tm+mt
source-wordcount: '2517'
ht-degree: 0%

---


# Cloud Service 빌드 및 배포로 AEM 디버깅

Adobe Cloud Manager는 Cloud Service으로 AEM에 대한 코드 작성 및 배포를 용이하게 합니다. 빌드 프로세스의 단계 중에 오류가 발생할 수 있으므로 오류를 해결하기 위한 작업이 필요합니다. 이 안내서에서는 배포의 일반적인 실패를 파악하고 이러한 오류에 가장 잘 접근하는 방법을 안내합니다.

![클라우드 관리 빌드 파이프라인](./assets/build-and-deployment/build-pipeline.png)

## 유효성 검사

유효성 검사 단계는 기본적인 Cloud Manager 구성이 유효한지 간단히 확인할 수 있습니다. 일반적인 유효성 검사 실패:

### 환경이 잘못된 상태입니다.

+ __오류 메시지: 환경__ 이 잘못된 상태입니다.
   ![환경이 잘못된 상태입니다.](./assets/build-and-deployment/validation__invalid-state.png)
+ __원인:__ 파이프라인의 대상 환경이 새 빌드를 적용할 수 없는 전환 상태에 있습니다.
+ __해결__ 방법: 상태가 실행(또는 업데이트 사용 가능) 상태로 해결될 때까지 기다립니다. 환경이 삭제되는 경우 환경을 다시 만들거나 다른 환경을 선택하여 구축할 수 있습니다.

### 파이프라인과 연결된 환경을 찾을 수 없습니다.

+ __오류 메시지:__ 환경이 삭제된 것으로 표시됩니다.
   ![환경이 삭제됨으로 표시됩니다.](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __원인:__ 파이프라인이 사용하도록 구성된 환경이 삭제되었습니다.
동일한 이름의 새 환경이 다시 만들어져도 Cloud Manager는 파이프라인을 동일한 이름의 환경에 자동으로 다시 연결하지 않습니다.
+ __해상도:__ 파이프라인 구성을 편집하고 배포할 환경을 다시 선택합니다.

### 파이프라인과 연결된 Git 분기를 찾을 수 없습니다.

+ __오류 메시지:__ 잘못된 파이프라인:XXXXXX. Reason=Branch=xxxx 보관소에 없습니다.
   ![잘못된 파이프라인:XXXXXX. Reason=Branch=xxxx 리포지토리에서](./assets/build-and-deployment/validation__branch-not-found.png)을(를) 찾을 수 없습니다.
+ __원인:__ 파이프라인이 사용하도록 구성된 Git 분기가 삭제되었습니다.
+ __해상도:__ 동일한 이름을 사용하여 누락된 Git 분기를 다시 만들거나 다른 기존 분기에서 빌드하도록 파이프라인을 다시 구성합니다.

## 구축 및 단위 테스트

![구축 및 단위 테스트](./assets/build-and-deployment/build-and-unit-testing.png)

빌드 및 단위 테스트 단계는 파이프라인의 구성된 Git 분기에서 체크 아웃된 프로젝트의 마벤 빌드(`mvn clean package`)를 수행합니다.

이 단계에서 확인된 오류는 다음과 같은 예외를 제외하고 로컬에서 프로젝트를 다시 작성할 수 있어야 합니다.

+ [Maven Central](https://search.maven.org/)에 대해 사용할 수 없는 마비사 종속성이 사용되고 종속성이 포함된 Maven 저장소가 다음 중 하나입니다.
   + 비공개 내부 마웬 리포지토리 등 Cloud Manager에서 연결할 수 없습니다. Maven 리포지토리를 사용하려면 인증이 필요하며 잘못된 자격 증명이 제공되었습니다.
   + 프로젝트의 `pom.xml`에 명시적으로 등록되지 않았습니다. 빌드 시간이 증가함에 따라 마웬 리포지토리를 비롯한 침대는 비활성화됩니다.
+ 시간 설정 문제로 인해 단위 테스트가 실패했습니다. 이것은 단위 테스트가 타이밍에 민감한 경우 발생할 수 있습니다. 강력한 표시기가 테스트 코드의 `.sleep(..)`에 의존하고 있습니다.
+ 지원되지 않는 Maven 플러그인의 사용.

## 코드 스캔

![코드 스캔](./assets/build-and-deployment/code-scanning.png)

코드 스캔은 Java 및 AEM 관련 모범 사례를 조합하여 정적 코드 분석을 수행합니다.

코드에 치명적인 보안 취약점이 있는 경우 코드 스캔을 수행하면 빌드 오류가 발생합니다. 가벼운 위반은 재정의할 수 있지만, 이를 수정하는 것이 좋습니다. 코드 스캔은 불완전하며 [잘못된 포지티브](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/understand-test-results.html#dealing-with-false-positives)로 이어질 수 있습니다.

코드 스캔 문제를 해결하려면 **세부 정보 다운로드** 단추를 통해 Cloud Manager에서 제공하는 CSV 형식의 보고서를 다운로드하고 모든 항목을 검토하십시오.

자세한 내용은 AEM 관련 규칙을 참조하십시오. Cloud Manager 설명서&#39; [사용자 정의 AEM 관련 코드 스캔 규칙](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html)을 참조하십시오.

## 이미지 만들기

![이미지 만들기](./assets/build-and-deployment/build-images.png)

빌드 이미지는 빌드 및 단위 테스트 단계에서 만든 빌드 코드 아티팩트를 AEM 릴리스와 결합하여 배포 가능한 단일 아티팩트를 만드는 작업을 수행합니다.

빌드 및 단위 테스트 중에 코드 작성 및 컴파일 문제가 발견되지만 사용자 지정 빌드 아티팩트를 AEM 릴리스와 결합하려고 할 때 구성 또는 구조적 문제가 발견될 수 있습니다.

### OSGi 구성 복제

여러 OSGi 구성이 대상 AEM 환경에 대한 런타임 모드를 통해 확인되면 이미지 작성 단계가 실패하고 다음 오류가 발생합니다.

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### 원인 1

+ __원인:__ AEM 프로젝트의 모든 패키지에 여러 코드 패키지가 들어 있으며, 둘 이상의 코드 패키지에서 동일한 OSGi 구성이 제공되어 충돌이 발생하며, 이미지 작성 단계에서 사용할 항목을 결정할 수 없으므로 빌드가 실패합니다. OSGi 팩토리 구성에는 고유 이름이 있을 경우 이 구성은 적용되지 않습니다.
+ __해결__ 방법: AEM 애플리케이션의 일부로 배포되는 모든 코드 패키지(포함된 타사 코드 패키지 포함)를 검토하고, 런타임 모드를 통해 대상 환경으로 해결되는 중복 OSGi 구성을 찾습니다. &quot;mergeConfigurations 플래그를 true로 설정&quot;에 대한 오류 메시지의 지침은 AEM에서 클라우드 서비스로 사용할 수 없으므로 무시해야 합니다.

#### 원인 2

+ __원인:__ AEM 프로젝트에 동일한 코드 패키지가 두 번 잘못 포함되어 해당 패키지에 포함된 OSGi 구성이 중복됩니다.
+ __해상도:__ 모든 프로젝트에 포함된 패키지의 모든 pom.xml을 검토하고 구성 `filevault-package-maven-plugin` [](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) 이 로 설정되어 있는지 확인합니다 `<cloudManagerTarget>none</cloudManagerTarget>`.

### 잘못된 참조 스크립트

리포인트 스크립트는 기본 컨텐츠, 사용자, ACL 등을 정의합니다. Cloud Service으로 AEM에서는 이미지 작성 중에 원격 스크립트가 적용되지만 AEM SDK의 로컬 빠른 시작에서는 OSGi 리포인트 팩터리 구성이 활성화되면 해당 스크립트가 적용됩니다. 이로 인해 Repoinit 스크립트가 AEM SDK의 로컬 빠른 시작(로깅 사용)에서 조용히 실패할 수 있지만 이미지 작성 단계가 실패하여 배포를 중단합니다.

+ __원인:__ 추천 스크립트의 형식이 잘못되었습니다. 실패한 스크립트가 저장소에 대해 실행된 후 리포인트 스크립트로 저장소가 불완전한 상태로 남아 있을 수 있습니다.
+ __해결__ 방법: 참조 스크립트 OSGi 구성이 배포될 때 AEM SDK의 로컬 빠른 시작을 검토하여 오류가 있는지 여부 및 오류를 확인합니다.

### 충족되지 않은 참조 콘텐츠 종속성

리포인트 스크립트는 기본 컨텐츠, 사용자, ACL 등을 정의합니다. AEM SDK의 로컬 quickstart에서는 리포인트 OSGi 팩토리 구성이 활성화될 때 리포인트 스크립트가 적용되거나, 다시 말해 저장소가 활성화된 후에 직접 또는 컨텐츠 패키지를 통해 컨텐츠가 변경되었을 수 있습니다. AEM에서 Cloud Service으로, 리포인트 스크립트는 리포인트 스크립트가 종속되는 컨텐츠를 포함하지 않는 저장소에 대해 이미지 작성 중에 적용됩니다.

+ __원인:__ 추천 스크립트는 존재하지 않는 컨텐츠에 따라 다릅니다.
+ __해상도:__ 참조 스크립트가 종속된 내용이 있는지 확인합니다. 종종, 누락되지만 필수 컨텐츠 구조를 정의하는 지시어가 누락된 잘못 정의된 리포인트 스크립트가 표시됩니다. 이것은 AEM을 삭제하고, Jar를 압축 해제하고, 리포인트 스크립트가 포함된 리포인트 OSGi 구성을 설치 폴더에 추가하고, AEM을 시작하여 로컬로 재현할 수 있습니다. 이 오류는 AEM SDK 로컬 quickstart의 error.log에 표시됩니다.


### 응용 프로그램의 핵심 구성 요소 버전이 배포된 버전보다 큽니다.

_이 문제는 최신 AEM 릴리스로 자동 업데이트되지 않는 비프로덕션 환경에만 영향을 줍니다._

CLOUD SERVICE은 모든 AEM 릴리스에 최신 핵심 구성 요소 버전을 자동으로 포함합니다. 즉, Cloud Service 환경으로 AEM이 자동으로 또는 수동으로 업데이트되면 최신 버전의 핵심 구성 요소가 해당 AEM에 배포됩니다.

다음 경우 이미지 작성 단계가 실패할 수 있습니다.

+ 배포 응용 프로그램은 `core`(OSGi 번들) 프로젝트의 핵심 구성 요소 상위 종속성 버전을 업데이트합니다.
+ 그런 다음 배포 응용 프로그램이 새 코어 구성 요소 버전이 포함된 AEM 릴리스를 사용하도록 업데이트되지 않은 Cloud Service 환경으로 샌드박스(비프로덕션) AEM에 배포됩니다.

이러한 실패를 방지하기 위해 Cloud Service 환경으로 AEM 업데이트를 사용할 수 있을 때마다 업데이트를 다음 빌드/배포의 일부로 포함시키고 응용 프로그램 코드 베이스에서 핵심 구성 요소 버전을 증가시킨 후 항상 업데이트가 포함되는지 확인하십시오.

+ __증상:__
이미지 만들기 단계가 실패하여 
`com.adobe.cq.wcm.core.components...` 특정 버전 범위의 패키지를 프로젝트에 의해 가져올 수  `core` 없습니다.

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __원인:__    `core` 애플리케이션의 OSGi 번들(프로젝트에서 정의됨)은 Cloud Service으로 AEM에 배포된 버전과 다른 버전 수준에서 핵심 구성 요소 핵심 종속성에서 Java 클래스를 가져옵니다.
+ __해상도:__
   + Git을 사용하여 핵심 구성 요소 버전 증가 이전에 존재하는 작업 커밋으로 되돌립니다. 이 커밋을 Cloud Manager Git 분기에 푸시하고 이 분기에서 환경 업데이트를 수행합니다. AEM은 최신 핵심 구성 요소 버전이 포함된 최신 AEM 릴리스의 Cloud Service으로 업그레이드됩니다. Cloud Service으로 AEM이 최신 핵심 구성 요소 버전이 있는 최신 AEM 릴리스로 업데이트되면 원래 실패한 코드를 다시 배포할 수 있습니다.
   + 이 문제를 로컬로 재현하려면 AEM SDK 버전이 Cloud Service 환경에서 사용하는 AEM과 동일한 AEM 릴리스 버전인지 확인하십시오.


### Adobe 지원 사례 만들기

위의 문제 해결 방법이 문제를 해결하지 못하는 경우 다음을 통해 Adobe 지원 사례를 만드십시오.

+ [Adobe Admin Console](https://adminconsole.adobe.com) > 지원 탭 > 사례 만들기

   _여러 Adobe 조직의 멤버인 경우 케이스를 생성하기 전에 Adobe 조직 전환기에서 파이프라인이 실패한 Adobe 조직이 선택되었는지 확인하십시오._

## 배포 대상

배포 대상 단계는 이미지 빌드에서 생성된 코드 아티팩트를 가져와 이를 사용하여 새 AEM 작성자 및 게시 서비스를 시작하고, 성공하면 모든 이전 AEM 작성자 및 게시 서비스를 제거합니다. 이 단계에서도 변경 가능한 컨텐츠 패키지와 색인이 설치 및 업데이트됩니다.

배포 대상 단계를 디버깅하기 전에 Cloud Service 로그](./logs.md)로 [AEM에 익숙해지십시오. `aemerror` 로그에는 발행물에 배포와 관련이 있을 수 있는 창의 시작 및 종료에 대한 정보가 포함되어 있습니다. Cloud Manager의 배포 대상 단계에서 다운로드 로그 단추를 통해 사용할 수 있는 로그는 `aemerror` 로그가 아니며 응용 프로그램 시작과 관련된 자세한 정보를 포함하지 않습니다.

![배포 대상](./assets/build-and-deployment/deploy-to.png)

배포 대상 단계가 실패할 수 있는 3가지 주요 이유:

### Cloud Manager 파이프라인에 이전 AEM 버전이 있음

+ __원인:__ Cloud Manager 파이프라인은 대상 환경에 배포된 이전 버전보다 이전 버전의 AEM을 보유합니다. 이 문제는 파이프라인이 다시 사용되고 최신 버전의 AEM을 실행 중인 새 환경을 가리킬 때 발생할 수 있습니다. 환경의 AEM 버전이 파이프라인의 AEM 버전보다 높은지 확인하여 식별할 수 있습니다.
   ![Cloud Manager 파이프라인에 이전 AEM 버전이 있음](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __해상도:__
   + 대상 환경에 [사용 가능한 업데이트]가 있는 경우 환경 작업의 [업데이트]를 선택한 다음 빌드를 다시 실행합니다.
   + 대상 환경에 사용 가능한 업데이트가 없는 경우 최신 버전의 AEM이 실행 중임을 의미합니다. 이 문제를 해결하려면 파이프라인을 삭제하고 다시 만듭니다.


### Cloud Manager 시간 초과

새로 배포된 AEM 서비스를 시작하는 동안 코드를 실행하는 데 시간이 너무 오래 소요되므로 Cloud Manager는 배포가 완료되기 전에 시간이 초과됩니다. 이러한 경우 Cloud Manager 상태가 실패라고 보고해도 결국 배포가 성공할 수 있습니다.

+ __원인:__ 사용자 지정 코드는 OSGi 번들 또는 구성 요소 라이프사이클에서 초기에 트리거된 큰 쿼리 또는 컨텐츠 순서와 같은 작업을 실행할 수 있으며 AEM의 시작 시간이 크게 지연됩니다.
+ __해결__ 방법: OSGi Bundle의 라이프사이클에서 초기에 실행되는 코드에 대한 구현을 검토하고, Cloud Manager에서 설명한 대로 장애 발생 시간(GMT의 로그인 시간)에 AEM 작성자 및 게시 서비스에 대한  `aemerror` 로그를 검토하고, 사용자 지정 로그 실행 프로세스를 나타내는 로그 메시지를 찾습니다.

### 호환되지 않는 코드 또는 구성

대부분의 코드 및 구성 위반은 빌드 초기에 발견되지만 사용자 지정 코드 또는 구성이 Cloud Service으로 AEM과 호환되지 않고 컨테이너에서 실행될 때까지 감지되지 않을 수 있습니다.

+ __원인:__ 사용자 지정 코드는 OSGi 번들 또는 구성 요소 라이프사이클에서 초기에 트리거된 큰 쿼리 또는 컨텐츠 순서와 같은 긴 작업을 호출할 수 있으며 AEM의 시작 시간이 크게 지연됩니다.
+ __해결__ 방법: 클라우드 관리자에  `aemerror` 표시된 대로 실패의 시간(GMT로 기록 시간)에 AEM 작성자 및 게시 서비스에 대한 로그를 검토합니다.
   1. 사용자 지정 응용 프로그램에서 제공하는 Java 클래스에서 throw된 모든 ERROR에 대한 로그를 검토합니다. 문제가 발견되면 문제를 해결하고 고정 코드를 푸시하며 파이프라인을 다시 빌드합니다.
   1. 사용자 정의 애플리케이션에서 확장/상호 작용하는 AEM의 측면에 의해 보고된 모든 ERROR에 대한 로그를 검토하고 이를 조사합니다.이러한 오류는 Java 클래스에 직접 속하지 않을 수 있습니다. 문제가 발견되면 문제를 해결하고 고정 코드를 푸시하며 파이프라인을 다시 빌드합니다.

### 컨텐트 패키지에 /var 포함

`/var` 을 변경할 수 있으며, 다양한 일시적인 런타임 컨텐츠를 포함할 수 있습니다. 컨텐츠 패키지에 `/var`을(를) 포함합니다(예: `ui.content`)를 Cloud Manager를 통해 배포하면 배포 대상 단계가 실패할 수 있습니다.

이 문제는 초기 배포의 실패를 초래하지 않고 이후 배포에서만 발생하므로 식별하기 어렵습니다. 눈에 띄는 증상은 다음과 같습니다.

+ 초기 배포는 성공하지만 배포에 포함된 신규 또는 변경 가능한 컨텐츠는 AEM 게시 서비스에 존재하지 않는 것으로 나타납니다.
+ AEM 작성자 컨텐츠의 활성화/비활성화 차단됨
+ 이후 배포는 배포 대상 단계에서 실패하며 배포 대상 단계는 약 60분 후에 실패합니다.

이 문제가 실패한 동작의 원인인지 확인하려면:

1. 배포에 포함된 하나 이상의 콘텐츠 패키지가 `/var`에 쓰인지 확인합니다.
1. 기본(볼드) 배포 큐가 다음 위치에서 차단되었는지 확인합니다.
   + AEM 작성자 > 도구 > 배포 > 배포
      ![차단된 배포 큐](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 후속 배포가 실패하면 다운로드 로그 단추를 사용하여 Cloud Manager의 &quot;배포 대상&quot; 로그를 다운로드합니다.

   ![로그에 배포 다운로드](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   .. 및 로그 문 사이에 약 60분이 있는지 확인합니다.

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... 및 ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   이 로그에는 후속 실패한 배포에만 해당되는 것이 아니라 성공적인 것으로 보고하는 초기 배포에 이러한 지표가 포함되지 않습니다.

+ __원인: AEM 게시 서비스에 콘텐츠 패키지를 배포하는 데 사용되는__ AEM 복제 서비스 사용자는 AEM 게시 `/var` 에 쓸 수 없습니다. 이로 인해 AEM 게시 서비스에 콘텐츠 패키지 배포가 실패합니다.
+ __해결__ 방법: 다음 해결 방법은 환경 설정의 순서로 나열되어 있습니다.
   1. `/var` 리소스가 필요하지 않은 경우 응용 프로그램의 일부로 배포된 컨텐츠 패키지에서 `/var` 아래의 리소스를 제거할 필요가 없습니다.
   2. `/var` 리소스가 필요한 경우 [repoinit](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)를 사용하여 노드 구조를 정의합니다. 참조 스크립트는 AEM 작성자, AEM 게시 또는 둘 모두를 OSGi 실행 모드를 통해 타깃팅할 수 있습니다.
   3. `/var` 리소스가 AEM 작성자에만 필요하고 [repoinit](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)을(를) 사용하여 합리적으로 모델링할 수 없는 경우, AEM 작성자 런타임 모드 폴더(`<target>/apps/example-packages/content/install.author</target>`)의 `all` 패키지에 ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds)만 포함하여 AEM 작성자에만 설치되는 개별 컨텐츠 패키지로 이동합니다.[

### Adobe 지원 사례 만들기

위의 문제 해결 방법이 문제를 해결하지 못하는 경우 다음을 통해 Adobe 지원 사례를 만드십시오.

+ [Adobe Admin Console](https://adminconsole.adobe.com) > 지원 탭 > 사례 만들기

   _여러 Adobe 조직의 멤버인 경우 케이스를 생성하기 전에 Adobe 조직 전환기에서 파이프라인이 실패한 Adobe 조직이 선택되었는지 확인하십시오._
