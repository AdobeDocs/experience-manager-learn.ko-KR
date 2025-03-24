---
title: 콘텐츠 조각 미리보기
description: 모든 작성자가 콘텐츠 조각 미리보기를 사용하여 콘텐츠 변경 사항이 AEM Headless 경험에 미치는 영향을 빠르게 확인하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
exl-id: 247d40a3-ff67-4c1f-86bf-3794d7ce3e32
duration: 463
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---

# 콘텐츠 조각 미리보기

AEM Headless 애플리케이션은 통합 작성 미리보기를 지원합니다. 미리보기 경험은 AEM 작성자의 콘텐츠 조각 편집기를 사용자 정의 앱(HTTP를 통해 주소 지정 가능)과 연결하여 미리보고 있는 콘텐츠 조각을 렌더링하는 앱에 대한 딥 링크를 허용합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

콘텐츠 조각 미리보기를 사용하려면 다음 몇 가지 조건을 충족해야 합니다.

1. 작성자가 액세스할 수 있는 URL에 앱을 배포해야 합니다.
1. AEM Publish 서비스가 아닌 AEM Author 서비스에 연결하도록 앱을 구성해야 합니다.
1. 앱은 [콘텐츠 조각 경로 또는 ID](#url-expressions)를 사용하여 앱 경험에서 미리 보기 위해 표시할 콘텐츠 조각을 선택할 수 있는 URL 또는 경로로 디자인되어야 합니다.

## URL 미리보기

[URL 표현식](#url-expressions)을(를) 사용하는 미리 보기 URL이 콘텐츠 조각 모델의 속성에서 설정됩니다.

![콘텐츠 조각 모델 미리 보기 URL](./assets/preview/cf-model-preview-url.png)

1. 관리자로 AEM 작성자 서비스에 로그인합니다
1. __도구 > 일반 > 콘텐츠 조각 모델__(으)로 이동
1. __콘텐츠 조각 모델__&#x200B;을(를) 선택하고 맨 위의 작업 표시줄에서 __속성__&#x200B;을(를) 선택합니다.
1. [URL 표현식](#url-expressions)을(를) 사용하여 콘텐츠 조각 모델의 미리 보기 URL을 입력하십시오.
   + 미리보기 URL은 AEM Author 서비스에 연결하는 앱 배포를 가리켜야 합니다.

### URL 표현식

각 콘텐츠 조각 모델에는 미리보기 URL이 설정될 수 있습니다. 미리보기 URL은 아래 표에 나열된 URL 표현식을 사용하여 콘텐츠 조각별로 매개 변수화할 수 있습니다. 여러 URL 표현식을 단일 미리보기 URL에 사용할 수 있습니다.

|                                         | URL 표현식 | 값 |
| --------------------------------------- | ----------------------------------- | ----------- |
| 컨텐츠 조각 경로 | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| 컨텐츠 조각 ID | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| 컨텐츠 조각 변형 | `${contentFragment.variation}` | `main` |
| 콘텐츠 조각 모델 경로 | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| 콘텐츠 조각 모델 이름 | `${contentFragment.model.name}` | `adventure` |

미리보기 URL 예:

+ __Adventure__ 모델의 미리 보기 URL은 `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`(으)로 확인되는 `https://preview.app.wknd.site/adventure${contentFragment.path}`처럼 보일 수 있습니다.
+ __Article__ 모델의 미리 보기 URL은 `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`을(를) 확인하는 `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}`처럼 보일 수 있습니다.

## 인앱 미리 보기

구성된 콘텐츠 조각 모델을 사용하는 모든 콘텐츠 조각에는 미리보기 버튼이 있습니다. 미리 보기 단추를 사용하면 콘텐츠 조각 모델의 미리 보기 URL이 열리고 열린 콘텐츠 조각의 값이 [URL 표현식](#url-expressions)에 삽입됩니다.

![미리보기 버튼](./assets/preview/preview-button.png)

앱에서 콘텐츠 조각 변경 내용을 미리 볼 때 하드 새로 고침(브라우저의 로컬 캐시 지우기)을 수행합니다.

## React 예

AEM Headless GraphQL API를 사용하여 AEM의 모험을 표시하는 간단한 React 애플리케이션인 WKND 앱을 살펴보겠습니다.

예제 코드는 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial)에서 사용할 수 있습니다.

## URL 및 경로

콘텐츠 조각을 미리 보는 데 사용되는 URL 또는 경로는 [URL 표현식](#url-expressions)을 사용하여 구성할 수 있어야 합니다. 이 미리 보기 사용 버전의 WKND 앱에서는 `/adventure<CONTENT FRAGMENT PATH>` 경로에 바인딩된 `AdventureDetail` 구성 요소를 통해 어드벤처 콘텐츠 조각이 표시됩니다. 따라서 이 경로를 확인하려면 WKND Adventure 모델의 미리 보기 URL을 `https://preview.app.wknd.site:3000/adventure${contentFragment.path}`(으)로 설정해야 합니다.

콘텐츠 조각 미리 보기는 앱에 미리 보기 가능한 방식으로 해당 콘텐츠 조각을 렌더링하는 [URL 표현식](#url-expressions)(으)로 채울 수 있는 주소 지정 가능한 경로가 있는 경우에만 작동합니다.

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### 작성된 콘텐츠 표시

`AdventureDetail` 구성 요소는 경로 URL에서 `${contentFragment.path}` [URL 식](#url-expressions)을 통해 미리 보기 URL에 삽입된 콘텐츠 조각 경로를 구문 분석하고 이를 사용하여 WKND 모험을 수집하고 렌더링합니다.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
