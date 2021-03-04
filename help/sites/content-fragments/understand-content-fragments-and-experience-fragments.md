---
title: 컨텐츠 조각 및 경험 조각 이해
description: Adobe Experience Manager의 컨텐츠 조각 및 경험 조각은 표면에서 비슷해 보일 수 있지만 각 컨텐츠 조각은 다른 사용 사례에서 주요 역할을 수행합니다. 컨텐츠 조각 및 경험 조각의 유사성, 차이점, 각 조각 사용 시기 및 방법에 대해 알아봅니다.
sub-product: 자산, 사이트, 콘텐츠 서비스
feature: 컨텐츠 조각, 경험 조각
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: 컨텐츠 관리
role: 비즈니스 전문가
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 5%

---


# 컨텐츠 조각 및 경험 조각 이해

Adobe Experience Manager의 컨텐츠 조각 및 경험 조각은 표면에서 비슷해 보일 수 있지만 각 컨텐츠 조각은 다른 사용 사례에서 주요 역할을 수행합니다. 컨텐츠 조각 및 경험 조각의 유사성, 차이점, 각 조각 사용 시기 및 방법에 대해 알아봅니다.

## 컨텐츠 조각 및 경험 조각 비교

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>컨텐츠 조각(CF)</strong></td>
<td><strong>경험 조각(XF)</strong></td>
</tr><tr><td><strong>정의</strong></td>
<td><ul>
<li>구조화된 데이터 요소(텍스트, 날짜, 참조 등)로 구성된 <strong>content</strong>을(를) 다시 사용할 수 있으며 프레젠테이션에 관계없이</li>
</ul>
</td>
<td><ul>
<li>고유한 <strong>experience</strong>를 구성하는 컨텐츠와 프레젠테이션을 정의하는 하나 이상의 AEM 구성 요소를 재사용할 수 있고 합성하는 구성 요소</li>
</ul>
</td>
</tr><tr><td><strong>핵심 임차인</strong></td>
<td><ul>
<li>컨텐츠 중심</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">구조화된 양식 기반 데이터 모델로 정의됨</a></li>
<li>디자인 및 레이아웃에 관계 없음</li>
<li>채널은 컨텐츠 조각 컨텐츠(레이아웃 및 디자인)의 프레젠테이션을 소유합니다.</li>
</ul>
</td>
<td><ul>
<li>프레젠테이션 중심</li>
<li>AEM 구성 요소의 구조화되지 않은 컴포지션으로 정의</li>
<li>컨텐츠의 디자인 및 레이아웃 정의</li>
<li>채널에서 "있는 그대로" 사용</li>
</ul>
</td>
</tr><tr><td><strong>기술 정보</strong></td>
<td><ul>
<li><strong>dam:Asset</strong>로 구현됨</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">컨텐츠 조각 모델</a>에 의해 정의됨</li>
</ul>
</td>
<td><ul>
<li><strong>cq:Page</strong>로 구현됨</li>
<li>편집 가능한 템플릿으로 정의</li>
<li>기본 HTML 변환</li>
</ul>
</td>
</tr><tr><td><strong>변형</strong></td>
<td><ul>
<li>변화기본은 표준 변수입니다</li>
<li>변형은 채널과 일치할 수 있는 사용 사례에 따라 다릅니다.</li>
</ul>
</td>
<td><ul>
<li>변형은 채널 또는 컨텍스트별</li>
<li>변형이 AEM Live Copy를 통해 동기화됩니다.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">다양한 </a> 변형을 통해 Blocksallow 컨텐츠 재사용 제작</li>
</ul>
</td>
</tr><tr><td><strong>기능</strong></td>
<td><ul>
<li>변형</li>
<li>버전</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank">다양한 </a> 변형을 통한 컨텐츠 동기화</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">컨텐츠 </a> 조각 버전의 시각적 차이</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank">여러 </a> 줄 텍스트 요소에 대한 주석</li>
<li>여러 줄 텍스트 요소의 지능적인 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">요약</a>.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">번역/로컬라이제이션</a></li>
</ul>
</td>
<td><ul>
<li>변형</li>
<li>Live Copy로 변형</li>
<li>버전</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">빌딩 블록</a></li>
<li>주석</li>
<li>반응형 레이아웃 및 미리 보기</li>
<li>번역/로컬라이제이션</li>
</ul>
</td>
</tr><tr><td><strong>사용</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM 핵심 구성 요소 </a> 컨텐츠 조각 구성 요소를 사용하여 AEM Sites, AEM Screens 또는 경험 조각에서 사용할 수 있습니다.</li>
<li>타사 소비에 대해 <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM 콘텐츠 서비스</a>를 통해 JSON 내보내기</li>
<li>제3자 소비를 위한 AEM HTTP 자산 API를 통한 JSON.</li>
</ul>
</td>
<td><ul>
<li>AEM Sites, AEM Screens 또는 기타 경험 조각에서 사용할 AEM 경험 조각 구성 요소입니다.</li>
<li>타사 시스템에서 사용하려면 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">일반 HTML</a>로 내보내기</li>
<li><a href="https://helpx.adobe.com/kr/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">타깃팅된 오퍼에 대해 Adobe </a> 타게팅으로 HTML 내보내기</li>
<li>타깃팅된 오퍼를 위해 Adobe Target으로 JSON 내보내기</li>
</ul>
</td>
</tr><tr><td><strong>일반적인 활용 사례</strong></td>
<td><ul>
<li>구조화된 데이터 입력/양식 기반 컨텐츠</li>
<li>긴 편집 내용(여러 줄 요소)</li>
<li>채널 라이프사이클에서 관리되는 컨텐츠 제공</li>
</ul>
</td>
<td><ul>
<li>채널당 다양한 변경을 사용하여 멀티채널 프로모션용 자료를 중앙에서 관리할 수 있습니다.</li>
<li>웹 사이트의 여러 페이지에 걸쳐 컨텐츠가 다시 사용됩니다.</li>
<li>웹 사이트 크롬(예: 머리글 및 바닥글)</li>
<li>채널 라이프사이클에서 벗어나 경험 전달</li>
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

+ **컨텐츠 조각** 모델에서는 컨텐츠 조각이 캡처 및 노출할 수 있는 컨텐츠를 정의하는 요소(또는 필드)를 정의합니다.
+ **컨텐츠 조각**&#x200B;은 논리 컨텐츠 엔티티를 나타내는 컨텐츠 조각 모델의 인스턴스입니다.
+ 컨텐츠 조각 **변형**&#x200B;은 컨텐츠 조각 모델을 따르지만 컨텐츠의 변형이 있습니다.
+ 컨텐츠 조각은 다음과 같은 방법으로 노출/사용될 수 있습니다.
   + AEM WCM 핵심 구성 요소의 컨텐츠 조각 구성 요소를 통해 **AEM Sites**(또는 AEM Screens)에 컨텐츠 조각 사용
   + AEM WCM 핵심 구성 요소의 컨텐츠 조각 구성 요소를 통해 **경험 조각**&#x200B;에 컨텐츠 조각을 포함하여 경험 조각 사용 사례에서 사용할 수 있습니다.
   + 읽기 전용 사용 사례를 위해 **AEM 컨텐츠 서비스** 및 API 페이지를 통해 컨텐츠 조각 변형 컨텐츠를 JSON으로 노출합니다.
   + CRUD 사용 사례용 **AEM Assets HTTP API**&#x200B;를 통해 AEM Assets에 대한 직접 호출을 통해 컨텐츠 조각 컨텐츠(모든 변형)를 JSON으로 직접 노출

## 경험 조각 아키텍처

!![경험 조각 아키텍처](./assets/experience-fragments-architecture.png)

+ **편집 가능** 템플릿은  **편집 가능 템플릿 유형 및** AEM  **페이지 구성 요소 구현에 의해 차례로 정의되며**, 경험 조각을 구성하는 데 사용할 수 있는 허용된 AEM 구성 요소를 정의합니다.
+ **경험 조각**&#x200B;은 논리적 경험을 나타내는 편집 가능한 템플릿의 인스턴스입니다.
+ 경험 조각 **변형**&#x200B;은 편집 가능한 템플릿을 따르지만 경험(컨텐츠 및 디자인)에 차이가 있습니다.
+ 경험 조각은 다음과 같이 노출/소비할 수 있습니다.
   + AEM 경험 조각 구성 요소를 통해 AEM Sites(또는 AEM Screens)에서 경험 조각 사용.
   + **AEM Content Services** 및 API 페이지를 통해 경험 조각 변형 컨텐츠를 JSON(임베디드 HTML 포함)으로 노출
   + 경험 조각 변형을 **&quot;일반 HTML&quot;**&#x200B;으로 직접 노출합니다.
   + 경험 조각을 HTML 또는 JSON 오퍼로 **Adobe Target**&#x200B;에 내보냅니다.
   + AEM Sites은 기본적으로 HTML 오퍼를 지원하지만 JSON 오퍼는 사용자 정의 개발이 필요합니다.

## 컨텐츠 조각 지원 자료

+ [컨텐츠 조각 사용자 안내서](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [AEM에서 컨텐츠 조각 사용](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM WCM 핵심 구성 요소의 컨텐츠 조각 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/content-fragment-component.html)
+ [컨텐츠 조각 및 AEM 컨텐츠 서비스 사용](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [AEM Content Services 시작하기](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## 경험 조각에 대한 지원 자료

+ [경험 조각에 대한 Adobe 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [AEM 경험 조각 이해](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [AEM 경험 조각 사용](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Adobe Target에서 AEM 경험 조각 사용](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
