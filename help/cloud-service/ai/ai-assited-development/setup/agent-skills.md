---
title: Set up AEM Agent Skills
description: Learn how to set up AEM Agent Skills for AI-assisted development.
feature: Developer Tools
version: Experience Manager as a Cloud Service
role: Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20900
thumbnail: KT-20900.png
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 4%

---


# Set up AEM Agent Skills

Learn how to set up AEM Agent Skills for AI-assisted development.

When you ask a coding agent through an AI-powered IDE to work on AEM development tasks, it can use **AEM Agent Skills** procedural guidance from Adobe instead of relying only on generic model training or whatever it can infer from your repository alone.

Adobe provides the AEM Agent Skills via the [Adobe Skills](https://github.com/adobe/skills) repository. Also see the [AI-assisted development](../overview.md) for how Adobe helps with AI-assisted development.

In this tutorial, you install the skills on a local clone of the [WKND Sites Project](https://github.com/adobe/aem-guides-wknd). You can use the same steps for your own AEM as a Cloud Service project.

## 사전 요구 사항

To follow this tutorial, you need the following:

- A local clone of the [WKND Sites Project](https://github.com/adobe/aem-guides-wknd) or your own AEM as a Cloud Service project.
- An AI-powered IDE such as Cursor, or Visual Studio Code with GitHub Copilot.

## Install AEM Agent Skills

Install AEM Agent Skills with the `npx` command (requires [Node.js](https://nodejs.org/) so `npx` is available). For other install options, for example, Claude Code plugins or the GitHub CLI extension, see the [Installation](https://github.com/adobe/skills/tree/main#installation) section in the Adobe Skills repository.

1. Clone the [WKND Sites Project](https://github.com/adobe/aem-guides-wknd) locally:

   ```shell
   $ git clone https://github.com/adobe/aem-guides-wknd.git
   ```

1. Open the cloned project in your AI-powered IDE (for example, Cursor) and open the integrated terminal.
   ![Open the terminal](../assets/agent-skills/wknd-in-cursor-ide-open-terminal.png)

1. Run the following command to add AEM Agent Skills for Cursor:

   ```shell
   $ npx skills add https://github.com/adobe/skills/tree/main/plugins/aem/cloud-service --agent cursor
   ```

   For other agent types, see the [Installation](https://github.com/adobe/skills/tree/main#installation) section in the Adobe Skills repository.

1. When prompted, choose which AEM Agent Skills to install.
   ![Select which AEM Agent Skills to install](../assets/agent-skills/select-aem-agent-skills-to-install.png)

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
