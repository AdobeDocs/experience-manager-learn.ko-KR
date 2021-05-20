---
title: 받은 편지함 사용자 지정
description: 워크플로우의 추가 데이터를 표시하려면 사용자 지정 열을 추가하십시오
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---


# 사용자 지정 열 추가

받은 편지함에 워크플로우 데이터를 표시하려면 워크플로우에서 변수를 정의하고 채워야 합니다. 작업을 사용자에게 할당하기 전에 변수 값을 설정해야 합니다. 먼저 AEM 서버에 배포할 준비가 된 샘플 워크플로우를 제공했습니다.

* [AEM에 로그인](http://localhost:4502/crx/de/index.jsp)
* [검토 워크플로우 가져오기](assets/review-workflow.zip)
* [워크플로우 검토](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

이 워크플로우에는 두 개의 변수가 정의되어 있으며(isWrided 및 imvenue) 값이 설정된 변수 구성 요소를 사용하여 설정됩니다. 이러한 변수는 AEM 받은 편지함에 추가할 열로 사용할 수 있습니다

## 서비스 만들기

받은 편지함에 표시해야 하는 모든 열에 대해 서비스를 작성해야 합니다. 다음 서비스를 통해 isWrided 변수의 값을 표시하는 열을 추가할 수 있습니다

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

* [AEM 웹 콘솔에 로그인합니다.](http://localhost:4502/system/console/bundles)
* [받은 편지함 사용자 지정 번들 배포 및 시작](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [받은 편지함 열기](http://localhost:4502/aem/inbox)
* _만들기_ 단추 옆에 있는 _목록 보기_ 아이콘을 클릭하여 관리 컨트롤을 엽니다
* 받은 편지함에 기여 열 추가 및 변경 내용 저장
* [FormsAndDocuments UI로 이동](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Createmenu 메뉴](assets/snap-form.zip) 에서 파일 업로드 _를 선택하여 샘플_ 형식을  __ 가져옵니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* _혼인 상태_를 선택하고 양식을 제출합니다
   [받은 편지함 보기](http://localhost:4502/aem/inbox)

양식을 제출하면 워크플로우가 트리거되고 작업이 &quot;관리자&quot; 사용자에게 할당됩니다. 이 스크린샷에 표시된 대로 기별 열 아래에 값이 표시됩니다

![기혼자](assets/married-column.PNG)
