---
title: 태그 속성 만들기
description: AEM과 통합하기 위해 최소 구성으로 태그 속성을 만드는 방법을 알아봅니다. 사용자가 태그 UI에 도입되고 확장, 규칙 및 게시 워크플로우에 대해 알아봅니다.
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

Adobe Experience Manager과 통합하기 위해 최소 구성으로 태그 속성을 만드는 방법을 알아봅니다. 사용자가 태그 UI에 도입되고 확장, 규칙 및 게시 워크플로우에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## 태그 속성 만들기

태그 속성을 만들려면 다음 단계를 완료하십시오.

1. 브라우저에서 [Adobe Experience Cloud 홈](https://experience.adobe.com/) 페이지를 방문하여 Adobe ID을 사용하여 로그인하십시오.

1. 을(를) 클릭합니다. **데이터 수집** 애플리케이션 _빠른 액세스_ Adobe Experience Cloud 홈 페이지의 섹션을 참조하십시오.

1. 을(를) 클릭합니다. **태그** 왼쪽 탐색 메뉴에서 메뉴 항목을 클릭한 다음 **새 속성** 오른쪽 상단 모서리에서

1. 를 사용하여 태그 속성에 이름을 지정합니다. **이름** 필수 필드입니다. 도메인 필드에 도메인 이름을 입력하거나 AEM as a Cloud Service 환경을 사용하는 경우 enter 키를 누릅니다 `adobeaemcloud.com` 을(를) 클릭합니다. **저장**.

   ![태그 속성](assets/tag-properties.png)

## 새 규칙 만들기

에서 해당 이름을 클릭하여 새로 만든 태그 속성을 엽니다 **태그 속성** 보기. 또한 아래 _내 최근 활동_ 제목 아래에 Core 확장이 추가되었음을 볼 수 있습니다. 코어 태그 확장은 기본 확장이며 페이지 로드, 브라우저, 양식 및 기타 이벤트 유형과 같은 기본 이벤트 유형을 제공합니다. 자세한 내용은 [코어 확장 개요](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) 추가 정보.

규칙을 사용하면 방문자가 AEM 사이트와 상호 작용할 때 일어나야 하는 일을 지정할 수 있습니다. 간소화된 환경을 유지하기 위해 브라우저 콘솔에 두 개의 메시지를 기록하여 데이터 수집 태그 통합이 AEM Project 코드를 업데이트하지 않고 AEM 사이트에 JavaScript 코드를 어떻게 주입할 수 있는지 설명하겠습니다.

규칙을 만들려면 다음 단계를 완료하십시오.

1. 클릭 **규칙** 에서 _작성_ 왼쪽 탐색 섹션의 섹션을 클릭한 다음 **새 규칙 만들기**

1. 규칙 이름을 **이름** 필수 필드입니다.

1. 클릭 **추가** 에서 _이벤트_ 섹션에서 다음을 수행합니다. _이벤트 구성_ 양식, **이벤트 유형** 드롭다운 선택 _라이브러리가 로드됨(페이지 상단)_ 옵션을 선택하고 **변경 내용 유지**.

1. 클릭 **추가** 에서 _작업_ 섹션에서 다음을 수행합니다. _작업 구성_ 양식, **작업 유형** 드롭다운 선택 _사용자 지정 코드_ 옵션을 선택하고 **편집기 열기**.

1. 에서 _코드 편집_ 모달에서 다음 JavaScript 코드 조각을 입력한 다음 **저장**&#x200B;를 클릭하고 **변경 내용 유지**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 클릭 **저장** 규칙 만들기 프로세스를 완료합니다.

   ![새 규칙](assets/new-rule.png)

## 라이브러리 추가 및 게시

Tag 속성 _규칙_ 라이브러리를 사용하여 활성화되면 라이브러리를 JavaScript 코드가 포함된 패키지로 생각해 보십시오. 단계에 따라 새로 만든 규칙을 활성화합니다.

1. 클릭 **게시 흐름** 에서 _게시_ 왼쪽 탐색 영역에서 섹션을 클릭한 다음 **라이브러리 추가**

1. 라이브러리에 이름을 **이름** 필드 및 선택 _개발(개발)_ 옵션 **환경** 드롭다운.

1. 태그 속성 생성 이후 변경된 모든 리소스를 선택하려면 **+ 변경된 모든 리소스 추가**. 이렇게 하면 새로 만든 규칙 및 코어 확장 리소스가 라이브러리에 추가됩니다. 마지막으로 클릭 **개발에 저장 및 구축**.

1. 라이브러리가 **개발** 수영선을 이용해서 _줄임표_ 선택 **승인을 위한 제출**

1. 그런 다음 **제출됨** 수영 레인 _줄임표_ 선택 **게시 승인**&#x200B;마찬가지로 **빌드 및 프로덕션에 게시** 에서 **승인됨** 수영선.

![게시된 라이브러리](assets/published-library.png)


위의 단계에서는 페이지가 로드될 때 브라우저 콘솔에 메시지를 기록하는 규칙이 있는 간단한 태그 속성 생성을 완료합니다. 또한 규칙 및 코어 확장은 라이브러리를 만들어 게시됩니다.

## 다음 단계

[IMS를 사용하여 AEM과 태그 속성 연결](connect-aem-tag-property-using-ims.md)


## 추가 리소스 {#additional-resources}

* [태그 속성 만들기](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
