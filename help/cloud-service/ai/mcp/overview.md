---
title: AEM의 MCP 서버
description: 선호하는 AI 기반 IDE 또는 채팅 기반 애플리케이션에서 AEM 모델 컨텍스트 프로토콜(MCP) 서버를 사용하여 AEM 콘텐츠 작업을 간소화하고 가속화하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20473
source-git-commit: c5f1c7f57181b1e9de6dd91aa2428f2fe1a04893
workflow-type: tm+mt
source-wordcount: '881'
ht-degree: 0%

---

# AEM의 MCP 서버

선호하는 AI 기반 IDE 또는 채팅 기반 애플리케이션에서 AEM _모델 컨텍스트 프로토콜(MCP) 서버_&#x200B;를 사용하여 AEM 콘텐츠 작업을 간소화하고 가속화하는 방법에 대해 알아봅니다. 낮은 수준의 API 코드를 작성하거나 AEM UI를 탐색하는 대신 원하는 내용을 자연어로 설명합니다.

## AEM MCP 서버 목록

모든 AEM MCP 서버는 `https://mcp.adobeaemcloud.com/adobe/mcp/`에서 사용할 수 있습니다. 자세한 내용은 [AEM as a Cloud Service과 MCP 사용](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service)을 참조하세요.

- **컨텐츠**(`/content`) — 페이지, 조각 및 자산을 만들고, 읽고, 업데이트하고, 삭제할 수 있는 전체 액세스 권한입니다.
- **콘텐츠(읽기 전용)**(`/content-readonly`) — 페이지, 조각 및 에셋을 나열하고 가져올 수 있는 읽기 전용(변경 사항 없음).
- **Cloud Manager**(`/cloudmanager`) — Adobe Cloud Manager 프로그램, 환경, 저장소 및 파이프라인을 관리합니다.

>[!TIP]
>
>각 서버가 노출하는 도구는 시간이 지남에 따라 변경될 수 있습니다. 현재 사용 가능한 기능을 보려면 AI에게 모든 AEM MCP 도구(예: `List all AEM MCP tools available from this server and describe what they do`)를 나열하도록 요청하거나 IDE에서 `tools/list` 프롬프트를 입력하십시오.

## MCP 서버 사용 패턴

AEM MCP 서버를 사용하기 전에 MCP 서버에 대한 두 가지 주요 사용 패턴을 살펴보겠습니다.

- **사람 중심** — 운전석에 있습니다. AI가 IDE에서 도구를 제안하거나 실행합니다.
- **에이전트** — 에이전트 응용 프로그램(에이전트 또는 하위 에이전트)이 직접 서버를 호출하여 도구를 선택하고 사람의 입력이 거의 없이 목표를 향해 작업합니다.

다음은 이러한 두 사용 패턴을 비교하는 방법입니다.

| 측면 | 인간 중심 | 무발생 |
| ------ | ------------- | ------- |
| **작업을 유도하는 사용자** | 너. <br> AI가 IDE 또는 채팅 기반 응용 프로그램에서 도구를 제안하거나 실행합니다. | AI. <br> 사용할 도구를 선택하고 최소한의 지침으로 계속 진행합니다. |
| **결정 권한** | 넌 통제권을 갖고있어 각 단계를 승인하거나 트리거합니다. | AI는 더 많은 자유를 가지고 있습니다. 영향을 많이 주는 작업에는 보호 기능 또는 승인이 필요할 수 있습니다. |
| **일반적인 사용 패턴** | **개발자당**, 자체 IDE 또는 채팅 기반 응용 프로그램에서 사용하며 세션당 한 명의 개발자가 매일 개발 작업에 적합합니다. | 많은 사용자 또는 에이전트의 공유 서비스 및 게이트웨이로 에이전트 응용 프로그램을 통해 **공유**. |
| **가장 적합한 항목** | 루프에 있는 동안 콘텐츠 검토, 안내식 업데이트, 탐색 또는 반복 작업. | 에이전트 워크플로, 일괄 처리 작업, 파이프라인 및 시스템이 최소한의 개입으로 실행되어야 하는 목표입니다. |

### Agentic Systems에서 MCP 사용 시

MCP 서버는 대화형 UX와 인간의 감독을 통해 **인간이 운영하는 MCP 클라이언트**&#x200B;용으로 설계되었습니다. MCP 도구 사양은 도구 호출을 승인하거나 거부할 수 있는 _루프 안의 사람_&#x200B;을(를) 권장합니다.

에이전트 또는 자율 시스템에서 MCP 서버를 사용하는 경우 이를 별도의 호환성 티어로 취급합니다. **프롬프트**, _허용 목록_ 또는 _라우팅 논리_&#x200B;에서 도구 이름을 _하드코딩하지 않음_&#x200B;하십시오. MCP에서 _도구 이름_&#x200B;은(는) 프로그램 식별자이고 _설명_&#x200B;은(는) LLM에 대한 모델 표시 힌트입니다. 프롬프트 및 선택 기반의 기능 또는 설명을 선호합니다.

`tools/list`을(를) 통해 런타임 검색을 구현하고 도구 목록 변경(`notifications/tools/list_changed`)을 처리하며, 프로토콜 기준선을 넘어서는 안정성 보장이 필요한 경우 온보딩 및 버전 관리 시 MCP 서버 공급자와 일치합니다.

## MCP 엔티티 및 매핑

MCP는 **호스트**, **클라이언트** 및 **서버** 등 세 가지 엔터티를 기반으로 빌드되었습니다. [MCP 사양](https://modelcontextprotocol.io/docs/getting-started/intro)은(는) 공식적으로 정의합니다. 그러나 아래 표는 AEM MCP 서버를 사용할 때 각 항목을 일반 용어로 설명하고 매핑합니다.

| 구성 요소 | 표준 정의 | AEM MCP 서버 사용 시 |
| --------- | ------------------- | ---------------- |
| **호스트** | 모든 것을 실행하는 앱은 컨텍스트를 수집하고 AI와 대화하며 권한을 처리하고 클라이언트를 만듭니다. | **IDE**(커서) 또는 채팅 기반 응용 프로그램이 호스트입니다. MCP 클라이언트를 실행하고 세션이 사용할 수 있는 도구 및 서버를 결정합니다. |
| **클라이언트** | 호스트에서 하나의 서버로 단일 접속 메시지를 앞뒤로 전달하고 서버의 액세스를 다른 액세스와 분리합니다. | **MCP 클라이언트**&#x200B;이(가) IDE 또는 채팅 기반 응용 프로그램에 있습니다. 설정에 AEM Content MCP 서버를 추가하면 IDE 또는 채팅 기반 응용 프로그램은 해당 서버와 통신하는 클라이언트를 만듭니다. 프롬프트 및 도구 호출이 이 클라이언트를 통과합니다. |
| **서버** | MCP에 도구, 데이터 및 프롬프트를 표시하는 서비스입니다. 컴퓨터에서 실행되거나 원격으로 실행될 수 있습니다. | **AEM MCP 서버**&#x200B;에서 호스팅하는 Adobe은 IDE 또는 채팅 기반 응용 프로그램의 AI가 AEM 환경에서 작동할 수 있도록 페이지, 콘텐츠 조각 및 자산을 만들고, 읽고, 업데이트하고, 삭제할 수 있는 도구를 제공합니다. |

간단히 말해, **Host**&#x200B;은(는) IDE 또는 채팅 기반 응용 프로그램이고, **Client**&#x200B;은(는) IDE 또는 채팅 기반 응용 프로그램에서 AEM으로 연결되어 있으며, **Server**&#x200B;은(는) 작업을 수행하는 Adobe 호스팅 AEM MCP 서버입니다.

## 설정

AEM MCP 서버는 정의된 MCP 호환 애플리케이션 세트와 호환되도록 설계되었습니다. 공식적으로 지원되는 애플리케이션은 다음과 같습니다.

- [인류 클라우드](https://claude.com/product/overview)
- [커서](https://www.cursor.com/)
- [OpenAI ChatGPT](https://chatgpt.com/)
- [Microsoft Copilot Studio](https://www.microsoft.com/en-us/microsoft-365-copilot/microsoft-copilot-studio)

자세한 내용은 [설치 개요](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service#setup-overview)를 참조하십시오.

## 사용 사례

<!-- CARDS
{target = _self}

* ./accelerate-content-operations-with-aem-mcp-server.md    
  {title = Accelerate Content Operations with AEM MCP Server}
  {description = Learn how to use the AEM Content MCP Server from Cursor IDE to streamline and accelerate your AEM content work.}
  {image = ../assets/content-mcp-server/update-adventure-price-prompt-response.png}
  {cta = Learn Content MCP Server}

* ./cloud-manager.md
  {title = Cloud Manager MCP Server}
  {description = Learn how to use the AEM Cloud Manager MCP Server from Cursor IDE to streamline and accelerate your AEM cloud manager work.}
  {image = ../assets/cm-mcp-server/start-pipeline.png}
  {cta = Learn Cloud Manager MCP Server}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Accelerate Content Operations with AEM MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./accelerate-content-operations-with-aem-mcp-server.md" title="AEM MCP 서버를 사용하여 콘텐츠 작업 가속화" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/content-mcp-server/update-adventure-price-prompt-response.png" alt="AEM MCP 서버를 사용하여 콘텐츠 작업 가속화"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" title="AEM MCP 서버를 사용하여 콘텐츠 작업 가속화">AEM MCP 서버로 콘텐츠 작업 가속화</a>
                    </p>
                    <p class="is-size-6">Cursor IDE에서 AEM Content MCP 서버를 사용하여 AEM 콘텐츠 작업을 간소화하고 가속화하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">콘텐츠 MCP 서버 학습</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud Manager MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./cloud-manager.md" title="Cloud Manager 서버" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/cm-mcp-server/start-pipeline.png" alt="Cloud Manager 서버"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./cloud-manager.md" target="_self" rel="referrer" title="Cloud Manager 서버">Cloud Manager MCP 서버</a>
                    </p>
                    <p class="is-size-6">Cursor IDE에서 AEM Cloud Manager MCP 서버를 사용하여 AEM Cloud Manager 작업을 간소화하고 가속화하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./cloud-manager.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Cloud Manager MCP 서버 학습</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
