---
title: 코드 프로젝트 만들기
description: 범용 편집기를 사용하여 편집할 수 있는 Edge Delivery Services 코드 프로젝트를 만듭니다.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 1%

---


# Edge Delivery Services 코드 프로젝트 만들기

Edge Delivery Services 및 유니버설 편집기용 AEM 웹 사이트를 만들려면 Adobe의 [AEM Boilerplate XWalk 프로젝트 템플릿](https://github.com/adobe-rnd/aem-boilerplate-xwalk)을 사용하십시오. 이 템플릿은 웹 사이트 경험을 만드는 데 사용되는 CSS 및 JavaScript을 포함하는 새 코드 프로젝트를 만듭니다. 이 템플릿은 새 GitHub 저장소를 생성하고 Adobe의 표준 코드 및 구성과 함께 로드하여 AEM 웹 사이트 프로젝트를 위한 견고한 기반을 제공합니다.

[Edge Delivery Services에서 제공하는 AEM 웹 사이트](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview)에는 클라이언트측(브라우저) 코드만 있습니다. 웹 사이트 코드는 AEM Author 또는 Publish 서비스에서 실행되지 않습니다.

![새 Edge Delivery Services 프로젝트](./assets/1-new-project/new-project.png)

범용 편집기에서 콘텐츠를 편집할 수 있는 Edge Delivery Services 코드 프로젝트를 만들려면 아래 단계를 따르십시오.

1. **GitHub 계정을 설정합니다.** 조직에 대한 프로젝트를 만드는 경우 조직에 GitHub 계정이 있는지, 그리고 회원인지 확인하십시오.
2. [AEM Boilerplate XWalk 프로젝트 템플릿을 사용하여 **새 코드 프로젝트를 만듭니다**](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **AEM 코드 동기화 GitHub 앱을 설치**&#x200B;하고 저장소에 대한 액세스 권한을 부여합니다. [앱은 여기에서 찾을 수 있습니다](https://github.com/apps/aem-code-sync).
4. **올바른 AEM 작성자 서비스를 가리키도록 새 프로젝트의`fstab.yaml`**&#x200B;을(를) 구성합니다.

   * 실험하기 위해 더 낮은 AEM as a Cloud Service 환경(Stage, Dev 또는 RDE)을 사용할 수 있지만, 실제 웹 사이트 구현은 프로덕션 AEM Author 서비스를 사용하도록 구성해야 합니다.

5. **새 프로젝트의`paths.json`**&#x200B;을(를) 편집하여 AEM 작성자 서비스 경로를 웹 사이트의 루트에 매핑합니다.

자세한 지침은 시작 안내서에서 [GitHub 프로젝트 만들기](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)를 확인하십시오.
