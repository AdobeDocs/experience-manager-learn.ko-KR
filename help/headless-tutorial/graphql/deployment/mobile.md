---
title: AEM Headless 모바일 배포
description: 모바일 AEM Headless 배포에 대한 배포 고려 사항에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 2%

---


# AEM Headless 모바일 배포

AEM Headless 모바일 배포는 iOS, Android™ 등을 위한 기본 모바일 앱입니다. 헤드리스 방식으로 AEM에서 컨텐츠를 사용하고 상호 작용할 수 있습니다.

AEM Headless API에 대한 HTTP 연결이 브라우저 컨텍스트에서 시작되지 않으므로 모바일 배포에서는 최소 구성이 필요합니다.

## 배포 구성

모바일 앱 배포에 대해 다음 배포 구성을 적소에 배치해야 합니다.

| 모바일 앱 연결 대상 | AEM Author | AEM 게시 | AEM 미리 보기 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [디스패처 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| CORS(원본 간 리소스 공유) | ✘ | ✘ | ✘ |
| [AEM 호스트](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 예제 모바일 앱

Adobe은 iOS 및 Android™ 모바일 앱의 예를 제공합니다.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="iOS 앱" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="iOS 앱">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="iOS 앱">iOS 앱</a></p>
                   <p class="is-size-6">AEM Headless GraphQL API의 컨텐츠를 사용하는 SwiftUI로 작성된 iOS 앱의 예입니다.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기 예</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="Android™ 앱" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Android 앱">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Android™ 앱">Android™ 앱</a></p>
                   <p class="is-size-6">AEM Headless GraphQL API의 컨텐츠를 사용하는 Java™ Android™ 앱의 예입니다.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기 예</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>


