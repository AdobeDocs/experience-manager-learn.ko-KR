---
title: 컨텐츠 조각 및 경험 조각 이해
description: 컨텐츠 조각과 경험 조각 간의 유사성 및 차이점과 각 유형을 사용하는 시기와 방법을 알아봅니다.
sub-product: assets, sites, content services
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 2%

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
<li>다시 사용 가능하고 프레젠테이션에 관계없이 사용 가능 <strong>콘텐츠</strong>구조화된 데이터 요소(텍스트, 날짜, 참조 등)로 구성된</li>
</ul>
</td>
<td><ul>
<li>컨텐츠 및 프레젠테이션을 정의하는 하나 이상의 AEM 구성 요소를 재사용할 수 있는 복합 구성 요소 <strong>경험</strong> 스스로</li>
</ul>
</td>
</tr><tr><td><strong>핵심 테넌트</strong></td>
<td><ul>
<li>콘텐츠 중심</li>
<li>에 의해 정의됨 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">구조화된 양식 기반의 데이터 모델.</a></li>
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
<li>로 구현됨 <strong>dam:Asset</strong></li>
<li>에 의해 정의됨 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">컨텐츠 조각 모델</a></li>
</ul>
</td>
<td><ul>
<li>로 구현됨 <strong>cq:Page</strong></li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">빌딩 블록</a> 다양한 변형에서 컨텐츠 재사용 허용</li>
</ul>
</td>
</tr><tr><td><strong>기능</strong></td>
<td><ul>
<li>변형</li>
<li>버전</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">동기화</a> 다양한 변형에 걸친 컨텐츠</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">시각적 비교</a> 컨텐츠 조각 버전 의</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">주석</a> 여러 줄 텍스트 요소의</li>
<li>지능형 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">요약</a> 여러 줄 텍스트 요소의 경우</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">번역/로컬라이제이션</a></li>
</ul>
</td>
<td><ul>
<li>변형</li>
<li>변형을 Live Copy로</li>
<li>버전</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">빌딩 블록</a></li>
<li>주석</li>
<li>응답형 레이아웃 및 미리 보기</li>
<li>번역/로컬라이제이션</li>
</ul>
</td>
</tr><tr><td><strong>사용</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM 코어 구성 요소 컨텐츠 조각 구성 요소</a> AEM Sites, AEM Screens 또는 경험 조각에서 사용할 수 있습니다.</li>
<li>를 통한 JSON 내보내기 <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM 컨텐츠 서비스</a> 타사 소비</li>
<li>타사 사용을 위해 AEM HTTP Assets API를 통한 JSON입니다.</li>
</ul>
</td>
<td><ul>
<li>AEM Sites, AEM Screens 또는 기타 경험 조각에서 사용할 AEM 경험 조각 구성 요소입니다.</li>
<li>다음으로 내보내기 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">일반 HTML</a> 타사 시스템에서 사용</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Adobe Target으로 HTML 내보내기</a> 타깃팅된 오퍼에 대해</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM 컨텐츠 조각 사용 안내서</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">AEM에서 컨텐츠 조각 사용</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">경험 조각에 대한 Adobe 설명서</a></li>
</ul>
</td>
</tr></tbody></table>

## 컨텐츠 조각 아키텍처

다음 다이어그램은 AEM 컨텐츠 조각에 대한 전체 아키텍처를 보여 줍니다

!![컨텐츠 조각 아키텍처](./assets/content-fragments-architecture.png)

+ **컨텐츠 조각 모델** 컨텐츠 조각이 캡처하고 노출할 수 있는 컨텐츠를 정의하는 요소(또는 필드)를 정의합니다.
+ 다음 **컨텐츠 조각** 는 논리 컨텐츠 엔티티를 나타내는 컨텐츠 조각 모델의 인스턴스입니다.
+ 컨텐츠 조각 **변형** 하지만 컨텐츠 조각 모델을 준수하면 컨텐츠 변형이 있습니다.
+ 컨텐츠 조각은 다음과 같은 방법으로 노출/소비할 수 있습니다.
   + 컨텐츠 조각에서 사용 **AEM Sites** AEM WCM 코어 구성 요소의 컨텐츠 조각 구성 요소를 통해 (또는 AEM Screens) 을 생성할 수 있습니다.
   + 에 컨텐츠 조각 포함 **경험 조각** 를 통해 AEM WCM 코어 구성 요소의 컨텐츠 조각 구성 요소를 통해 경험 조각 사용 사례에서 사용할 수 있습니다.
   + 를 통해 컨텐츠 조각 변형 컨텐츠를 JSON으로 노출합니다. **AEM 컨텐츠 서비스** 및 API 페이지 를 참조하십시오.
   + 를 통해 AEM Assets에 대한 직접 호출을 통해 컨텐츠 조각 컨텐츠(모든 변형)를 JSON으로 직접 노출합니다. **AEM Assets HTTP API** CRUD 사용 사례용.

## 경험 조각 아키텍처

!![경험 조각 아키텍처](./assets/experience-fragments-architecture.png)

+ **편집 가능한 템플릿**: 차례로 **편집 가능한 템플릿 유형** 그리고 **AEM 페이지 구성 요소 구현**&#x200B;를 정의하면 경험 조각을 작성하는 데 사용할 수 있는 허용된 AEM 구성 요소를 정의할 수 있습니다.
+ 다음 **경험 조각** 는 논리 경험을 나타내는 편집 가능한 템플릿의 인스턴스입니다.
+ 경험 조각 **변형** 편집 가능한 템플릿을 준수하지만 경험(컨텐츠 및 디자인)의 변형을 사용할 수 있습니다.
+ 경험 조각은 다음과 같은 방법으로 노출/소비할 수 있습니다.
   + AEM 경험 조각 구성 요소를 통해 AEM Sites(또는 AEM Screens)에서 경험 조각 사용.
   + 를 통해 경험 조각 변형 컨텐츠를 JSON(포함된 HTML 포함)으로 노출합니다. **AEM 컨텐츠 서비스** 및 API 페이지를 참조하십시오.
   + 경험 조각 변형을 다음으로 직접 노출 **&quot;일반 HTML&quot;**.
   + 경험 조각 내보내기 **Adobe Target** HTML 또는 JSON 오퍼 중 하나로 사용할 수 있습니다.
   + AEM Sites은 기본적으로 HTML 오퍼을 지원하지만, JSON 오퍼에는 사용자 지정 개발이 필요합니다.

## 컨텐츠 조각용 지원 자료

+ [컨텐츠 조각 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [AEM에서 컨텐츠 조각 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM WCM 코어 구성 요소의 컨텐츠 조각 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [컨텐츠 조각 및 AEM 헤드리스 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [AEM 컨텐츠 서비스 시작하기](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## 경험 조각 지원 자료

+ [경험 조각에 대한 Adobe 설명서](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [AEM 경험 구성요소 이해](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [AEM 경험 구성요소 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Adobe Target에서 AEM 경험 구성요소 사용](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
