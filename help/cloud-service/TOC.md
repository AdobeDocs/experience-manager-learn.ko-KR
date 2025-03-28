---
user-guide-title: Adobe Experience Manager as a Cloud Service 튜토리얼
user-guide-description: Adobe Experience Manager as a Cloud Service를 위한 튜토리얼 모음입니다.
breadcrumb-title: AEM as a Cloud Service 튜토리얼
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Experience Manager as a Cloud Service
team: TM
source-git-commit: 52aad0b0e568ff7e4acd23742fc70f10b1dd14ee
workflow-type: tm+mt
source-wordcount: '1383'
ht-degree: 15%

---


# Adobe Experience Manager as a Cloud Service 튜토리얼 {#cloud-service}

+ [개요](./overview.md)
+ AEM 평가판 {#aem-trials}
   + [이미지](./aem-trials/images.md)
+ 재생 목록{#playlists}
   + [AEM 개발](./playlists/development.md)
+ AEM as a Cloud Service 소개{#introduction}
   + [AEM as a Cloud Service란?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [아키텍처](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 전략 및 사고 리더십{#strategy}
      + [Experience Manager - 거버넌스 및 인력 모델 및 원형](./introduction/experience-manager-governance-and-staffing-models.md)
+ Experience Cloud 통합{#integrations}
   + [통합](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ 기본 기술 {#underlying-technology}
   + [AEM 아키텍처](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java 콘텐츠 저장소](./underlying-technology/introduction-jcr.md)
   + [슬링](./underlying-technology/introduction-sling.md)
   + [Author 및 Publish 서비스](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [AEM Assets Sidekick 플러그인](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [프로그램](./cloud-manager/programs.md)
   + [환경](./cloud-manager/environments.md)
   + [GitHub 저장소 사용](./cloud-manager/byogithub.md)
   + [CI/CD 프로덕션 파이프라인](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD 비프로덕션 파이프라인](./cloud-manager/cicd-non-production-pipeline.md)
   + [활동](./cloud-manager/activity.md)
   + [사용자 지정 도메인 이름](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names){target=_blank}
   + [콘텐츠 복원](./cloud-manager/content-restore.md)
   + 개발 작업 {#devops}
      + [코드 배포](./cloud-manager/devops/deploy-code.md)
      + [프로젝트 병합](./cloud-manager/devops/merge-projects.md)
      + [파이프라인 구성](./cloud-manager/devops/configure-pipelines.md)
      + [지속적인 통합](./cloud-manager/devops/continuous-integration.md)
      + [테스트 결과 분석](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher 구성](./cloud-manager/devops/dispatcher-configurations.md)
      + [CDN 로그 분석](./cloud-manager/devops/cdn-log-analysis.md)
+ 로컬 개발 환경 설정 {#local-development-environment-set-up}
   + [개요](./local-development-environment/overview.md)
   + [개발 도구](./local-development-environment/development-tools.md)
   + [로컬 AEM SDK](./local-development-environment/aem-runtime.md)
   + [로컬 Dispatcher 도구](./local-development-environment/dispatcher-tools.md)
+ 개발{#developing}
   + 확장성{#extensibility}
      + App Builder{#app-builder}
         + [JWT 액세스 토큰 생성](./developing/extensibility/app-builder/jwt-auth.md)
         + [서버 간 액세스 토큰 생성](./developing/extensibility/app-builder/server-to-server-auth.md)
         + [Github 웹후크 확인](./developing/extensibility/app-builder/github-webhook-verification.md)
      + UI 확장성{#ui}
         + [개요](./developing/extensibility/ui/overview.md)
         + [Adobe Developer Console 프로젝트](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [앱 초기화](./developing/extensibility/ui/app-initialization.md)
         + [확장 등록](./developing/extensibility/ui/extension-registration.md)
         + [양식](./developing/extensibility/ui/modal.md)
         + [Adobe I/O Runtime 작업](./developing/extensibility/ui/runtime-action.md)
         + [확인](./developing/extensibility/ui/verify.md)
         + [배포](./developing/extensibility/ui/deploy.md)
         + 콘텐츠 조각{#content-fragments}
            + [개요](./developing/extensibility/ui/content-fragments/overview.md)
            + 예{#examples}
               + [AI 이미지 생성](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [벌크 속성 업데이트](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [사용자 정의 그리드 열](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [XML로 내보내기](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [RTE 도구 모음 단추](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [RTE 위젯](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [RTE 배지](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
               + [사용자 정의 필드](./developing/extensibility/ui/content-fragments/examples/editor-custom-field.md)
   + 개발 기본 사항{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [로컬 개발 환경](./developing/basics/local-development-environment.md)
      + [AEM Project Archetype](./developing/basics/aem-project-archetype.md)
      + [AEM 프로젝트 구조](./developing/basics/project-structure.md)
      + [변경 가능한 콘텐츠 및 변경 불가능한 콘텐츠](./developing/basics/mutable-immutable.md)
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
      + [페이지 변형 캐싱](./developing/advanced/variant-caching.md)
      + [CSRF 보호](./developing/advanced/csrf-protection.md)
      + [사용자 정의 네임스페이스](./developing/advanced/custom-namespaces.md)
      + [HTL에서 Sling 모델 매개 변수화](./developing/advanced/sling-model-parameters.md)
      + [비밀](./developing/advanced/secrets.md)
      + [서비스 사용자](./developing/advanced/service-users.md)
      + [웹에 최적화된 이미지 API](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [AEM 작성자의 리더 인스턴스에서 작업 실행](./developing/advanced/run-job-on-leader-instance-in-aem-author.md)
   + 신속한 개발 환경{#rde}
      + [개요](./developing/rde/overview.md)
      + [설정 방법](./developing/rde/how-to-setup.md)
      + [사용 방법](./developing/rde/how-to-use.md)
      + [개발 수명 주기](./developing/rde/development-life-cycle.md)
   + 유니버설 편집기{#universal-editor}
      + React 앱 편집{#react-app-editing}
         + [개요](./developing/universal-editor/react-app/overview.md)
         + [로컬 개발 설정](./developing/universal-editor/react-app/local-development-setup.md)
         + [Instrument React 앱](./developing/universal-editor/react-app/instrument-to-edit-content.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
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
      + [빌드 및 배포](./debugging/cloud-service/build-and-deployment.md)
      + [개발자 콘솔](./debugging/cloud-service/developer-console.md)
      + [저장소 브라우저](./debugging/cloud-service/repository-browser.md)
      + 위험{#risks}
         + [순회 경고](./debugging/cloud-service/risks/traversals.md)
+ AEM API{#aem-apis}
   + [개요](./apis/overview.md)
   + OpenAPI{#openapis}
      + [개요](./apis/openapis/overview.md)
      + [설정 방법](./apis/openapis/setup.md)
      + [서버 간 인증](./apis/openapis/use-cases/invoke-api-using-oauth-s2s.md)
      + [사용자 인증(웹 앱)](./apis/openapis/use-cases/invoke-api-using-oauth-web-app.md)
      + [사용자 인증(SPA)](./apis/openapis/use-cases/invoke-api-using-oauth-single-page-app.md)
      + {#how-to}하는 방법
         + [자격 증명 및 제품 프로필 관리](./apis/openapis/how-to/credentials-and-product-profile-management.md)
         + [권한 관리](./apis/openapis/how-to/services-user-group-permission-management.md)
+ 컨텐츠 전달{#content-delivery}
   + [사용자 정의 도메인 이름](./content-delivery/custom-domain-names.md)
   + [Adobe 관리 CDN을 사용한 사용자 정의 도메인 이름](./content-delivery/custom-domain-name-with-adobe-managed-cdn.md)
   + [고객 CDN을 사용한 사용자 정의 도메인 이름](./content-delivery/custom-domain-names-with-customer-managed-cdn.md)
   + [캐싱](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/overview){target=_blank}
   + [Adobe CDN - 캐싱 이상](./content-delivery/adobe-cdn-beyond-caching.md)
   + [사용자 정의 오류 페이지](./content-delivery/custom-error-pages.md)
   + [URL 리디렉션](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ 캐싱{#caching}
   + [개요](./caching/overview.md)
   + [AEM Publish 서비스](./caching/publish.md)
   + [AEM Author 서비스](./caching/author.md)
   + [CDN 캐시 적중률 분석](./caching/cdn-cache-hit-ratio-analysis.md)
   + {#how-to}하는 방법
      + [캐싱 활성화](./caching/how-to/enable-caching.md)
      + [캐싱 비활성화](./caching/how-to/disable-caching.md)
      + [캐시 제거](./caching/how-to/purge-cache.md)
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
   + [유연한 포트 전송](./networking/flexible-port-egress.md)
   + [전용 송신 IP 주소](./networking/dedicated-egress-ip-address.md)
   + [가상 비공개 네트워크](./networking/vpn.md)
   + 코드 예{#examples}
      + [유연한 포트 이그레스용 비표준 포트에서의 HTTP/HTTPS](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [전용 이그레스 IP 주소/VPN용 HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [DataSourcePool을 사용한 SQL 연결](./networking/examples/sql-datasourcepool.md)
      + [Java SQL API를 사용한 SQL 연결](./networking/examples/sql-java-apis.md)
      + [이메일 서비스](./networking/examples/email-service.md)
+ 보안 {#security}
   + [트래픽 필터 규칙을 사용한 DoS/DDoS 공격 차단](./security/blocking-dos-attack-using-traffic-filter-rules.md)
   + WAF 규칙을 포함한 트래픽 필터 규칙{#traffic-filter-and-waf-rules}
      + [개요](./security/traffic-filter-rules/overview.md)
      + [설정 방법](./security/traffic-filter-rules/how-to-setup.md)
      + [예제 및 결과 분석](./security/traffic-filter-rules/examples-and-analysis.md)
      + [모범 사례](./security/traffic-filter-rules/best-practices.md)
+ AEM 이벤트{#aem-eventing}
   + [개요](./eventing/overview.md)
   + 예{#examples}
      + [Webhook - AEM 이벤트 수신](./eventing/examples/webhook.md)
      + [저널링 - AEM 이벤트 로드](./eventing/examples/journaling.md)
      + [Adobe I/O Runtime 작업 - AEM 이벤트 수신](./eventing/examples/runtime-action.md)
      + [Adobe I/O Runtime 작업 - AEM 이벤트 처리](./eventing/examples/event-processing-using-runtime-action.md)
      + [AEM Assets 이벤트 - PIM 통합](./eventing/examples/assets-pim-integration.md)
+ 마이그레이션 {#migration}
   + [콘텐츠 전송 도구](./migration/content-transfer-tool.md)
   + [자산 일괄 가져오기](./migration/bulk-import.md)
   + AEM as a Cloud Service로 이동 {#moving-to-aem-as-a-cloud-service}
      + [소개](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [온보딩](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA 및 CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM 현대화 도구](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [저장소 현대화](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Asset Compute Microservices](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [검색 및 색인 지정](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 콘텐츠 마이그레이션 {#content-migration}
         + [일괄 가져오기 서비스](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [콘텐츠 전송 도구](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [FAQ](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [문제 해결](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [소개](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [디지털 등록](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [통신](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [소개](./migration/cloud-acceleration-manager/introduction.md)
      + [준비 및 Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [구현 단계](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [코드 리팩터링 도구](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [코드 리포지토리 현대화 도구](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher 변환기](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [인덱스 변환기](./migration/cloud-acceleration-manager/index-converter.md)
      + [자산 워크플로 마이그레이션 도구](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Cloud Acceleration Manager 탐색](./migration/cloud-acceleration-manager/navigating.md)
      + [Cloud Acceleration Manager 사용](./migration/cloud-acceleration-manager/using.md)
+ [콘텐츠 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ 양식{#forms}
   + Forms as a Cloud Service용 개발{#developing-for-cloud-service}
      + [1 - 시작](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - IntelliJ 설치](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - Git 설정](./forms/developing-for-cloud-service/setup-git.md)
      + [4 - IntelliJ와 AEM 동기화](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 - 양식 작성](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - 사용자 지정 제출 핸들러](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - 리소스 유형을 사용하여 서블릿 등록](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 - Forms 포털 구성 요소 활성화](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 - Cloud Services 및 FDM 포함](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 - 컨텍스트 인식 클라우드 구성](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 - Cloud Manager으로 푸시](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 - 개발 환경에 배포](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 - Maven Archetype 업데이트](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + 적응형 양식 만들기{#create-first-af}
      + [소개](./forms/create-first-af/introduction.md)
      + [테마 만들기](./forms/create-first-af/create-theme.md)
      + [템플릿 만들기](./forms/create-first-af/create-template.md)
      + [조각 만들기](./forms/create-first-af/create-fragments.md)
      + [양식 만들기](./forms/create-first-af/create-af.md)
      + [루트 패널 구성](./forms/create-first-af/configure-root-panel.md)
      + [사용자 패널 구성](./forms/create-first-af/configure-people-panel.md)
      + [소득 패널 구성](./forms/create-first-af/configure-income-panel.md)
      + [에셋 패널 구성](./forms/create-first-af/configure-assets-panel.md)
      + [시작 패널 구성](./forms/create-first-af/configure-start-panel.md)
      + [도구 모음 추가 및 구성](./forms/create-first-af/add-configure-toolbar.md)
   + Headless 양식 {#custom-submit-headless-forms}을(를) 사용한 사용자 정의 제출 서비스
      + [1 - 소개](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - 사용자 정의 제출 서비스 만들기](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - 응답 표시](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + 주소 차단 구성 요소 만들기{#create-address-block}
      + [1 - 소개](./forms/create-address-block-component/introduction.md)
      + [2 - 설정](./forms/create-address-block-component/set-up.md)
      + [3 - 구성 요소 만들기](./forms/create-address-block-component/creating-address-component.md)
      + [4 - 구성 요소 배포](./forms/create-address-block-component/deploy-your-project.md)
   + 클릭 가능한 이미지 구성 요소 만들기{#clickable-image-component}
      + [1 - 소개](./forms/clickable-image-component/introduction.md)
      + [2 - 구성 요소 만들기](./forms/clickable-image-component/create-component.md)
      + [3 - 클릭 이벤트 처리](./forms/clickable-image-component/handle-click-event.md)
   + AEM Forms 및 Analytics{#forms-and-analytics}
      + [소개](./forms/form-data-analytics/introduction.md)
      + [데이터 요소 만들기](./forms/form-data-analytics/data-elements.md)
      + [규칙 만들기](./forms/form-data-analytics/rules.md)
      + [솔루션 테스트](./forms/form-data-analytics/test.md)
   + 국가 드롭다운 구성 요소 만들기{#countries-drop-down}
      + [소개](./forms/countries-drop-down/introduction.md)
      + [구성 요소 만들기](./forms/countries-drop-down/component.md)
      + [대화 상자 만들기](./forms/countries-drop-down/dialog.md)
      + [Sling 모델 만들기](./forms/countries-drop-down/slingmodel.md)
      + [빌드 및 테스트](./forms/countries-drop-down/build.md)
   + 단추 변형 만들기{#style-system}
      + [소개](./forms/style-system/introduction.md)
      + [정책 정의](./forms/style-system/style-policy.md)
      + [변형 정의](./forms/style-system/create-variations.md)
      + [테스트 변형](./forms/style-system/build.md)
   + 세로 탭 사용{#using-vertical-tabs}
      + [1. 소개](./forms/using-vertical-tabs/introduction.md)
      + [2. 양식 만들기](./forms/using-vertical-tabs/create-af.md)
      + [3. 탐색](./forms/using-vertical-tabs/navigation.md)
      + [4. 아이콘 추가](./forms/using-vertical-tabs/icons.md)
   + 출력 및 양식 서비스 사용{#forms-cs-output-and-forms-service}
      + [PDF 생성](./forms/forms-cs-output-and-forms-service/outputservice.md)
   + AEM Forms CS{#doc-gen-formscs}에서 문서 생성
      + [소개](./forms/doc-gen-forms-cs/introduction.md)
      + [서비스 자격 증명 만들기](./forms/doc-gen-forms-cs/service-credentials.md)
      + [JWT 토큰 만들기](./forms/doc-gen-forms-cs/create-jwt.md)
      + [액세스 토큰 만들기](./forms/doc-gen-forms-cs/create-access-token.md)
      + [템플릿과 데이터 병합](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [솔루션 테스트](./forms/doc-gen-forms-cs/test.md)
      + [과제](./forms/doc-gen-forms-cs/challenge.md)
   + Forms 문서 서비스 API 사용{#forms-document-services-api}
      + [소개](./forms/forms-document-services/introduction.md)
      + [OpenAPI 구성](./forms/forms-document-services/using-open-api.md)
      + [액세스 토큰 생성](./forms/forms-document-services/generate-access-token.md)
      + [사용 권한 적용](./forms/forms-document-services/make-api-calls.md)
      + [샘플 코드](./forms/forms-document-services/sample-project.md)
   + 일괄 처리 API를 사용하여 문서 생성{#formscs-batch-api}
      + [소개](./forms/formscs-batch-api/introduction.md)
      + [Azure 스토리지 구성](./forms/formscs-batch-api/configure-azure-storage.md)
      + [USC 일괄 처리 구성 만들기](./forms/formscs-batch-api/configure-usc-batch.md)
      + [일괄 구성 만들기](./forms/formscs-batch-api/create-batch-config.md)
      + [일괄 처리 실행](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Forms CS{#forms-cs-assembler}의 PDF 조작
      + [소개](./forms/forms-cs-assembler/introduction.md)
      + [서비스 자격 증명 만들기](./forms/forms-cs-assembler/service-credentials.md)
      + [JWT 토큰 만들기](./forms/forms-cs-assembler/create-jwt.md)
      + [액세스 토큰 만들기](./forms/forms-cs-assembler/create-access-token.md)
      + [PDF 파일 조합](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A 유틸리티](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [솔루션 테스트](./forms/forms-cs-assembler/test.md)
      + [과제](./forms/forms-cs-assembler/challenge.md)
   + Marketo과 통합{#froms-cs-with-marketo}
      + [소개](./forms/forms-cs-with-marketo/part1.md)
      + [데이터 Source 만들기](./forms/forms-cs-with-marketo/part2.md)
      + [Forms 데이터 모델 만들기](./forms/forms-cs-with-marketo/part3.md)
   + Blob 인덱스 태그를 사용하여 양식 제출을 저장{#store-submiited-data-with-metadata-tags}
      + [소개](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [선택 그룹 구성 요소 확장](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [OSGi 구성 만들기](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [색인 태그 만들기](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [사용자 정의 제출 만들기](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + 핵심 구성 요소 기반 양식 {#prefill-core-component-based-form} 미리 채우기
      + [소개](./forms/prefill-core-component-form/introduction.md)
      + [미리 채우기 서비스 쓰기](./forms/prefill-core-component-form/pre-fill-service.md)
      + [솔루션 테스트](./forms/prefill-core-component-form/test-solution.md)
   + Azure 포털 저장소{#forms-cs-azure-portal}
      + [소개](./forms/forms-cs-azure-portal/introduction.md)
      + [Forms 데이터 모델 만들기](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Azure Storage에 양식 데이터 저장](./forms/forms-cs-azure-portal/create-af.md)
      + [미리 채우기 양식](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [쿼리 제출](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 양식 채우기 저장 및 다시 시작{#prefill-azure-storage}
      + [1- 소개](./forms/prefill-azure-storage/introduction.md)
      + [2- 페이지 구성 요소 만들기](./forms/prefill-azure-storage/page-component.md)
      + [3- 적응형 양식 템플릿 만들기](./forms/prefill-azure-storage/associate-page-component.md)
      + [4- Azure 스토리지 통합 만들기](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - SendGrid 통합 만들기](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - 적응형 양식 만들기](./forms/prefill-azure-storage/create-af.md)
      + [7 - 샘플 자산 배포](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + 검토 워크플로우 만들기{#create-aem-workflow}
      + [워크플로 스토리지 외부화](./forms/create-aem-workflow/externalize-workflow.md)
      + [워크플로우 모델 만들기](./forms/create-aem-workflow/create-workflow.md)
      + [워크플로우 트리거](./forms/create-aem-workflow/configure-af.md)
   + AEM Forms{#forms-and-sign}이(가) 있는 Acrobat Sign
      + [소개](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API 애플리케이션](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign 클라우드 구성](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [적응형 양식 만들기](./forms/forms-and-sign/create-adaptive-form.md)
      + [채우기 및 서명에 대한 구성](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Microsoft Power Automate와 통합{#forms-cs-and-power-automate}
      + [통합 구성](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [제출된 양식 데이터 구문 분석](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [DoR을 전자 메일 첨부 파일로 보내기](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [제출된 데이터에서 양식 첨부 파일 추출](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Microsoft Dynamics과 통합{#formscs-dynamics-crm}
      + [Dynamics 응용 프로그램 만들기](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Data Source 구성](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Forms 데이터 모델 만들기](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [적응형 양식 만들기](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Salesforce과 통합{#integrate-with-salesforce}
      + [소개](./forms/integrate-with-salesforce/introduction.md)
      + [연결된 앱 만들기](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Swagger 파일 만들기](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [데이터 소스 만들기](./forms/integrate-with-salesforce/create-data-source.md)
      + [양식 데이터 모델 만들기](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [테스트 양식 제출](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [클릭 이벤트 테스트](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + 하나의 드라이브 및 sharepoint{#one-drive}에 양식 제출을 저장합니다.
      + [하나의 드라이브에 양식 데이터 저장](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [Sharepoint에 양식 데이터 저장](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [SharePoint 목록의 데이터로 양식 미리 채우기](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [워크플로우를 사용하여 SharePoint 목록에 데이터 삽입](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Asset Compute 확장성{#asset-compute}
   + [개요](./asset-compute/overview.md)
   + {#set-up} 설정
      + [계정 및 서비스 프로비저닝](./asset-compute/set-up/accounts-and-services.md)
      + [로컬 개발 환경](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + {#develop} 개발
      + [Asset Compute 프로젝트 만들기](./asset-compute/develop/project.md)
      + [환경 변수 구성](./asset-compute/develop/environment-variables.md)
      + [manifest.yml 구성](./asset-compute/develop/manifest.md)
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

+ 여러 단계 자습서{#multi-step-tutorials}
   + [AEM Sites 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ko-KR){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=ko-KR){target=_blank}
   + [SPA 편집기(반응)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites 및 Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [토큰 기반 인증](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ 전문가 리소스 {#expert-resources}
   + AEM 챔피언 {#aem-champions}
      + [Cloud Manager 온보딩 플레이북](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Cloud Manager 환경 유형](./expert-resources/aem-champions/environment-types.md)
      + [CLOUD MANAGER UI](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM 전문가 시리즈](./expert-resources/expert-series/aem-experts-series.md)
   + 클라우드 5{#cloud-5}
      + [소개](./expert-resources/cloud-5/cloud5-introduction.md)
      + [시즌](./expert-resources/cloud-5/cloud5-season-4.md)
      + [시즌](./expert-resources/cloud-5/cloud5-season-3.md)
      + [시즌](./expert-resources/cloud-5/cloud5-season-2.md)
      + [시즌](./expert-resources/cloud-5/cloud5-season-1.md)
      + [AEM CDN 1부](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN 2부](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [AEM 로그 파일](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [로그인 토큰](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Cloud Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [마이그레이션 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Dispatcher 검사기](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [검색 및 색인 지정](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + 시즌 2{#season-2}
         + [조각](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Repo Modernizer](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Sling 작업 스케줄러](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [캐시 수정](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [리쓰기 수정](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager - 경험 감사](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager - 단위 테스트](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager - 기능 테스트](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + 시즌 3{#season-3}
         + [타사 검색](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
         + [Edge 노동자](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [Edge Delivery Services에서 이벤트 게시, 게시 취소](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [쿼리 인덱스 및 Excel 공식](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [고유한 Cloudflare CDN 가져오기](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [AEM Assets 통합](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
         + [AEM Sites용 생성 AI](./expert-resources/cloud-5/season-3/cloud5-generative-ai-for-aem-sites.md)
         + [유니버설 편집기 살펴보기](./expert-resources/cloud-5/season-3/cloud5-exploring-universal-editor.md)
         + [사이트 가져오기](./expert-resources/cloud-5/season-3/cloud5-import-sites-to-edge-delivery-services.md)
         + [관리 API 사용](./expert-resources/cloud-5/season-3/cloud5-using-admin-api.md)
         + [등대 점수 최적화 - Part1](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part1.md)
         + [등대 점수 최적화 - Part2](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part2.md)
         + [등대 점수 최적화 - Part3](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part3.md)
      + 시즌 4{#season-4}
         + [모범 사례](./expert-resources/cloud-5/season-4/cloud5-edge-delivery-services-best-practices.md)
         + [검색 최적화](./expert-resources/cloud-5/season-4/cloud5-search-optimization.md)
         + [Google 맵](./expert-resources/cloud-5/season-4/cloud5-google-maps.md)
