---
title: 사용자 정의 열 추가
description: 사용자 정의 열을 추가하여 워크플로우의 추가 데이터 표시
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
duration: 75
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---

# 사용자 정의 열 추가

받은 편지함에 워크플로우 데이터를 표시하려면 워크플로우에서 변수를 정의하고 채워야 합니다. 작업을 사용자에게 할당하기 전에 변수 값을 설정해야 합니다. 먼저 AEM 서버에 배포할 준비가 된 샘플 워크플로우를 제공했습니다.

* [AEM에 로그인](http://localhost:4502/crx/de/index.jsp)
* [검토 워크플로우 가져오기](assets/review-workflow.zip)
* [워크플로 검토](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

이 워크플로우에는 두 개의 변수(isMarked 및 income)가 정의되어 있으며 변수 설정 구성 요소를 사용하여 값이 설정됩니다. 이러한 변수는 AEM 받은 편지함에 추가할 열로 사용할 수 있습니다

## 서비스 만들기

받은 편지함에 표시해야 하는 모든 열에 대해 서비스를 작성해야 합니다. 다음 서비스를 통해 isMarked 변수의 값을 표시하는 열을 추가할 수 있습니다

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>위의 코드가 작동하려면 프로젝트에 AEM 6.5.5 Uber.jar를 포함해야 합니다

![uber-jar](assets/uber-jar.PNG)

## 서버에서 테스트

* [AEM 웹 콘솔에 로그인](http://localhost:4502/system/console/bundles)
* [받은 편지함 사용자 지정 번들 배포 및 시작](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [받은 편지함 열기](http://localhost:4502/aem/inbox)
* _만들기_ 단추 옆에 있는 _목록 보기_ 아이콘을 클릭하여 Admin Control 열기
* 받은 편지함에 기혼 열 추가 및 변경 내용 저장
* [FormsAndDocuments UI로 이동](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* _만들기_ 메뉴에서 _파일 업로드_&#x200B;를 선택하여 [샘플 양식 가져오기](assets/snap-form.zip)
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* _결혼 상태_를 선택하고 양식을 제출하세요.
  [받은 편지함 보기](http://localhost:4502/aem/inbox)

양식을 제출하면 워크플로우가 트리거되고 작업이 &quot;관리자&quot; 사용자에게 할당됩니다. 이 스크린샷에 표시된 대로 기혼 열 아래에 값이 표시됩니다

![기혼 열](assets/married-column.PNG)

## 다음 단계

[기혼 열 표시](./use-sightly-template.md)
