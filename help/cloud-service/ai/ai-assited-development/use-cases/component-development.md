---
title: AEM 에이전트 기술을 사용한 구성 요소 개발
description: AI 지원 개발의 일부로 AEM 에이전트 기술을 사용하여 AEM 구성 요소를 개발하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20901
thumbnail: KT-20901.png
exl-id: bd9b74e8-81ab-4d42-bd0a-5443248b5770
source-git-commit: f93359e731b6c3fa549e9499ef693042eba3aad7
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 2%

---

# AEM 에이전트 기술을 사용한 구성 요소 개발

[AI 지원 개발](../overview.md)의 일부로 AEM 에이전트 기술을 사용하여 AEM 구성 요소를 개발하는 방법에 대해 알아봅니다.

이 연습에서는 AI 기반 IDE에서 자연어(예: Cursor)를 사용하여 [WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)에서 **프로모션 배너** 구성 요소를 개발합니다. 코딩 에이전트는 `create-component` AEM 에이전트 스킬을 적용하여 구현을 생성합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3484952/?learn=on&enablevpops)

## 사전 요구 사항

이 자습서를 수행하려면 다음이 필요합니다.

- GitHub Copilot이 있는 Visual Studio 코드 또는 커서와 같은 AI 기반 IDE입니다.
- _로컬 AEM SDK_ 인스턴스에 빌드되고 배포된 [WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)의 로컬 복제입니다.
- 해당 프로젝트에 _AEM 에이전트 기술_&#x200B;이(가) 설치되었습니다. 아직 완료하지 않았다면 [AEM 에이전트 기술 설정](../setup/agent-skills.md)을 완료하십시오.

## 구성 요소 요구 사항

WKND 팀이 홈 페이지에 프로모션 배너를 표시하려고 하며 디자인 참조는 다음과 같다고 가정해 보겠습니다.

![프로모션 배너 디자인 참조](../assets/component-development/promo-banner-design-reference.png)

작성자는 구성 요소 대화 상자에서 _프로모션 레이블_, _CTA 레이블_ 및 _CTA 링크_ 필드를 설정할 수 있어야 합니다.

디자인 참조는 와이어프레임, 목업 또는 정적 마크업 캡처를 통해 얻은 스크린샷입니다.

## 구성 요소 개발

1. IDE에서 WKND 프로젝트를 엽니다. AEM 에이전트 기술이 있는지 확인한 후(예: `.agents/skills` 아래) 새 에이전트 채팅을 시작합니다.
   ![AEM 에이전트 기술이 설치되었는지 확인](../assets/component-development/verify-aem-agent-skills-installed.png)

1. 다음과 같이 프롬프트를 입력합니다. IDE가 채팅에서 이미지를 지원하는 경우 구성 요소 디자인 스크린샷(와이어프레임, 목업 또는 정적 마크업 캡처를 통해 획득)을 첨부합니다.

   ```text
   Create a WKND Promo Banner Component. Please see attached screenshot for design reference.
   
   Dialog specification are:
   
   1. Promo Label - Textfield, required
   2. CTA Text - Textfield, required
   3. CTA Link - Pathfield, required
   ```

1. 코딩 에이전트는 `create-component` AEM 에이전트 스킬을 사용하여 구성 요소를 생성합니다. 제안된 HTL, 슬링 모델, 대화 상자 XML 및 관련 파일을 검토합니다.
   ![생성된 코드를 검토합니다](../assets/component-development/review-generated-code.png)

>[!TIP]
>
>디자인 참조를 스크린샷으로 제공하는 대신 [Figma MCP 서버](https://www.figma.com/mcp-catalog/)를 통해 Figma 디자인을 제공하여 구성 요소를 생성할 수도 있습니다. `create-component` 스킬은 [그림 디자인 통합](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/references/figma-design-rules.md)을 지원합니다.


1. 로컬 AEM 인스턴스/SDK에 구성 요소 배포

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 작성 시 홈 페이지에 프로모션 배너를 배치하고 동작을 확인합니다. 구현 기준이 디자인 참조에서 벗어나는 경우 구현을 세분화합니다.
   ![프로모션 배너 구성 요소 작성](../assets/component-development/author-promo-banner-component.png)

1. 페이지를 게시하거나 게시됨으로 보기를 클릭하여 새로 만든 구성 요소를 검토합니다.
   ![새로 만든 구성 요소 검토](../assets/component-development/review-newly-created-component.png)

축하합니다! AI 지원 개발의 일부로 AEM 에이전트 기술을 사용하여 새 AEM 구성 요소를 성공적으로 만들었습니다.

## 단순한 구성 요소 이상

이 연습에서는 간단한 구성 요소를 사용합니다. 동일한 `create-component` 스킬은 다음을 포함하여 더 풍부한 경우도 지원합니다.

- 다중 필드 및 중첩 대화 상자 필드
- AEM 핵심 구성 요소 확장 (Sling 리소스 병합 패턴 포함)
- IDE에서 Figma MCP 서버(예: `plugin-figma-figma`)가 활성화된 경우 레이아웃 및 스타일링을 위한 Figma 파일 또는 프레임 URL

필드 유형, 대화 상자 패턴, Figure 규칙 및 예제를 보려면 설치된 스킬 폴더(예: `.agents/skills/create-component/SKILL.md`)에서 `SKILL.md`을(를) 읽으십시오.

개요, IDE별 설치 경로 및 문제 해결은 Adobe 기술 저장소의 [AEM 구성 요소 개발 에이전트](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/README.md)를 참조하십시오.

## AGENTS.md

정리하기 전에 구성 요소 생성의 일부로 AGENTS.md가 어떻게 생성되었는지 알아보겠습니다.

AEM as a Cloud Service 프로젝트의 경우 `ensure-agents-md` 부트스트랩 스킬([AEM 에이전트 스킬 설정](../setup/agent-skills.md) 중 선택됨)이 **누락**&#x200B;일 때 저장소 루트에서 `AGENTS.md`을(를) 만듭니다. 프로젝트 레이아웃에서 학습한 내용을 사용합니다.

기존 `AGENTS.md` 파일을 **덮어쓰지**&#x200B;않습니다.

![AGENTS.md 만들기](../assets/component-development/agents-md-creation.png)

## 추가 리소스

- [AI 도구를 사용한 로컬 개발](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [AI 코딩 에이전트를 위한 Adobe 기술](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [에이전트 스킬](https://agentskills.io/home)
