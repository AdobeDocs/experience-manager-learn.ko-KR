---
title: 받은 편지함 사용자 지정
description: sightly 템플릿을 사용하여 워크플로우의 추가 데이터를 표시하려면 사용자 지정 열을 추가합니다
feature: 적응형 양식
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: 개발
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 1%

---

# sightly 템플릿을 사용하여 받은 편지함 데이터 표시

sightly 템플릿을 사용하여 받은 편지함 열에 표시할 데이터의 형식을 지정할 수 있습니다. 이 예에서는 소득 열의 값에 따라 coral-ui 아이콘이 표시됩니다. 다음 스크린샷에서는 소득 열에서 아이콘 사용을 보여 줍니다
![수입-icon](assets/income-column.PNG)

[사용자 ](assets/sightly-template.zip) 지정 coral ui 아이콘을 표시하는 데 사용되는 sightly 템플릿은 이 문서의 일부로 제공됩니다.

## Sightly 템플릿

다음은 sightly 템플릿입니다. 템플릿에 있는 코드는 수입에 따라 아이콘을 표시합니다. 아이콘은 AEM과 함께 제공되는 [coral ui 아이콘 라이브러리](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons)의 일부로 사용할 수 있습니다.

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## 서비스 구현

다음 코드는 수입 열을 표시하는 서비스 구현입니다.

12라인은 열을 sightly 템플릿과 연결합니다

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## 서버에서 테스트

>[!NOTE]
>
>이 문서에서는 사용자가 이 시리즈의 이전 문서](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.md)에서 [샘플 워크플로우](assets/review-workflow.zip) 및 [샘플 양식](assets/snap-form.zip)을 설치했다고 가정합니다.[

* [관리 사용자로 crx에 로그인](http://localhost:4502/crx/de/index.jsp)
* [sightly 템플릿 가져오기](assets/sightly-template.zip)
* [AEM 웹 콘솔에 로그인합니다.](http://localhost:4502/system/console/bundles)
* [받은 편지함 사용자 지정 번들 배포 및 시작](assets/income-column-customization.jar)
* [받은 편지함 열기](http://localhost:4502/aem/inbox)
* 만들기 단추 옆에 있는 목록 보기를 클릭하여 Admin Control을 엽니다.
* 받은 편지함에 소득 열을 추가하고 변경 사항을 저장합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* _혼인 상태_&#x200B;를 선택하고 양식을 제출합니다
* [받은 편지함 보기](http://localhost:4502/aem/inbox)

양식을 제출하면 워크플로우가 트리거되고 작업이 &quot;관리자&quot; 사용자에게 할당됩니다. 수입 열 아래에 적절한 아이콘이 표시됩니다
