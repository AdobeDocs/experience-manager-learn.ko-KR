---
title: AEM Forms에서 사용자 지정 자산 유형 나열
seo-title: AEM Forms에서 사용자 지정 자산 유형 나열
description: AEM Forms의 사용자 지정 자산 유형 목록 2부
seo-description: AEM Forms의 사용자 지정 자산 유형 목록 2부
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: 개발
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# AEM Forms {#listing-custom-asset-types-in-aem-forms}에 사용자 지정 자산 유형 나열

## 사용자 지정 템플릿 {#creating-custom-template} 만들기


이 문서의 목적에 따라, 동일한 페이지에 사용자 지정 자산 유형과 OTB 자산 유형을 표시하는 사용자 지정 템플릿을 만듭니다. 사용자 정의 템플릿을 만들려면 다음 지침을 따르십시오

1. 슬링 만들기:폴더 아래의 /apps. 이름을 &quot; myportalcomponent &quot;
1. &quot;fpContentType&quot; 속성을 추가합니다. 값을 &quot;**/libs/fd/ fp/formTemplate&quot;로 설정합니다.**
1. &quot;title&quot; 속성을 추가하고 해당 값을 &quot;사용자 지정 템플릿&quot;으로 설정합니다. 검색 및 작성자 구성 요소의 드롭다운 목록에 표시되는 이름입니다.
1. 이 폴더 아래에 &quot;template.html&quot;을 만듭니다. 이 파일에는 스타일을 지정할 코드가 들어 있으며 다양한 에셋 유형이 표시됩니다.

![appsfolder](assets/appsfolder_.png)

다음 코드에서는 검색 및 라이브러리 구성 요소를 사용하는 다양한 유형의 자산을 나열합니다. 데이터 유형 = &quot;비디오&quot; 태그로 표시된 각 유형의 자산에 대해 별도의 html 요소를 만듭니다. &quot;비디오&quot;의 자산 유형의 경우 &lt;video> 요소를 사용하여 비디오 인라인을 재생합니다. &quot;worddocuments&quot;의 자산 유형에 대해 서로 다른 html 마크업을 사용합니다.

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>11행 - DAM에서 원하는 이미지를 가리키도록 이미지 src를 변경하십시오.
>
>이 템플릿에 적응형 Forms을 나열하려면 새 div를 만들고 해당 데이터 유형 속성을 &quot;guide&quot;로 설정합니다. data-type=&quot;printForm&quot;인 div를 복사하여 붙여 넣고 새로 복사한 div의 data-type을 &quot;guide&quot;로 설정할 수 있습니다.

## 검색 및 라이브러리 구성 요소 구성 {#configure-search-and-lister-component}

사용자 지정 템플릿을 정의했으면 이제 이 사용자 지정 템플릿을 &quot;검색 및 라이브러리&quot; 구성 요소와 연결해야 합니다. 브라우저 [을 이 url ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html)에 지정합니다.

디자인 모드로 전환하고 허용되는 구성 요소 그룹에 검색 및 작성자 구성 요소를 포함하도록 단락 시스템을 구성합니다. 검색 및 작성자 구성 요소는 문서 서비스 그룹의 일부입니다.

편집 모드로 전환하고 Search 및 Lister 구성 요소를 ParSys에 추가합니다.

&quot;검색 및 라이브러리&quot; 구성 요소의 구성 속성을 엽니다. &quot;자산 폴더&quot; 탭이 선택되어 있는지 확인합니다. 검색 및 라이브러리 구성 요소의 자산을 나열할 폴더를 선택합니다. 이 아티클을 위해

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

탭을 &quot;표시&quot; 탭으로 이동합니다. 검색 및 라이브러리 구성 요소에 자산을 표시할 템플릿을 선택합니다.

아래에 표시된 것처럼 드롭다운에서 &quot;사용자 지정 템플릿&quot;을 선택합니다.

![searchanlister](assets/searchandlistercomponent.gif)

포털에 나열할 자산 유형을 구성합니다. 자산 탭의 유형을 &quot;자산 목록&quot;에 구성하고 자산 유형을 구성하려면. 이 예에서는 다음과 같은 유형의 자산을 구성합니다.

1. MP4 파일
1. Word 문서
1. 문서(OOTB 자산 유형)
1. 양식 템플릿(OOTB 자산 유형)

다음 스크린샷은 나열을 위해 구성된 자산 유형을 보여줍니다

![assettypes](assets/assettypes.png)

Search 및 Lister Portal 구성 요소를 구성했으므로 이제 시작 메뉴를 볼 때입니다. 브라우저 [을 이 url ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled)에 지정합니다. 결과는 아래 이미지와 비슷해야 합니다.

>[!NOTE]
>
>포털에서 게시 서버에 사용자 지정 자산 유형을 나열하는 경우 **/apps/fd/fp/extensions/querybuilder** 노드에 &quot;fd-service&quot; 사용자에게 &quot;읽기&quot; 권한을 부여하십시오.

![자산 ](assets/assettypeslistings.png)
[유형패키지 관리자를 사용하여 이 패키지를 다운로드하고 설치하십시오.](assets/customassettypekt1.zip) 여기에는 검색 및 라이브러리 구성 요소를 사용하여 나열하는 에셋 유형으로 사용되는 샘플 mp4 및 word 문서 및 xdp 파일이 포함됩니다
