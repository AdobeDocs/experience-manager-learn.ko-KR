---
title: 시작 키트 구성 요소 만들기
description: 제출된 양식 데이터를 기반으로 자산을 다운로드할 수 있는 링크가 있는 AEM 사이트 페이지를 만듭니다.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '74'
ht-degree: 0%

---

# 시작 키트 구성 요소

최종 사용자가 다운로드할 수 있는 페이지에 자산을 나열하기 위해 페이지 구성 요소를 만들었습니다. 개별 자산에 대한 경로는 **경로**. 제출된 양식 데이터는 포함할 자산을 결정합니다.

다음 코드는 페이지의 자산을 나열합니다.

```html
   <p class="cmp-press-kit__press-kit-size">
        Welcome kit contains ${pressKit.assets.size} assets.
    </p>
<ul class="cmp-press-kit__asset-list" data-sly-list.asset="${pressKit.assets}">
    <li class="cmp-press-kit__asset-item">
        <div class="cmp-press-kit__asset " >
            <div class="cmp-press-kit__asset-content">
                <img class="cmp-press-kit__asset-image" src="${asset.path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png" alt="${asset.name}"/>
                <p class="cmp-press-kit__asset-title">${asset.title}</p>
            </div>
            <div class="cmp-press-kit__asset-actions">
                <a class="cmp-press-kit__asset-download-button" href="${asset.path}">Download</a>
            </div>
        </div>
    </li>
</ul>
</sly>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!ready}"></sly>
```


