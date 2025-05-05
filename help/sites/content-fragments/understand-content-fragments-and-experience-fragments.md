---
title: 컨텐츠 조각 및 경험 조각
description: 콘텐츠 조각과 경험 조각 간의 유사점과 차이점, 각 유형 사용 시기 및 방법에 대해 알아봅니다.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
duration: 168
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 2%

---

# 컨텐츠 조각 및 경험 조각

Adobe Experience Manager의 콘텐츠 조각 및 경험 조각 은 표면적으로는 비슷해 보일 수 있지만 각각 다른 사용 사례에서 주요 역할을 합니다. 콘텐츠 조각 및 경험 조각 이 어떻게 유사하고 다른지, 언제 및 어떻게 각 조각을 사용하는지 알아봅니다.

## 비교

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>컨텐츠 조각(CF)</strong></td>
<td><strong>경험 조각(XF)</strong></td>
</tr><tr><td><strong>정의</strong></td>
<td><ul>
<li>구조화된 데이터 요소(텍스트, 날짜, 참조 등)로 구성된 프레젠테이션과 관계없는 <strong>content</strong>을(를) 다시 사용할 수 있습니다.</li>
</ul>
</td>
<td><ul>
<li>콘텐츠 및 프레젠테이션을 정의하는 하나 이상의 AEM 구성 요소로 구성된 재사용 가능한 합성 기능으로, 자체적으로 의미가 있는 <strong>경험</strong>을(를) 구성합니다.</li>
</ul>
</td>
</tr><tr><td><strong>핵심 개념</strong></td>
<td><ul>
<li>콘텐츠 중심</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=ko" target="_blank">구조화된 양식 기반 데이터 모델로 정의됩니다.</a></li>
<li>디자인 및 레이아웃 불가지론적.</li>
<li>채널은 콘텐츠 조각 콘텐츠(레이아웃 및 디자인) 프레젠테이션을 소유합니다</li>
</ul>
</td>
<td><ul>
<li>프레젠테이션 중심</li>
<li>AEM 구성 요소의 구조화되지 않은 구성으로 정의됨</li>
<li>컨텐츠의 디자인 및 레이아웃 정의</li>
<li>채널에서 "있는 그대로" 사용됨</li>
</ul>
</td>
</tr><tr><td><strong>기술 세부 정보</strong></td>
<td><ul>
<li><strong>dam:Asset</strong>(으)로 구현</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=ko" target="_blank">콘텐츠 조각 모델</a>에 의해 정의됨</li>
</ul>
</td>
<td><ul>
<li><strong>cq:Page</strong>(으)로 구현됨</li>
<li>편집 가능한 템플릿으로 정의됨</li>
<li>기본 HTML 렌디션</li>
</ul>
</td>
</tr><tr><td><strong>변형</strong></td>
<td><ul>
<li>기본 변형은 표준 변형입니다</li>
<li>변형은 사용 사례별로 다르며, 이는 채널과 일치할 수 있습니다.</li>
</ul>
</td>
<td><ul>
<li>변형은 채널별 또는 컨텍스트별</li>
<li>변형이 AEM Live Copy를 통해 동기화됨</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ko" target="_blank">빌딩 블록</a>을(를) 통해 변형 간 콘텐츠 재사용 허용</li>
</ul>
</td>
</tr><tr><td><strong>기능</strong></td>
<td><ul>
<li>변형</li>
<li>버전</li>
<li>변형 간 콘텐츠의 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=ko#synchronizing-with-master" target="_blank">동기화</a></li>
<li>콘텐츠 조각 버전의 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=ko#comparing-fragment-versions" target="_blank">시각적 차이</a></li>
<li>여러 줄 텍스트 요소의 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=ko#annotating-a-content-fragment" target="_blank">주석</a></li>
<li>여러 줄 텍스트 요소의 지능형 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=ko#summarizing-text" target="_blank">요약</a>.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=ko" target="_blank">번역/로컬라이제이션</a></li>
</ul>
</td>
<td><ul>
<li>변형</li>
<li>변형을 라이브 카피로 변환</li>
<li>버전</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ko#building-blocks" target="_blank">빌딩 블록</a></li>
<li>주석</li>
<li>응답형 레이아웃 및 미리보기</li>
<li>번역/로컬라이제이션</li>
<li>콘텐츠 조각 참조를 통한 복잡한 데이터 모델</li>
<li>인앱 미리 보기</li>
</ul>
</td>
</tr><tr><td><strong>사용</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=ko">AEM 헤드리스 GraphQL API</a>를 통해 JSON 내보내기</li>
<li>AEM Sites, AEM Screens 또는 경험 조각에서 사용할 <a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ko" target="_blank">AEM 핵심 구성 요소 콘텐츠 조각 구성 요소</a>.</li>
<li>서드파티 소비를 위해 <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=ko" target="_blank">AEM Content Services</a>를 통해 JSON 내보내기</li>
<li>타깃팅된 오퍼를 위해 Adobe Target으로 JSON 내보내기</li>
<li>타사 소비를 위해 AEM HTTP Assets API를 통한 JSON</li>
</ul>
</td>
<td><ul>
<li>AEM Sites, AEM Screens 또는 기타 경험 조각에서 사용할 AEM 경험 조각 구성 요소입니다.</li>
<li>타사 시스템에서 사용할 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ko" target="_blank">일반 HTML</a>(으)로 내보내기</li>
<li>타깃팅된 오퍼에 대해 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=ko" target="_blank">Adobe Target으로 HTML 내보내기</a></li>
<li>타깃팅된 오퍼를 위해 Adobe Target으로 JSON 내보내기</li>
</ul>
</td>
</tr><tr><td><strong>일반적인 사용 사례</strong></td>
<td><ul>
<li>GraphQL을 통한 헤드리스 사용 사례 강화</li>
<li>구조화된 데이터 입력/양식 기반 컨텐츠</li>
<li>긴 형식의 에디토리얼 콘텐츠(여러 줄 요소)</li>
<li>콘텐츠가 제공되는 채널의 수명 주기 밖에서 관리됨</li>
</ul>
</td>
<td><ul>
<li>채널별 변형을 사용하여 멀티채널 홍보 자료를 중앙 집중식으로 관리합니다.</li>
<li>웹 사이트의 여러 페이지에서 콘텐츠가 다시 사용됩니다.</li>
<li>웹 사이트 크롬(예: 머리글 및 바닥글)</li>
<li>채널을 제공하는 채널의 수명 주기 밖에서 관리되는 경험</li>
</ul>
</td>
</tr><tr><td><strong>설명서</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=ko&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM 콘텐츠 조각 사용 안내서</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=ko" target="_blank">AEM에서 컨텐츠 조각 사용</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ko" target="_blank">경험 조각의 Adobe 설명서</a></li>
</ul>
</td>
</tr></tbody></table>

## 콘텐츠 조각 아키텍처

다음 다이어그램은 AEM 콘텐츠 조각의 전체 아키텍처를 보여 줍니다

![콘텐츠 조각 아키텍처](./assets/content-fragments-architecture.png)

+ **콘텐츠 조각 모델**&#x200B;은(는) 콘텐츠 조각이 캡처하고 표시할 콘텐츠를 정의하는 요소(또는 필드)를 정의합니다.
+ **콘텐츠 조각**&#x200B;은(는) 논리적 콘텐츠 엔터티를 나타내는 콘텐츠 조각 모델의 인스턴스입니다.
+ 콘텐츠 조각 **변형**&#x200B;은(는) 콘텐츠 조각 모델을 준수하지만 콘텐츠에 변형이 있습니다.
+ 컨텐츠 조각은 다음 사용자가 노출/사용할 수 있습니다.
   + AEM WCM 핵심 구성 요소의 콘텐츠 조각 구성 요소를 통해 **AEM Sites**(또는 AEM Screens)에서 콘텐츠 조각을 사용합니다.
   + AEM Headless GraphQL API를 사용하여 Headless 앱의 **콘텐츠 조각**&#x200B;을(를) 사용합니다.
   + 읽기 전용 사용 사례에 대해 **AEM Content Services** 및 API 페이지를 통해 콘텐츠 조각 변형 콘텐츠를 JSON으로 노출
   + CRUD 사용 사례에 대해 **AEM Assets HTTP API**&#x200B;를 통해 AEM Assets에 직접 호출하여 콘텐츠 조각 콘텐츠(모든 변형)를 JSON으로 직접 노출

## 경험 조각 아키텍처

![경험 조각 아키텍처](./assets/experience-fragments-architecture.png)

+ **편집 가능한 템플릿**&#x200B;은(는) **편집 가능한 템플릿 유형** 및 **AEM 페이지 구성 요소 구현**&#x200B;에 의해 정의되며, 경험 조각을 구성하는 데 사용할 수 있는 허용된 AEM 구성 요소를 정의합니다.
+ **경험 조각**&#x200B;은(는) 논리적 경험을 나타내는 편집 가능한 템플릿의 인스턴스입니다.
+ 경험 조각 **변형**&#x200B;은(는) 편집 가능한 템플릿을 준수하지만 경험(콘텐츠 및 디자인)에 변형이 있습니다.
+ 경험 조각은 다음 사용자가 노출/소비할 수 있습니다.
   + AEM 경험 조각 구성 요소를 통해 AEM Sites(또는 AEM Screens)에서 경험 조각 사용.
   + **AEM Content Services** 및 API 페이지를 통해 경험 조각 변형 컨텐츠를 JSON(임베드된 HTML 포함)으로 노출
   + 경험 조각 변형을 **&quot;일반 HTML&quot;**(으)로 직접 노출
   + 경험 조각을 **Adobe Target**&#x200B;에 HTML 또는 JSON 오퍼로 내보내는 중입니다.
   + AEM Sites은 기본적으로 HTML 오퍼를 지원하지만, JSON 오퍼에는 사용자 지정 개발이 필요합니다.

## 콘텐츠 조각 지원 리소스

+ [콘텐츠 조각 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=ko&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Adobe Experience Manager as a Headless CMS 소개](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=ko)
+ [AEM에서 콘텐츠 조각 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=ko)
+ [AEM WCM 핵심 구성 요소의 콘텐츠 조각 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ko)
+ [콘텐츠 조각 및 AEM Headless 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=ko)
+ [AEM Content Services 시작](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=ko)

## 경험 구성요소를 위한 지원 리소스

+ 경험 조각의 [Adobe 설명서](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ko)
+ [AEM 경험 조각 이해](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ko)
+ [AEM 경험 조각 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ko)
+ [Adobe Target에서 AEM 경험 조각 사용](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
