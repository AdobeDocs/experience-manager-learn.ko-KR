---
user-guide-title: Adobe Experience Manager as a Cloud Service 자습서
user-guide-description: Adobe Experience Manager as a Cloud Service를 위한 튜토리얼 모음입니다.
breadcrumb-title: AEM as a Cloud Service 튜토리얼
sub-product: 클라우드 서비스
team: TM
source-git-commit: aa90b2c1a066dc36d4ba26ecdb8b58939445ef34
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 21%

---


# Adobe Experience Manager as a Cloud Service 자습서 {#cloud-service}

+ [개요](./overview.md)
+ AEM as a Cloud Service 소개{#introduction}
   + [AEM as a Cloud Service 소개](./introduction/what-is-aem-as-a-cloud-service.md)
   + [진화](./introduction/evolution.md)
   + [아키텍처](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ 기본 기술 {#underlying-technology}
   + [AEM 아키텍처](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java Content Repository](./underlying-technology/introduction-jcr.md)
   + [슬링](./underlying-technology/introduction-sling.md)
   + [작성 및 게시 서비스](./underlying-technology/introduction-author-publish.md)
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
      + [Dispatcher 구성](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ 로컬 개발 환경 설정 {#local-development-environment-set-up}
   + [개요](./local-development-environment/overview.md)
   + [개발 도구](./local-development-environment/development-tools.md)
   + [로컬 AEM 런타임](./local-development-environment/aem-runtime.md)
   + [로컬 Dispatcher 도구](./local-development-environment/dispatcher-tools.md)
+ 개발{#developing}
   + 개발 기본 사항{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [로컬 개발 환경](./developing/basics/local-development-environment.md)
      + [AEM 프로젝트 전형](./developing/basics/aem-project-archetype.md)
      + [AEM 프로젝트 구조](./developing/basics/project-structure.md)
      + [가변 콘텐츠와 가변 콘텐츠 비교](./developing/basics/mutable-immutable.md)
      + [저장소 구조 패키지](./developing/basics/repository-structure-package.md)
      + [컨텐츠 게시](./developing/basics/content-publishing.md)
      + [OSGi 구성](./developing/basics/osgi-configurations.md)
      + [Dispatcher 구성 마이그레이션](./developing/basics/dispatcher-configuration.md)
   + AEM 프로젝트{#aem-projects}
      + [AEM Maven 프로젝트](./developing/projects/maven-project-structure.md)
   + OSGi 서비스{#osgi-services}
      + [OSGi 서비스 기본 사항](./developing/osgi-services/basics.md)
      + [OSGi 구성 요소 라이프사이클](./developing/osgi-services/lifecycle.md)
      + [OSGi 구성 기본 사항](./developing/osgi-services/configurations.md)
      + [OCD를 사용한 OSGi 구성](./developing/osgi-services/configurations-ocd.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ AEM{#debugging} 디버깅
   + AEM SDK{#debugging-aem-sdk} 디버깅
      + [개요](./debugging/aem-sdk-local-quickstart/overview.md)
      + [로그](./debugging/aem-sdk-local-quickstart/logs.md)
      + [원격 디버깅](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi 웹 콘솔](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher 도구](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [기타 도구](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + AEM을 Cloud Service{#debugging-aem-as-a-cloud-service}으로 디버깅
      + [개요](./debugging/cloud-service/overview.md)
      + [로그](./debugging/cloud-service/logs.md)
      + [작성 및 배포](./debugging/cloud-service/build-and-deployment.md)
      + [개발자 콘솔](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ AEM{#accessing}에 액세스
   + [개요](./accessing/overview.md)
   + [IMS 사용자 Adobe](./accessing/adobe-ims-users.md)
   + [Adobe IMS 사용자 그룹](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS 제품 프로필](./accessing/adobe-ims-product-profiles.md)
   + [AEM 사용자, 그룹 및 권한](./accessing/aem-users-groups-and-permissions.md)
   + [AEM 액세스 구성 둘러보기](./accessing/walk-through.md)
+ 마이그레이션 {#migration}
   + [컨텐츠 전송 도구](./migration/content-transfer-tool.md)
   + [자산의 벌크 가져오기](./migration/bulk-import.md)
+ 양식{#forms}
   + 적응형 양식 만들기{#create-first-af}
      + [소개](./forms/create-first-af/introduction.md)
      + [테마 만들기](./forms/create-first-af/create-theme.md)
      + [템플릿 만들기](./forms/create-first-af/create-template.md)
      + [조각 만들기](./forms/create-first-af/create-fragments.md)
      + [양식 만들기](./forms/create-first-af/create-af.md)
      + [루트 패널 구성](./forms/create-first-af/configure-root-panel.md)
      + [사용자 패널 구성](./forms/create-first-af/configure-people-panel.md)
      + [소득 패널 구성](./forms/create-first-af/configure-income-panel.md)
      + [자산 패널 구성](./forms/create-first-af/configure-assets-panel.md)
      + [시작 패널 구성](./forms/create-first-af/configure-start-panel.md)
      + [도구 모음 추가 및 구성](./forms/create-first-af/add-configure-toolbar.md)
   + Document Cloud API 및 AEM Forms CS{#doc-cloud-sdk}
      + [소개](./forms/doc-cloud-sdk/introduction.md)
      + [Adobe IO 프로젝트 만들기](./forms/doc-cloud-sdk/create-document-cloud-credentials.md)
      + [OSGI 구성 만들기](./forms/doc-cloud-sdk/create-doc-cloud-configuration.md)
      + [인터페이스 정의](./forms/doc-cloud-sdk/create-interface.md)
      + [인터페이스 구현](./forms/doc-cloud-sdk/implement-interface.md)
      + [JSON 부분 만들기](./forms/doc-cloud-sdk/get-content-analyzer.md)
      + [사용자 지정 프로세스 단계](./forms/doc-cloud-sdk/custom-process-step.md)
   + Azure 포털 저장소{#forms-cs-azure-portal}
      + [소개](./forms/forms-cs-azure-portal/introduction.md)
      + [양식 데이터 모델 작성](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Azure 저장소에 양식 데이터 저장](./forms/forms-cs-azure-portal/create-af.md)
      + [양식 미리 채우기](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [쿼리 제출](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 검토 작업 과정 만들기{#create-aem-workflow}
      + [워크플로우 모델 만들기](./forms/create-aem-workflow/create-workflow.md)
      + [워크플로우 트리거](./forms/create-aem-workflow/configure-af.md)
   + Adobe Sign과 AEM Forms{#forms-and-sign}
      + [소개](./forms/forms-and-sign/introduction.md)
      + [Adobe Sign API 애플리케이션](./forms/forms-and-sign/create-sign-api-application.md)
      + [Adobe Sign 클라우드 구성](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [적응형 양식 만들기](./forms/forms-and-sign/create-adaptive-form.md)
      + [채우기 및 서명 구성](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Salesforce{#integrate-with-salesforce}와 통합
      + [소개](./forms/integrate-with-salesforce/introduction.md)
      + [연결된 앱 만들기](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Swagger 파일 만들기](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [데이터 소스 만들기](./forms/integrate-with-salesforce/create-data-source.md)
      + [양식 데이터 모델 만들기](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [테스트 양식 제출](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [테스트 클릭 이벤트](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ asset compute 확장성{#asset-compute}
   + [개요](./asset-compute/overview.md)
   + {#set-up} 설정
      + [계정 및 서비스 프로비저닝](./asset-compute/set-up/accounts-and-services.md)
      + [로컬 개발 환경](./asset-compute/set-up/development-environment.md)
      + [Adobe 프로젝트 Firefly](./asset-compute/set-up/firefly.md)
   + 개발{#develop}
      + [asset compute 프로젝트 만들기](./asset-compute/develop/project.md)
      + [환경 변수 구성](./asset-compute/develop/environment-variables.md)
      + [manifest.html 구성](./asset-compute/develop/manifest.md)
      + [작업자 개발](./asset-compute/develop/worker.md)
      + [개발 도구 사용](./asset-compute/develop/development-tool.md)
   + 테스트 및 디버그{#test-debug}
      + [작업자 테스트](./asset-compute/test-debug/test.md)
      + [작업자 디버깅](./asset-compute/test-debug/debug.md)
   + 배포{#deploy}
      + [Adobe I/O Runtime에 배포](./asset-compute/deploy/runtime.md)
      + [AEM과 통합](./asset-compute/deploy/processing-profiles.md)
   + 고급{#advanced}
      + [메타데이터 작업자](./asset-compute/advanced/metadata.md)
   + [문제 해결](./asset-compute/troubleshooting.md)
+ 다중 단계 Tutorials{#multi-step-tutorials}
   + [AEM Sites 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA 편집기(반응)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [SPA 편집기(Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites 및 Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [토큰 기반 인증](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
