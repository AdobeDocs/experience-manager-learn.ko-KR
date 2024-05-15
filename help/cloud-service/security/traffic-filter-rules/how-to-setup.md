---
title: WAF 규칙을 포함한 트래픽 필터 규칙을 설정하는 방법
description: WAF 규칙을 포함한 트래픽 필터 규칙의 결과를 만들고, 배포하고, 테스트하고, 분석하도록 설정하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: b67bf642-3341-48d0-8ea9-5f262febf414
duration: 292
source-git-commit: c7c78ca56c1d72f13d2dc80229a10704ab0f14ab
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 3%

---

# WAF 규칙을 포함한 트래픽 필터 규칙을 설정하는 방법

학습 **설정 방법** WAF 규칙을 포함한 트래픽 필터 규칙. 결과 생성, 배포, 테스트 및 분석에 대해 알아보십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## 설정

설정 프로세스에는 다음이 포함됩니다.

- _규칙 만들기_ 적절한 AEM 프로젝트 구조 및 구성 파일이 포함되어 있습니다.
- _규칙 배포_ Adobe Cloud Manager의 구성 파이프라인 사용.
- _규칙 테스트_ 다양한 도구를 사용하여 트래픽을 생성합니다.
- _결과 분석_ aemcs CDN 로그 및 대시보드 도구 사용.

### AEM 프로젝트에서 규칙 만들기

규칙을 만들려면 다음 단계를 수행합니다.

1. AEM 프로젝트의 최상위 수준에서 폴더를 만듭니다 `config`.

1. 다음 범위 내 `config` 폴더, (이)라는 새 파일을 만듭니다. `cdn.yaml`.

1. 에 다음 메타데이터 추가 `cdn.yaml` 파일:

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
```

의 예 보기 `cdn.yaml` AEM Guides WKND Sites 프로젝트 내의 파일:

![WKND AEM 프로젝트 규칙 파일 및 폴더](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Cloud Manager를 통해 규칙 배포 {#deploy-rules-through-cloud-manager}

규칙을 배포하려면 다음 단계를 수행합니다.

1. [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/)에서 Cloud Manager에 로그인한 다음 적절한 조직과 프로그램을 선택합니다.

1. 다음 위치로 이동 _파이프라인_ 다음에서 카드 _프로그램 개요_ 페이지를 클릭하고 **+추가** 버튼을 클릭하고 원하는 파이프라인 유형을 선택합니다.

   ![Cloud Manager 파이프라인 카드](./assets/cloud-manager-pipelines-card.png)

   위의 예에서, 데모 목적 _비프로덕션 파이프라인 추가_ 개발 환경이 사용되므로 이(가) 선택됩니다.

1. 다음에서 _비프로덕션 파이프라인 추가_ 대화 상자에서 다음 세부 정보를 선택하고 입력합니다.

   1. 구성 단계:

      - **유형**: 배포 파이프라인
      - **파이프라인 이름**: Dev-Config

      ![Cloud Manager 파이프라인 구성 대화 상자](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. 소스 코드 단계:

      - **배포할 코드**: 타깃팅된 배포
      - **포함**: 구성
      - **배포 환경**: 환경의 이름(예: wknd-program-dev)입니다.
      - **저장소**: 파이프라인이 코드를 검색해야 하는 Git 저장소입니다. 예: `wknd-site`
      - **Git 분기**: Git 저장소 분기의 이름입니다.
      - **코드 위치**: `/config`: 이전 단계에서 만든 최상위 구성 폴더에 해당합니다.

      ![Cloud Manager 파이프라인 구성 대화 상자](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### 트래픽을 생성하여 규칙 테스트

규칙을 테스트하기 위해 사용할 수 있는 다양한 타사 도구가 있으며 조직에 선호하는 도구가 있을 수 있습니다. 데모 목적으로 다음 도구를 사용해 보겠습니다.

- [컬](https://curl.se/) URL 호출 및 응답 코드 확인과 같은 기본 테스트용입니다.

- [Vegeta](https://github.com/tsenart/vegeta) 서비스 거부(DOS)를 수행합니다. 의 설치 지침을 따릅니다. [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [니토](https://github.com/sullo/nikto/wiki) XSS, SQL 주입 등과 같은 잠재적인 문제 및 보안 취약점을 찾을 수 있습니다. 의 설치 지침을 따르십시오. [Nikto GitHub](https://github.com/sullo/nikto).

- 아래 명령을 실행하여 터미널에 도구가 설치되어 있고 사용할 수 있는지 확인하십시오.

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### 대시보드 도구를 사용하여 결과 분석

규칙을 만들고, 배포하고, 테스트한 후 다음을 사용하여 결과를 분석할 수 있습니다 **CDN** 로그 및 **AEMCS-CDN-Log-Analysis-Tooling**. 이 툴은 Splunk 및 ELK(Elasticsearch, Logstash 및 Kibana) 스택의 결과를 시각화하기 위한 대시보드 세트를 제공합니다.

도구를에서 복제할 수 있습니다. [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) GitHub 리포지토리. 그런 다음 지침에 따라 을 설치하고 로드합니다. **CDN 트래픽 대시보드** 및 **WAF 대시보드** 선호하는 가시성 도구에 대한 대시보드

이 자습서에서는 ELK 스택을 사용하겠습니다. 다음 [AEMCS CDN 로그 분석용 ELK Docker 컨테이너](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) ELK 스택을 설정하는 지침.

- 샘플 대시보드를 로드한 후 Elastic Dashboard Tool 페이지는 다음과 같이 표시되어야 합니다.

  ![ELK 트래픽 필터 규칙 대시보드](./assets/elk-dashboard.png)

>[!NOTE]
>
>    아직 수집된 AEMCS CDN 로그가 없으므로 대시보드가 비어 있습니다.


## 다음 단계

에서 WAF 규칙을 포함한 트래픽 필터 규칙을 선언하는 방법을 알아봅니다. [예제 및 결과 분석](./examples-and-analysis.md) 챕터, AEM WKND Sites 프로젝트 사용
