---
title: 적응형 양식 만들기
description: 양식 데이터 모델의 자동 완성 서비스를 사용하도록 적응형 양식 만들기 및 구성
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---


# 적응형 양식 만들기

지금까지 다음을 만들었습니다.

* 테이블 2개가 있는 데이터베이스 - `newhire` 및 `beneficiaries`
* Apache Sling 연결 풀링된 DataSource 구성
* RDBMS 기반 양식 데이터 모델

다음 단계는 양식 데이터 모델을 사용하도록 적응형 양식을 만들고 구성하는 것입니다.  [샘플 양식을 다운로드하여 가져올 수 있습니다. ](assets/fdm-demo-af.zip) 샘플 양식에는 직원 세부 사항을 표시하는 섹션과 사원의 수익자를 나열하는 다른 섹션이 있습니다.

## 양식 데이터 모델과 양식 연결

이 강좌와 함께 제공된 샘플 양식은 양식 데이터 모델과 연결되어 있지 않습니다. 양식 데이터 모델을 사용하도록 양식을 구성하려면 다음을 수행해야 합니다.

* FDMDemo 양식 선택
* _속성_->_양식 모델_&#x200B;을 클릭합니다.
* 드롭다운 목록에서 양식 데이터 모델 선택
* 이전 단원에서 만든 양식 데이터 모델을 검색하고 선택합니다.
* _저장 및 닫기_&#x200B;를 클릭합니다.

## 자동 완성 서비스 구성

첫 번째 단계는 양식 자동 완성 서비스를 연결하는 것입니다. 자동 완성 서비스를 연결하려면 아래 단계를 따르십시오

* `FDMDemo` 양식 선택
* _편집_&#x200B;을 클릭하여 편집 모드에서 양식을 엽니다.
* 컨텐츠 계층 구조에서 양식 컨테이너를 선택하고 렌치 아이콘을 클릭하여 속성 시트를 엽니다
* 자동 완성 서비스 드롭다운 목록에서 _양식 데이터 모델 자동 완성 서비스_&#x200B;를 선택합니다.
* 파란색을 ☑ 클릭하여 변경 내용을 저장합니다

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

다음 단계는 사원의 수혜자를 표 형식으로 표시하는 것입니다. 제공된 샘플 양식에는 4개의 열과 단일 행이 있는 표가 있습니다. 수혜자 수에 따라 테이블을 구성해야 합니다.

* 편집 모드에서 양식을 엽니다.
* 루트 패널->수혜자->표 확장
* 1행을 선택하고 렌치 아이콘을 클릭하여 속성 시트를 엽니다.
* 바인딩 참조를 **/newhire/GetEmployeeBenefiers**&#x200B;로 설정합니다.
* 반복 설정 - 최소 카운트를 1로, 최대 카운트를 5로 설정합니다.
* Row1 구성은 아래 스크린샷과 같아야 합니다.
   ![row-configure](assets/configure-row.PNG)
* 파란색을 ☑ 클릭하여 변경 내용을 저장합니다

## 행 셀 바인딩

마지막으로 행 셀을 양식 데이터 모델 요소로 바인딩해야 합니다.

* 루트 패널->수혜자->표->행1 확장
* 아래 표에 따라 각 행 셀의 바인딩 참조를 설정합니다.

| 행 셀 | 바인드 참조 |
|------------|----------------------------------------------|
| 이름 | /newhire/GetEmployeeInqueers/firstname |
| 성 | /newhire/GetEmployeeInqueers/lastname |
| 관계 | /newhire/GetEmployeeProviders/relation |
| 백분율 | /newhire/GetEmployeeBenefits/percentage |

* 파란색을 ☑ 클릭하여 변경 내용을 저장합니다

## 양식 테스트

이제 url에 적절한 empID가 있는 양식을 열어야 합니다. 다음 2개의 링크가 데이터베이스의 정보로 양식을 채웁니다
[empID가 있는 양식=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[empID가 있는 양식=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## 문제 해결

내 양식이 비어 있고 데이터가 없습니다.

* 양식 데이터 모델이 올바른 결과를 반환하는지 확인하십시오.
* 양식이 올바른 양식 데이터 모델과 연결되어 있습니다.
* 필드 바인딩 확인
* 체크아웃 로그 파일을 확인합니다. 이 값이 표시되지 않으면 양식이 제공된 사용자 지정 템플릿을 사용하지 않을 수 있습니다.

테이블이 채워지지 않음

* 행1 바인딩 확인
* 행1에 대한 반복 설정이 올바르게 설정되었는지 확인하십시오(최소 = 1 및 최대 = 5 이상).

