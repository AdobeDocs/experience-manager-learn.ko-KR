---
title: AEM Forms Workflow에서 쉼표로 구분된 문자열을 문자열 배열로 변환
description: 양식 데이터 모델에 입력 매개 변수 중 하나로 문자열이 있는 경우 양식 데이터 모델의 제출 액션을 호출하기 전에 적응형 양식의 제출 액션에서 생성된 데이터를 마사지해야 합니다.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-8507
exl-id: 9ad69407-2413-416f-9cec-43f88989b31d
last-substantial-update: 2021-06-09T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# 쉼표로 구분된 문자열을 문자열 배열로 변환 {#setting-value-of-json-data-element-in-aem-forms-workflow}

양식이 입력 매개 변수로 문자열 배열을 포함하는 양식 데이터 모델을 기반으로 하는 경우 제출된 적응형 양식 데이터를 조작하여 문자열 배열을 삽입해야 합니다. 예를 들어 확인란 필드를 문자열 배열 유형의 양식 데이터 모델 요소에 바인딩한 경우 확인란 필드의 데이터는 쉼표로 구분된 문자열 형식입니다. 아래 나열된 샘플 코드는 쉼표로 구분된 문자열을 문자열 배열로 바꾸는 방법을 보여 줍니다.

## 프로세스 단계 만들기

프로세스 단계는 AEM 워크플로에서 특정 논리를 실행하려는 경우 사용됩니다. 프로세스 단계는 ECMA 스크립트 또는 OSGi 서비스와 연결할 수 있습니다. 사용자 지정 프로세스 단계는 OSGi 서비스를 실행합니다.

제출된 데이터는 다음과 같은 형식입니다. businessUnits 요소의 값은 쉼표로 구분된 문자열로서, 문자열 배열로 변환해야 합니다.

![제출된 데이터](assets/submitted-data-string.png)

양식 데이터 모델과 연결된 나머지 끝점에 대한 입력 데이터에는 이 스크린샷에 표시된 대로 문자열 배열이 필요합니다. 프로세스 단계의 사용자 지정 코드는에서 제출된 데이터를 올바른 형식으로 변환합니다.

![fdm-string-array](assets/string-array-fdm.png)

프로세스 단계에 JSON 개체 경로 및 요소 이름을 전달합니다. 프로세스 단계의 코드는 요소의 쉼표로 구분된 값을 문자열 배열로 바꿉니다.
![프로세스 단계](assets/create-string-array.png)

>[!NOTE]
>
>적응형 양식의 제출 옵션에 있는 데이터 파일 경로가 &quot;Data.xml&quot;로 설정되어 있는지 확인합니다. 이는 프로세스 단계의 코드가 페이로드 폴더에서 Data.xml이라는 파일을 검색하기 때문입니다.

## 프로세스 단계 코드

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.google.gson.JsonArray;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

@Component(property = {
    Constants.SERVICE_DESCRIPTION + "=Create String Array",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Replace comma seperated string with string array"
})

public class CreateStringArray implements WorkflowProcess {
    private static final Logger log = LoggerFactory.getLogger(CreateStringArray.class);
    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
        log.debug("The string I got was ..." + arg2.get("PROCESS_ARGS", "string").toString());
        String[] arguments = arg2.get("PROCESS_ARGS", "string").toString().split(",");
        String objectName = arguments[0];
        String propertyName = arguments[1];

        String objects[] = objectName.split("\\.");
        System.out.println("The params is " + propertyName);
        log.debug("The params string is " + objectName);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload  in set Elmement Value in Json is  " + workItem.getWorkflowData().getPayload().toString());
        String dataFilePath = payloadPath + "/Data.xml/jcr:content";
        Session session = workflowSession.adaptTo(Session.class);
        Node submittedDataNode = null;
        try {
            submittedDataNode = session.getNode(dataFilePath);

            InputStream submittedDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(submittedDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();

            String inputStr;
            while ((inputStr = streamReader.readLine()) != null)
                stringBuilder.append(inputStr);
            JsonParser jsonParser = new JsonParser();
            JsonObject jsonObject = jsonParser.parse(stringBuilder.toString()).getAsJsonObject();
            System.out.println("The json object that I got was " + jsonObject);
            JsonObject targetObject = null;

            for (int i = 0; i < objects.length - 1; i++) {
                System.out.println("The object name is " + objects[i]);
                if (i == 0) {
                    targetObject = jsonObject.get(objects[i]).getAsJsonObject();
                } else {
                    targetObject = targetObject.get(objects[i]).getAsJsonObject();

                }

            }

            System.out.println("The final object is " + targetObject.toString());
            String businessUnits = targetObject.get(propertyName).getAsString();
            System.out.println("The values of " + propertyName + " are " + businessUnits);

            JsonArray jsonArray = new JsonArray();

            String[] businessUnitsArray = businessUnits.split(",");
            for (String name: businessUnitsArray) {
                jsonArray.add(name);
            }

            targetObject.add(propertyName, jsonArray);
            System.out.println(" After updating the property " + targetObject.toString());
            InputStream is = new ByteArrayInputStream(jsonObject.toString().getBytes());
            System.out.println("The changed json data  is " + jsonObject.toString());
            Binary binary = session.getValueFactory().createBinary(is);
            submittedDataNode.setProperty("jcr:data", binary);
            session.save();

        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

    }
}
```

샘플 번들은 다음과 같을 수 있습니다. [여기에서 다운로드됨](assets/CreateStringArray.CreateStringArray.core-1.0-SNAPSHOT.jar)
