---
title: 계단식 드롭다운 목록
description: 이전 드롭다운 목록 선택을 기반으로 드롭다운 목록을 채웁니다.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
source-git-commit: 15b57ec6792bc47d0041946014863b13867adf22
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# 계단식 드롭다운 목록

계단식 드롭다운 목록은 부모 컨트롤이나 이전 DropDownList 컨트롤에 종속되는 종속 DropDownList 컨트롤의 연속입니다. DropDownList 컨트롤의 항목은 다른 DropDownList 컨트롤에서 사용자가 선택한 항목을 기반으로 채워집니다.

## 사용 사례 데모

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=9&learn=on)

이 자습서를 위해 [REST API](http://api.geonames.org/) 이 기능을 보여 줍니다.
이러한 종류의 서비스를 제공하는 조직이 많이 있으며, REST API를 문서화한 적이 있는 한 데이터 통합 기능을 사용하여 AEM Forms과 쉽게 통합할 수 있습니다

AEM Forms에서 계단식 드롭다운 목록을 구현하려면 다음 단계를 수행해야 합니다

## 개발자 계정 만들기

다음 아이콘을 사용하여 개발자 계정 만들기 [건가즈](https://www.geonames.org/login). 사용자 이름을 메모하십시오. 이 사용자 이름은 Geneames.org의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI Specification(이전 Swagger Specification)은 REST API에 대한 API 설명 형식입니다. OpenAPI 파일을 사용하면 다음을 포함한 전체 API를 설명할 수 있습니다.

* 각 엔드포인트에서 사용 가능한 엔드포인트(/users) 및 작업(GET /users, POST /users)
* 작업 매개변수 각 작업에 대한 입력 및 출력 인증 방법
* 연락처 정보, 라이센스, 사용 약관 및 기타 정보입니다.
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 이 형식은 인간과 컴퓨터 모두에서 쉽게 배우고 읽을 수 있습니다.

첫 번째 Swagger/OpenAPI 파일을 만들려면 [OpenAPI 설명서](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms은 OpenAPI 사양 버전 2.0(FKA Swagger)을 지원합니다.

를 사용하십시오 [swagger 편집기](https://editor.swagger.io/) 국가 또는 주의 모든 국가 및 하위 요소를 가져오는 작업을 설명하는 swagger 파일을 만듭니다. swagger 파일은 JSON 또는 YAML 형식으로 만들 수 있습니다. 완료된 swagger 파일은 [여기](assets/swagger-files.zip)
swagger 파일에는 다음 REST API에 대해 설명합니다.
* [모든 국가 가져오기](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [Thoname 개체의 자식 가져오기](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## 데이터 소스 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 다음을 수행해야 합니다 [데이터 소스 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) ( 클라우드 서비스 구성) 아래에 그룹화됩니다. 다음 코드를 사용하십시오 [swagger 파일](assets/swagger-files.zip) 를 눌러 데이터 소스를 생성합니다.
2개의 데이터 소스를 만들어야 합니다(하나는 모든 국가와 다른 하나는 하위 요소를 가져오기 위해)


## 양식 데이터 모델 만들기

AEM Forms 데이터 통합은 을 만들고 작업할 수 있는 직관적인 사용자 인터페이스를 제공합니다 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 이전 단계에서 만든 데이터 소스를 기반으로 양식 데이터 모델을 만듭니다. 데이터 소스가 2개인 양식 데이터 모델

![fdm](assets/geonames-fdm.png)


## 적응형 양식 만들기

양식 데이터 모델의 GET 호출을 적응형 양식과 통합하여 드롭다운 목록을 채웁니다.
2개의 드롭다운 목록을 사용하여 적응형 양식을 만듭니다. 국가를 나열하는 것과 선택된 국가에 따라 주/도를 나열하는 것.

### 국가 채우기 드롭다운 목록

양식을 처음 초기화하면 국가 목록이 채워집니다. 다음 화면에서는 국가 드롭다운 목록의 옵션을 채우도록 구성된 규칙 편집기를 보여줍니다. 이 작업을 수행하려면 사용자 이름과 Geneames 계정을 제공해야 합니다.
![get-country](assets/get-countries-rule-editor.png)

#### 시/도 드롭다운 목록 채우기

선택한 국가를 기준으로 시/도 드롭다운 목록을 채워야 합니다. 다음 화면에서는 규칙 편집기 구성을 보여줍니다
![주/도 옵션](assets/state-province-options.png)

### 연습

선택한 국가와 시/도를 기반으로 카운티와 시를 나열하기 위해 양식에 군 및 시라고 불리는 2개의 드롭다운 목록을 추가합니다.
![연습](assets/cascading-drop-down-exercise.png)





