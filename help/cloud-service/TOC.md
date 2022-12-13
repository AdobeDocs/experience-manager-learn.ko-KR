---
user-guide-title: Adobe Experience Manager as a Cloud Service 튜토리얼
user-guide-description: Adobe Experience Manager as a Cloud Service를 위한 튜토리얼 모음입니다.
breadcrumb-title: AEM as a Cloud Service 튜토리얼
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '866'
ht-degree: 21%

---


# Adobe Experience Manager as a Cloud Service 튜토리얼 {#cloud-service}

+ [개요](./overview.md)
+ AEM as a Cloud Service 소개{#introduction}
   + [AEM as a Cloud Service 소개](./introduction/what-is-aem-as-a-cloud-service.md)
   + [진화](./introduction/evolution.md)
   + [아키텍처](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 전략 및 사고 리더십{#strategy}
      + [Experience Manager - 거버넌스 및 스태핑 모델 및 원형](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Adobe Experience Manager을 사용하여 컨텐츠 속도를 높이는 방법](./introduction/drive-content-velocity-for-sites.md)
      + [AEM 스타일 시스템을 사용하여 컨텐츠 속도 향상](./introduction/accelerate-content-velocity-aem.md)
+ [Experience Cloud 통합](./experience-cloud/integrations.md)
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
   + 확장성{#extensibility}
      + 컨텐츠 조각 콘솔{#content-fragments}
         + [개요](./developing/extensibility/content-fragments/overview.md)
         + [확장 등록](./developing/extensibility/content-fragments/extension-registration.md)
         + [헤더 메뉴](./developing/extensibility/content-fragments/header-menu.md)
         + [작업 표시줄](./developing/extensibility/content-fragments/action-bar.md)
         + [양식](./developing/extensibility/content-fragments/modal.md)
         + [Adobe I/O Runtime 작업](./developing/extensibility/content-fragments/runtime-action.md)
         + [테스트](./developing/extensibility/content-fragments/test.md)
         + [배포](./developing/extensibility/content-fragments/deploy.md)
         + 예시 확장{#example-extensions}
            + [벌크 속성 업데이트 확장](./developing/extensibility/content-fragments/example-extensions/bulk-property-update.md)
   + 개발 기본 사항{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [로컬 개발 환경](./developing/basics/local-development-environment.md)
      + [AEM Project Archetype](./developing/basics/aem-project-archetype.md)
      + [AEM 프로젝트 구조](./developing/basics/project-structure.md)
      + [가변 콘텐츠와 가변 콘텐츠 비교](./developing/basics/mutable-immutable.md)
      + [저장소 구조 패키지](./developing/basics/repository-structure-package.md)
      + [컨텐츠 게시](./developing/basics/content-publishing.md)
      + [OSGi 구성](./developing/basics/osgi-configurations.md)
      + [Dispatcher 구성 마이그레이션](./developing/basics/dispatcher-configuration.md)
   + AEM 프로젝트{#aem-projects}
      + [AEM Maven 프로젝트](./developing/projects/maven-project-structure.md)
      + [AEM Maven 프로젝트 정리](./developing/projects/remove-samples.md)
   + OSGi 서비스{#osgi-services}
      + [OSGi 서비스 기본 사항](./developing/osgi-services/basics.md)
      + [OSGi 구성 요소 라이프사이클](./developing/osgi-services/lifecycle.md)
      + [OSGi 구성 기본 사항](./developing/osgi-services/configurations.md)
      + [OCD를 사용한 OSGi 구성](./developing/osgi-services/configurations-ocd.md)
   + 고급{#advanced}
      + [서비스 사용자](./developing/advanced/service-users.md)
      + [페이지 변형 캐싱](./developing/advanced/variant-caching.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ AEM 디버깅{#debugging}
   + AEM SDK 디버깅{#debugging-aem-sdk}
      + [개요](./debugging/aem-sdk-local-quickstart/overview.md)
      + [로그](./debugging/aem-sdk-local-quickstart/logs.md)
      + [원격 디버깅](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi 웹 콘솔](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher 도구](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [기타 도구](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + AEM as a Cloud Service 디버깅{#debugging-aem-as-a-cloud-service}
      + [개요](./debugging/cloud-service/overview.md)
      + [로그](./debugging/cloud-service/logs.md)
      + [작성 및 배포](./debugging/cloud-service/build-and-deployment.md)
      + [개발자 콘솔](./debugging/cloud-service/developer-console.md)
      + [저장소 브라우저](./debugging/cloud-service/repository-browser.md)
      + 위험{#risks}
         + [탐색 경고](./debugging/cloud-service/risks/traversals.md)
+ 컨텐츠 전달{#content-delivery}
   + [URL 리디렉션](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html)
+ AEM 액세스{#accessing}
   + [개요](./accessing/overview.md)
   + [Adobe IMS 사용자](./accessing/adobe-ims-users.md)
   + [Adobe IMS 사용자 그룹](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS 제품 프로필](./accessing/adobe-ims-product-profiles.md)
   + [AEM 사용자, 그룹 및 권한](./accessing/aem-users-groups-and-permissions.md)
   + [AEM 액세스 구성 둘러보기](./accessing/walk-through.md)
+ 인증{#authentication}
   + [개요](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 고급 네트워킹{#networking}
   + [개요](./networking/advanced-networking.md)
   + [유연한 포트 송신](./networking/flexible-port-egress.md)
   + [전용 송신 IP 주소](./networking/dedicated-egress-ip-address.md)
   + [가상 사설 네트워크](./networking/vpn.md)
   + 코드 예{#examples}
      + [유연한 포트 전송을 위한 비표준 포트에서 HTTP/HTTPS](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [전용 송신 IP 주소/VPN용 HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [DataSourcePool을 사용한 SQL 연결](./networking/examples/sql-datasourcepool.md)
      + [Java SQL API를 사용한 SQL 연결](./networking/examples/sql-java-apis.md)
      + [이메일 서비스](./networking/examples/email-service.md)
+ 마이그레이션 {#migration}
   + [콘텐츠 전송 도구](./migration/content-transfer-tool.md)
   + [자산의 벌크 가져오기](./migration/bulk-import.md)

   + AEM as a Cloud Service로 이동 {#moving-to-aem-as-a-cloud-service}
      + [소개](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [온보딩](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA 및 CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM 현대화 도구](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [저장소 현대화](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset compute 마이크로서비스](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [검색 및 색인 지정](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 콘텐츠 마이그레이션 {#content-migration}
         + [대량 가져오기 서비스](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [콘텐츠 전송 도구](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [FAQ](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [문제 해결](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [소개](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [디지털 등록](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [통신](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [소개](./migration/cloud-acceleration-manager/introduction.md)
      + [준비 및 모범 사례 분석기](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [구현 단계](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [콘텐츠 전송 도구](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [코드 리팩터링 도구](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [코드 리포지토리 현대화](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher 변환기](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [인덱스 변환기](./migration/cloud-acceleration-manager/index-converter.md)
      + [에셋 워크플로 마이그레이션 도구](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Cloud Acceleration Manager 탐색](./migration/cloud-acceleration-manager/navigating.md)
      + [Cloud Acceleration Manager 사용](./migration/cloud-acceleration-manager/using.md)
+ 양식{#forms}

   + Forms as a Cloud Service 개발{#developing-for-cloud-service}
      + [시작하기](./forms/developing-for-cloud-service/getting-started.md)
      + [IntelliJ 설치](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [Git 설정](./forms/developing-for-cloud-service/setup-git.md)
      + [AEM과 IntelliJ 동기화](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [양식 작성](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [Forms Portal 구성 요소 활성화](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [Cloud Services 및 FDM 포함](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [컨텍스트 인식 클라우드 구성](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [Cloud Manager로 푸시](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [개발 환경에 배포](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [Maven 원형 업데이트](./forms/developing-for-cloud-service/updating-project-archetype.md)
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
   + AEM Forms CS에서 문서 생성{#doc-gen-formscs}
      + [소개](./forms/doc-gen-forms-cs/introduction.md)
      + [서비스 자격 증명 만들기](./forms/doc-gen-forms-cs/service-credentials.md)
      + [JWT 토큰 만들기](./forms/doc-gen-forms-cs/create-jwt.md)
      + [액세스 토큰 만들기](./forms/doc-gen-forms-cs/create-access-token.md)
      + [템플릿과 데이터 병합](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [솔루션 테스트](./forms/doc-gen-forms-cs/test.md)
      + [과제](./forms/doc-gen-forms-cs/challenge.md)
   + 배치 API를 사용하여 문서 생성{#formscs-batch-api}
      + [소개](./forms/formscs-batch-api/introduction.md)
      + [Azure 저장소 구성](./forms/formscs-batch-api/configure-azure-storage.md)
      + [USC 배치 구성 만들기](./forms/formscs-batch-api/configure-usc-batch.md)
      + [배치 구성 만들기](./forms/formscs-batch-api/create-batch-config.md)
      + [배치 실행](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Forms CS의 PDF 조작{#forms-cs-assembler}
      + [소개](./forms/forms-cs-assembler/introduction.md)
      + [서비스 자격 증명 만들기](./forms/forms-cs-assembler/service-credentials.md)
      + [JWT 토큰 만들기](./forms/forms-cs-assembler/create-jwt.md)
      + [액세스 토큰 만들기](./forms/forms-cs-assembler/create-access-token.md)
      + [PDF 파일 조합](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A 유틸리티](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [솔루션 테스트](./forms/forms-cs-assembler/test.md)
      + [과제](./forms/forms-cs-assembler/challenge.md)
   + Azure 포털 저장소{#forms-cs-azure-portal}
      + [소개](./forms/forms-cs-azure-portal/introduction.md)
      + [양식 데이터 모델 만들기](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Azure 저장소에 양식 데이터 저장](./forms/forms-cs-azure-portal/create-af.md)
      + [양식 미리 채우기](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [쿼리 제출](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 검토 작업 과정 만들기{#create-aem-workflow}
      + [워크플로우 저장소 표면화](./forms/create-aem-workflow/externalize-workflow.md)
      + [워크플로우 모델 만들기](./forms/create-aem-workflow/create-workflow.md)
      + [워크플로우 트리거](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign과 AEM Forms{#forms-and-sign}
      + [소개](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API 애플리케이션](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign Cloud 구성](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [적응형 양식 만들기](./forms/forms-and-sign/create-adaptive-form.md)
      + [채우기 및 서명 구성](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Microsoft Power와 통합 자동화{#forms-cs-and-power-automate}
      + [통합 구성](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [제출된 양식 데이터를 구문 분석합니다.](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [전자 메일 첨부 파일로 DoR 보내기](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [제출된 데이터에서 양식 첨부 파일 추출](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Microsoft Dynamics와 통합{#formscs-dynamics-crm}
      + [Dynamics 응용 프로그램 만들기](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [데이터 소스 구성](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [양식 데이터 모델 만들기](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [적응형 양식 만들기](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Salesforce와 통합{#integrate-with-salesforce}
      + [소개](./forms/integrate-with-salesforce/introduction.md)
      + [연결된 앱 만들기](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Swagger 파일 만들기](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [데이터 소스 만들기](./forms/integrate-with-salesforce/create-data-source.md)
      + [양식 데이터 모델 만들기](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [테스트 양식 제출](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [테스트 클릭 이벤트](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ asset compute 확장성{#asset-compute}
   + [개요](./asset-compute/overview.md)
   + 설정{#set-up}
      + [계정 및 서비스 프로비저닝](./asset-compute/set-up/accounts-and-services.md)
      + [로컬 개발 환경](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + 개발{#develop}
      + [asset compute 프로젝트 만들기](./asset-compute/develop/project.md)
      + [환경 변수 구성](./asset-compute/develop/environment-variables.md)
      + [manifest.html 구성](./asset-compute/develop/manifest.md)
      + [작업자 개발](./asset-compute/develop/worker.md)
      + [개발 도구 사용](./asset-compute/develop/development-tool.md)
   + 테스트 및 디버그{#test-debug}
      + [작업자 테스트](./asset-compute/test-debug/test.md)
      + [작업자 디버깅](./asset-compute/test-debug/debug.md)
   + {#deploy} 배포
      + [Adobe I/O Runtime에 배포](./asset-compute/deploy/runtime.md)
      + [AEM과 통합](./asset-compute/deploy/processing-profiles.md)
   + 고급{#advanced}
      + [메타데이터 작업자](./asset-compute/advanced/metadata.md)
   + [문제 해결](./asset-compute/troubleshooting.md)
+ 클라우드 5{#cloud-5}
   + [소개](./cloud-5/cloud5-introduction.md)
   + [시즌 1](./cloud-5/cloud5-season-1.md)
   + [시즌 2](./cloud-5/cloud5-season-2.md)
   + [AEM CDN 1부](./cloud-5/cloud5-aem-cdn-part1.md)
   + [AEM CDN Part 2](./cloud-5/cloud5-aem-cdn-part2.md)
   + [AEM 로그 파일](./cloud-5/cloud5-aem-log-files.md)
   + [로그인 토큰](./cloud-5/cloud5-getting-login-token-integrations.md)
   + [Cloud Dispatcher](./cloud-5/cloud5-aem-dispatcher-cloud.md)
   + [마이그레이션 1](./cloud-5/cloud5-aem-content-migration-part-1.md)
   + [마이그레이션 2](./cloud-5/cloud5-aem-content-migration-part-2.md)
   + [Dispatcher 유효성 검사기](./cloud-5/cloud5-aem-dispatcher-validator.md)
   + [검색 및 색인 지정](./cloud-5/cloud5-aem-search-and-indexing.md)
   + [Adobe 앱 빌더](./cloud-5/cloud5-adobe-app-builder.md)
   + 시즌 2{#season-2}
      + [조각](./cloud-5/season-2/cloud5-experience-v-content-fragments.md)
      + [Repo Modernizer](./cloud-5/season-2/cloud5-repo-modernizer.md)
      + [Admin Console](./cloud-5/season-2/cloud5-admin-console.md)
      + [포인트](./cloud-5/season-2/cloud5-repoinit.md)
      + [Sling 작업 스케줄러](./cloud-5/season-2/cloud5-sling-job-scheduler.md)
      + [캐시 수정](./cloud-5/season-2/cloud5-fix-your-cache.md)
      + [재작성 수정](./cloud-5/season-2/cloud5-fix-your-rewrites.md)
      + [Cloud Manager - Experience Audit](./cloud-5/season-2/cloud5-mocm-experience-audit.md)
      + [Cloud Manager - 단위 테스트](./cloud-5/season-2/cloud5-mocm-unit-tests.md)
      + [Cloud Manager - 기능 테스트](./cloud-5/season-2/cloud5-mocm-functional-tests.md)
+ [AEM Experts Series](./aem-experts-series.md)
+ 여러 단계 Tutorials{#multi-step-tutorials}
   + [AEM Sites 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ko-KR)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA 편집기(반응)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [AEM Sites 및 Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [토큰 기반 인증](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
