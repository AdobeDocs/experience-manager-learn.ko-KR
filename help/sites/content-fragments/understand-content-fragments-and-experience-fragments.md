---
title: 컨텐츠 조각 및 경험 조각 이해
description: Adobe Experience Manager의 컨텐츠 조각 및 경험 조각은 표면에서는 유사해 보일 수 있지만 각 기능은 다른 사용 사례에서 주요 역할을 합니다. 컨텐츠 조각 및 경험 조각이 유사하고, 서로 다른 방식을 사용하며, 각각의 조각을 사용하는 시기와 방법을 알아봅니다.
sub-product: 자산, 사이트, 컨텐츠 서비스
feature: 컨텐츠 조각, 경험 조각
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: 컨텐츠 관리
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1004'
ht-degree: 5%

---


# 컨텐츠 조각 및 경험 조각 이해

Adobe Experience Manager의 컨텐츠 조각 및 경험 조각은 표면에서는 유사해 보일 수 있지만 각 기능은 다른 사용 사례에서 주요 역할을 합니다. 컨텐츠 조각 및 경험 조각이 유사하고, 서로 다른 방식을 사용하며, 각각의 조각을 사용하는 시기와 방법을 알아봅니다.

## 컨텐츠 조각 및 경험 조각 비교

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>컨텐츠 조각(CF)</strong></td>
<td><strong>경험 조각(XF)</strong></td>
</tr><tr><td><strong>정의</strong></td>
<td><ul>
<li>구조화된 데이터 요소(텍스트, 날짜, 참조 등)로 구성된 <strong>content</strong>을 다시 사용할 수 있습니다.</li>
</ul>
</td>
<td><ul>
<li>컨텐츠 및 프레젠테이션을 정의하는 하나 이상의 AEM 구성 요소를 재사용할 수 있으며, 이 구성 요소는 자체에서 적절할 수 있는 <strong>경험</strong>을 구성하는 것입니다</li>
</ul>
</td>
</tr><tr><td><strong>핵심 테넌트</strong></td>
<td><ul>
<li>콘텐츠 중심</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">구조화된 양식 기반 데이터 모델로 정의됩니다.</a></li>
<li>디자인 및 레이아웃에 구애받지 않음.</li>
<li>채널은 컨텐츠 조각의 컨텐츠(레이아웃 및 디자인)에 대한 프레젠테이션을 소유합니다</li>
</ul>
</td>
<td><ul>
<li>프레젠테이션 중심</li>
<li>AEM 구성 요소의 구조화되지 않은 구성에 의해 정의됨</li>
<li>컨텐츠의 디자인 및 레이아웃을 정의합니다</li>
<li>채널에서 "있는 그대로" 사용됨</li>
</ul>
</td>
</tr><tr><td><strong>기술 세부 사항</strong></td>
<td><ul>
<li><strong>dam:Asset</strong>로 구현됨</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">컨텐츠 조각 모델</a>에 의해 정의됨</li>
</ul>
</td>
<td><ul>
<li><strong>cq:Page</strong>로 구현됨</li>
<li>편집 가능한 템플릿으로 정의</li>
<li>기본 HTML 표현물</li>
</ul>
</td>
</tr><tr><td><strong>변형</strong></td>
<td><ul>
<li>기본 변형은 표준 변수입니다</li>
<li>변형은 채널과 일치할 수 있는 사용 사례별로 다릅니다.</li>
</ul>
</td>
<td><ul>
<li>변형은 채널별 또는 컨텍스트별</li>
<li>변형은 AEM Live Copy를 통해 계속 동기화됩니다</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">변형 </a> 간 블록 판매 컨텐츠 재사용</li>
</ul>
</td>
</tr><tr><td><strong>기능</strong></td>
<td><ul>
<li>변형</li>
<li>버전</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank"></a> 변형 간 컨텐츠 동기화</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">시각적</a> 으로 다른 컨텐츠 조각 버전</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank"></a> 여러 줄 텍스트 요소에 대한 주석</li>
<li>여러 줄 텍스트 요소의 지능형 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">요약</a>.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">번역/로컬라이제이션</a></li>
</ul>
</td>
<td><ul>
<li>변형</li>
<li>변형을 Live Copy로</li>
<li>버전</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">빌딩 블록</a></li>
<li>주석</li>
<li>응답형 레이아웃 및 미리 보기</li>
<li>번역/로컬라이제이션</li>
</ul>
</td>
</tr><tr><td><strong>사용</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM Sites, AEM Screens 또는 경험 </a> 조각에서 사용할 AEM 코어 구성 요소 컨텐츠 조각 구성 요소입니다.</li>
<li>타사 소비에 대해 <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a>를 통해 JSON 내보내기</li>
<li>타사 사용을 위해 AEM HTTP Assets API를 통한 JSON입니다.</li>
</ul>
</td>
<td><ul>
<li>AEM Sites, AEM Screens 또는 기타 경험 조각에서 사용할 AEM 경험 조각 구성 요소입니다.</li>
<li>타사 시스템에서 사용하기 위해 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">일반 HTML</a>로 내보내기</li>
<li><a href="https://helpx.adobe.com/kr/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">타깃팅된 오퍼에 대해 Adobe </a> Target으로 HTML 내보내기</li>
<li>타깃팅된 오퍼를 위해 Adobe Target으로 JSON 내보내기</li>
</ul>
</td>
</tr><tr><td><strong>일반적인 사용 사례</strong></td>
<td><ul>
<li>고도로 구조화된 데이터 입력/양식 기반 컨텐츠</li>
<li>긴 형식의 편집 콘텐츠(여러 줄 요소)</li>
<li>컨텐츠를 전달하는 채널의 수명 주기 외부에서 관리되는 컨텐츠</li>
</ul>
</td>
<td><ul>
<li>채널별 변형을 사용하여 멀티채널 프로모션 담소의 중앙 집중식 관리</li>
<li>컨텐츠는 웹 사이트의 여러 페이지에서 다시 사용됩니다.</li>
<li>웹 사이트 chrome(예: 머리글 및 바닥글)</li>
<li>채널을 전달하는 채널의 수명 주기 외부에서 관리되는 경험</li>
</ul>
</td>
</tr><tr><td><strong>설명서</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM 컨텐츠 조각 사용 안내서</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">AEM에서 컨텐츠 조각 사용</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">경험 조각에 대한 Adobe 설명서</a></li>
</ul>
</td>
</tr></tbody></table>

## 컨텐츠 조각 아키텍처

다음 다이어그램은 AEM 컨텐츠 조각에 대한 전체 아키텍처를 보여 줍니다

!![컨텐츠 조각 아키텍처](./assets/content-fragments-architecture.png)

+ **컨텐츠 조각** 모델은 컨텐츠 조각이 캡처하고 노출할 수 있는 컨텐츠를 정의하는 요소(또는 필드)를 정의합니다.
+ **컨텐츠 조각**&#x200B;은 논리 컨텐츠 엔티티를 나타내는 컨텐츠 조각 모델의 인스턴스입니다.
+ 컨텐츠 조각 **변형**&#x200B;은 컨텐츠 조각 모델을 준수하지만, 컨텐츠의 변형이 있습니다.
+ 컨텐츠 조각은 다음과 같은 방법으로 노출/소비할 수 있습니다.
   + AEM WCM 코어 구성 요소의 컨텐츠 조각 구성 요소를 통해 **AEM Sites**(또는 AEM Screens)에 컨텐츠 조각을 사용.
   + AEM WCM 코어 구성 요소의 컨텐츠 조각 구성 요소를 통해 **경험 조각**&#x200B;에 컨텐츠 조각을 포함함으로써 경험 조각 사용 사례에서 사용할 수 있습니다.
   + 읽기 전용 사용 사례를 위해 **AEM Content Services** 및 API 페이지를 통해 컨텐츠 조각 변형 컨텐츠를 JSON으로 노출합니다.
   + CRUD 사용 사례에 대해 **AEM Assets HTTP API**&#x200B;를 통해 AEM Assets에 대한 직접 호출을 통해 컨텐츠 조각 컨텐츠(모든 변형)를 JSON으로 직접 노출합니다.

## 경험 조각 아키텍처

!![경험 조각 아키텍처](./assets/experience-fragments-architecture.png)

+ **편집 가능한 템플릿**&#x200B;은  **편집 가능한 템플릿 유형** 및  **AEM 페이지 구성 요소 구현**&#x200B;으로 정의되며, 경험 조각을 구성하는 데 사용할 수 있는 허용된 AEM 구성 요소를 정의합니다.
+ **경험 조각**&#x200B;은 논리 경험을 나타내는 편집 가능한 템플릿의 인스턴스입니다.
+ 경험 조각 **변형**&#x200B;은 편집 가능한 템플릿을 준수하지만 경험(컨텐츠 및 디자인)의 변형을 보유합니다.
+ 경험 조각은 다음과 같은 방법으로 노출/소비할 수 있습니다.
   + AEM 경험 조각 구성 요소를 통해 AEM Sites(또는 AEM Screens)에서 경험 조각 사용.
   + **AEM Content Services** 및 API 페이지를 통해 경험 조각 변형 컨텐츠를 JSON(포함된 HTML이 있음)으로 노출합니다.
   + 경험 조각 변형을 **&quot;일반 HTML&quot;**&#x200B;로 직접 노출합니다.
   + 경험 조각을 **Adobe Target**&#x200B;로 HTML 또는 JSON 오퍼로 내보냅니다.
   + AEM Sites은 기본적으로 HTML 오퍼를 지원하지만, JSON 오퍼에는 사용자 지정 개발이 필요합니다.

## 컨텐츠 조각용 지원 자료

+ [컨텐츠 조각 사용 안내서](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [AEM에서 컨텐츠 조각 사용](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM WCM 코어 구성 요소의 컨텐츠 조각 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/content-fragment-component.html)
+ [컨텐츠 조각 및 AEM Content Services 사용](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [AEM 컨텐츠 서비스 시작하기](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## 경험 조각 지원 자료

+ [경험 조각에 대한 Adobe 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [AEM 경험 구성요소 이해](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [AEM 경험 구성요소 사용](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Adobe Target에서 AEM 경험 구성요소 사용](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
