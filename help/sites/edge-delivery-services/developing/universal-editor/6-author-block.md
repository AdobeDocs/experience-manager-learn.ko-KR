---
title: 블록 작성
description: 범용 편집기를 사용하여 Edge Delivery Services 블록을 작성합니다.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: ca356d38-262d-4c30-83a0-01c8a1381ee6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '376'
ht-degree: 100%

---

# 블록 작성

[티저 블록의 JSON](./5-new-block.md)을 `teaser` 분기로 푸시한 후에는 블록을 AEM 범용 편집기에서 편집할 수 있습니다.

개발 중 블록을 작성하는 것은 여러 가지 이유에서 중요합니다.

1. 블록의 정의와 모델이 정확한지 확인할 수 있습니다.
1. 개발자가 개발의 기반이 되는 유의미한 HTML을 검토할 수 있습니다.
1. 콘텐츠와 유의미한 HTML을 미리보기 환경에 배포할 수 있어 블록 개발 속도를 높일 수 있습니다.

## `teaser` 분기의 코드를 사용하여 범용 편집기 열기

1. AEM 작성자 인스턴스에 로그인합니다.
2. **Sites**&#x200B;로 이동하여 [이전 장](./2-new-aem-site.md)에서 만든 사이트(WKND(범용 편집기))를 선택합니다.

   ![AEM Sites](./assets/6-author-block/open-new-site.png)

3. 페이지를 만들거나 편집하여 새 블록을 추가합니다. 로컬 개발을 지원할 수 있도록 컨텍스트를 확보해야 합니다. 페이지는 사이트 내 어디서든 만들 수 있지만 일반적으로 새로운 작업 단위마다 별도의 페이지를 만드는 것이 좋습니다. **분기**&#x200B;라는 새 “폴더” 페이지를 만듭니다. 각 하위 페이지는 같은 이름의 Git 분기 개발을 지원하는 데 사용됩니다.

   ![AEM Sites - 분기 페이지 만들기](./assets/6-author-block/branches-page-3.png)

4. **분기** 페이지 아래에 개발 분기 이름과 일치하도록 **티저**&#x200B;라는 제목의 새 페이지를 만듭니다. 페이지를 편집하려면 **열기**&#x200B;를 클릭합니다.

   ![AEM Sites - 티저 페이지 만들기](./assets/6-author-block/teaser-page-3.png)

5. 범용 편집기의 URL에 `?ref=teaser`를 추가하여 `teaser` 분기의 코드를 로드하도록 업데이트합니다. 이 쿼리 매개변수는 `#` 기호 **앞에** 추가해야 합니다.

   ![범용 편집기 - 티저 분기 선택](./assets/6-author-block/select-branch.png)

6. **기본** 아래 첫 번째 섹션을 선택하고 **추가** 버튼을 클릭한 다음 **티저** 블록을 선택합니다.

   ![범용 편집기 - 블록 추가](./assets/6-author-block/add-teaser-2.png)

7. 캔버스에서 새로 추가한 티저를 선택하고 오른쪽에 있는 필드를 작성하거나 인라인 편집 기능을 사용하여 작성합니다.

   ![범용 편집기 - 블록 작성](./assets/6-author-block/author-block.png)

8. 작성이 완료되면 범용 편집기 오른쪽 상단의 **게시** 버튼을 선택하고 **미리보기**&#x200B;에 게시하도록 선택하여 변경 사항을 미리보기 환경에 게시합니다. 이렇게 하면 변경 사항이 웹 사이트의 `aem.page` 도메인에 게시됩니다.
   ![AEM Sites - 게시 또는 미리 보기](./assets/6-author-block/publish-to-preview.png)

9. 변경 사항이 미리보기 환경에 게시될 때까지 기다린 다음 [AEM CLI](./3-local-development-environment.md#install-the-aem-cli)를 통해 [http://localhost:3000/branches/teaser](http://localhost:3000/branches/teaser)에서 웹 페이지를 엽니다.

   ![로컬 사이트 - 새로 고침](./assets/6-author-block/preview.png)

이제 작성된 티저 블록의 콘텐츠와 유의미한 HTML을 미리보기 웹 사이트에서 확인할 수 있으며, 로컬 개발 환경에서 AEM CLI를 사용하여 개발할 준비가 되었습니다.
