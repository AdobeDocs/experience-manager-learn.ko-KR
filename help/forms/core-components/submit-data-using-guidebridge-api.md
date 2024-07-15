---
title: GuideBridge API를 사용하여 양식 데이터 액세스
description: 핵심 구성 요소 기반 적응형 양식용 GuideBridge API를 사용하여 양식 데이터 및 첨부 파일에 액세스합니다.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-15286
last-substantial-update: 2024-04-05T00:00:00Z
exl-id: 099aaeaf-2514-4459-81a7-2843baa1c981
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# GuideBridge API를 사용하여 양식 데이터 POST

양식에 대한 &quot;저장 및 다시 시작&quot;에는 사용자가 양식 작성 진행 상황을 저장하고 나중에 다시 시작할 수 있도록 허용하는 작업이 포함됩니다.
이 사용 사례를 달성하려면 저장 및 검색을 위해 GuideBridge API를 사용하여 양식 데이터에 액세스하여 REST 엔드포인트로 전송해야 합니다.

양식 데이터는 규칙 편집기를 사용하여 단추의 클릭 이벤트에 저장됩니다
![규칙 편집기](assets/rule-editor.png)

다음 JavaScript 함수를 작성하여 지정된 엔드포인트로 데이터를 전송합니다

```javascript
/**
* Submits data and attachments 
* @name submitFormDataAndAttachments Submit form data and attachments to REST end point
* @param {string} endpoint in Stringformat
* @return {string} 
 */
 
 function submitFormDataAndAttachments(endpoint) {
    guideBridge.getFormDataObject({
        success: function(resultObj) {
            const afFormData = resultObj.data.data;
            const formData = new FormData();
            formData.append("dataXml", afFormData);
            resultObj.data.attachments.forEach(attachment => {
                console.log(attachment.name);
                formData.append(attachment.name, attachment.data);
            });
            fetch(endpoint, {
                method: 'POST',
                body: formData
            })
            .then(response => {
                if (response.ok) {
                    console.log("successfully saved");
                    const fld = guideBridge.resolveNode("$form.confirmation");
                    return "Form data was saved successfully";
                } else {
                    throw new Error('Failed to save form data');
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    });
}
```



## 서버측 코드

양식 데이터를 처리하기 위해 다음 서버측 Java 코드가 작성되었습니다. 다음은 위의 JavaScript에서 XHR 호출을 통해 호출되는 AEM에서 실행되는 Java 서블릿입니다.

```java
package com.azuredemo.core.servlets;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import javax.servlet.Servlet;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.Serializable;
import java.util.List;
@Component(
   service = {
      Servlet.class
   }
)
@SlingServletResourceTypes(
   resourceTypes = "azure/handleFormSubmission",
   methods = "POST",
   extensions = "json"
)
public class StoreFormSubmission extends SlingAllMethodsServlet implements Serializable {
   private static final long serialVersionUID = 1 L;
   private final transient Logger log = LoggerFactory.getLogger(this.getClass());
   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      List < RequestParameter > listOfRequestParameters = request.getRequestParameterList();
      log.debug("The size of list is " + listOfRequestParameters.size());
      for (int i = 0; i < listOfRequestParameters.size(); i++) {
         RequestParameter requestParameter = listOfRequestParameters.get(i);
         log.debug("is this request parameter a form field?" + requestParameter.isFormField());
         if (!requestParameter.isFormField()) {
            Document attachmentDOC = new Document(requestParameter.getInputStream());
            attachmentDOC.copyToFile(new File(requestParameter.getName()));
         } else {
            log.debug("Not a form field " + requestParameter.getName());
            log.debug(requestParameter.getString());
         }
      }
      response.setStatus(HttpServletResponse.SC_OK);
   }
}
```
