---
title: WAF 규칙을 포함한 트래픽 필터 규칙을 설정하는 방법
description: WAF 규칙을 포함한 트래픽 필터 규칙의 결과를 생성, 배포, 테스트 및 분석하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '575'
ht-degree: 100%

---

# WAF 규칙을 포함한 트래픽 필터 규칙을 설정하는 방법

WAF 규칙을 포함한 트래픽 필터 규칙을 **설정하는 방법**&#x200B;을 알아봅니다. 결과 생성, 배포, 테스트 및 분석에 대해 읽어보십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## 설정

설정 프로세스에는 다음이 포함됩니다.

- 적절한 AEM 프로젝트 구조와 구성 파일을 사용하여 _규칙 만들기_
- Adobe Cloud Manager의 구성 파이프라인을 사용하여 _규칙 배포_
- 트래픽 생성을 위해 다양한 도구를 사용하여 _규칙 테스트_
- AEMCS CDN 로그와 대시보드 도구를 사용하여 _결과 분석_

### AEM 프로젝트에서 규칙 만들기

규칙을 만들려면 다음 단계를 따릅니다.

1. AEM 프로젝트의 최상위 수준에서 `config` 폴더를 만듭니다.

1. `config` 폴더 내에 `cdn.yaml`이라는 새 파일을 만듭니다.

1. `cdn.yaml` 파일에 다음 메타데이터를 추가합니다.

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

AEM Guides WKND Sites 프로젝트 내에서 `cdn.yaml` 파일의 예제를 확인해 보십시오.

![WKND AEM 프로젝트 규칙 파일 및 폴더](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Cloud Manager를 통해 규칙 배포 {#deploy-rules-through-cloud-manager}

규칙을 배포하려면 다음 단계를 따릅니다.

1. [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/)에서 Cloud Manager에 로그인한 다음 적절한 조직과 프로그램을 선택합니다.

1. _프로그램 개요_ 페이지에서 _파이프라인_ 카드로 이동하고 **+추가** 버튼을 클릭한 다음 원하는 파이프라인 유형을 선택합니다.

   ![Cloud Manager 파이프라인 카드](./assets/cloud-manager-pipelines-card.png)

   위 예제에서는 데모 목적으로 개발 환경이 사용되므로 _비프로덕션 파이프라인 추가_&#x200B;를 선택했습니다.

1. _비프로덕션 파이프라인 추가_ 대화 상자에서 다음 세부 정보를 선택하고 입력합니다.

   1. 구성 단계:

      - **유형**: 배포 파이프라인
      - **파이프라인 이름**: Dev-Config

      ![Cloud Manager 구성 파이프라인 대화 상자](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. 소스 코드 단계:

      - **코드 배포 방식**: 타기팅 배포
      - **포함 항목**: 구성
      - **배포 환경**: 환경 이름(예: wknd-program-dev).
      - **저장소**: 파이프라인이 코드를 가져올 Git 저장소(예: `wknd-site`)
      - **Git 분기**: Git 저장소 분기 이름.
      - **코드 위치**: 이전 단계에서 생성한 최상위 구성 폴더에 해당하는 `/config`

      ![Cloud Manager 구성 파이프라인 대화 상자](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### 트래픽을 생성하여 규칙 테스트

규칙을 테스트하기 위해 다양한 서드파티 도구를 사용할 수 있으며, 조직마다 선호하는 도구가 있을 수 있습니다. 데모 목적으로 다음 도구를 사용해 보겠습니다.

- [Curl](https://curl.se/): URL 호출 및 응답 코드 확인과 같은 기본 테스트에 사용합니다.

- [Vegeta](https://github.com/tsenart/vegeta): 서비스 거부(DoS) 테스트 수행에 사용합니다. [Vegeta GitHub](https://github.com/tsenart/vegeta#install)의 설치 지침을 따르십시오.

- [Nikto](https://github.com/sullo/nikto/wiki): XSS, SQL 주입 등 잠재적인 문제 및 보안 취약점 탐지에 사용합니다. [Nikto GitHub](https://github.com/sullo/nikto)의 설치 지침을 따르십시오.

- 아래 명령을 실행하여 도구들이 터미널에서 정상적으로 설치되었는지 확인합니다.

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

규칙을 만들고 배포하고 테스트한 후에는 **CDN** 로그와 **AEMCS-CDN-Log-Analysis-Tooling**&#x200B;을 사용하여 결과를 분석할 수 있습니다. 이 도구는 Splunk 및 ELK(Elasticsearch, Logstash 및 Kibana) 스택의 결과를 시각화하는 대시보드 세트를 제공합니다.

[AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) GitHub 저장소에서 복제할 수 있습니다. 그런 다음 지침을 따라 선호하는 가시성 도구에 맞게 **CDN 트래픽 대시보드**&#x200B;와 **WAF 대시보드**&#x200B;를 설치하고 로드합니다.

이 튜토리얼에서는 ELK 스택을 사용해 보겠습니다. [AEMCS CDN 로그 분석을 위한 ELK Docker 컨테이너](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) 지침을 따라 ELK 스택을 설정합니다.

- 샘플 대시보드를 로드하면 Elastic 대시보드 도구 페이지가 다음과 같이 표시됩니다.

  ![ELK 트래픽 필터 규칙 대시보드](./assets/elk-dashboard.png)

>[!NOTE]
>
>    아직 AEMCS CDN 로그가 수집되지 않았으므로 대시보드는 비어 있습니다.


## 다음 단계

AEM WKND Sites 프로젝트를 사용하여 WAF 규칙을 포함한 트래픽 필터 규칙을 선언하는 방법은 [예시 및 결과 분석](./examples-and-analysis.md) 장에서 학습할 수 있습니다.
