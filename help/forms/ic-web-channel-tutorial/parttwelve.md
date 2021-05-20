---
title: 웹 채널 문서 전달 설정
seo-title: 웹 채널 문서 전달 설정
description: 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계의 자습서의 마지막 부분입니다. 이 부분에서는 이메일을 통해 웹 채널 문서의 게재를 살펴봅니다.
seo-description: 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계의 자습서의 마지막 부분입니다. 이 부분에서는 이메일을 통해 웹 채널 문서의 게재를 살펴봅니다.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: 대화형 통신
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---


# 웹 채널 문서 {#setting-up-the-delivery-of-web-channel-document} 게재 설정


이 부분에서는 이메일을 통해 웹 채널 문서의 게재를 살펴봅니다.

웹 채널 대화형 통신 문서를 정의하고 테스트한 후에는 웹 채널 문서를 수신자에게 제공하기 위한 게재 메커니즘이 필요합니다.

이메일을 웹 채널 문서의 게재 메커니즘으로 사용하려면 양식 데이터 모델을 약간 변경해야 합니다.

[이메일을 통해 웹 채널 게재에 대해 자세히 알아보려면](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

AEM Forms에 로그인합니다.

* Forms ->데이터 통합으로 이동합니다

* 편집 모드에서 RestrationAccountStatement 데이터 모델을 엽니다.

* 잔액 객체를 선택하고 편집 버튼을 누릅니다.

* &quot;연필&quot; 아이콘을 선택하여 편집 모드에서 id 인수를 엽니다.

* 바인딩을 &quot;RequestAttribute&quot;로 변경합니다.

* 아래와 같이 바인딩 값에서 계정 번호를 설정합니다.

* 이 방법으로 요청 속성을 통해 양식 데이터 모델에 계정 번호를 전달합니다

* 변경 사항을 저장했는지 확인합니다.
   ![fdm](assets/requestattribute.gif)

## 웹 채널 문서의 전자 메일 배달 테스트 {#test-email-delivery-of-web-channel-document}

* [패키지 관리자를 사용하여 샘플 자산 설치](assets/webchanneldelivery.zip)
* [crx에 로그인](http://localhost:4502/crx/de/index.jsp#)

* /home/users로 이동합니다.

* 사용자의 노드에서 관리 사용자를 검색합니다.

* 관리 사용자의 프로필 노드를 선택합니다.

* &quot;accountnumber&quot;라는 속성을 만듭니다. 속성 유형이 문자열인지 확인합니다.

* 이 accountnumber 속성 값을 &quot;3059827&quot;으로 설정합니다. 이 값을 원하는 대로 임의의 숫자로 설정할 수 있습니다.

* [getad.html을 엽니다.](http://localhost:4502/content/getad.html)

* 이 URL과 연결된 코드는 로그인한 사용자의 계정 번호를 가져옵니다. 그런 다음 이 계정 번호가 FDM에 requestattribute로 전달됩니다. 그런 다음 FDM이 이 계정 번호와 연관된 데이터를 가져오고 웹 채널 문서를 채웁니다.

>[!NOTE]
>
>crx에서 **/apps/AEMForms/fetchad/GET.jsp** 파일을 살펴보십시오. String 변수 webChannelDocument가 올바른 통신 문서 경로를 가리키는지 확인하십시오.
