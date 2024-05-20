---
title: CDN 캐시 적중률 분석
description: AEM as a Cloud Service으로 제공된 CDN 로그를 분석하는 방법에 대해 알아봅니다. 캐시 적중률, 최적화를 위한 MISS 및 PASS 캐시 유형의 상위 URL과 같은 통찰력을 얻으십시오.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
exl-id: 43aa7133-7f4a-445a-9220-1d78bb913942
duration: 276
source-git-commit: 4111ae0cf8777ce21c224991b8b1c66fb01041b3
workflow-type: tm+mt
source-wordcount: '1476'
ht-degree: 0%

---

# CDN 캐시 적중률 분석

CDN에서 캐시된 컨텐츠는 요청이 Apache/dispatcher 또는 AEM 게시로 돌아올 때까지 기다릴 필요가 없는 웹 사이트 사용자가 경험하는 지연을 줄입니다. 이를 고려하여 CDN 캐시 적중률을 최적화하여 CDN에서 캐시할 수 있는 컨텐츠의 양을 최대화하는 것이 좋습니다.

as a Cloud Service으로 제공된 AEM을 분석하는 방법 알아보기 **CDN 로그** 및 다음과 같은 통찰력을 얻으십시오. **캐시 적중률**, 및 **의 상위 URL _미스_ 및 _통과_ 캐시 유형**&#x200B;을 참조하십시오.


CDN 로그는 다음을 포함한 다양한 필드가 포함된 JSON 형식으로 사용할 수 있습니다. `url`, `cache`. 자세한 내용은 [CDN 로그 형식](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=en#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). 다음 `cache` 필드에 다음에 대한 정보가 제공됩니다. _캐시 상태_ 가능한 값은 HIT, MISS 또는 PASS입니다. 가능한 값에 대한 세부 사항을 검토해 보겠습니다.

| 캐시 상태 </br> 가능한 값 | 설명 |
|------------------------------------|:-----------------------------------------------------:|
| 히트 | 요청한 데이터: _CDN 캐시에서 찾으며 페치를 수행할 필요가 없습니다._ AEM 서버에 요청합니다. |
| 미스 | 요청한 데이터: _cdn 캐시에서 찾을 수 없어 요청해야 합니다._ AEM 서버에서 가져옵니다. |
| 통과 | 요청한 데이터: _캐시되지 않도록 명시적으로 설정_ 항상 AEM 서버에서 검색됩니다. |

이 자습서의 목적상 [AEM WKND 프로젝트](https://github.com/adobe/aem-guides-wknd) AEM as a Cloud Service 환경에 배포되고 을 사용하여 소규모 성능 테스트가 트리거됩니다. [Apache JS 편집기](https://jmeter.apache.org/).

이 튜토리얼은 다음 프로세스를 안내하도록 구성되어 있습니다.

1. Cloud Manager를 통해 CDN 로그 다운로드
1. 이러한 CDN 로그를 분석하려면 로컬로 설치된 대시보드나 원격으로 액세스하는 Splunk 또는 Jupityer Notebook(Adobe Experience Platform의 라이선스를 구입한 사용자용)의 두 가지 접근 방식을 사용하여 수행할 수 있습니다
1. CDN 캐시 구성 최적화

## CDN 로그 다운로드

CDN 로그를 다운로드하려면 다음 단계를 수행합니다.

1. 에서 Cloud Manager에 로그인합니다. [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) 조직 및 프로그램을 선택합니다.

1. 원하는 AEM CS 환경의 경우 **로그 다운로드** 줄임표 메뉴에서 을 클릭합니다.

   ![로그 다운로드 - Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. 다음에서 **로그 다운로드** 대화 상자에서 **게시** 드롭다운 메뉴에서 서비스 를 클릭한 다음, **CDN** 행.

   ![CDN 로그 - Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


다운로드한 로그 파일이에서 _오늘_ 파일 확장명은 입니다. `.log` 그렇지 않으면 이전 로그 파일의 경우 확장자는 `.log.gz`.

## 다운로드한 CDN 로그 분석

캐시 적중률, MISS 및 PASS 캐시 유형의 상위 URL과 같은 통찰력을 얻으려면 다운로드된 CDN 로그 파일을 분석하십시오. 이러한 통찰력은 를 최적화하는 데 도움이 됩니다. [CDN 캐시 구성](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching) 사이트 성능을 향상시킬 수 있습니다.

CDN 로그를 분석하기 위해 이 자습서에서는 다음 세 가지 옵션을 제공합니다.

1. **Elasticsearch, Logstash 및 Kibana(ELK)**: [ELK 대시보드 도구](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) 로컬에 설치할 수 있습니다.
1. **스플렁크**: [Splunk 대시보드 도구](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md) Splunk 및 [AEMCS 로그 전달 사용](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) CDN 로그를 수집합니다.
1. **Jupyter Notebook**: 의 일부로 원격으로 액세스할 수 있습니다. [Adobe Experience Platform](https://experienceleague.adobe.com/en/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data) 추가 소프트웨어를 설치하지 않고 Adobe Experience Platform 라이선스를 구입한 고객을 위한 것입니다.

### 옵션 1: ELK 대시보드 도구 사용

다음 [스택](https://www.elastic.co/elastic-stack) 는 데이터를 검색, 분석 및 시각화할 수 있는 확장 가능한 솔루션을 제공하는 툴 세트입니다. 그것은 Elasticsearch, Logstash 및 Kibana로 구성되어 있습니다.

주요 세부 정보를 식별하려면 [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) 프로젝트. 이 프로젝트는 ELK 스택의 도커 컨테이너와 CDN 로그를 분석하기 위해 사전 구성된 Kibana 대시보드를 제공합니다.

1. 다음 단계를 따릅니다. [ELK 도커 컨테이너를 설정하는 방법](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container) 및 을(를) 가져와야 합니다. **CDN 캐시 적중률** 키바나 대시보드

1. CDN 캐시 적중률 및 상위 URL을 식별하려면 다음 단계를 수행합니다.

   1. 다운로드한 CDN 로그 파일을 환경별 로그 폴더 내에 복사합니다. 예: `ELK/logs/stage`.

   1. 를 엽니다. **CDN 캐시 적중률** 왼쪽 상단 모서리를 클릭하여 대시보드 _탐색 메뉴 > Analytics > 대시보드 > CDN 캐시 적중률_.

      ![CDN 캐시 적중률 - Kibana Dashboard](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. 오른쪽 상단에서 원하는 시간 범위를 선택합니다.

      ![시간 범위 - 키바나 대시보드](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. 다음 **CDN 캐시 적중률** 대시보드는 설명이 따로 필요하지 않습니다.

   1. 다음 _총 요청 분석_ 섹션에는 다음 세부 정보가 표시됩니다.
      - 캐시 유형별 캐시 비율
      - 캐시 유형별 캐시 카운트

      ![총 요청 분석 - Kibana Dashboard](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. 다음 _요청 또는 MIME 유형별 분석_ 은 다음 세부 정보를 표시합니다.
      - 캐시 유형별 캐시 비율
      - 캐시 유형별 캐시 카운트
      - 상위 누락 및 URL 전달

      ![요청 또는 MIME 유형별 분석 - Kibana Dashboard](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### 환경 이름 또는 프로그램 ID별 필터링

수집된 로그를 환경 이름별로 필터링하려면 아래 단계를 수행합니다.

1. CDN 캐시 적중률 대시보드에서 **필터 추가** 아이콘.

   ![필터 - 키바나 대시보드](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. 다음에서 **필터 추가** 모달, 다음을 선택합니다. `aem_env_name.keyword` 드롭다운 메뉴의 필드, `is` 다음 필드에 대한 연산자 및 원하는 환경 이름을 입력하고 마지막으로 _필터 추가_.

   ![필터 추가 - 키바나 대시보드](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### 호스트 이름으로 필터링

호스트 이름별로 수집된 로그를 필터링하려면 아래 단계를 따르십시오.

1. CDN 캐시 적중률 대시보드에서 **필터 추가** 아이콘.

   ![필터 - 키바나 대시보드](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. 다음에서 **필터 추가** 모달, 다음을 선택합니다. `host.keyword` 드롭다운 메뉴의 필드, `is` 다음 필드에 대한 연산자 및 원하는 호스트 이름을 입력하고 마지막으로 _필터 추가_.

   ![호스트 필터 - 키바나 대시보드](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

마찬가지로 분석 요구 사항에 따라 대시보드에 필터를 더 추가합니다.

### 옵션 2: Splunk 대시보드 도구 사용

다음 [스플렁크](https://www.splunk.com/) 는 모니터링 및 문제 해결 목적으로 로그를 집계하고 분석하고 시각화를 만드는 데 도움이 되는 인기 있는 로그 분석 도구입니다.

주요 세부 정보를 식별하려면 [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) 프로젝트. 이 프로젝트는 CDN 로그를 분석하는 Splunk 대시보드를 제공합니다.

1. 다음 단계를 따릅니다. [AEMCS CDN 로그 분석용 Splunk 대시보드](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md) 및 을(를) 가져와야 합니다. **CDN 캐시 적중률** Splunk 대시보드.
1. 필요한 경우 _색인, 소스 유형 및 기타_ 필터 값은 Splunk 대시보드에 있습니다.

   ![Splunk 대시보드](assets/cdn-logs-analysis/splunk-CHR-dashboard.png){width="500" zoomable="yes"}

>[!NOTE]
>
>splunk 대시보드의 UI 및 그래프는 ELK 대시보드와 다르지만 주요 세부 정보는 유사합니다.

### 옵션 3: Jupyter Notebook 사용

로컬로 소프트웨어를 설치하지 않으려는 사용자(즉, 이전 섹션의 ELK 대시보드 도구)에게는 다른 옵션이 있지만 Adobe Experience Platform에 대한 라이센스가 필요합니다.

다음 [Jupyter Notebook](https://jupyter.org/) 는 코드, 텍스트 및 시각화가 포함된 문서를 만들 수 있는 오픈 소스 웹 애플리케이션입니다. 데이터 변환, 시각화 및 통계 모델링에 사용됩니다. 원격으로 액세스할 수 있음 [Adobe Experience Platform의 일부](https://experienceleague.adobe.com/en/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data).

#### 대화형 Python Notebook 파일 다운로드

먼저 를 다운로드합니다. [AEM-as-a-CloudService - CDN 로그 분석 - Jupyter Notebook](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb) CDN 로그 분석에 도움이 되는 파일입니다. 이 &quot;대화형 Python Notebook&quot; 파일은 설명이 가능하지만, 각 섹션의 주요 사항은 다음과 같습니다.

- **추가 라이브러리 설치**: 를 설치합니다. `termcolor` 및 `tabulate` Python 라이브러리.
- **CDN 로그 로드**: 를 사용하여 CDN 로그 파일을 로드합니다. `log_file` 변수 값입니다. 값을 업데이트해야 합니다. 또한 이 CDN 로그를 로 변환합니다. [Pandas DataFrame](https://pandas.pydata.org/docs/reference/frame.html).
- **분석 수행**: 첫 번째 코드 블록은 _총, HTML, JS/CSS 및 이미지 요청에 대한 분석 결과 표시_; 캐시 적중률, 막대 및 파이 차트를 제공합니다.
두 번째 코드 블록은 _HTML, JS/CSS 및 이미지에 대한 상위 5개 MISS 및 PASS 요청 URL_; 테이블 형식으로 URL과 해당 카운트가 표시됩니다.

#### Jupyter Notebook 실행

다음 단계를 수행하여 Adobe Experience Platform에서 Jupyter Notebook을 실행합니다.

1. 에 로그인 [Adobe Experience Cloud](https://experience.adobe.com/), 홈 페이지 > **빠른 액세스** section > click the **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. Adobe Experience Platform 홈 페이지 > 데이터 과학 섹션에서 다음을 클릭합니다. **노트북** 메뉴 항목. Jupyter Notebooks 환경을 시작하려면 **Jupyterlab** 탭.

   ![수첩 로그 파일 값 업데이트](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. JupyterLab 메뉴에서 **파일 업로드** 아이콘, 다운로드한 CDN 로그 파일 업로드 및 `aemcs_cdn_logs_analysis.ipynb` 파일.

   ![파일 업로드 - JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. 를 엽니다. `aemcs_cdn_logs_analysis.ipynb` 파일을 두 번 클릭하여 만듭니다.

1. 다음에서 **CDN 로그 파일 로드** 전자 필기장의 섹션, `log_file` 값.

   ![수첩 로그 파일 값 업데이트](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. 선택한 셀을 실행하고 진행하려면 **재생** 아이콘.

   ![수첩 로그 파일 값 업데이트](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. 를 실행한 후 **총, HTML, JS/CSS 및 이미지 요청에 대한 분석 결과 표시** 코드 셀인 경우 출력에는 캐시 적중률 백분율, 막대 및 파이 차트가 표시됩니다.

   ![수첩 로그 파일 값 업데이트](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. 를 실행한 후 **HTML, JS/CSS 및 이미지에 대한 상위 5개 MISS 및 PASS 요청 URL** 코드 셀에 상위 5개의 MISS 및 PASS 요청 URL이 출력됩니다.

   ![수첩 로그 파일 값 업데이트](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

요구 사항에 따라 CDN 로그를 분석하도록 Jupyter Notebook을 향상시킬 수 있습니다.

## CDN 캐시 구성 최적화

CDN 로그를 분석한 후에는 CDN 캐시 구성을 최적화하여 사이트 성능을 향상시킬 수 있습니다. AEM 우수 사례는 캐시 적중률이 90% 이상인 것입니다.

자세한 내용은 [CDN 캐시 구성 최적화](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching).

AEM WKND 프로젝트에는 참조 CDN 구성이 있습니다. 자세한 내용은 다음을 참조하십시오. [CDN 구성](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) 다음에서 `wknd.vhost` 파일.
