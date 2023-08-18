---
title: ModSecurity를 사용하여 DoS 공격으로부터 AEM 사이트 보호
description: ModSecurity를 사용하여 OWASP CRS(ModSecurity Core Rule Set)를 사용하여 DoS(Denial of Service) 공격으로부터 사이트를 보호하는 방법을 알아봅니다.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
kt: 10385
thumbnail: KT-10385.png
doc-type: article
last-substantial-update: 2023-08-18T00:00:00Z
source-git-commit: c6f954968fdc43ea9070df52a4709da2ed04cacc
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 1%

---


# ModSecurity를 사용하여 AEM 사이트를 DoS 공격으로부터 보호

ModSecurity를 사용하여 DoS(서비스 거부) 공격으로부터 사이트를 보호하는 방법에 대해 알아봅니다. **OWASP ModSecurity 코어 규칙 세트(CRS)** Adobe Experience Manager (AEM) Publish Dispatcher에서.

>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## 개요

다음 [Open Web Application Security Project® (OWASP)](https://owasp.org/) foundation에서는 [**OWASP 상위 10**](https://owasp.org/www-project-top-ten/) 웹 애플리케이션에 가장 중요한 10가지 보안 문제를 요약한 것입니다.

ModSecurity는 웹 애플리케이션에 대한 다양한 공격으로부터 보호하는 오픈 소스 크로스 플랫폼 솔루션입니다. 또한 HTTP 트래픽 모니터링, 로깅 및 실시간 분석을 허용합니다.

OWSAP®는 [OWASP® ModSecurity 코어 규칙 세트(CRS)](https://github.com/coreruleset/coreruleset). CRS는 일반적인 일련의 집합이다 **공격 탐지** ModSecurity에 사용할 규칙입니다. 따라서 CRS는 최소한의 거짓 경고로 OWASP Top Ten을 포함한 광범위한 공격으로부터 웹 애플리케이션을 보호하는 것을 목표로 합니다.

이 튜토리얼에서는 **보호 기능** 잠재적인 DoS 공격으로부터 사이트를 보호하기 위한 CRS 규칙.

>[!TIP]
>
>AEM의 as a Cloud Service을 참고하십시오. [관리 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) 대부분의 고객 성능 및 보안 요구 사항을 충족합니다. 그러나 ModSecurity는 추가 보안 계층을 제공하고 고객별 규칙 및 구성을 허용합니다.

## Dispatcher 프로젝트 모듈에 CRS 추가

1. 다운로드 및 추출 [최신 OWASP ModSecurity 핵심 규칙 세트](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. 만들기 `modsec/crs` 다음 범위 내의 폴더 `dispatcher/src/conf.d/` AEM 프로젝트의 코드에서. 예를 들어 의 로컬 복사본에서 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd).

   ![AEM 프로젝트 코드 내 CRS 폴더 - ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. 다음을 복사합니다. `coreruleset-X.Y.Z/rules` 다운로드한 CRS 릴리스 패키지의 폴더를 `dispatcher/src/conf.d/modsec/crs` 폴더를 삭제합니다.
1. 다음을 복사합니다. `coreruleset-X.Y.Z/crs-setup.conf.example` 다운로드한 CRS 릴리스 패키지의 파일을에 `dispatcher/src/conf.d/modsec/crs` 폴더를 만들고 이름을 로 변경합니다. `crs-setup.conf`.
1. 에서 복사된 모든 CRS 규칙 비활성화 `dispatcher/src/conf.d/modsec/crs/rules` 이름을 로 변경하여 `XXXX-XXX-XXX.conf.disabled`. 아래 명령을 사용하여 모든 파일의 이름을 한 번에 바꿀 수 있습니다.

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   WKND 프로젝트 코드에서 이름이 변경된 CRS 규칙 및 구성 파일을 참조하십시오.

   ![AEM 프로젝트 코드 내에서 비활성화된 CRS 규칙 - ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## DoS(서비스 거부) 보호 규칙 활성화 및 구성

서비스 거부(DoS) 보호 규칙을 활성화하고 구성하려면 아래 단계를 따르십시오.

1. 이름을 변경하여 DoS 보호 규칙을 활성화합니다. `REQUEST-912-DOS-PROTECTION.conf.disabled` 끝 `REQUEST-912-DOS-PROTECTION.conf` (또는 제거 `.disabled` (rulename 확장에서) `dispatcher/src/conf.d/modsec/crs/rules` 폴더를 삭제합니다.
1. 다음을 정의하여 규칙 구성  **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT** 변수를 채우는 방법에 따라 페이지를 순서대로 표시합니다.
   1. 만들기 `crs-setup.custom.conf` 다음 내에 있는 파일: `dispatcher/src/conf.d/modsec/crs` 폴더를 삭제합니다.
   1. 아래 규칙 코드 조각을 새로 만든 파일에 추가합니다.

   ```
   # The Denial of Service (DoS) protection against clients making requests too quickly.
   # When a client is making more than 25 requests (excluding static files) within
   # 60 seconds, this is considered a 'burst'. After two bursts, the client is
   # blocked for 600 seconds.
   SecAction \
       "id:900700,\
       phase:1,\
       nolog,\
       pass,\
       t:none,\
       setvar:'tx.dos_burst_time_slice=60',\
       setvar:'tx.dos_counter_threshold=25',\
       setvar:'tx.dos_block_timeout=600'"    
   ```

이 예제 규칙 구성에서 **DOS_COUNTER_THRESHOLD** 은(는) 25이고, **DOS_BURST_TIME_SLICE** 은(는) 60초이며, **DOS_BLOCK_TIMEOUT** 시간 제한은 600초입니다. 이 구성은 정적 파일을 제외하고 60초 이내에 두 번 이상 발생한 25개 요청을 식별하여 DoS 공격으로 자격을 얻으므로 요청 클라이언트가 600초(또는 10분) 동안 차단됩니다.

>[!WARNING]
>
>요구 사항에 적합한 값을 정의하려면 웹 보안 팀과 공동 작업하십시오.

## CRS 초기화

CRS를 초기화하려면 일반적인 긍정 오류(false positive)를 제거하고 사이트에 대한 로컬 예외를 추가하려면 아래 단계를 따르십시오.

1. CRS를 초기화하려면 `.disabled` 다음에서 **REQUEST-901-INITIALIZATION** 파일. 즉, `REQUEST-901-INITIALIZATION.conf.disabled` 파일 위치: `REQUEST-901-INITIALIZATION.conf`.
1. 로컬 IP(127.0.0.1) ping과 같은 일반적인 긍정 오류(false positive)를 제거하려면 다음을 제거합니다. `.disabled` 다음에서 **REQUEST-905-COMMON-EXCEPTION** 파일.
1. AEM 플랫폼 또는 사이트별 경로와 같은 로컬 예외를 추가하려면 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` 끝 `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. 새로 이름이 변경된 파일에 AEM 플랫폼별 경로 예외를 추가합니다.

   ```
   ########################################################
   # AEM as a Cloud Service exclusions                    #
   ########################################################
   # Ignoring AEM-CS Specific internal and reserved paths
   
   SecRule REQUEST_URI "@beginsWith /systemready" \
       "id:1010,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"    
   
   SecRule REQUEST_URI "@beginsWith /system/probes" \
       "id:1011,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   SecRule REQUEST_URI "@beginsWith /gitinit-status" \
       "id:1012,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   ########################################################
   # ADD YOUR SITE related exclusions                     #
   ########################################################
   ...
   ```

1. 또한 `.disabled` 출처: **REQUEST-910-IP-REFERENTIAL.conf.disabled** IP 신뢰도 블록 확인 및 `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` 예외 항목 점수 확인의 경우.

>[!TIP]
>
>AEM 6.5에서 를 구성할 때 위의 경로를 AEM의 상태를 확인하는 각 AMS 또는 온프레미스 경로(즉, 하트비트 경로)로 바꾸십시오.

## ModSecurity Apache 구성 추가

ModSecurity(즉, `mod_security` Apache 모듈)에서 아래 단계를 수행합니다.

1. 만들기 `modsecurity.conf` 위치: `dispatcher/src/conf.d/modsec/modsecurity.conf` (아래 주요 구성 포함)

   ```
   # Include the baseline crs setup
   Include conf.d/modsec/crs/crs-setup.conf
   
   # Include your customizations to crs setup if exist
   IncludeOptional conf.d/modsec/crs/crs-setup.custom.conf
   
   # Select all available CRS rules:
   #Include conf.d/modsec/crs/rules/*.conf
   
   # Or alternatively list only specific ones you want to enable e.g.
   Include conf.d/modsec/crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   Include conf.d/modsec/crs/rules/REQUEST-901-INITIALIZATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-905-COMMON-EXCEPTIONS.conf
   Include conf.d/modsec/crs/rules/REQUEST-910-IP-REPUTATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf
   Include conf.d/modsec/crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
   
   # Start initially with engine off, then switch to detection and observe, and when sure enable engine actions
   #SecRuleEngine Off
   #SecRuleEngine DetectionOnly
   SecRuleEngine On
   
   # Remember to use relative path for logs:
   SecDebugLog logs/httpd_mod_security_debug.log
   
   # Start with low debug level
   SecDebugLogLevel 0
   #SecDebugLogLevel 1
   
   # Start without auditing
   SecAuditEngine Off
   #SecAuditEngine RelevantOnly
   #SecAuditEngine On
   
   # Tune audit accordingly:
   SecAuditLogRelevantStatus "^(?:5|4(?!04))"
   SecAuditLogParts ABIJDEFHZ
   SecAuditLogType Serial
   
   # Remember to use relative path for logs:
   SecAuditLog logs/httpd_mod_security_audit.log
   
   # You might still use /tmp for temporary/work files:
   SecTmpDir /tmp
   SecDataDir /tmp
   ```

1. 원하는 을 선택합니다 `.vhost` AEM 프로젝트의 Dispatcher 모듈에서 `dispatcher/src/conf.d/available_vhosts`, 예: `wknd.vhost`을(를) 클릭하고 바깥쪽에 아래 항목을 추가합니다. `<VirtualHost>` 차단합니다.

   ```
   # Enable the ModSecurity and OWASP CRS
   <IfModule mod_security2.c>
       Include conf.d/modsec/modsecurity.conf
   </IfModule>
   
   ...
   
   <VirtualHost *:80>
       ServerName    "publish"
       ...
   </VirtualHost>
   ```

위의 모든 항목 _ModSecurity CRS_ 및 _보호 기능_ 구성은 AEM WKND Sites 프로젝트의 [tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) 리뷰를 위한 분기.

### Dispatcher 구성 유효성 검사

AEM을 as a Cloud Service으로 사용할 때 _Dispatcher 구성_ 변경 내용을 적용하려면 다음을 사용하여 로컬에서 유효성을 검사하는 것이 좋습니다. `validate` 스크립트 [AEM SDK의 Dispatcher 도구](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## 배포

Cloud Manager를 사용하여 로컬에서 검증된 Dispatcher 구성 배포 [웹 계층](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#web-tier-config) 또는 [전체 스택](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#full-stack-code) 파이프라인. 다음을 사용할 수도 있습니다 [신속한 개발 환경](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) 더 빠른 처리 시간.

## 확인

DoS 보호를 확인하기 위해 이 예제에서는 60초 이내에 50개 이상의 요청(25개 요청 임계값과 2회 발생 횟수)을 전송해 보겠습니다. 그러나 이러한 요청은 AEM as a Cloud Service으로 전달되어야 합니다 [기본](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) 또는 모두 [기타 CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?#point-to-point-CDN) 웹 사이트를 탐색합니다.

CDN 패스스루를 수행하는 한 가지 기술은 가 있는 쿼리 매개 변수를 추가하는 것입니다. **각 사이트 페이지 요청에 대한 새 임의 값**.

짧은 기간(예: 60초) 내에 더 많은 요청(50초 이상)을 트리거하려면 Apache가 [JMeter](https://jmeter.apache.org/) 또는 [벤치마크 또는 ab 도구](https://httpd.apache.org/docs/2.4/programs/ab.html) 를 사용할 수 있습니다.

### JMeter 스크립트를 사용하여 DoS 공격 시뮬레이션

JMeter를 사용하여 DoS 공격을 시뮬레이션하려면 아래 단계를 따르십시오.

1. [Apache JMeter 다운로드](https://jmeter.apache.org/download_jmeter.cgi) 및 [설치](https://jmeter.apache.org/usermanual/get-started.html#install) 로컬로
1. [실행](https://jmeter.apache.org/usermanual/get-started.html#running) 다음을 사용하여 로컬에서 `jmeter` 다음에서 스크립트: `<JMETER-INSTALL-DIR>/bin` 디렉토리.
1. 샘플 열기 [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) JMX 스크립트를 JMeter로 변환 **열기** 도구 메뉴입니다.

   ![샘플 WKND DoS 공격 JMX 테스트 스크립트 열기 - ModSecurity](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. 업데이트 **서버 이름 또는 IP** 의 필드 값 _홈 페이지_ 및 _어드벤처 페이지_ 테스트 AEM 환경 URL과 일치하는 HTTP 요청 샘플러. 샘플 JMeter 스크립트의 다른 세부 사항을 검토하십시오.

   ![AEM 서버 이름 HTTP 요청 JMetere - ModSecurity](assets/modsecurity-crs/aem-server-name-http-request.png)

1. 다음을 눌러 스크립트 실행 **시작** 도구 메뉴에서 단추를 클릭합니다. 스크립트는 WKND 사이트의 인스턴스에 대해 50개의 HTTP 요청(사용자 5명, 루프 카운트 10개)을 전송합니다 _홈 페이지_ 및 _어드벤처 페이지_. 따라서 정적이 아닌 파일에 대한 총 100개의 요청으로 **보호 기능** CRS 규칙 사용자 정의 구성.

   ![JMeter 스크립트 실행 - ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. 다음 **테이블에서 결과 보기** JMeter 리스너를 표시합니다. **실패** 요청 번호 ~ 53 이상에 대한 응답 상태입니다.

   ![테이블 JMeter - ModSecurity의 결과 보기에서 실패한 응답](assets/modsecurity-crs/failed-response-jmeter.png)

1. 다음 **503 HTTP 응답 코드** 이(가) 실패한 요청에 대해 반환되면 다음을 사용하여 세부 사항을 볼 수 있습니다. **결과 트리 보기** JMeter 리스너입니다.

   ![503 응답 JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### 로그 검토

ModSecurity 로거 구성은 DoS 공격 사건의 세부 정보를 기록합니다. 세부 정보를 보려면 아래 단계를 따르십시오.

1. 을(를) 다운로드하여 엽니다. `httpderror` 의 로그 파일 **Dispatcher 게시**.
1. 단어 검색 `burst` 로그 파일에서 **오류** 라인

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. 다음과 같은 세부 사항을 검토합니다. _클라이언트 IP 주소_, 작업, 오류 메시지 및 요청 세부 정보.

## ModSecurity의 성능에 미치는 영향

ModSecurity 및 관련 규칙을 활성화하면 성능에 몇 가지 영향을 미치므로 필요, 중복 및 건너뛴 규칙을 염두에 두어야 합니다. CRS 규칙을 활성화하고 사용자 지정하려면 웹 보안 전문가와 협력하십시오.

### 추가 규칙

이 튜토리얼에서는 **보호 기능** 데모용 CRS 규칙. 적절한 규칙을 이해하고 검토하고 구성하려면 Web Security 전문가와 협력하는 것이 좋습니다.
