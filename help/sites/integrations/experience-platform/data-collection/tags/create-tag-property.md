---
title: 태그 속성 만들기
description: AEM과 통합하기 위한 최소 구성으로 태그 속성을 만드는 방법을 알아봅니다. 사용자는 Tag UI에 대해 소개하고 확장, 규칙 및 게시 워크플로우에 대해 알아봅니다.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# 태그 속성 만들기 {#create-tag-property}

Adobe Experience Manager과 통합하기 위한 최소 구성으로 태그 속성을 만드는 방법을 알아봅니다. 사용자는 Tag UI에 대해 소개하고 확장, 규칙 및 게시 워크플로우에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## 태그 속성 만들기

태그 속성을 만들려면 다음 단계를 완료하십시오.

1. 브라우저에서 다음 위치로 이동합니다. [Adobe Experience Cloud 홈](https://experience.adobe.com/) Adobe ID을 사용하여 페이지를 만들고 로그인합니다.

1. 다음을 클릭합니다. **데이터 수집** 다음에서 응용 프로그램 _빠른 액세스_ Adobe Experience Cloud 섹션에 있는 마지막 항목이 될 필요가 없습니다.

1. 다음을 클릭합니다. **태그** 왼쪽 탐색에서 메뉴 항목을 클릭한 다음 **새 속성** 오른쪽 상단 모서리에서

1. 다음을 사용하여 Tag 속성에 이름 지정 **이름** 필수 필드. 도메인 필드에 도메인 이름을 입력하거나, AEM as a Cloud Service 환경을 사용하는 경우 을 입력합니다. `adobeaemcloud.com` 및 클릭 **저장**.

   ![태그 속성](assets/tag-properties.png)

## 새 규칙 만들기

에서 해당 이름을 클릭하여 새로 만든 Tag 속성을 엽니다. **태그 속성** 보기. 다음 아래에 _내 최근 활동_ 머리글에서 코어 확장이 추가되었음을 알 수 있습니다. 코어 태그 확장은 기본 확장이며 페이지 로드, 브라우저, 양식 및 기타 이벤트 유형과 같은 기본 이벤트 유형을 제공합니다. [코어 확장 개요](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) 추가 정보.

규칙을 사용하면 방문자가 AEM 사이트와 상호 작용할 때 일어나야 하는 일을 지정할 수 있습니다. 간단하게 하기 위해 두 개의 메시지를 브라우저 콘솔에 기록하여 데이터 수집 Tag 통합이 AEM Project 코드를 업데이트하지 않고 JavaScript 코드를 AEM 사이트에 삽입하는 방법을 보여 주겠습니다.

규칙을 만들려면 다음 단계를 완료하십시오.

1. 클릭 **규칙** 다음에서 _작성_ 왼쪽 탐색 영역에 있는 섹션을 클릭한 다음 **새 규칙 만들기**

1. 다음을 사용하여 규칙 이름 지정 **이름** 필수 필드.

1. 클릭 **추가** 다음에서 _이벤트_ 섹션, 다음 위치 _이벤트 구성_ 양식, **이벤트 유형** 드롭다운 선택 _라이브러리가 로드됨 (페이지 상단)_ 옵션 및 클릭 **변경 내용 유지**.

1. 클릭 **추가** 다음에서 _작업_ 섹션, 다음 위치 _작업 구성_ 양식, **작업 유형** 드롭다운 선택 _사용자 지정 코드_ 옵션 및 클릭 **편집기 열기**.

1. 다음에서 _코드 편집_ 모달에서 다음 JavaScript 코드 조각을 입력한 다음 **저장**&#x200B;을 클릭한 다음 마지막으로 **변경 내용 유지**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 클릭 **저장** 을 눌러 규칙 생성 프로세스를 완료합니다.

   ![새 규칙](assets/new-rule.png)

## 라이브러리 추가 및 게시

Tag 속성 _규칙_ 는 라이브러리를 사용하여 활성화되며, 라이브러리는 JavaScript 코드가 포함된 패키지로 생각할 수 있습니다. 단계에 따라 새로 생성된 규칙을 활성화합니다.

1. 클릭 **게시 플로우** 다음에서 _게시_ 왼쪽 탐색 영역에 있는 섹션을 클릭한 다음 **라이브러리 추가**

1. 라이브러리를 명명하려면 **이름** 필드 및 선택 _Development(개발)_ 옵션 **환경** 드롭다운입니다.

1. Tag 속성을 만든 후 변경된 모든 리소스를 선택하려면 **+ 변경된 모든 리소스 추가**. 이 작업을 수행하면 새로 만든 규칙 및 코어 확장 리소스가 라이브러리에 추가됩니다. 마지막으로 **개발에 저장 및 구축**.

1. 용 라이브러리가 빌드되면 **개발** 스윔레인, 사용 _생략 부호_ 선택 **승인을 위해 제출**

1. 그런 다음 **제출됨** 스윔레인 _생략 부호_ 선택 **게시를 위해 승인**, 마찬가지로 **빌드 및 프로덕션에 게시** 다음에서 **승인됨** 헤엄쳐 레인이요

![게시된 라이브러리](assets/published-library.png)


위 단계에서는 페이지가 로드될 때 브라우저 콘솔에 메시지를 기록하는 규칙이 있는 간단한 태그 속성 만들기를 완료합니다. 또한 라이브러리를 만들어 규칙 및 코어 확장을 게시합니다.

## 다음 단계

[IMS를 사용하여 AEM과 태그 속성 연결](connect-aem-tag-property-using-ims.md)


## 추가 리소스 {#additional-resources}

* [태그 속성 만들기](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
