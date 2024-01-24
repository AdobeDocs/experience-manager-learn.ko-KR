---
title: 양식 데이터 저장
description: 데이터베이스에 새 첨부 파일 맵과 함께 양식 데이터 저장
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6538
thumbnail: 6538.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 2bd9fe63-8f42-4b89-95a0-13ade49bc31b
duration: 50
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 2%

---

# 양식 데이터 저장

다음 단계는 적응형 양식 데이터 및 관련 첨부 파일 정보를 저장하기 위해 데이터베이스에 새 행을 삽입하는 서비스를 만드는 것입니다.
다음 스크린샷은 데이터베이스의 행을 보여 줍니다.


![샘플 행](assets/sample-row.JPG)


다음 코드는 적절한 데이터가 있는 새 행을 데이터베이스에 삽입합니다

```java
public String storeFormData(String formData, String attachmentsInfo, String telephoneNumber) {
    log.debug("******Inside my AEMFormsWith DB service*****");
    log.debug("### Inserting data ... " + formData + "and the telephone number to insert is  " + telephoneNumber);
    String insertRowSQL = "INSERT INTO aemformstutorial.formdatawithattachments(guid,afdata,attachmentsInfo,telephoneNumber) VALUES(?,?,?,?)";
    UUID uuid = UUID.randomUUID();
    String randomUUIDString = uuid.toString();
    log.debug("The insert query is " + insertRowSQL);
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {
        pstmt = null;
        pstmt = c.prepareStatement(insertRowSQL);
        pstmt.setString(1, randomUUIDString);
        pstmt.setString(2, formData);
        pstmt.setString(3, attachmentsInfo);
        pstmt.setString(4, telephoneNumber);
        log.debug("Executing the insert statment  " + pstmt.executeUpdate());
        c.commit();
    } catch (SQLException e) {

        log.error("unable to insert data in the table", e.getMessage());
    } finally {
        if (pstmt != null) {
            try {
                pstmt.close();
            } catch (SQLException e) {
                log.debug("error in closing prepared statement " + e.getMessage());
            }
        }
        if (c != null) {
            try {
                c.close();
            } catch (SQLException e) {
                log.debug("error in closing connection " + e.getMessage());
            }
        }
    }
    return randomUUIDString;
}
```

## 다음 단계

[저장 및 종료 구현](./create-servlet.md)

