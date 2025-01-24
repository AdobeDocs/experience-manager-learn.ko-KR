---
title: AEM 사이트 만들기
description: 범용 편집기를 사용하여 편집할 수 있는 Edge Delivery Services을 위한 AEM Sites 사이트를 만듭니다.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# AEM 사이트 만들기

AEM 사이트에서는 웹 사이트의 콘텐츠를 편집, 관리 및 게시할 수 있습니다. Edge Delivery Services을 통해 전달되고 범용 편집기를 사용하여 작성된 AEM 사이트를 만들려면 [AEM 작성 사이트 Edge Delivery Services을 사용하여](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)을(를) 사용하여 AEM 작성자에 새 사이트를 만듭니다.

AEM 사이트는 웹 사이트의 콘텐츠를 저장 및 작성하는 곳입니다. 최종 경험은 AEM 사이트 콘텐츠와 [웹 사이트의 코드](./1-new-code-project.md)의 조합입니다.

![Edge Delivery Services 및 유니버설 편집기를 위한 새 AEM 사이트](./assets/2-new-aem-site/new-site.png)

새 AEM 사이트를 만들려면 아래 단계를 따르십시오.

1. AEM 작성자에 **새 사이트를 만듭니다**. 이 자습서에서는 다음 사이트 이름을 사용합니다.
   * 사이트 제목: `WKND (Universal Editor)`
   * 사이트 이름: `aem-wknd-eds-ue`
2. **AEM 작성 사이트 서식 파일이 있는 Edge Delivery Services [에서 최신 서식 파일을 가져오기**&#x200B;합니다](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **GitHub 저장소 이름과 일치하도록 사이트 이름을**&#x200B;로 지정하고 GitHub URL을 저장소의 URL로 설정합니다.

자세한 지침은 시작 안내서에서 [새 AEM 사이트 만들기 및 편집](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site)을 확인하세요.

## 미리 볼 새 사이트 Publish

AEM Author에서 사이트를 만든 후 [로컬 개발 환경](./3-local-development-environment.md)에서 사용할 수 있도록 Edge Delivery Services 미리 보기에 게시합니다.

1. **AEM 작성자**&#x200B;에 로그인하고 **사이트**(으)로 이동합니다.
2. **새 사이트**(`WKND (Universal Editor)`)를 선택하고 **게시 관리**&#x200B;를 클릭합니다.
3. **대상**&#x200B;에서 **미리 보기**&#x200B;를 선택하고 **다음**&#x200B;을 클릭합니다.
4. **하위 설정 포함**&#x200B;에서 **하위 설정 포함**&#x200B;을 선택하고 다른 옵션을 선택 취소한 다음 **확인**&#x200B;을 클릭합니다.
5. 미리 볼 사이트의 콘텐츠를 게시하려면 **Publish**&#x200B;을(를) 클릭하십시오.
