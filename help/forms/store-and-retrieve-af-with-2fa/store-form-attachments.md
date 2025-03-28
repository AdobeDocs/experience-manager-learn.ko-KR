---
title: 양식 첨부 파일 저장
description: 양식 첨부 파일을 추출하고 CRX 저장소의 새 위치에 저장합니다.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
duration: 65
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---

# 양식 첨부 파일 저장

적응형 양식에 첨부 파일을 추가하면 첨부 파일은 CRX 저장소의 임시 위치에 저장됩니다. 사용 사례가 제대로 작동하려면 양식 첨부 파일을 CRX 저장소의 새 위치에 저장해야 합니다.

OSGi 서비스는 양식 첨부 파일을 CRX 저장소의 새 위치에 저장하도록 만들어집니다. CRX에서 첨부 파일의 새 위치를 사용하여 새 파일 맵이 만들어지고 호출 애플리케이션으로 반환됩니다.
다음은 서블릿으로 전송되는 파일 맵입니다. 키는 적응형 양식 필드이고 값은 첨부 파일의 임시 위치입니다. 서블릿에서 첨부 파일을 추출하여 AEM 저장소의 새 위치에 저장하고 파일 맵을 새 위치로 업데이트합니다

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

요청에서 첨부 파일을 추출하여 **/content/afattachments** 폴더에 저장하는 코드는 다음과 같습니다

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

양식 첨부 파일의 업데이트된 위치가 포함된 새로운 FileMap입니다.

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## 다음 단계

[양식 데이터 저장](./store-form-data.md)