---
title: AEM 사이트 만들기
description: 범용 편집기를 사용하여 편집 가능한 Edge Delivery Services용 AEM Sites 사이트를 만듭니다.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: d1ebcaf4-cea6-4820-8b05-3a0c71749d33
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '302'
ht-degree: 100%

---

# AEM 사이트 만들기

AEM 사이트는 웹 사이트의 콘텐츠가 편집되고, 관리되고, 게시되는 곳입니다. Edge Delivery Services를 통해 게재되고 범용 편집기를 사용하여 작성되는 AEM 사이트를 만들려면 [AEM 작성 환경용 Edge Delivery Services 사이트 템플릿](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)을 사용하여 AEM 작성자 인스턴스에서 새 사이트를 만드십시오.

AEM 사이트는 웹 사이트의 콘텐츠가 저장되고 작성되는 곳입니다. 최종 경험은 AEM 사이트 콘텐츠와 [웹 사이트 코드](./1-new-code-project.md)의 조합입니다.

![Edge Delivery Services 및 범용 편집기용 새 AEM 사이트](./assets/2-new-aem-site/new-site.png)

[설명서에 기술된 자세한 단계](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site)에 따라 새 AEM 사이트를 만듭니다.  이 튜토리얼에서 사용된 값을 포함한 단계의 요약 목록은 다음과 같습니다.
1. AEM 작성자 인스턴스에서 **새 사이트를 만듭니다**. 이 튜토리얼에서는 다음과 같은 사이트 명명법을 사용합니다.
   * 사이트 제목: `WKND (Universal Editor)`
   * 사이트 이름: `aem-wknd-eds-ue`

      * 사이트 이름 값은 [`paths.json`에 추가된](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping) 사이트 경로 이름과 일치해야 합니다.

2. [AEM 작성 환경용 Edge Delivery Services 사이트 템플릿](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)에서 **최신 템플릿을 가져옵니다**.
3. GitHub 저장소 이름과 일치하도록 **사이트 이름을 지정**&#x200B;하고, GitHub URL을 저장소 URL로 설정합니다.

## 미리보기용으로 새 사이트 게시

AEM 작성자 인스턴스에서 사이트를 만든 후 Edge Delivery Services 미리보기 환경에 게시하여 [로컬 개발 환경](./3-local-development-environment.md)에서 콘텐츠를 사용할 수 있도록 합니다.

1. **AEM 작성자 인스턴스**&#x200B;에 로그인한 다음 **Sites**&#x200B;로 이동합니다.
2. **새 사이트**(`WKND (Universal Editor)`)를 선택한 다음 **게시 관리**&#x200B;를 클릭합니다.
3. **대상**&#x200B;에서 **미리보기**&#x200B;를 선택하고 **다음**&#x200B;을 클릭합니다.
4. **하위 설정 포함**&#x200B;에서 **하위 항목 포함**&#x200B;을 선택하고 다른 옵션을 선택 해제한 다음 **확인**&#x200B;을 클릭합니다.
5. **게시**&#x200B;를 클릭하여 사이트 콘텐츠를 미리 볼 수 있도록 게시합니다.
6. 미리보기로 게시된 페이지는 Edge Delivery Services 미리보기 환경에서 사용할 수 있습니다. AEM 미리보기 서비스에는 페이지가 나타나지 않습니다.
