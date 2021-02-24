---
user-guide-title: Adobe Experience Manager as a Cloud Service 자습서
user-guide-description: Adobe Experience Manager as a Cloud Service를 위한 튜토리얼 모음입니다.
breadcrumb-title: AEM as a Cloud Service 튜토리얼
sub-product: 클라우드 서비스
team: TM
translation-type: tm+mt
source-git-commit: 59b786d95d1428916adad37ceca4412b93463e9b
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 24%

---


# Adobe Experience Manager as a Cloud Service 자습서 {#cloud-service}

+ [개요](./overview.md)
+ AEM as a Cloud Service 소개{#introduction}
   + [AEM은 Cloud Service란?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [진화](./introduction/evolution.md)
   + [아키텍처](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ 기본 기술 {#underlying-technology}
   + [AEM 아키텍처](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java 컨텐츠 저장소](./underlying-technology/introduction-jcr.md)
   + [슬링](./underlying-technology/introduction-sling.md)
   + [서비스 작성 및 게시](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [프로그램](./cloud-manager/programs.md)
   + [환경](./cloud-manager/environments.md)
   + [CI/CD 프로덕션 파이프라인](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD 비프로덕션 파이프라인](./cloud-manager/cicd-non-production-pipeline.md)
   + [활동](./cloud-manager/activity.md)
   + 개발 운영{#devops}
      + [코드 배포](./cloud-manager/devops/deploy-code.md)
      + [프로젝트 병합](./cloud-manager/devops/merge-projects.md)
      + [파이프라인 구성](./cloud-manager/devops/configure-pipelines.md)
      + [지속적인 통합](./cloud-manager/devops/continuous-integration.md)
      + [테스트 결과 분석](./cloud-manager/devops/analyze-test-results.md)
      + [발송자 구성](./cloud-manager/devops/dispatcher-configurations.md)
      + [클라우드 관리자 API](./cloud-manager/devops/cloud-manager-apis.md)
+ 로컬 개발 환경 설정 {#local-development-environment-set-up}
   + [개요](./local-development-environment/overview.md)
   + [개발 도구](./local-development-environment/development-tools.md)
   + [로컬 AEM 런타임](./local-development-environment/aem-runtime.md)
   + [로컬 디스패처 도구](./local-development-environment/dispatcher-tools.md)
+ 개발{#developing}
   + 개발 기본 사항{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [로컬 개발 환경](./developing/basics/local-development-environment.md)
      + [AEM 프로젝트 전형](./developing/basics/aem-project-archetype.md)
      + [AEM 프로젝트 구조](./developing/basics/project-structure.md)
      + [변경 가능한 컨텐츠와 변경 불가능한 컨텐츠 비교](./developing/basics/mutable-immutable.md)
      + [저장소 구조 패키지](./developing/basics/repository-structure-package.md)
      + [컨텐츠 게시](./developing/basics/content-publishing.md)
      + [OSGi 구성](./developing/basics/osgi-configurations.md)
      + [발송자 구성 마이그레이션](./developing/basics/dispatcher-configuration.md)
   + [AEM SDK API JavaDocs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service-javadoc/)
+ AEM{#debugging} 디버깅
   + AEM SDK 디버깅{#debugging-aem-sdk}
      + [개요](./debugging/aem-sdk-local-quickstart/overview.md)
      + [로그](./debugging/aem-sdk-local-quickstart/logs.md)
      + [원격 디버깅](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi 웹 콘솔](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher 도구](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [기타 툴](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + AEM을 Cloud Service{#debugging-aem-as-a-cloud-service}(으)로 디버깅
      + [개요](./debugging/cloud-service/overview.md)
      + [로그](./debugging/cloud-service/logs.md)
      + [구축 및 배포](./debugging/cloud-service/build-and-deployment.md)
      + [개발자 콘솔](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ AEM{#accessing} 액세스
   + [개요](./accessing/overview.md)
   + [Adobe IMS 사용자](./accessing/adobe-ims-users.md)
   + [Adobe IMS 사용자 그룹](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS 제품 프로필](./accessing/adobe-ims-product-profiles.md)
   + [AEM 사용자, 그룹 및 권한](./accessing/aem-users-groups-and-permissions.md)
   + [AEM 설치 과정에 대한 액세스 구성](./accessing/walk-through.md)
+ 마이그레이션 {#migration}
   + [컨텐츠 전송 도구](./migration/content-transfer-tool.md)
   + [자산의 일괄 가져오기](./migration/bulk-import.md)
+ asset compute 확장성{#asset-compute}
   + [개요](./asset-compute/overview.md)
   + {#set-up} 설정
      + [계정 및 서비스 제공](./asset-compute/set-up/accounts-and-services.md)
      + [로컬 개발 환경](./asset-compute/set-up/development-environment.md)
      + [Adobe 프로젝트 Firefly](./asset-compute/set-up/firefly.md)
   + 개발{#develop}
      + [asset compute 프로젝트 만들기](./asset-compute/develop/project.md)
      + [환경 변수 구성](./asset-compute/develop/environment-variables.md)
      + [manifest.yml 구성](./asset-compute/develop/manifest.md)
      + [근로자 개발](./asset-compute/develop/worker.md)
      + [개발 도구 사용](./asset-compute/develop/development-tool.md)
   + 테스트 및 디버그{#test-debug}
      + [작업자 테스트](./asset-compute/test-debug/test.md)
      + [작업자 디버그](./asset-compute/test-debug/debug.md)
   + 배포{#deploy}
      + [Adobe I/O Runtime에 배포](./asset-compute/deploy/runtime.md)
      + [AEM과 통합](./asset-compute/deploy/processing-profiles.md)
   + 고급{#advanced}
      + [메타데이터 작업자](./asset-compute/advanced/metadata.md)
   + [문제 해결](./asset-compute/troubleshooting.md)
+ 다단계 Tutorials{#multi-step-tutorials}
   + [AEM Sites 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/develop-wknd-tutorial.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA 편집기(응답)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [SPA 편집기(Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites 및 Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [토큰 기반 인증](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)

