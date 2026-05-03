---
title: AEM 에이전트 스킬 설정
description: AI 지원 개발을 위한 AEM 에이전트 기술을 설정하는 방법에 대해 알아봅니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
role: Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20900
thumbnail: KT-20900.png
exl-id: c92d9124-4b92-4ee1-b04f-b6d1f82d53aa
source-git-commit: f93359e731b6c3fa549e9499ef693042eba3aad7
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 4%

---

# AEM 에이전트 스킬 설정

AI 지원 개발을 위한 AEM 에이전트 기술을 설정하는 방법에 대해 알아봅니다.

AI 기반 IDE를 통해 코딩 에이전트에게 AEM 개발 작업을 요청하면 일반 모델 교육이나 저장소에서만 유추할 수 있는 모든 것에 의존하지 않고 Adobe의 **AEM 에이전트 기술** 절차 지침을 사용할 수 있습니다.

Adobe은 [Adobe 기술](https://github.com/adobe/skills) 리포지토리를 통해 AEM 에이전트 기술을 제공합니다. Adobe이 AI 지원 개발에 어떻게 도움이 되는지에 대한 자세한 내용은 [AI 지원 개발](../overview.md)도 참조하세요.

이 자습서에서는 [WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)의 로컬 클론에 기술을 설치합니다. 고유한 AEM as a Cloud Service 프로젝트에 동일한 단계를 사용할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3484940/?learn=on&enablevpops)

## 사전 요구 사항

이 자습서를 수행하려면 다음이 필요합니다.

- [WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd) 또는 고유한 AEM as a Cloud Service 프로젝트의 로컬 복제입니다.
- GitHub Copilot이 있는 Visual Studio 코드 또는 커서와 같은 AI 기반 IDE입니다.

## AEM 에이전트 기술 설치

`npx` 명령을 사용하여 AEM 에이전트 기술을 설치합니다(`npx`을(를) 사용하려면 [Node.js](https://nodejs.org/)이(가) 필요합니다). Cloud Code 플러그인 또는 GitHub CLI 확장 프로그램과 같은 다른 설치 옵션에 대해서는 Adobe 기술 저장소의 [설치](https://github.com/adobe/skills/tree/main#installation) 섹션을 참조하십시오.

1. 로컬에서 [WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)를 복제합니다.

   ```shell
   $ git clone https://github.com/adobe/aem-guides-wknd.git
   ```

1. AI 기반 IDE에서 복제된 프로젝트를 열고(예: Cursor) 통합 단말기를 엽니다.
   ![터미널 열기](../assets/agent-skills/wknd-in-cursor-ide-open-terminal.png)

1. 다음 명령을 실행하여 커서에 대한 AEM 에이전트 기술을 추가합니다.

   ```shell
   $ npx skills add https://github.com/adobe/skills/tree/main/plugins/aem/cloud-service --agent cursor
   ```

   다른 에이전트 유형의 경우 Adobe 기술 저장소의 [설치](https://github.com/adobe/skills/tree/main#installation) 섹션을 참조하십시오.

1. 메시지가 표시되면 설치할 AEM 에이전트 기술을 선택합니다.
   ![설치할 AEM 에이전트 기술 선택](../assets/agent-skills/select-aem-agent-skills-to-install.png)

   설치 관리자가 저장소 루트에서 **AGENTS.md** 및 **CLAUDE.md** 파일을 만들 수 있도록 **ensure-agent-md** 스킬을 선택하십시오. 이 부트스트랩 스킬은 프로젝트(예: 루트 `pom.xml` 및 모듈)를 검사하고 맞춤 에이전트 지침을 생성합니다.

   **AGENTS.md**&#x200B;이(가) 이미 있는 경우 **덮어쓰지 않음**&#x200B;입니다.

1. 설치 범위를 선택합니다. 이 연습에서는 **프로젝트** 범위가 일반적이므로 기술 파일이 리포지토리에 있습니다.
   ![설치 범위 선택](../assets/agent-skills/select-installation-scope.png)

1. `.agents/skills`에서 설치를 확인합니다. **SKILLS.md** 및 관련 참조 및 자산 폴더가 표시됩니다.
   ![설치된 스킬 검토](../assets/agent-skills/review-installed-skills.png)

1. Adobe에서 기술을 추가하거나 업데이트할 때 CLI를 사용하여 기술을 추가, 업데이트, 제거 또는 나열합니다. 모든 명령을 보려면 다음을 수행합니다.

   ```shell
   $ npx skills --help
   ```

   ![사용 가능한 스킬 명령 검토](../assets/agent-skills/review-available-skills-commands.png)

## 사용 사례

<!-- 
CARDS
{target = _self}

* ../use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ../assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../use-cases/component-development.md" title="AI 지원 개발로 AEM 구성 요소 만들기" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/component-development/review-generated-code.png" alt="AI 지원 개발로 AEM 구성 요소 만들기"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../use-cases/component-development.md" target="_self" rel="referrer" title="AI 지원 개발로 AEM 구성 요소 만들기">AI 지원 개발로 AEM 구성 요소 만들기</a>
                    </p>
                    <p class="is-size-6">AI 지원 개발을 사용하여 AEM 구성 요소를 개발하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="../use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">AEM 구성 요소 만들기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 추가 리소스

- [AI 도구를 사용한 로컬 개발](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [AI 코딩 에이전트를 위한 Adobe 기술](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [에이전트 스킬](https://agentskills.io/home)
