---
title: 데이터베이스 테이블 만들기
description: 양식 데이터 모델에서 사용할 데이터베이스 만들기
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
duration: 21
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# 데이터베이스 테이블 만들기

양식 데이터 모델은 RDBMS, RESTfull, SOAP 또는 OData 소스를 기반으로 할 수 있습니다. 이 교육 과정은 RDBMS 데이터 소스에서 지원하는 양식 데이터 모델을 사용하여 적응형 양식을 미리 작성하는 데 중점을 둡니다. 이 자습서에서는 MYSQL 데이터베이스를 사용했습니다. 사용 사례를 보여 주기 위해 다음 두 표를 만들었습니다

* **newhire** 테이블 - 이 테이블은 전체 정보를 저장합니다.

  ![전체](assets/newhire-table.png)


* **수혜자** 테이블 - 수혜자를 저장할 수 없습니다.

  ![수혜자](assets/beneficiaries-table.png)

MySQL Workbench를 사용하여 [sql 파일](assets/db-schema.sql)을 가져와 일부 샘플 데이터가 있는 테이블을 만들 수 있습니다.

## 다음 단계

[양식 데이터 모델 구성](./configuring-form-data-model.md)
