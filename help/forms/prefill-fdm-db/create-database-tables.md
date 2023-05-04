---
title: 데이터베이스 테이블 만들기
description: 양식 데이터 모델에서 사용할 데이터베이스 만들기
feature: Adaptive Forms
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# 데이터베이스 테이블 만들기

양식 데이터 모델은 RDBMS, RESTfull, SOAP 또는 OData 소스를 기반으로 할 수 있습니다. 이 교육 과정의 초점은 RDBMS 데이터 소스의 양식 데이터 모델을 사용하여 적응형 양식을 미리 작성하는 것입니다. 이 자습서를 위해 MYSQL 데이터베이스가 사용되었습니다. 사용 사례를 보여주기 위해 다음 두 표를 만들었습니다

* **신음** 표 - 이 표에는 전체 정보가 저장됩니다.

   ![신음](assets/newhire-table.png)


* **수혜자** 테이블 - 이 상점은 수혜자가 필요 없음

   ![수혜자](assets/beneficiaries-table.png)

을(를) 가져올 수 있습니다 [sql 파일](assets/db-schema.sql) mySQL Workbench를 사용하여 일부 샘플 데이터가 있는 테이블에 만듭니다.

## 다음 단계

[양식 데이터 모델 구성](./configuring-form-data-model.md)
