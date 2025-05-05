---
title: 양식 데이터 모델 구성
description: RDBMS 데이터 소스를 기반으로 양식 데이터 모델 만들기
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
duration: 103
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 0%

---

# 양식 데이터 모델 구성

## Apache Sling 연결의 풀링된 데이터 소스

RDBMS 지원 양식 데이터 모델을 만드는 첫 번째 단계는 Apache Sling 연결의 풀링된 데이터 소스를 구성하는 것입니다. 데이터 소스를 구성하려면 아래 단계를 따르십시오.

* 브라우저를 [configMgr](http://localhost:4502/system/console/configMgr)&#x200B;(으)로 지정
* **Apache Sling 연결의 풀링된 데이터 소스** 검색
* 새 항목을 추가하고 스크린샷에 표시된 대로 값을 제공합니다.
* ![데이터 원본](assets/data-source.png)
* 변경 사항 저장

>[!NOTE]
>JDBC 연결 URI, 사용자 이름 및 암호는 MySQL 데이터베이스 구성에 따라 변경됩니다.


## 양식 데이터 모델 만들기

* 브라우저를 [데이터 통합](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)&#x200B;(으)로 지정
* _만들기_->_양식 데이터 모델_&#x200B;을 클릭합니다.
* **직원**&#x200B;과 같은 양식 데이터 모델에 의미 있는 이름과 제목을 입력하십시오.
* _다음_ 클릭
* 이전 섹션(포럼)에서 만든 데이터 소스를 선택하십시오.
* 새로 만든 양식 데이터 모델을 편집 모드로 열려면 _만들기_->편집을 클릭합니다
* _포럼_ 노드를 확장하여 직원 스키마를 확인합니다. 직원 노드를 확장하여 2개의 테이블 보기

## 모델에 엔티티 추가

* 직원 노드가 확장되었는지 확인합니다.
* 필수 및 수혜자 엔터티를 선택하고 _선택한 항목 추가_&#x200B;를 클릭합니다.

## 새 엔터티에 읽기 서비스 추가

* 새 엔티티 선택
* _속성 편집_&#x200B;을 클릭합니다.
* 읽기 서비스 드롭다운 목록에서 가져오기 를 선택합니다.
* &#x200B;+ 아이콘을 클릭하여 get 서비스에 매개 변수를 추가합니다.
* 스크린샷에 표시된 대로 값을 지정합니다
* ![get-service](assets/get-service.png)
>[!NOTE]
> get 서비스에서는 새 엔터티의 empID 열에 매핑된 값을 예상합니다. 이 값을 전달하는 방법에는 여러 가지가 있으며 이 자습서에서는 empID가 empID라는 요청 매개 변수를 통해 전달됩니다.
>* get 서비스에 대한 인수를 저장하려면 _완료_&#x200B;를 클릭하십시오.
>* 양식 데이터 모델에 변경 사항을 저장하려면 _완료_&#x200B;를 클릭하십시오.

## 2개 엔티티 간 연결 추가

데이터베이스 엔티티 간에 정의된 연결은 양식 데이터 모델에서 자동으로 만들어지지 않습니다. 양식 데이터 모델 편집기를 사용하여 엔티티 간 연결을 정의해야 합니다. 모든 신생기업은 하나 이상의 수혜자를 가질 수 있으므로, 우리는 신생기업과 수혜자 개체 간의 일대다 연관을 정의해야 한다.
다음 단계는 일대다 연결을 만드는 프로세스를 안내합니다

* 새 엔터티를 선택하고 _연결 추가_&#x200B;를 클릭합니다.
* 아래 스크린샷과 같이 의미 있는 제목 및 식별자를 연결 및 기타 속성에 제공합니다
  ![연결](assets/association-entities-1.png)

* 인수 섹션 아래의 _편집_ 아이콘을 클릭합니다.

* 이 스크린샷에 표시된 대로 값을 지정합니다.
* ![연결-2](assets/association-entities.png)
* **수혜자 및 필수 엔터티의 empID 열을 사용하여 두 엔터티를 연결하고 있습니다.**
* 변경 내용을 저장하려면 _완료_&#x200B;를 클릭하십시오.

## 양식 데이터 모델 테스트

이제 양식 데이터 모델에 empID를 수락하고 뉴셔와 해당 수익자에 대한 세부 정보를 반환하는 **_get_** 서비스가 있습니다. 서비스 가져오기를 테스트하려면 아래 단계를 따르십시오.

* 새 엔티티 선택
* _테스트 모델 개체_ 클릭
* 유효한 empID를 입력하고 _테스트_&#x200B;를 클릭합니다.
* 아래 스크린샷에 표시된 대로 결과를 얻어야 합니다
* ![test-fdm](assets/test-form-data-model.png)

## 다음 단계

[URL에서 empID 가져오기](./get-request-parameter.md)