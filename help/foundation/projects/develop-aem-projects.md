---
title: AEM에서 프로젝트 개발
description: AEM Projects를 개발하는 방법을 소개하는 개발 자습서입니다.  이 자습서에서는 컨텐츠 제작 워크플로우 및 작업을 관리하기 위해 AEM 내에서 새 프로젝트를 만드는 데 사용할 수 있는 사용자 정의 프로젝트 템플릿을 만듭니다.
version: 6.3, 6.4, 6.5
feature: projects, workflow
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '4652'
ht-degree: 0%

---


# AEM에서 프로젝트 개발

개발 방법을 소개하는 개발 자습서입니다 [!DNL AEM Projects].  이 자습서에서는 컨텐츠 제작 워크플로우 및 작업을 관리하기 위해 AEM 내에서 새 프로젝트를 만드는 데 사용할 수 있는 사용자 정의 프로젝트 템플릿을 만듭니다.

>[!VIDEO](https://video.tv.adobe.com/v/16904/?quality=12&learn=on)

*이 비디오에서는 아래 튜토리얼에서 만든 완성된 워크플로우에 대한 간단한 데모를 제공합니다.*

## 소개 {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) is a feature of AEM designed to make it easier manage all of the workflows and tasks associated with content creation as part of an AEM Sites or Assets implementation.

AEM 프로젝트에는 여러 개의 [OOTB 프로젝트 템플릿이 포함되어 있습니다](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates). 새 프로젝트를 만들 때 작성자는 사용 가능한 이러한 템플릿 중에서 선택할 수 있습니다. 고유한 비즈니스 요구 사항을 갖는 대규모 AEM 구현에서는 요구 사항에 맞게 사용자 요구에 맞는 맞춤형 프로젝트 템플릿을 만들어야 합니다. 사용자 정의 프로젝트 템플릿 개발자는 프로젝트 대시보드를 구성하고 사용자 정의 워크플로우에 연결하며 프로젝트에 대한 추가 비즈니스 역할을 만들 수 있습니다. 프로젝트 템플릿의 구조를 살펴보고 샘플 템플릿을 만듭니다.

![사용자 지정 프로젝트 카드](./assets/develop-aem-projects/custom-project-card.png)

## 설정

이 자습서는 사용자 지정 프로젝트 템플릿을 만드는 데 필요한 코드를 단계별로 진행합니다. 이 튜토리얼을 따라 로컬 환경에 [첨부된 패키지를](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) 다운로드하여 설치할 수 있습니다. 또한 [GitHub에서 호스팅되는 전체 Maven 프로젝트에 액세스할 수도 있습니다](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [완성된 자습서 패키지](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub의 전체 코드 저장소](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

이 자습서에서는 [AEM 개발 방침에 대한 기본적인 지식과](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/the-basics.html) AEM Maven 프로젝트 [](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ht-projects-maven.html)설정에 대해 잘 알고 있다고 가정합니다. 언급된 모든 코드는 참조로 사용되어야 하며 [로컬 개발 AEM 인스턴스에만 배포해야 합니다](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted).

## 프로젝트 템플릿 구조

프로젝트 템플릿은 소스 제어 하에 두어야 하며 /apps 아래의 응용 프로그램 폴더 아래에 있어야 합니다. 이름 지정 규칙이 ***/projects/templates/**&lt;my-template>인 하위 폴더에 배치하는 것이 좋습니다. 이 명명 규칙에 따라, 새 사용자 지정 템플릿은 프로젝트를 만들 때 작성자가 자동으로 사용할 수 있게 됩니다. 사용 가능한 프로젝트 템플릿의 구성은 **/content/projects/jcr:content** node by the **cq:allowedTemplates** property 기본적으로 정규식입니다. **/(apps|libs)/.*/projects/templates/.***

프로젝트 템플릿의 루트 노드에는 **cq:Template의 jcr:primaryType** 이 **있습니다**. 의 루트 노드 아래에 3개의 노드가 있습니다. **가젯**, **역할**&#x200B;및 **워크플로우**. 이러한 노드는 모두 **비정형**&#x200B;노드입니다. 루트 노드 아래에도 프로젝트 만들기 마법사에서 템플릿을 선택할 때 표시되는 thumbnail.png 파일이 있을 수 있습니다.

전체 노드 구조:

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### 프로젝트 템플릿 루트

프로젝트 템플릿의 루트 노드는 **cq:Template 유형이 됩니다**. 이 노드에서 프로젝트 만들기 마법사에 표시할 **jcr:title** 및 **jcr:description** 속성을 구성할 수 있습니다. 프로젝트의 속성을 채울 양식을 가리키는 **마법사** 속성도 있습니다. 기본값: **사용자가 기본 프로젝트 속성을 채우고 그룹 구성원을 추가할 수 있으므로 /libs/cq/core/content/projects/wizard/steps/defaultproject.html** 은 대부분의 경우 올바르게 작동해야 합니다.

**프로젝트 만들기 마법사는 Sling POST 서블릿을 사용하지 않습니다. 대신 값이 사용자 지정 서블릿에 게시됩니다.**com.adobe.cq.projects.impl.servlet.ProjectServlet**. 사용자 정의 필드를 추가할 때 고려해야 합니다.*

번역 프로젝트 템플릿에 대한 사용자 지정 마법사의 예는 다음과 같습니다. **/libs/cq/core/content/projects/wizard/translationproject/defaultproject**.

### 가젯 {#gadgets}

이 노드에는 추가 속성이 없지만 가젯 노드의 자식은 새 프로젝트를 만들 때 프로젝트 타일이 프로젝트 대시보드를 채우는 것을 제어합니다. [프로젝트 타일](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) (가젯 또는 포드라고도 함)은 프로젝트의 작업장을 채우는 간단한 카드입니다. ootb 타일의 전체 목록은**/libs/cq/gui/components/projects/admin/pod **프로젝트 소유자는 프로젝트를 만든 후 항상 타일을 추가/제거할 수 있습니다.

### 역할 {#roles}

모든 프로젝트에 대한 [기본 역할은](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject) 다음과 같습니다. **옵저버**, **편집자**&#x200B;및 **소유자**. 역할 노드 아래에 하위 노드를 추가하면 템플릿에 대한 비즈니스 특정 프로젝트 역할을 추가할 수 있습니다. 그런 다음 이러한 역할을 프로젝트와 연관된 특정 워크플로우에 연결할 수 있습니다.

### 워크플로우 {#workflows}

사용자 지정 프로젝트 템플릿을 만드는 가장 매력적인 이유 중 하나는 프로젝트에 사용할 수 있는 워크플로우를 구성할 수 있다는 것입니다. OOTB 워크플로우 또는 사용자 정의 워크플로우를 수행할 수 있습니다. 워크플로우 **노드** 아래에 사용 가능한 워크플로우 모델 **지정 아래에** 모델 `nt:unstructured`노드(또한)와 하위 노드가 있어야합니다. **modelId **는 /etc/workflow 아래의 워크플로우 모델을 가리키며 속성 **마법사는 워크플로우를 시작할 때 사용되는 대화 상자를 가리킵니다** . Projects의 큰 장점은 워크플로우 시작 시 사용자 정의 대화 상자(마법사)를 추가하여 비즈니스별 메타데이터를 캡처하여 워크플로우 내에서 추가 작업을 수행할 수 있다는 것입니다.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Creating a project template {#creating-project-template}

주로 노드를 복사/구성할 것이므로 CRXDE Lite을 사용합니다. 로컬 AEM 인스턴스에서 [CRXDE Lite을 엽니다](http://localhost:4502/crx/de/index.jsp).

1. 먼저 명명된 아래에 새 폴더를 `/apps/&lt;your-app-folder&gt;` 만듭니다 `projects`. 이름이 지정된 폴더 아래에 다른 폴더를 만듭니다 `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. 보다 쉽게 만들기 위해 기존 간단한 프로젝트 템플릿에서 사용자 지정 템플릿을 시작합니다.

   1. 1단계에서 만든 **템플릿** 폴더 아래에 노드/libs/cq/core/content/projects/templates/default ** 노드를 복사하여 붙여넣습니다.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. 이제 /apps/aem-guides/projects-tasks/projects/templates/authoring-project와 같은 경로가 있어야 합니다 ****.

   1. 작성자 프로젝트 노드의 **jcr:title** 및 **jcr:description** 속성을 사용자 지정 제목 및 설명 값으로 편집합니다.

      1. 기본 프로젝트 속성을 가리키는 **마법사** 속성을 그대로 둡니다.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. 이 프로젝트 템플릿의 경우 작업을 사용하려고 합니다.
   1. 작업이라는 저작-프로젝트/가젯 아래에 새로운 **nt:unstructured** 노드를 **추가합니다**.
   1. cardWeight **= &quot;100&quot;,** jcr:title **=&quot;Tasks&quot; 및 sling:resourceType******=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;에 대한 작업 노드에 문자열 속성을 추가합니다.

   이제 [새 프로젝트를 만들면 기본적으로 작업](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#Tasks) 타일이 표시됩니다.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. 프로젝트 템플릿에 사용자 지정 승인자 역할을 추가합니다.

   1. 프로젝트 템플릿(authoring-project) 노드 아래에 새 **nt:unstructured** 노드 레이블이 **지정된 노드를 추가합니다**.
   1. 다른 **nt:unstructured** 노드 레이블 승인자를 역할 노드의 하위로 추가합니다.
   1. 문자열 속성 **을** 추가합니다.**= &quot;** Title **** &quot;,**rolececector**= &quot; ******** ownerOwnerRoledid&quot;,Rolelass id=&quot;클래스 승인자&quot;를 추가합니다.
      1. 승인자 노드의 이름뿐만 아니라 jcr:title 및 roleid도 문자열 값(roleid가 고유한 경우)이 될 수 있습니다.
      1. **roleclass** 는 [3개의 OOTB 역할을 기준으로 해당 역할에 적용된 권한을 제어합니다](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User프로젝트에 역할 추가). **소유자**, **편집기**&#x200B;및 **관찰자**.
      1. 일반적으로 사용자 지정 역할이 관리 역할보다 더 많은 경우 롤렉이 **소유자가 될 수 있습니다.** 포토그래퍼 또는 디자이너와 같은 보다 구체적인 저작 역할이면 **편집자** 역할 클래스로 충분합니다. 소유자와 **편집자** 간의 큰 차이는 프로젝트 소유자가 프로젝트 속성을 업데이트하고 프로젝트에 새 사용자를 추가할 수 있다는 것입니다 **** .

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. 단순 프로젝트 템플릿을 복사하면 4개의 OOTB 워크플로우가 구성됩니다. 워크플로우/모델 아래의 각 노드는 특정 워크플로우와 해당 워크플로우의 시작 대화 상자를 가리킵니다. 이 튜토리얼의 후반부에서 이 프로젝트에 대한 사용자 정의 워크플로우를 만들어 봅니다. 이제 워크플로우/모델 아래의 노드를 삭제합니다.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. 컨텐츠 작성자가 프로젝트 템플릿을 쉽게 식별할 수 있도록 사용자 정의 축소판을 추가할 수 있습니다. 권장되는 크기는 319x319픽셀입니다.
   1. CRXDE Lite에서 **thumbnail.png라는 가젯, 역할 및 워크플로우 노드의 동위 멤버로 새 파일을 만듭니다**.
   1. 저장한 다음 `jcr:content` `jcr:data` 노드로 이동하고 속성을 두 번 클릭합니다(&#39;보기&#39;를 클릭하지 않음).
      1. 파일 편집 대화 상자가 표시되면 사용자 정의 축소판을 업로드할 수 있습니다. `jcr:data`

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

프로젝트 템플릿의 XML 표현을 완료했습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## 사용자 지정 프로젝트 템플릿 테스트

이제 새 프로젝트를 만들어 프로젝트 템플릿을 테스트할 수 있습니다.

1. 사용자 지정 템플릿은 프로젝트를 만드는 옵션 중 하나로 봐야 합니다.

   ![템플릿 선택](./assets/develop-aem-projects/choose-template.png)

1. 사용자 지정 템플릿을 선택한 후 &#39;다음&#39;을 클릭하고 프로젝트 구성원을 채울 때 이들을 승인자 역할로 추가할 수 있음을 확인합니다.

   ![승인](./assets/develop-aem-projects/user-approver.png)

1. &#39;만들기&#39;를 클릭하여 사용자 지정 템플릿을 기반으로 프로젝트 만들기를 완료합니다. 프로젝트 대시보드에서 작업 타일과 가젯 아래에 구성된 다른 타일이 자동으로 나타납니다.

   ![작업 타일](./assets/develop-aem-projects/tasks-tile.png)


## 워크플로우

일반적으로 승인 프로세스를 중심으로 하는 AEM 워크플로우에서는 참가자 워크플로우 단계를 사용했습니다. AEM Inbox에는 작업 및 워크플로우에 대한 세부 사항과 AEM 프로젝트와의 향상된 통합 기능이 포함되어 있습니다. 이러한 기능을 사용하면 프로젝트 만들기 작업 프로세스 단계를 사용하는 것이 더 매력적인 옵션이 됩니다.

### 작업 이유

기존 참가자 단계에 비해 작업 생성 단계를 사용하면 다음과 같은 이점이 있습니다.

* **시작 및 기한** - 작성자가 시간을 손쉽게 관리할 수 있도록 해주는 새로운 달력 기능을 사용하면 이러한 날짜를 사용할 수 있습니다.
* **우선 순위** - [낮음], [보통] 및 [높음]의 우선 순위가 설정되므로 작성자는 작업의 우선 순위를 지정할 수 있습니다
* **스레드 주석** - 작성자가 작업을 수행할 때 주석을 남겨 두고 공동 작업을 향상시킬 수 있습니다.
* **가시성** - 프로젝트 작업 타일 및 뷰를 통해 관리자는 소요 시간을 확인할 수 있습니다
* **프로젝트 통합** - 작업은 이미 프로젝트 역할 및 대시보드와 통합되었습니다.

참가자 단계와 마찬가지로 작업을 동적으로 할당하고 라우팅할 수 있습니다. 제목, 우선 순위와 같은 작업 메타데이터는 다음 튜토리얼에서 볼 수 있듯이 이전 작업을 기반으로 동적으로 설정할 수도 있습니다.

작업은 참가자 단계보다 몇 가지 이점이 있지만, 추가적인 간접비를 부담하므로 프로젝트 외부에서는 그다지 유용하지 않습니다. 또한 작업의 모든 동적 동작은 고유한 제한이 있는 ecma 스크립트를 사용하여 코딩되어야 합니다.

## 샘플 사용 사례 요구 사항 {#goals-tutorial}

![워크플로우 프로세스 다이어그램](./assets/develop-aem-projects/workflow-process-diagram.png)

위 다이어그램은 샘플 승인 워크플로우에 대한 높은 수준의 요구 사항에 대한 개요를 설명합니다.

첫 번째 단계는 컨텐츠 편집을 마치는 작업을 만드는 것입니다. 워크플로우 개시자는 이 첫 번째 작업의 담당자를 선택할 수 있습니다.

첫 번째 작업이 완료되면 할당자는 워크플로우를 라우팅하는 세 가지 옵션을 갖게 됩니다.

**일반 **- 일반적인 공정순서에 따라 검토 및 승인할 프로젝트의 승인자 그룹에 지정된 작업이 생성됩니다. 작업의 우선 순위는 일반이며 기한 날짜는 작성 후 5일입니다.

**러시** - 러시 라우팅을 사용하면 프로젝트의 승인자 그룹에 지정된 작업도 생성됩니다. 작업의 우선 순위는 높음이고 기한 날짜는 1일입니다.

**우회** - 이 샘플 워크플로우에서 초기 참가자는 승인 그룹을 우회하는 옵션을 사용할 수 있습니다. (예. &#39;승인&#39; 워크플로우의 목적을 달성할 수 없지만 추가 라우팅 기능을 설명할 수 있습니다.)

승인자 그룹은 컨텐츠를 승인하거나 재작업을 위해 초기 할당자에게 다시 보낼 수 있습니다. 재작업을 위해 다시 보내는 경우 새 작업이 만들어지고 &#39;재작업을 위해 다시 전송&#39; 레이블이 적절하게 표시됩니다.

워크플로우의 마지막 단계에서는 ootb 페이지/자산 활성화 프로세스 단계를 사용하고 페이로드를 복제합니다.

## 워크플로우 모델 만들기

1. AEM 시작 메뉴에서 도구 -> 워크플로우 -> 모델로 이동합니다. 오른쪽 상단 모서리의 &#39;만들기&#39;를 클릭하여 새 워크플로우 모델을 만듭니다.

   새 모델에 제목을 지정합니다.&quot;컨텐츠 승인 워크플로우&quot; 및 URL 이름:&quot;content-approval-workflow&quot;.

   ![워크플로우 만들기 대화 상자](./assets/develop-aem-projects/workflow-create-dialog.png)

   워크플로우 [만들기와 관련된 자세한 내용은 여기를 참조하십시오](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html).

1. 우수 사례 사용자 지정 워크플로우는 /etc/workflow/models 아래의 자체 폴더로 그룹화해야 합니다. CRXDE Lite에서 /etc/workflow/models named **&quot;aem-guides&quot;** 아래에 새 **&#39;nt:folder&#39;를 만듭니다**. 하위 폴더를 추가하면 업그레이드 또는 서비스 팩 설치 과정에서 사용자 정의 워크플로우를 실수로 덮어쓰지 않습니다.

   *전체 하위 폴더도 업그레이드 또는 서비스 팩으로 덮어쓸 수 있으므로 /etc/workflow/models/dam 또는 /etc/workflow/models/projects와 같은 otb 하위 폴더 아래에 폴더나 사용자 정의 워크플로우를 배치하지 않는 것이 중요합니다.

   ![6.3의 워크플로우 모델 위치](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   6.3의 워크플로우 모델 위치

   >[!NOTE]
   >
   >AEM 6.4+를 사용하는 경우 워크플로우의 위치가 변경되었습니다. 자세한 내용은 [여기를 참조하십시오.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   AEM 6.4+를 사용하는 경우 워크플로우 모델이 아래에 생성됩니다 `/conf/global/settings/workflow/models`. 위의 단계를 /conf 디렉토리와 함께 반복하고 이름이 지정된 하위 폴더를 추가하고 이 하위 폴더 `aem-guides` 에서 해당 하위 폴더를 `content-approval-workflow` 이동합니다.

   ![최신 워크플로우 정의 위치](./assets/develop-aem-projects/modern-workflow-definition-location.png)워크플로우 모델의 위치(6.4 이상)

1. AEM 6.3에서 도입된 기능은 지정된 워크플로우에 워크플로우 단계를 추가하는 기능입니다. 단계는 워크플로우 정보 탭의 받은 편지함에서 사용자에게 표시됩니다. 워크플로우의 현재 단계와 이전 단계 및 다음 단계를 사용자에게 보여 줍니다.

   단계를 구성하려면 SideKick에서 페이지 속성 대화 상자를 엽니다. 네 번째 탭은 &quot;단계&quot;로 표시되어 있습니다. 다음 값을 추가하여 이 워크플로우의 세 단계를 구성합니다.

   1. 컨텐츠 편집
   1. 승인
   1. 게시

   ![워크플로우 단계 구성](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   페이지 속성 대화 상자에서 워크플로우 단계를 구성합니다.

   ![워크플로 진행률 막대](./assets/develop-aem-projects/workflow-info-progress.png)

   AEM 받은 편지함에서 볼 수 있는 워크플로우 진행률 표시줄입니다.

   선택적으로 **이미지를** 사용자가 선택할 때 워크플로우 축소판으로 사용할 페이지 속성에 업로드할 수 있습니다. 이미지 크기는 319x319픽셀이어야 합니다. 페이지 **속성에** 설명을 추가하면 사용자가 워크플로우를 선택할 때도 표시됩니다.

1. 프로젝트 작업 만들기 워크플로우 프로세스는 워크플로우의 단계로 작업을 생성하도록 설계되었습니다. 작업을 완료한 후에만 워크플로우가 앞으로 이동합니다. 프로젝트 작업 생성 단계의 강력한 장점은 워크플로우 메타 데이터 값을 읽고 이러한 값을 사용하여 작업을 동적으로 만들 수 있다는 것입니다.

   먼저 기본적으로 생성되는 참가자 단계를 삭제합니다. 구성 요소 메뉴의 사이드 킥에서 **&quot;프로젝트&quot;** 하위 머리글을 **확장하고** &quot;프로젝트 작업 만들기&quot;를 모델에드래그하여 놓습니다.

   &quot;프로젝트 작업 만들기&quot; 단계를 두 번 클릭하여 워크플로우 대화 상자를 엽니다. 다음 속성을 구성합니다.

   이 탭은 모든 워크플로우 프로세스 단계에서 일반적으로 사용되며 제목 및 설명을 설정합니다(최종 사용자는 볼 수 없음). 설정할 중요한 속성은 드롭다운 메뉴에서 **&quot;컨텐트 편집&quot;으로** 워크플로우 스테이지입니다.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   프로젝트 작업 만들기 워크플로우 프로세스는 워크플로우의 단계로 작업을 생성하도록 설계되었습니다. 작업 탭에서는 작업의 모든 값을 설정할 수 있습니다. Adobe의 경우, Assignee가 역동적이기를 바라기 때문에 비워 둡니다. 나머지 속성 값:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   라우팅 탭은 작업을 완료하는 사용자에 대해 사용 가능한 작업을 지정할 수 있는 선택 대화 상자입니다. 이러한 작업은 단순히 문자열 값이며 워크플로우의 메타데이터에 저장됩니다. 이러한 값은 워크플로우의 후반부에 있는 스크립트 및/또는 프로세스 단계별로 읽어 워크플로우의 경로를 동적으로 &quot;라우트&quot;할 수 있습니다. 워크플로우 [목표를](#goals-tutorial) 기반으로 이 탭에 세 가지 작업을 추가합니다.

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   이 탭에서는 작업을 만들기 전에 프로그래밍 방식으로 다양한 작업 값을 결정할 수 있는 미리 만들기 작업 스크립트를 구성할 수 있습니다. 스크립트를 외부 파일에 가리키거나 대화 상자에 짧은 스크립트를 직접 포함시킬 수 있습니다. 이 경우 작업 사전 생성 스크립트를 외부 파일로 지정합니다. 5단계에서 해당 스크립트를 만듭니다.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. 이전 단계에서 작업 사전 생성 스크립트를 참조합니다. Adobe는 워크플로우 메타데이터 값 &quot;할당자&quot;의 값을 기반으로 이제 Task의 담당자를 설정하는 스크립트를&#x200B;**만듭니다**. &quot; **할당자&quot;** 값은 워크플로우가 시작될 때 설정됩니다. 또한 워크플로우 메타데이터를 읽고 워크플로우 메타데이터의 &quot;taskPriority&quot;**** 값 및 첫 번째 작업이 만기될 때 동적으로 설정되는 **&quot;taskDueDate&quot; **를 읽어 작업의 우선 순위를 동적으로 선택할 수 있습니다.

   조직에서 사용할 목적으로 모든 프로젝트 관련 스크립트를 저장할 수 있는 앱 폴더 아래에 폴더를 만들었습니다. **/apps/aem-guides/projects-tasks/projects/scripts**. 이 폴더 아래에 **&quot;start-task-config.ecma&quot;라는 새 파일을 만듭니다**. *start-task-config.ecma 파일의 경로가 4단계의 고급 설정 탭에 설정된 경로와 일치하는지 확인하십시오.

   파일의 컨텐츠로 다음을 추가합니다.

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. 컨텐츠 승인 워크플로우로 돌아갑니다. 작업 단계 **시작** 아래의 사이드 킥에 있는 OR 분할 **구성 요소(작업** 시작단계 아래)를 끌어다 놓습니다. 공통 대화 상자에서 3개 분기 라디오 단추를 선택합니다. OR 분할은 워크플로우 메타데이터 값 **&quot;lastTaskAction&quot;을** 읽고 워크플로우의 경로를 결정합니다. &quot;lastTaskAction&quot; **** 속성은 4단계에 구성된 라우팅 탭에서 값 중 하나로 설정됩니다. 각 분기 탭에 대해 다음 값으로 **스크립트** 텍스트 영역을 채웁니다.

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   *분기 스크립트에서 설정된 값이 4단계에서 설정된 경로 값과 일치하도록 직접 문자열 일치를 수행하여 경로를 확인합니다.

1. 다른 &quot;**프로젝트 작업**&#x200B;만들기&quot; 단계를 드래그하여 OR 분할 아래의 맨 왼쪽(분기 1)으로 모델에 놓습니다. 다음 속성으로 대화 상자를 채웁니다.

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   표준 승인 경로이므로 작업의 우선 순위가 보통으로 설정됩니다. 또한 승인자 그룹에 5일 동안 작업을 완료할 수 있습니다. Assignor는 [고급 설정] 탭에서 동적으로 할당하므로 [작업] 탭에 비어 있습니다. 이 작업을 완료하면 승인자 그룹에 다음 두 가지 가능한 경로를 제공합니다. **&quot;승인 및 게시&quot;** (컨텐츠가 승인되고 게시할 수 있으며 원래 편집기가 수정해야 하는 문제가 있는 경우 **&quot;수정** 시 다시 전송&quot;)을 수행할 수 있습니다. 승인자는 워크플로우가 자신에게 반환되는지 확인할 주석을 남길 수 있습니다.

이 자습서의 앞부분에서 승인자 역할을 포함하는 프로젝트 템플릿을 만들었습니다. 이 템플릿에서 새 프로젝트를 만들 때마다 승인자 역할에 대해 프로젝트별 그룹이 만들어집니다. 참가자 단계와 마찬가지로 작업은 사용자 또는 그룹에만 할당할 수 있습니다. 승인자 그룹에 해당하는 프로젝트 그룹에 이 작업을 할당하려고 합니다. 프로젝트 내에서 실행되는 모든 워크플로우에는 프로젝트 역할을 프로젝트 특정 그룹에 매핑하는 메타데이터가 포함됩니다.

**고급 설정 **탭 **의 스크립트** 텍스트 영역에 다음 코드를 복사하여 붙여넣습니다. 이 코드는 워크플로우 메타데이터를 읽고 프로젝트의 승인자 그룹에 작업을 지정합니다. 승인자 그룹 값을 찾을 수 없으면 관리자 그룹에 작업을 다시 할당할 수 없게 됩니다.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. 다른 &quot;**프로젝트 작업**&#x200B;만들기&quot; 단계를 드래그하여 OR 분할 아래의 중간 분기(분기 2)로 이동합니다. 다음 속성으로 대화 상자를 채웁니다.

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   이 경로가 러시아어 승인 경로이므로 작업의 우선 순위가 높음으로 설정됩니다. 또한 승인자 그룹에는 단 하루 동안만 작업을 완료할 수 있습니다. Assignor는 [고급 설정] 탭에서 동적으로 할당하므로 [작업] 탭에 비어 있습니다.

   7단계와 동일한 스크립트 조각을 다시 사용하여** 고급 설정 **탭의 **스크립트** 텍스트 영역을 채울 수 있습니다. Copy+아래 코드를 붙여넣습니다.

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. 맨 오른쪽 분기(분기 3)로 드래그하여* 작업 없음** 구성 요소를 드래그합니다. 작업 없음 구성 요소는 작업을 수행하지 않으며 즉시 고급 상태가 되어 승인 단계를 건너뛰고 싶은 원래 편집자의 욕구를 나타냅니다. 기술적으로 워크플로우 단계 없이 이 분기를 떠날 수 있지만, 작업 없음 단계를 추가하는 것이 가장 좋습니다. 이를 통해 다른 개발자는 분기 3의 용도를 명확하게 알 수 있습니다.

   워크플로우 단계를 두 번 클릭하고 제목 및 설명을 구성합니다.

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![워크플로우 모델 또는 분할](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   OR 분할의 세 개 분기가 모두 구성된 후에는 워크플로우 모델이 이와 같아야 합니다.

1. 승인자 그룹에는 추가 수정을 위해 워크플로우를 원래 편집기로 다시 보낼 수 있는 옵션이 있으므로 마지막으로 수행한 작업을 읽고 **워크플로우를 시작 부분에 전달하거나 계속 진행할 수 있도록 하기** 위해 goto 단계를 사용합니다.

   이동 단계 구성 요소(워크플로우 아래의 사이드 킥에서 찾음)가 다시 연결되는 OR 분할 아래의 드래그 앤 드롭 대화 상자에서 다음 속성을 두 번 클릭하고 구성합니다.

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   마지막으로 구성할 부분은 goto 프로세스 단계의 일부로 스크립트입니다. [스크립트] 값은 대화 상자를 통해 임베드하거나 외부 파일을 가리키도록 구성할 수 있습니다. 이동 스크립트에는 **함수 check()** 가 포함되어야 하며, 워크플로우가 지정된 단계로 이동해야 하는 경우 true를 반환해야 합니다. 워크플로우가 진행되면서 잘못된 결과가 반환됩니다.

   승인자 그룹이 **&quot;개정을 위해 뒤로 보내기&quot;** 작업(7단계 및 8단계에서 구성됨)을 선택하는 경우 워크플로우를 **&quot;작업 생성 시작&quot;** 단계로되돌립니다.

   프로세스 탭에서 스크립트 텍스트 영역에 다음 코드 조각을 추가합니다.

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. 페이로드를 게시하기 위해 페이지/자산 프로세스 **활성화 단계를** 사용합니다. 이 프로세스 단계에서는 구성이 거의 필요하지 않으며 활성화를 위해 워크플로우의 페이로드를 복제 큐에 추가합니다. 이동 단계 아래에 단계를 추가하며, 이렇게 하려면 승인자 그룹이 게시용 컨텐츠를 승인했거나 원래 편집자가 승인 무시 경로를 선택한 경우에만 도달할 수 있습니다.

   모델의 이동 단계 아래에 있는 **페이지/자산** 프로세스 활성화 단계(WCM 워크플로우 아래의 사이드 킥에 있음)를 드래그하여 놓습니다.

   ![워크플로우 모델 완료](assets/develop-aem-projects/workflow-model-final.png)

   이동 단계 및 페이지/자산 활성화 단계를 추가한 후 워크플로우 모델이 어떻게 표시되어야 합니까?

1. 승인자 그룹이 개정하기 위해 컨텐트를 다시 보내는 경우 원래 편집자에게 알리고자 합니다. 작업 생성 속성을 동적으로 변경하여 이를 수행할 수 있습니다. Adobe는 &quot;개정으로 다시 전송&quot;의 마지막ActionTaken 속성 값 **을 키웁니다**. 이 값이 있는 경우 제목과 설명을 수정하여 이 작업이 수정하기 위해 다시 전송된 컨텐츠의 결과임을 표시합니다. 또한 우선 순위를 **&quot;높음&quot;으로** 업데이트하여 편집자가 사용하는 첫 번째 항목입니다. 마지막으로 작업 만기 날짜를 수정하기 위해 워크플로우가 다시 전송된 날짜로 설정합니다.

   5단계에서 만든 시작 `start-task-config.ecma` 스크립트를 다음으로 바꿉니다.

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## &quot;워크플로우 시작&quot; 마법사 만들기 {#start-workflow-wizard}

프로젝트 내에서 워크플로우를 시작할 때는 마법사를 지정하여 워크플로우를 시작해야 합니다. 기본 마법사: `/libs/cq/core/content/projects/workflowwizards/default_workflow` 워크플로우 제목, 시작 주석 및 워크플로우 실행 페이로드 경로를 입력할 수 있습니다. 다음 몇 가지 다른 예도 있습니다. `/libs/cq/core/content/projects/workflowwizards`.

사용자 정의 마법사를 만들면 워크플로우가 시작되기 전에 중요한 정보를 수집할 수 있으므로 매우 강력할 수 있습니다. 데이터는 워크플로우의 메타데이터 및 워크플로우 프로세스의 일부로 저장되므로 이를 읽을 수 있고 입력한 값에 따라 동작을 동적으로 변경할 수 있습니다. 사용자 정의 마법사를 만들어 워크플로우 중 첫 번째 작업을 시작 마법사 값에 따라 동적으로 할당합니다.

1. CRXDE-Lite에서는 &quot;wizards&quot;라는 폴더 아래에 하위 폴더를 `/apps/aem-guides/projects-tasks/projects` 만듭니다. 다음 위치에서 기본 마법사를 복사합니다. `/libs/cq/core/content/projects/workflowwizards/default_workflow` 새로 만든 마법사 폴더에서 **content-approval-start로 이름을 변경합니다**. 전체 경로는 다음과 같아야 합니다. `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   기본 마법사는 워크플로우 모델의 제목, 설명 및 축소판이 선택된 첫 번째 열이 있는 2열 마법사입니다. 두 번째 열에는 워크플로우 제목, 주석 시작 및 페이로드 경로에 대한 필드가 포함됩니다. 이 마법사는 표준 터치 UI 양식이며 표준 [ [화강암 UI 양식] 구성 요소를](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/index.html) 사용하여 필드를 채웁니다.

   ![콘텐츠 승인 워크플로우 마법사](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. 워크플로우에서 첫 번째 작업의 담당자를 설정하는 데 사용할 추가 필드를 마법사에 추가합니다(워크플로우 모델 [만들기 참조](#create-workflow-model):5단계).

   아래의 `../content-approval-start/jcr:content/items/column2/items` &quot;assign&quot;이라는 이름의 `nt:unstructured` 새 유형 노드 **를 만듭니다**. [프로젝트 사용자 피커] 구성 요소를 사용하게 됩니다( [[화강암 사용자 피커 구성 요소]를 기반으로 함](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/userpicker/index.html)). 이 양식 필드를 사용하면 사용자 및 그룹 선택을 현재 프로젝트에 속하는 사용자만 제한할 수 있습니다.

   다음은 **assign** 노드의 XML 표현입니다.

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. 워크플로우에서 첫 번째 작업의 우선 순위를 결정하는 우선 순위 선택 필드도 추가합니다(워크플로우 모델 [만들기 참조](#create-workflow-model):5단계).

   아래 `/content-approval-start/jcr:content/items/column2/items` 에서 `nt:unstructured` priority라는 이름의 새 유형을 **만듭니다**. [ [화강암 UI 선택] 구성 요소를](https://docs.adobe.com/docs/en/aem/6-2/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/select/index.html) 사용하여 양식 필드를 채웁니다.

   우선 **순위** 노드 아래에 **nt:unstructured의** 항목 **노드를**&#x200B;추가합니다. 항목 노드 아래에 **노드** 3개를 더 추가하여 높음, 보통 및 낮음 선택 옵션을 채웁니다. 각 노드는 **nt:unstructured** 유형이며 **텍스트** 및 **값** 속성이 있어야 합니다. 텍스트와 값 모두 같은 값이어야 합니다.

   1. 높음
   1. 중간
   1. 낮음

   Medium 노드의 경우 &quot;**selected&quot;라는 이름의** 추가 부울 속성을 **true로 설정합니다**. 이렇게 하면 선택 필드의 기본값이 보통(Medium)이 됩니다.

   다음은 노드 구조 및 속성의 XML 표현입니다.

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. 워크플로우 초기자가 초기 작업의 기한을 설정할 수 있도록 허용합니다. [ [화강암 UI 날짜 선택](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/datepicker/index.html) 양식] 필드를 사용하여 이 입력을 캡처합니다. 또한 TypeHint가 있는 숨김 필드 [를 추가하여](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) 입력이 JCR에 Date 유형 속성으로 저장되도록 합니다.

   다음 속성이 XML로 **표시된 두 개의 nt:unstructured** 노드를 추가합니다.

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. 시작 마법사 대화 상자의 전체 코드를 [여기에서 볼 수 있습니다](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## 워크플로우 및 프로젝트 템플릿 연결 {#connecting-workflow-project}

워크플로우 모델이 프로젝트 중 하나에서 시작되도록 하는 것이 마지막으로 필요합니다. 이를 위해서는 이 시리즈의 1부에서 만든 프로젝트 템플릿을 다시 방문해야 합니다.

워크플로우 구성은 해당 프로젝트에 사용할 수 있는 워크플로우를 지정하는 프로젝트 템플릿의 영역입니다. 이 구성은 워크플로우 시작 마법사를 지정하여 워크플로우 시작( [이전 단계에서 생성됨)을 실행할 수도 있습니다](#start-workflow-wizard). 프로젝트 템플릿의 워크플로우 구성은 &quot;라이브&quot;입니다. 즉, 워크플로우 구성을 업데이트하면 해당 템플릿을 사용하는 기존 프로젝트뿐만 아니라 새로 만든 프로젝트에 영향을 줍니다.

1. CRXDE-Lite에서 에서 이전에 만든 제작 프로젝트 템플릿으로 이동합니다 `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   모델 노드 아래에 노드 유형이 **nt:unstructured** 인 contentapproval이라는 새 노드를 **추가합니다**. 노드에 다음 속성을 추가합니다.

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >AEM 6.4를 사용하는 경우 워크플로우의 위치가 변경되었습니다. 런타임 워크플로우 모델 `modelId` 의 위치를 가리킵니다. `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >워크플로우 위치의 변경에 대한 자세한 내용은 [여기를 참조하십시오.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. 콘텐츠 승인 워크플로우가 프로젝트 템플릿에 추가되면 프로젝트의 워크플로우 타일에서 시작할 수 있어야 합니다. Go and launch and play around with the various routs we created.

## 지원 자료

* [완성된 자습서 패키지 다운로드](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub의 전체 코드 저장소](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [AEM 프로젝트 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
