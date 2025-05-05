---
title: 태그 속성 만들기
description: AEM과 통합하기 위한 최소 구성으로 태그 속성을 만드는 방법을 알아봅니다. 사용자는 Tag UI에 대해 소개하고 확장, 규칙 및 게시 워크플로우에 대해 알아봅니다.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 606
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# 태그 속성 만들기 {#create-tag-property}

Adobe Experience Manager과 통합하기 위한 최소 구성으로 태그 속성을 만드는 방법을 알아봅니다. 사용자는 Tag UI에 대해 소개하고 확장, 규칙 및 게시 워크플로우에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/327452?quality=12&learn=on&captions=kor)

## 태그 속성 만들기

태그 속성을 만들려면 다음 단계를 완료하십시오.

1. 브라우저에서 [Adobe Experience Cloud 홈](https://experience.adobe.com/) 페이지로 이동하고 Adobe ID을 사용하여 로그인합니다.

1. Adobe Experience Cloud 홈 페이지의 _빠른 액세스_ 섹션에서 **데이터 수집** 응용 프로그램을 클릭합니다.

1. 왼쪽 탐색에서 **태그** 메뉴 항목을 클릭한 다음 오른쪽 상단 모서리에서 **새 속성**&#x200B;을 클릭합니다.

1. **이름** 필수 필드를 사용하여 Tag 속성에 이름을 지정합니다. 도메인 필드에 도메인 이름을 입력하거나 AEM as a Cloud Service 환경을 사용하는 경우 `adobeaemcloud.com`을(를) 입력하고 **저장**&#x200B;을(를) 클릭합니다.

   ![태그 속성](assets/tag-properties.png)

## 새 규칙 만들기

**태그 속성** 보기에서 해당 이름을 클릭하여 새로 만든 태그 속성을 엽니다. 또한 _내 최근 활동_ 제목 아래에 Core 확장이 추가되었음을 알 수 있습니다. 코어 태그 확장은 기본 확장이며 페이지 로드, 브라우저, 양식 및 기타 이벤트 유형과 같은 기본 이벤트 유형을 제공합니다. 자세한 내용은 [코어 확장 개요](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html?lang=ko)를 참조하십시오.

규칙을 사용하면 방문자가 AEM 사이트와 상호 작용할 때 일어나야 하는 일을 지정할 수 있습니다. 간단하게 하기 위해 브라우저 콘솔에 두 개의 메시지를 기록하여 데이터 수집 Tag 통합이 AEM 프로젝트 코드를 업데이트하지 않고도 AEM 사이트에 JavaScript 코드를 삽입하는 방법을 보여 드리겠습니다.

규칙을 만들려면 다음 단계를 완료하십시오.

1. 왼쪽 탐색 메뉴의 _작성_ 섹션에서 **규칙**&#x200B;을 클릭한 다음 **새 규칙 만들기**&#x200B;를 클릭합니다

1. **이름** 필수 필드를 사용하여 규칙 이름을 지정하십시오.

1. _이벤트_ 섹션에서 **추가**&#x200B;를 클릭한 다음 _이벤트 구성_ 양식의 **이벤트 유형** 드롭다운에서 _라이브러리 로드됨(페이지 상단)_ 옵션을 선택하고 **변경 내용 유지**&#x200B;를 클릭합니다.

1. _작업_ 섹션에서 **추가**&#x200B;를 클릭한 다음 _작업 구성_ 양식의 **작업 유형** 드롭다운에서 _사용자 지정 코드_ 옵션을 선택하고 **편집기 열기**&#x200B;를 클릭합니다.

1. _코드 편집_ 모달에서 다음 JavaScript 코드 조각을 입력한 다음 **저장**&#x200B;을 클릭하고 마지막으로 **변경 내용 유지**&#x200B;를 클릭합니다.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 규칙 만들기 프로세스를 완료하려면 **저장**&#x200B;을 클릭하세요.

   ![새 규칙](assets/new-rule.png)

## 라이브러리 추가 및 게시

Tag 속성 _규칙_&#x200B;이(가) 라이브러리를 사용하여 활성화되었습니다. 라이브러리는 JavaScript 코드가 포함된 패키지로 간주됩니다. 단계에 따라 새로 생성된 규칙을 활성화합니다.

1. 왼쪽 탐색 메뉴의 _게시_ 섹션에서 **게시 흐름**&#x200B;을 클릭한 다음 **라이브러리 추가**&#x200B;를 클릭합니다

1. **이름** 필드를 사용하여 라이브러리 이름을 지정하고 **환경** 드롭다운에 대한 _개발(개발)_ 옵션을 선택하십시오.

1. Tag 속성을 만든 후 변경된 모든 리소스를 선택하려면 **+ 변경된 모든 리소스 추가**&#x200B;를 클릭하십시오. 이 작업을 수행하면 새로 만든 규칙 및 코어 확장 리소스가 라이브러리에 추가됩니다. 마지막으로 **개발에 저장 및 빌드**&#x200B;를 클릭합니다.

1. **개발** 수영 레인에 대한 라이브러리를 빌드했으면 _생략 부호_&#x200B;를 사용하여 **승인을 위해 제출**&#x200B;을 선택합니다.

1. 그런 다음 **제출됨** 수영 레일에서 _생략 부호_&#x200B;를 사용하여 **게시 승인**&#x200B;을(를) 선택합니다. 마찬가지로 **승인됨** 수영 레일에서 **프로덕션에 빌드 및 Publish**&#x200B;을(를) 선택합니다.

![게시된 라이브러리](assets/published-library.png)


위 단계에서는 페이지가 로드될 때 브라우저 콘솔에 메시지를 기록하는 규칙이 있는 간단한 태그 속성 만들기를 완료합니다. 또한 라이브러리를 만들어 규칙 및 코어 확장을 게시합니다.

## 다음 단계

[IMS를 사용하여 AEM과 태그 속성 연결](connect-aem-tag-property-using-ims.md)


## 추가 리소스 {#additional-resources}

* [태그 속성 만들기](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=ko)
