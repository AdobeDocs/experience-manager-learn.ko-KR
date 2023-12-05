---
title: 선택 그룹 구성 요소에 DAM 폴더 항목 추가
description: 선택 그룹 구성 요소에 항목을 동적으로 추가
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 29f56d13-c2e2-4bc2-bfdc-664c848dd851
duration: 122
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 선택 그룹 구성 요소에 동적으로 항목 추가

AEM Forms 6.5에는 CheckBox, 라디오 단추 및 이미지 목록과 같은 적응형 Forms 선택 그룹 구성 요소에 항목을 동적으로 추가하는 기능이 도입되었습니다. 이 문서에서는 선택 그룹 구성 요소를 DAM 폴더 콘텐츠로 채우는 사용 사례를 살펴봅니다. 스크린샷에서 3개의 파일은 newsletter라는 폴더에 있습니다.새 뉴스레터가 폴더에 추가될 때마다 선택 그룹 구성 요소가 업데이트되어 해당 콘텐츠가 자동으로 나열됩니다. 사용자는 다운로드할 하나 이상의 뉴스레터를 선택할 수 있습니다.

![규칙 편집기](assets/newsletters-download.png)

## DAM 폴더 컨텐츠를 반환하는 서블릿 만들기

다음 코드는 DAM 폴더 콘텐츠를 JSON 형식으로 반환하기 위해 작성되었습니다.

```java
package com.newsletters.core.servlets;
import static com.day.cq.commons.jcr.JcrConstants.JCR_CONTENT;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.Gson;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=get",
  "sling.servlet.paths=/bin/listfoldercontents"
})
public class ListFolderContent extends SlingSafeMethodsServlet {
  private static final long serialVersionUID = 1 L;
  private static final Logger log = LoggerFactory.getLogger(ListFolderContent.class);
  protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    Resource resource = request.getResourceResolver().getResource(request.getParameter("damFolder"));
    List < JsonObject > results = new ArrayList < > ();
    resource.getChildren().forEach(child -> {
      if (!JCR_CONTENT.equals(child.getName())) {
        JsonObject asset = new JsonObject();
        log.debug("##The child name is " + child.getName());
        asset.addProperty("assetname", child.getName());
        asset.addProperty("assetpath", child.getPath());
        results.add(asset);

      }
    });
    PrintWriter out = null;
    try {
      out = response.getWriter();
    } catch (IOException e) {

      log.debug(e.getMessage());
    }
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    Gson gson = new Gson();
    out.print(gson.toJson(results));
    out.flush();
  }

}
```

## JavaScript 기능으로 클라이언트 라이브러리 만들기

서블릿은 JavaScript 함수에서 호출됩니다. 함수는 선택 그룹 구성 요소를 채우는 데 사용할 배열 개체를 반환합니다.

```javascript
/**
 * Populate drop down/choice group  with assets from specified folder
 * @return {string[]} 
 */
function getDAMFolderAssets(damFolder) {
   // strUrl is whatever URL you need to call
   var strUrl = '/bin/listfoldercontents?damFolder=' + damFolder;
   var documents = [];
   $.ajax({
      url: strUrl,
      success: function(jsonData) {
         for (i = 0; i < jsonData.length; i++) {
            documents.push(jsonData[i].assetpath + "=" + jsonData[i].assetname);
         }
      },
      async: false
   });
   return documents;
}
```

## 적응형 양식 만들기

적응형 양식 만들기 및 양식과 클라이언트 라이브러리 연결 **listfolderassets**. 양식에 확인란 구성 요소를 추가합니다. 규칙 편집기를 사용하여 스크린샷에 표시된 대로 확인란의 옵션을 채웁니다
![옵션 설정](assets/set-options-newsletter.png)

이름이 인 JavaScript 함수를 호출하고 있습니다. **getDAMFolderAssets** 및 를 사용하여 DAM 폴더의 에셋 경로를 양식으로 나열할 수 있습니다.

## 다음 단계

[선택한 에셋 결합](./assemble-selected-newsletters.md)
