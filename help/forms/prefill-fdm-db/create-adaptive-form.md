---
title: 적응형 양식 만들기
description: 양식 데이터 모델의 자동 완성 서비스를 사용하도록 적응형 양식 만들기 및 구성
feature: 적응형 양식
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: 개발
role: 비즈니스 전문가
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 2%

---


# 적응형 양식 만들기

지금까지 다음을 만들었습니다.

* 2개의 테이블이 있는 데이터베이스 - `newhire` 및 `beneficiaries`
* 구성된 Apache Sling 연결 풀링된 DataSource
* RDBMS 기반 양식 데이터 모델

다음 단계는 양식 데이터 모델을 사용하도록 적응형 양식을 만들고 구성하는 것입니다.  [샘플 양식을 다운로드하여 가져올 수 있습니다. ](assets/fdm-demo-af.zip) 샘플 양식에는 사원 상세내역을 표시하는 섹션과 사원의 수익자를 나열하는 다른 섹션이 있습니다.

## 양식 데이터 모델과 양식 연결

이 강좌와 함께 제공된 샘플 양식이 양식 데이터 모델과 연결되어 있지 않습니다. 양식 데이터 모델을 사용하도록 양식을 구성하려면 다음을 수행해야 합니다.

* FDMDemo 양식 선택
* _속성_->_양식 모델_&#x200B;을 클릭합니다.
* 드롭다운 목록에서 양식 데이터 모델 선택
* 이전 단원에서 만든 양식 데이터 모델을 검색하고 선택합니다.
* _저장 및 닫기_&#x200B;를 클릭합니다.

## 프리플라이트 서비스 구성

첫 번째 단계는 양식 자동 완성 서비스를 연결하는 것입니다. 자동 완성 서비스를 연결하려면 아래 단계를 따르십시오

* `FDMDemo` 양식 선택
* _편집_&#x200B;을 클릭하여 편집 모드에서 양식을 엽니다.
* 컨텐츠 계층에서 양식 컨테이너를 선택하고 렌치 아이콘을 클릭하여 속성 시트를 엽니다
* 프리플라이트 서비스 드롭다운 목록에서 _양식 데이터 모델 프리플라이트 서비스_&#x200B;를 선택합니다.
* 파란색 ☑을 클릭하여 변경 내용을 저장합니다.

* ![프리필서비스](assets/fdm-prefill.png)

## 직원 세부 사항 구성

다음 단계는 적응형 양식의 텍스트 필드를 양식 데이터 모델 요소로 바인딩하는 것입니다. 다음 필드의 속성 시트를 열고 아래와 같이 bindRef를 설정해야 합니다


| 필드 이름 | 바인딩 참조 |
|------------|--------------------|
| 이름 | /newhire/FirstName |
| 성 | /newhire/lastName |

>[!NOTE]
>
>추가 텍스트 필드를 추가하여 적절한 양식 데이터 모델 요소에 바인딩할 수 있습니다.

## 수혜자 테이블 구성

다음 단계는 사원의 수혜자를 테이블 형식으로 표시하는 것입니다. 제공된 샘플 양식에는 4개의 열과 단일 행이 있는 표가 있습니다. 수혜자 수에 따라 테이블을 구성해야 합니다.

* 편집 모드에서 양식을 엽니다.
* 루트 패널->수혜자->테이블 확장
* Row1을 선택하고 렌치 아이콘을 클릭하여 속성 시트를 엽니다.
* 바인딩 참조를 **/newhire/GetEmployeeRecients**&#x200B;로 설정합니다.
* 반복 설정 - 최소 카운트는 1로, 최대 카운트는 5로 설정합니다.
* Row1 구성은 아래의 스크린샷과 같아야 합니다
   ![row-configure](assets/configure-row.PNG)
* 파란색 ☑을 클릭하여 변경 내용을 저장합니다.

## 행 셀 바인딩

마지막으로, 행 셀을 양식 데이터 모델 요소에 바인딩해야 합니다.

* 루트 패널->수혜자->표->행1 확장
* 아래 표에 따라 각 행 셀의 바인딩 참조를 설정합니다.

| 행 셀 | 바인드 참조 |
|------------|----------------------------------------------|
| 이름 | /kr/newhire/GetEmployeeRecures/firstname |
| 성 | /newhire/GetEmployeeRecients/lastname |
| 관계 | /newhire/GetEmployeeRecients/relation |
| 백분율 | /newhire/GetEmployeeRecients/percentage |

* 파란색 ☑을 클릭하여 변경 내용을 저장합니다.

## 양식 테스트

이제 url에 적절한 empID가 있는 양식을 열어야 합니다. 다음 2개의 링크는 데이터베이스의 정보로 양식을 채웁니다.
[empID=207이 있는 양식](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[empID=208이 있는 양식](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## 문제 해결

내 양식이 비어 있고 데이터가 없습니다.

* 양식 데이터 모델이 올바른 결과를 반환하는지 확인하십시오.
* 양식이 올바른 양식 데이터 모델과 연결되어 있습니다.
* 필드 바인딩 확인
* 체크아웃 로그 파일을 확인합니다. empID가 파일에 기록되어 있어야 합니다.이 값이 표시되지 않으면 양식이 제공된 사용자 지정 템플릿을 사용하지 않을 수 있습니다.

테이블이 채워지지 않음

* 행1 바인딩 확인
* 행1에 대한 반복 설정이 올바르게 설정되었는지 확인합니다(최소 = 1 및 최대 = 5 이상).

