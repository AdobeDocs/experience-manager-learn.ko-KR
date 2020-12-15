---
title: 데이터베이스 테이블 만들기
description: 양식 데이터 모델에서 사용할 데이터베이스 만들기
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# 데이터베이스 테이블 만들기

양식 데이터 모델은 RDBMS, RESTfull, SOAP 또는 OData 소스를 기반으로 할 수 있습니다. 이 과정의 초점은 RDBMS 데이터 소스의 양식 데이터 모델을 사용하여 적응형 양식을 미리 작성하는 데 중점을 두고 있습니다. 이 자습서를 위해 MYSQL 데이터베이스를 사용했습니다. 사용 사례를 보여주기 위해 다음 두 개의 표를 만들었습니다.

* **새** 테이블 - 이 표는 새 정보를 저장합니다.

   ![승_newhire](assets/newhire-table.png)


* **수익률** - 수익자가 필요한 스토어

   ![수혜자](assets/beneficiaries-table.png)

MySQL 워크벤치를 사용하여 [sql 파일](assets/db-schema.sql)을 가져와서 일부 샘플 데이터가 있는 테이블에 만들 수 있습니다.