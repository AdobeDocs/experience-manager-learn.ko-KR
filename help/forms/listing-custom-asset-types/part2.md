---
title: AEM Forms에서 사용자 지정 에셋 유형 나열
description: AEM Forms에서 사용자 지정 에셋 유형 나열 중 2부
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
duration: 136
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# AEM Forms에서 사용자 지정 에셋 유형 나열 {#listing-custom-asset-types-in-aem-forms}

## 사용자 지정 템플릿 만들기 {#creating-custom-template}

이 문서에서는 사용자 지정 자산 유형과 OOTB 자산 유형을 동일한 페이지에 표시하는 사용자 지정 템플릿을 만들고 있습니다. 사용자 지정 템플릿을 만들려면 다음 지침을 따르십시오

1. /apps 아래에 sling: 폴더를 만듭니다. 이름을 &quot; myportalcomponent &quot; 로 지정합니다.
1. &quot;fpContentType&quot; 속성을 추가합니다. 값을 &quot;(으)로 설정&#x200B;**/libs/fd/ fp/formTemplate&quot;.**
1. &quot;title&quot; 속성을 추가하고 값을 &quot;custom template&quot;으로 설정합니다. 검색 및 목록 구성 요소의 드롭다운 목록에 표시되는 이름입니다
1. 이 폴더 아래에 &quot;template.html&quot;을 만듭니다. 이 파일에는 스타일을 지정하고 다양한 에셋 유형을 표시하는 코드가 있습니다.

![appsfolder](assets/appsfolder_.png)

다음 코드는 검색 및 목록 구성 요소를 사용하는 다양한 유형의 자산을 나열합니다. data-type = &quot;videos&quot; 태그로 표시된 대로 각 에셋 유형에 대해 별도의 html 요소를 만듭니다. &quot;비디오&quot;의 자산 유형에는 &lt;video> 요소를 사용하여 비디오를 인라인으로 재생합니다. &quot;worddocuments&quot;의 에셋 유형의 경우 다른 html 표시를 사용합니다.

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
>11행 - DAM에서 선택한 이미지를 가리키도록 src 이미지를 변경하십시오.
>
>이 템플릿에 적응형 Forms을 나열하려면 새 div를 만들고 데이터 유형 속성을 &quot;guide&quot;로 설정합니다. data-type=&quot;printForm&quot;인 div를 복사하여 붙여넣고 새로 복사된 div의 데이터 유형을 &quot;안내서&quot;로 설정할 수 있습니다.

## 검색 및 목록 구성 요소 구성 {#configure-search-and-lister-component}

사용자 지정 템플릿을 정의한 후에는 이 사용자 지정 템플릿을 &quot;검색 및 목록&quot; 구성 요소와 연결해야 합니다. 브라우저 지정 [이 URL로](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

디자인 모드로 전환하고 허용된 구성 요소 그룹에 검색 및 목록 구성 요소를 포함하도록 단락 시스템을 구성합니다. 검색 및 목록 작성자 구성 요소는 문서 서비스 그룹의 일부입니다.

편집 모드로 전환하고 검색 및 목록 작성기 구성 요소를 ParSys에 추가합니다.

&quot;검색 및 목록&quot; 구성 요소의 구성 속성을 엽니다. &quot;에셋 폴더&quot; 탭이 선택되어 있는지 확인합니다. 검색 및 목록 구성 요소의 에셋을 나열할 폴더를 선택합니다. 이 문서의 목적을 위해 다음을 선택했습니다.

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

탭으로 이동하여 &quot;표시&quot; 탭을 표시합니다. 여기에서 검색 및 목록 구성 요소에 에셋을 표시할 템플릿을 선택합니다.

아래 표시된 대로 드롭다운에서 &quot;사용자 지정 템플릿&quot;을 선택합니다.

![searchandlister](assets/searchandlistercomponent.gif)

포털에 나열할 자산 유형을 구성합니다. 에셋의 탭 유형을 &quot;에셋 목록&quot;으로 구성하고 에셋 유형을 구성합니다. 이 예제에서는 다음 유형의 자산을 구성했습니다

1. MP4 파일
1. Word 문서
1. 문서(OOTB 자산 유형)
1. 양식 템플릿(OOTB 에셋 유형)

다음 스크린샷은 목록에 대해 구성된 자산 유형을 보여 줍니다

![assettypes](assets/assettypes.png)

이제 검색 및 목록 포털 구성 요소를 구성했으므로 목록 목록이 작동 중인지 확인할 차례입니다. 브라우저 지정 [이 URL로](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). 결과는 아래 표시된 이미지와 같아야 합니다.

>[!NOTE]
>
>포털이 게시 서버에 사용자 지정 자산 유형을 나열하는 경우 &quot;fd-service&quot; 사용자에게 노드에 대한 &quot;읽기&quot; 권한을 부여했는지 확인하십시오. **/apps/fd/fp/extensions/querybuilder**

![assettypes](assets/assettypeslistings.png)
[패키지 관리자를 사용하여 이 패키지를 다운로드하여 설치하십시오.](assets/customassettypekt1.zip) 여기에는 검색 및 목록 구성 요소를 사용하여 나열할 자산 유형으로 사용되는 샘플 mp4, word 문서 및 xdp 파일이 포함되어 있습니다
