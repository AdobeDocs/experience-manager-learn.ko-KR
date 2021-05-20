---
title: 데이터베이스 테이블 만들기
description: 양식 데이터 모델에서 사용할 데이터베이스 만들기
feature: 적응형 양식
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 2%

---


# 데이터베이스 테이블 만들기

양식 데이터 모델은 RDBMS, RESTfull, SOAP 또는 OData 소스를 기반으로 할 수 있습니다. 이 교육 과정의 초점은 RDBMS 데이터 소스의 양식 데이터 모델을 사용하여 적응형 양식을 미리 작성하는 것입니다. 이 자습서를 위해 MYSQL 데이터베이스가 사용되었습니다. 사용 사례를 보여주기 위해 다음 두 표를 만들었습니다

* **** newhiretable - 이 테이블에는 다음과 같은 정보가 저장됩니다.

   ![신음](assets/newhire-table.png)


* **** 혜택 가능 - 이 상점은 수익자가 필요합니다

   ![수혜자](assets/beneficiaries-table.png)

MySQL Workbench를 사용하여 [sql 파일](assets/db-schema.sql)을 가져와 일부 샘플 데이터가 있는 테이블에 만들 수 있습니다.