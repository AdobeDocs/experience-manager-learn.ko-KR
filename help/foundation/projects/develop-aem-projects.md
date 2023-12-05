---
title: AEM에서 프로젝트 개발
description: AEM 프로젝트용으로 개발하는 방법을 설명하는 개발 자습서입니다.  이 튜토리얼에서는 콘텐츠 작성 워크플로 및 작업 관리를 위해 AEM 내에서 새 프로젝트를 만드는 데 사용할 수 있는 사용자 지정 프로젝트 템플릿을 만듭니다.
version: 6.4, 6.5
feature: Projects, Workflow
doc-type: Tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
duration: 1753
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '4460'
ht-degree: 0%

---

# AEM에서 프로젝트 개발

용으로 개발하는 방법을 보여 주는 개발 튜토리얼입니다. [!DNL AEM Projects].  이 튜토리얼에서는 콘텐츠 작성 워크플로 및 작업 관리를 위해 AEM 내에서 새 프로젝트를 만드는 데 사용할 수 있는 사용자 지정 프로젝트 템플릿을 만듭니다.

>[!VIDEO](https://video.tv.adobe.com/v/16904?quality=12&learn=on)

*이 비디오에서는 아래 자습서에서 만든 완료된 워크플로에 대한 간단한 데모를 제공합니다.*

## 소개 {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) 는 AEM Sites 또는 Assets 구현의 일부로 콘텐츠 생성과 관련된 모든 워크플로 및 작업을 보다 쉽게 관리하고 그룹화할 수 있도록 설계된 AEM의 기능입니다.

AEM Projects에는 여러 개가 포함되어 있습니다 [OOTB 프로젝트 템플릿](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates). 새 프로젝트를 만들 때 작성자는 사용 가능한 이러한 템플릿 중에서 선택할 수 있습니다. 고유한 비즈니스 요구 사항이 있는 대규모 AEM 구현에서는 요구 사항에 맞게 사용자 지정 프로젝트 템플릿을 만들어야 합니다. 사용자 지정 프로젝트 템플릿을 만들면 개발자는 프로젝트 대시보드를 구성하고, 사용자 지정 워크플로에 연결하고, 프로젝트에 대한 추가 비즈니스 역할을 만들 수 있습니다. 프로젝트 템플릿의 구조를 보고 샘플 템플릿을 만들겠습니다.

![사용자 정의 프로젝트 카드](./assets/develop-aem-projects/custom-project-card.png)

## 설정

이 튜토리얼에서는 사용자 지정 프로젝트 템플릿을 만드는 데 필요한 코드를 단계별로 설명합니다. 다음을 다운로드하여 설치할 수 있습니다. [첨부된 패키지](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) 튜토리얼과 함께 따를 로컬 환경입니다. 에서 호스팅되는 전체 Maven 프로젝트에 액세스할 수도 있습니다. [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [완료된 튜토리얼 패키지](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub의 전체 코드 저장소](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

이 자습서에서는 다음과 같은 몇 가지 기본 지식이 있다고 가정합니다 [AEM 개발 사례](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/the-basics.html) and some familiar친숙함 with [AEM Maven 프로젝트 설정](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ht-projects-maven.html). 언급된 모든 코드는 참조로 사용되기 위한 것이며 에만 배포해야 합니다. [로컬 개발 AEM 인스턴스](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted).

## 프로젝트 템플릿 구조

프로젝트 템플릿은 소스 제어에서 사용해야 하며 /apps 하의 애플리케이션 폴더 아래에 있어야 합니다. 이상적으로는 다음과 같은 명명 규칙을 사용하여 하위 폴더에 배치해야 합니다. **&#42;/projects/templates/**&lt;my-template>. 이 명명 규칙에 따라 배치하면 작성자는 프로젝트를 만들 때 새 사용자 지정 템플릿을 자동으로 사용할 수 있습니다. 사용 가능한 프로젝트 템플릿 구성은 다음 위치에 설정됩니다. **/content/projects/jcr:content** 노드의 노드 **cq:allowedTemplates** 속성. 기본적으로 정규 표현식입니다. **/(apps|libs)/.&#42;/projects/templates/.&#42;**

프로젝트 템플릿의 루트 노드에는 **jcr:primaryType** / **cq:Template**. 의 루트 노드 아래에는 3개의 노드가 있습니다. **가젯**, **역할**, 및 **워크플로**. 이 노드들은 모두 **nt:unstructured**. 또한 루트 노드 아래에는 프로젝트 만들기 마법사에서 템플릿을 선택할 때 표시되는 thumbnail.png 파일이 있을 수 있습니다.

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

프로젝트 템플릿의 루트 노드가 유형임 **cq:Template**. 이 노드에서 속성을 구성할 수 있습니다. **jcr:title** 및 **jcr:description** 프로젝트 만들기 마법사에 표시됩니다. 라는 속성도 있습니다. **마법사** 프로젝트의 속성을 채우는 폼을 가리킵니다. 기본값: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** 는 사용자가 기본 프로젝트 속성을 채우고 그룹 구성원을 추가할 수 있으므로 대부분의 경우 제대로 작동합니다.

*&#42;참고: 프로젝트 만들기 마법사는 Sling POST 서블릿을 사용하지 않습니다. 대신 값이 사용자 정의 서블릿에 게시됩니다.**com.adobe.cq.projects.impl.servlet.ProjectServlet**. 사용자 정의 필드를 추가할 때 이를 고려해야 합니다.*

번역 프로젝트 템플릿에 대한 사용자 지정 마법사의 예를 찾을 수 있습니다. **/libs/cq/core/content/projects/wizard/translationproject/defaultproject**.

### 가젯 {#gadgets}

이 노드에는 추가 속성이 없지만 가젯 노드의 하위 항목은 새 프로젝트를 만들 때 프로젝트의 대시보드를 채우는 프로젝트 타일을 제어합니다. [프로젝트 타일](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) (가젯 또는 포드라고도 함)는 프로젝트의 작업 공간을 채우는 간단한 카드입니다. ootb 타일의 전체 목록은 아래에서 찾을 수 있습니다 : **/libs/cq/gui/components/projects/admin/pod. **프로젝트가 생성된 후 프로젝트 소유자는 언제든지 타일을 추가/제거할 수 있습니다.

### 역할 {#roles}

3개 있습니다. [기본 역할](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject) 모든 프로젝트에 대해: **옵저버**, **편집자**, 및 **소유자**. 역할 노드 아래에 하위 노드를 추가하여 템플릿에 대한 비즈니스별 프로젝트 역할을 추가할 수 있습니다. 그런 다음 이러한 역할을 프로젝트와 연결된 특정 워크플로우에 연결할 수 있습니다.

### 워크플로 {#workflows}

사용자 지정 프로젝트 템플릿을 만드는 가장 매력적인 이유 중 하나는 프로젝트에서 사용할 수 있는 워크플로를 구성할 수 있다는 점입니다. OOTB 워크플로 또는 사용자 지정 워크플로일 수 있습니다. 아래 **워크플로** 노드가 있어야 합니다. **모델** 노드(또한 `nt:unstructured`) 및 아래의 하위 노드는 사용 가능한 워크플로우 모델을 지정합니다. **modelId**속성은 /etc/workflow 아래의 워크플로 모델 및 속성을 가리킵니다 **마법사** 은 워크플로를 시작할 때 사용되는 대화 상자를 가리킵니다. 프로젝트의 큰 장점은 워크플로가 시작될 때 비즈니스별 메타데이터를 캡처하는 사용자 지정 대화 상자(마법사)를 추가하여 워크플로 내에서 추가 작업을 추진할 수 있다는 것입니다.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## 프로젝트 템플릿 만들기 {#creating-project-template}

주로 노드를 복사/구성하기 때문에 CRXDE Lite을 사용합니다. 로컬 AEM 인스턴스에서 을 엽니다. [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. 아래에 새 폴더를 만들어 시작 `/apps/&lt;your-app-folder&gt;` 명명된 `projects`. 다음 이름의 폴더 아래에 다른 폴더 만들기 `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. 작업을 보다 쉽게 수행하기 위해 기존 단순 프로젝트 템플릿에서 사용자 지정 템플릿을 시작합니다.

   1. 노드 복사 및 붙여넣기 **/libs/cq/core/content/projects/templates/default** 아래에 *템플릿* 1단계에서 폴더를 만들었습니다.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. 이제 다음과 같은 경로가 있어야 합니다. **/apps/aem-guides/projects-tasks/projects/templates/authoring-project**.

   1. 편집 **jcr:title** 및 **jcr:description** 사용자 정의 제목 및 설명 값에 대한 작성자-프로젝트 노드의 속성입니다.

      1. 나가기 **마법사** 기본 프로젝트 속성을 가리키는 속성입니다.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. 이 프로젝트 템플릿의 경우 Tasks를 사용하겠습니다.
   1. 새 항목 추가 **nt:unstructured** authoring-project/gadgets 아래의 노드가 호출됨 **작업**.
   1. 작업 노드에 대한 문자열 속성 추가 **cardWeight** = &quot;100&quot;, **jcr:title**=&quot;Tasks&quot;, 및 **sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   이제 [작업 타일](https://experienceleague.adobe.com/docs/#Tasks) 새 프로젝트가 생성되면 기본적으로 표시됩니다.

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

   1. 프로젝트 템플릿(작성 프로젝트) 노드 아래에 **nt:unstructured** 레이블이 지정된 노드 **역할**.
   1. 다른 항목 추가 **nt:unstructured** 노드가 approvers를 roles 노드의 하위 노드로 레이블 지정했습니다.
   1. 문자열 속성 추가 **jcr:title** = &quot;**승인자**&quot;, **롤레클라스** =&quot;**소유자**&quot;, **장미빛**=&quot;**승인자**&quot;.
      1. jcr:title 및 roleid뿐만 아니라 승인자 노드의 이름도 모든 문자열 값이 될 수 있습니다(roleid가 고유한 경우).
      1. **롤레클라스** 은(는) 다음을 기반으로 해당 역할에 적용되는 권한을 제어합니다. [3개의 OOTB 역할](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User%20Roles%20in%20a%20Project): **소유자**, **편집자**, 및 **관찰자**.
      1. 일반적으로 사용자 정의 역할이 관리자 역할보다 크면 역할은 다음과 같을 수 있습니다. **소유자;** 사진 작가나 디자이너와 같은 보다 구체적인 작성 역할인 경우 **편집자** 롤렉스면 충분할 거야 의 큰 차이점 **소유자** 및 **편집자** 프로젝트 소유자는 프로젝트 속성을 업데이트하고 새 사용자를 프로젝트에 추가할 수 있습니다.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. 간단한 프로젝트 템플릿을 복사하면 4개의 OOTB 워크플로우가 구성됩니다. 워크플로우/모델 아래의 각 노드는 특정 워크플로우와 해당 워크플로우에 대한 대화 상자 시작 마법사를 가리킵니다. 나중에 이 자습서에서 이 프로젝트에 대한 사용자 지정 워크플로우를 만듭니다. 의 경우 워크플로/모델 아래의 노드를 삭제합니다.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. 콘텐츠 작성자가 프로젝트 템플릿을 쉽게 식별할 수 있도록 사용자 정의 썸네일을 추가할 수 있습니다. 권장 크기는 319x319픽셀입니다.
   1. CRXDE Lite에서 다음 가젯, 역할 및 워크플로 노드의 형제 파일로 새 파일을 만듭니다. **thumbnail.png**.
   1. 저장한 다음 `jcr:content` 노드를 클릭하고 `jcr:data` 속성(&#39;보기&#39;를 클릭하지 마십시오).
      1. 편집을 묻는 메시지가 표시됩니다. `jcr:data` 파일 대화 상자를 통해 사용자 지정 썸네일을 업로드할 수 있습니다.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

프로젝트 템플릿의 완료된 XML 표현:

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

1. 사용자 지정 템플릿이 프로젝트 만들기 옵션 중 하나로 표시됩니다.

   ![템플릿 선택](./assets/develop-aem-projects/choose-template.png)

1. 사용자 지정 템플릿을 선택한 후 &#39;다음&#39;을 클릭하고 프로젝트 구성원을 채울 때 승인자 역할로 추가할 수 있습니다.

   ![승인](./assets/develop-aem-projects/user-approver.png)

1. &#39;만들기&#39;를 클릭하여 사용자 지정 템플릿을 기반으로 하는 프로젝트 만들기를 완료합니다. 프로젝트 대시보드에서 작업 타일과 가젯에 구성된 다른 타일이 자동으로 표시된다는 것을 알 수 있습니다.

   ![작업 타일](./assets/develop-aem-projects/tasks-tile.png)


## 왜 워크플로우입니까?

일반적으로 승인 프로세스를 중심으로 하는 AEM 워크플로에서는 참가자 워크플로 단계를 사용했습니다. AEM 받은 편지함에는 작업 및 워크플로에 대한 세부 정보와 AEM 프로젝트와의 향상된 통합이 포함되어 있습니다. 이러한 기능을 통해 프로젝트 작업 만들기 프로세스 단계를 사용하는 것이 더 매력적인 옵션이 됩니다.

### 작업을 수행하는 이유

기존 참여자 단계보다 작업 생성 단계를 사용하면 다음과 같은 두 가지 이점이 있습니다.

* **시작 및 기한** - 작성자가 시간을 쉽게 관리할 수 있도록 해주는 새로운 캘린더 기능을 사용하면 이러한 날짜를 사용할 수 있습니다.
* **우선 순위** - 낮음, 보통, 높음의 우선 순위를 기본으로 하여 작성자는 작업의 우선 순위를 지정할 수 있습니다.
* **스레드된 주석** - 작성자가 작업을 수행할 때 의견을 남길 수 있으므로 공동 작업이 향상됩니다.
* **가시성** - 작업 타일 및 프로젝트 보기 를 통해 관리자는 체류 시간을 확인할 수 있습니다.
* **프로젝트 통합** - 작업이 프로젝트 역할 및 대시보드와 이미 통합되었습니다.

참여자 단계와 마찬가지로 임무를 동적으로 지정 및 경로지정할 수 있습니다. 다음 자습서에서 볼 수 있듯이 제목, 우선 순위 등의 작업 메타데이터는 이전 작업을 기반으로 동적으로 설정할 수도 있습니다.

작업은 참가자 단계에 비해 몇 가지 이점이 있지만 추가 오버헤드가 발생하며 프로젝트 외부에서 그다지 유용하지 않습니다. 또한 작업의 모든 동적 동작은 자체 제한 사항이 있는 ecma 스크립트를 사용하여 코딩해야 합니다.

## 샘플 사용 사례 요구 사항 {#goals-tutorial}

![워크플로우 프로세스 다이어그램](./assets/develop-aem-projects/workflow-process-diagram.png)

위의 다이어그램은 샘플 승인 작업 과정의 높은 수준 요구 사항을 간략하게 설명합니다.

첫 번째 단계는 콘텐츠 편집을 완료하기 위해 작업을 만드는 것입니다. 워크플로 개시자가 이 첫 번째 작업의 피할당자를 선택할 수 있도록 허용합니다.

첫 번째 작업이 완료되면 할당자는 세 가지 옵션을 사용하여 워크플로우를 라우팅합니다.

**일반 ** - 일반 공정순서는 검토 및 승인할 프로젝트의 승인자 그룹에 지정된 태스크를 생성합니다. 작업의 우선 순위는 정상이며 기한은 작업 생성 후 5일입니다.

**러시** - 러시 라우팅은 또한 프로젝트의 승인자 그룹에 할당된 작업을 만듭니다. 작업의 우선순위가 높고 기한은 1일에 불과합니다.

**우회** - 이 샘플 워크플로우에서는 초기 참가자가 승인 그룹을 우회할 수 있습니다. (예, &#39;승인&#39; 워크플로의 목적을 달성할 수 없지만 추가 라우팅 기능을 설명할 수 있습니다.)

승인자 그룹은 콘텐츠를 승인하거나 재작업을 위해 초기 할당자에게 콘텐츠를 다시 보낼 수 있습니다. 재작업을 위해 다시 전송되는 경우 새 작업이 생성되고 적절하게 &#39;재작업을 위해 다시 전송됨&#39;이라는 레이블이 지정됩니다.

워크플로우의 마지막 단계에서는 ootb 페이지/자산 활성화 프로세스 단계를 사용하고 페이로드를 복제합니다.

## 워크플로우 모델 만들기

1. AEM 시작 메뉴에서 도구 -> 워크플로 -> 모델로 이동합니다. 오른쪽 상단에 있는 &#39;만들기&#39;를 클릭하여 새 워크플로 모델을 만듭니다.

   새 모델에 제목: &quot;Content Approval Workflow&quot; 및 URL 이름: &quot;content-approval-workflow&quot;를 지정합니다.

   ![워크플로 만들기 대화 상자](./assets/develop-aem-projects/workflow-create-dialog.png)

   에 관한 추가 정보를 위하여 [워크플로우 만들기: 여기 참조](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html).

1. 가장 좋은 방법은 사용자 지정 워크플로를 /etc/workflow/models 아래에 있는 자체 폴더로 그룹화하는 것입니다. CRXDE Lite에서 새로 만들기 **&#39;nt:folder&#39;** /etc/workflow/models 아래에 **&quot;aem-guides&quot;**. 하위 폴더를 추가하면 업그레이드나 서비스 팩 설치 중에 사용자 지정 워크플로우를 실수로 덮어쓰지 않습니다.

   &#42;전체 하위 폴더는 업그레이드나 서비스 팩으로 덮어쓸 수 있으므로 폴더나 사용자 지정 워크플로를 /etc/workflow/models/dam 또는 /etc/workflow/models/projects와 같은 ootb 하위 폴더 아래에 배치하지 않는 것이 중요합니다.

   ![6.3의 워크플로 모델 위치](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   6.3의 워크플로 모델 위치

   >[!NOTE]
   >
   >AEM 6.4 이상을 사용하는 경우 워크플로 위치가 변경되었습니다. 다음을 참조하십시오 [자세한 내용은 여기 를 참조하십시오.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   AEM 6.4+를 사용하는 경우 워크플로 모델이 아래에 만들어집니다. `/conf/global/settings/workflow/models`. /conf 디렉토리로 위의 단계를 반복하고 라는 하위 폴더를 추가합니다. `aem-guides` 및 이동 `content-approval-workflow` 그 밑에..

   ![최신 워크플로 정의 위치](./assets/develop-aem-projects/modern-workflow-definition-location.png)
6.4+의 워크플로 모델 위치

1. 지정된 워크플로우에 워크플로우 단계를 추가하는 기능이 AEM 6.3에 도입되었습니다. 단계는 워크플로우 정보 탭의 받은 편지함에서 사용자에게 표시됩니다. 워크플로우의 현재 단계와 워크플로의 이전 및 이후 단계를 사용자에게 보여 줍니다.

   단계를 구성하려면 Sidekick에서 페이지 속성 대화 상자를 엽니다. 네 번째 탭에는 &quot;단계&quot;라는 레이블이 지정됩니다. 다음 값을 추가하여 이 워크플로우의 세 단계를 구성합니다.

   1. 콘텐츠 편집
   1. 승인
   1. 게시

   ![워크플로 단계 구성](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   페이지 속성 대화 상자에서 워크플로 단계를 구성합니다.

   ![워크플로우 진행률 표시줄](./assets/develop-aem-projects/workflow-info-progress.png)

   AEM 받은 편지함에서 볼 수 있는 워크플로 진행률 표시줄.

   선택적으로 다음을 업로드할 수 있습니다. **이미지** 을 추가합니다. 이미지 크기는 319x319픽셀이어야 합니다. 추가 **설명** 사용자가 워크플로우를 선택하려고 하면 페이지 속성에 대한 도 표시됩니다.

1. 프로젝트 작업 만들기 워크플로 프로세스는 워크플로의 단계로 작업을 만들도록 설계되었습니다. 작업을 완료한 후에만 워크플로가 진행됩니다. 프로젝트 만들기 작업 단계의 강력한 측면은 워크플로 메타데이터 값을 읽고 이를 사용하여 작업을 동적으로 만들 수 있다는 것입니다.

   먼저 기본적으로 생성되는 참가자 단계를 삭제합니다. 구성 요소 메뉴의 Sidekick에서 **&quot;프로젝트&quot;** 하위 제목 을 클릭하고 **&quot;프로젝트 작업 만들기&quot;** 모델에 연결합니다.

   &quot;프로젝트 작업 만들기&quot; 단계를 두 번 클릭하여 워크플로 대화 상자를 엽니다. 다음 속성을 구성합니다.

   이 탭은 모든 워크플로우 프로세스 단계에 공통적이며 제목 및 설명 을 설정합니다(최종 사용자는 볼 수 없음). 설정할 중요한 속성은 워크플로 단계입니다. **&quot;컨텐츠 편집&quot;** 드롭다운 메뉴에서 을(를) 선택합니다.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   프로젝트 작업 만들기 워크플로 프로세스는 워크플로의 단계로 작업을 만들도록 설계되었습니다. 작업 탭에서는 작업의 모든 값을 설정할 수 있습니다. 이 경우 피할당자가 동적이길 원하므로 비워 둡니다. 나머지 속성 값은 다음과 같습니다.

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   라우팅 탭은 작업을 완료하는 사용자에게 사용 가능한 작업을 지정할 수 있는 선택 대화 상자입니다. 이러한 작업은 문자열 값일 뿐이며 워크플로의 메타데이터에 저장됩니다. 이러한 값은 워크플로의 후반부에 있는 스크립트 및/또는 프로세스 단계를 통해 읽어서 워크플로를 동적으로 &quot;라우팅&quot;할 수 있습니다. 를 기반으로 함 [워크플로 목표](#goals-tutorial) 다음 세 가지 작업을 이 탭에 추가합니다.

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   이 탭에서는 작업을 만들기 전에 작업의 다양한 값을 프로그래밍 방식으로 결정할 수 있는 사전 만들기 작업 스크립트를 구성할 수 있습니다. 스크립트를 외부 파일로 지정하거나 대화 상자에 바로 짧은 스크립트를 포함하는 옵션이 있습니다. 이 예제에서는 미리 만들기 작업 스크립트를 외부 파일로 지정합니다. 5단계에서 해당 스크립트를 만듭니다.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. 이전 단계에서 사전 생성 작업 스크립트를 참조했습니다. 이제 이 스크립트를 만들어 워크플로 메타데이터 값 &quot;&quot;을(를) 기반으로 작업의 피할당자를 설정합니다.**할당자**&quot;. 다음 **&quot;할당자&quot;** 워크플로우를 시작할 때 값이 설정됩니다. 또한 워크플로 메타데이터를 읽고 &quot;&quot;을(를) 읽어서 작업의 우선 순위를 동적으로 선택합니다.**taskPriority&quot;** 워크플로우의 메타데이터 값과 **&quot;taskDueDate&quot; 값**첫 번째 작업이 만료될 때 동적으로 설정됩니다.

   조직적인 목적으로 모든 프로젝트 관련 스크립트를 보관할 폴더를 앱 폴더 아래에 만들었습니다. **/apps/aem-guides/projects-tasks/projects/scripts**. 이 폴더 아래에 새 파일 만들기 **&quot;start-task-config.ecma&quot;**. &#42;참고 start-task-config.ecma 파일의 경로가 4단계의 고급 설정 탭에 설정된 경로와 일치하는지 확인합니다.

   다음을 파일의 내용으로 추가합니다.

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

1. 콘텐츠 승인 워크플로로 다시 이동합니다. 드래그 앤 드롭 **OR 분할** 구성 요소(&#39;워크플로&#39; 카테고리 아래 Sidekick에 있음) **작업 시작** 단계. 공통 대화 상자에서 3 개의 분기에 대한 라디오 버튼을 선택합니다. OR 분할은 워크플로 메타데이터 값을 읽습니다. **&quot;lastTaskAction&quot;** 을 클릭하여 워크플로의 경로를 결정합니다. 다음 **&quot;lastTaskAction&quot;** 속성은 4단계에서 구성된 라우팅 탭의 값 중 하나로 설정됩니다. 각 분기 탭에 대해 **스크립트** 다음 값이 있는 텍스트 영역:

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

   &#42;참고: 경로를 결정하기 위해 직접 문자열 일치를 수행하고 있으므로 분기 스크립트에 설정된 값이 4단계에서 설정된 경로 값과 일치해야 합니다.

1. 다른 &quot; 드래그 앤 드롭&#x200B;**프로젝트 작업 만들기**&quot;OR 분할 아래의 맨 왼쪽(분기 1)에 있는 모델로 이동합니다. 다음 속성을 사용하여 대화 상자를 채웁니다.

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

   일반 승인 경로이므로 작업의 우선 순위가 보통으로 설정됩니다. 또한 승인자 그룹에 작업을 완료하는 데 5일이 제공됩니다. 할당자는 고급 설정 탭에서 동적으로 할당되므로 작업 탭에 비어 있습니다. 이 작업을 완료할 때 승인자 그룹에 두 가지 가능한 경로를 제공합니다. **&quot;승인 및 게시&quot;** 콘텐츠를 승인하고 게시할 수 있는 경우 **&quot;수정을 위해 다시 전송&quot;** 원본 편집기에서 수정해야 하는 문제가 있는 경우. 승인자는 워크플로가 반환되는지 여부를 원래 편집자가 보게 되는 주석을 달 수 있습니다.

이 자습서의 앞부분에서 승인자 역할을 포함한 프로젝트 템플릿을 만들었습니다. 이 템플릿에서 새 프로젝트가 생성될 때마다 승인자 역할에 대한 프로젝트별 그룹이 생성됩니다. 참가자 단계와 마찬가지로 작업도 사용자 또는 그룹에게만 할당할 수 있습니다. 승인자 그룹에 해당하는 프로젝트 그룹에 이 작업을 할당하려고 합니다. 프로젝트 내에서 실행되는 모든 워크플로에는 프로젝트 역할을 프로젝트별 그룹에 매핑하는 메타데이터가 있습니다.

Copy+다음 코드를 **스크립트** **고급 설정**탭의 텍스트 영역입니다. 이 코드는 워크플로 메타데이터를 읽고 프로젝트의 승인자 그룹에 작업을 할당합니다. 승인자 그룹 값을 찾을 수 없는 경우 관리자 그룹에 작업을 할당하는 방식으로 폴백됩니다.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. 다른 &quot; 드래그 앤 드롭&#x200B;**프로젝트 작업 만들기**&quot;OR 분할 아래의 중간 분기(분기 2)에 있는 모델로 이동합니다. 다음 속성을 사용하여 대화 상자를 채웁니다.

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

   이는 Rush Approval 경로이므로 작업의 우선 순위가 High로 설정됩니다. 또한 승인자 그룹에게 작업을 완료하는 데 단 하루만 제공합니다. 할당자는 고급 설정 탭에서 동적으로 할당되므로 작업 탭에 비어 있습니다.

   7단계에서와 동일한 스크립트 코드 조각을 다시 사용하여 **스크립트** 고급 설정 탭** 텍스트 **. 아래 코드를 복사하여 붙여 넣습니다.

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. 작업 없음** 구성 요소** 맨 오른쪽 분기(분기 3)로 드래그 앤 드롭합니다. 작업 없음 구성 요소는 작업을 수행하지 않고 승인 단계를 무시하려는 원래 편집기의 욕구를 나타내는 즉시 진행됩니다. 기술적으로 워크플로우 단계 없이 이 분기를 떠날 수 있지만 우수 사례로 작업 없음 단계를 추가합니다. 이는 Branch 3의 목적이 무엇인지 다른 개발자에게 명확하게 합니다.

   워크플로우 단계를 두 번 클릭하고 제목 및 설명 을 구성합니다.

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![워크플로우 모델 또는 분할](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   OR 분할의 세 분기가 모두 구성된 후에는 워크플로 모델이 다음과 같이 표시되어야 합니다.

1. 승인자 그룹에는 추가 개정을 위해 워크플로우를 원래 편집기로 다시 전송하는 옵션이 있으므로 **이동** 단계에서 수행한 마지막 작업을 읽고 워크플로를 시작 부분으로 라우팅하거나 계속하도록 합니다.

   다시 조인하는 OR 분할 아래에 있는 이동 단계 구성 요소(워크플로 아래의 Sidekick에 있음)를 드래그 앤 드롭합니다. 두 번 클릭하고 대화 상자에서 다음 속성을 구성합니다.

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   마지막으로 구성할 부분은 Goto 프로세스 단계의 일부로 스크립트입니다. 스크립트 값은 대화 상자를 통해 포함되거나 외부 파일을 가리키도록 구성할 수 있습니다. 이동 스크립트에는 **함수 check()** 워크플로우가 지정된 단계로 이동해야 하는 경우 true를 반환합니다. false를 반환하면 워크플로우가 진행됩니다.

   승인자 그룹이 **&quot;수정을 위해 다시 전송&quot;** 작업(7단계 및 8단계에서 구성)을 수행한 다음 워크플로우를 **&quot;작업 만들기 시작&quot;** 단계.

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

1. 페이로드를 게시하려면 ootb를 사용합니다. **페이지/자산 활성화** 프로세스 단계. 이 프로세스 단계는 구성이 거의 필요하지 않으며 활성화를 위해 워크플로의 페이로드를 복제 큐에 추가합니다. 이동 단계 아래에 단계를 추가하겠습니다. 이 방법으로 승인자 그룹이 게시할 콘텐츠를 승인하거나 원래 편집자가 승인 무시 경로를 선택한 경우에만 단계에 도달할 수 있습니다.

   드래그 앤 드롭 **페이지/자산 활성화** 모델의 이동 단계 아래에 있는 프로세스 단계(WCM 워크플로우 아래 Sidekick에 있음)입니다.

   ![워크플로우 모델 완료](assets/develop-aem-projects/workflow-model-final.png)

   이동 단계 및 페이지/자산 활성화 단계를 추가한 후 워크플로우 모델의 모습

1. 승인자 그룹이 수정을 위해 콘텐츠를 다시 전송하는 경우 원래 편집자에게 알리려고 합니다. Task 생성 속성을 동적으로 변경하여 이를 수행할 수 있습니다. 의 lastActionTaken 속성 값을 키로 사용하지 않습니다. **&quot;수정을 위해 다시 전송&quot;**. 해당 값이 있으면 제목과 설명을 수정하여 이 작업이 수정을 위해 다시 전송된 콘텐츠의 결과임을 나타냅니다. 우선 순위도 다음으로 업데이트하겠습니다. **&quot;높음&quot;** 따라서 편집자가 작업하는 첫 번째 항목입니다. 마지막으로 작업 기한을 수정을 위해 워크플로우가 다시 전송된 날짜로부터 하루로 설정합니다.

   시작 바꾸기 `start-task-config.ecma` 스크립트(5단계에서 생성):

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

프로젝트 내에서 워크플로를 시작할 때 워크플로를 시작할 마법사를 지정해야 합니다. 기본 마법사: `/libs/cq/core/content/projects/workflowwizards/default_workflow` 사용자는 워크플로 제목, 시작 설명 및 워크플로를 실행할 페이로드 경로를 입력할 수 있습니다. 아래에 몇 가지 다른 예제가 있습니다. `/libs/cq/core/content/projects/workflowwizards`.

워크플로가 시작되기 전에 중요한 정보를 수집할 수 있으므로 사용자 지정 마법사를 만드는 것은 매우 강력한 작업입니다. 데이터는 워크플로우의 메타데이터의 일부로 저장되며 워크플로우 프로세스는 이를 읽고 입력한 값에 따라 동작을 동적으로 변경할 수 있습니다. 시작 마법사 값에 따라 워크플로우의 첫 번째 작업을 동적으로 할당하는 사용자 지정 마법사를 만듭니다.

1. CRXDE-Lite에서는 아래에 하위 폴더를 만듭니다 `/apps/aem-guides/projects-tasks/projects` &quot;wizards&quot;라는 폴더입니다. 다음에서 기본 마법사를 복사합니다. `/libs/cq/core/content/projects/workflowwizards/default_workflow` 새로 만든 wizards 폴더 아래에서 이름을 로 바꿉니다. **content-approval-start**. 이제 전체 경로는 다음과 같습니다. `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   기본 마법사는 2열 마법사로, 워크플로 모델의 제목, 설명 및 썸네일을 표시하는 첫 번째 열이 선택되어 있습니다. 두 번째 열에는 워크플로우 제목, 시작 설명 및 페이로드 경로에 대한 필드가 포함되어 있습니다. 마법사는 표준 Touch UI 양식이며 표준을 사용합니다 [Granite UI 양식 구성 요소](https://experienceleague.adobe.com/docs/) 필드를 채웁니다.

   ![콘텐츠 승인 워크플로 마법사](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. 워크플로우에서 첫 번째 작업의 피할당자를 설정하는 데 사용되는 추가 필드를 마법사에 추가합니다( 참조) [워크플로우 모델 만들기](#create-workflow-model): 5단계).

   아래에 `../content-approval-start/jcr:content/items/column2/items` 유형의 새 노드 만들기 `nt:unstructured` 명명된 **&quot;assign&quot;**. 다음과같이 기반으로 하는 프로젝트 사용자 선택기 구성 요소를 사용합니다. [Granite 사용자 선택기 구성 요소](https://experienceleague.adobe.com/docs/)). 이 양식 필드를 사용하면 사용자 및 그룹 선택을 현재 프로젝트에 속하는 사용자만 제한할 수 있습니다.

   다음은 의 XML 표현입니다. **할당** 노드:

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

1. 또한 워크플로우에서 첫 번째 작업의 우선 순위를 결정하는 우선 순위 선택 필드를 추가합니다( 참조) [워크플로우 모델 만들기](#create-workflow-model): 5단계).

   아래에 `/content-approval-start/jcr:content/items/column2/items` 유형의 새 노드 만들기 `nt:unstructured` 명명된 **우선 순위**. 다음을 사용합니다. [Granite UI 선택 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html) 을 클릭하여 양식 필드를 채웁니다.

   아래 **우선 순위** 노드를 추가하겠습니다. **개 항목** 노드 **nt:unstructured**. 아래 **개 항목** 노드 3개의 노드를 추가하여 높음, 중간, 낮음에 대한 선택 옵션을 채웁니다. 각 노드의 유형은 다음과 같습니다 **nt:unstructured** 및 이(가) 있어야 함 **텍스트** 및 **값** 속성. 텍스트와 값 모두 동일한 값이어야 합니다.

   1. 높음
   1. 중간
   1. 낮음

   Medium 노드의 경우 &quot;&quot;라는 이름의 추가 부울 속성을 추가합니다.**선택됨&quot;** 값이 로 설정된 경우 **true**. 이렇게 하면 선택 필드의 기본값이 미디엄으로 지정됩니다.

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

1. 워크플로우 개시자가 초기 작업의 기한을 설정할 수 있도록 허용합니다. 다음을 사용합니다. [Granite UI DatePicker](https://experienceleague.adobe.com/docs/) 이 입력을 캡처할 양식 필드. 또한 가 있는 숨겨진 필드를 추가합니다. [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) 입력이 JCR에서 날짜 유형 속성으로 저장되도록 합니다.

   두 개 추가 **nt:unstructured** xml의 아래에 표시되는 속성이 다음과 같은 노드:

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

1. 마법사 시작 대화 상자의 전체 코드를 볼 수 있습니다 [여기](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## 워크플로우 및 프로젝트 템플릿 연결 {#connecting-workflow-project}

마지막으로 해야 할 일은 프로젝트 중 하나에서 워크플로우 모델을 시작할 수 있도록 하는 것입니다. 이렇게 하려면 이 시리즈의 1부에서 만든 프로젝트 템플릿을 다시 방문해야 합니다.

워크플로 구성은 해당 프로젝트에 사용할 수 있는 워크플로를 지정하는 프로젝트 템플릿의 영역입니다. 또한 구성에서 만든 워크플로우를 시작할 때 워크플로우 시작 마법사를 지정합니다. [이전 단계)](#start-workflow-wizard). 프로젝트 템플릿의 워크플로 구성은 &quot;라이브&quot;입니다. 즉, 워크플로 구성을 업데이트하면 새 프로젝트가 만들어지고 템플릿을 사용하는 기존 프로젝트가 영향을 받습니다.

1. CRXDE-Lite에서 이전에 만든 제작 프로젝트 템플릿으로 이동합니다. `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   모델 노드 아래에 라는 새 노드를 추가합니다. **contentapproval** 노드 유형이 다음과 같은 경우 **nt:unstructured**. 노드에 다음 속성을 추가합니다.

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >AEM 6.4를 사용하는 경우 워크플로우의 위치가 변경되었습니다. 가리킨 항목 `modelId` 속성을 런타임 워크플로 모델의 위치에 추가합니다. `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >다음을 참조하십시오 [워크플로우의 위치 변경에 대한 자세한 내용은 여기 를 참조하십시오.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. 콘텐츠 승인 워크플로가 프로젝트 템플릿에 추가되면 프로젝트의 워크플로 타일에서 시작할 수 있습니다. 계속 진행하여 우리가 만든 다양한 라우팅을 실행하고 재생하십시오.

## 지원 자료

* [완료된 튜토리얼 패키지 다운로드](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [GitHub의 전체 코드 저장소](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [AEM 프로젝트 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
