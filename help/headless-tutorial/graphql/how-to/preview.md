---
title: 컨텐츠 조각 미리 보기
description: 컨텐츠 조각 미리 보기를 모든 작성자에게 사용하여 컨텐츠 변경 사항이 AEM Headless 경험에 미치는 영향을 신속하게 확인하는 방법을 알아봅니다.
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
source-git-commit: ea7cd118d9cba97d2b497f6659f74d2fe8331c66
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# 컨텐츠 조각 미리 보기

AEM Headless 애플리케이션은 통합 작성 미리 보기를 지원합니다. 미리 보기 경험은 AEM Author의 컨텐츠 조각 편집기를 사용자 지정 앱(HTTP를 통해 주소 지정 가능)과 연결하여 미리 보기 중인 컨텐츠 조각을 렌더링하는 앱에 딥 링크를 허용합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3416906/?quality=12&learn=on)

컨텐츠 조각 미리 보기를 사용하려면 몇 가지 조건을 충족해야 합니다.

1. 작성자가 액세스할 수 있는 URL에 앱을 배포해야 합니다
1. AEM 게시 서비스가 아닌 AEM 작성자 서비스에 연결하도록 앱을 구성해야 합니다
1. 이 앱은 사용할 수 있는 URL 또는 경로를 사용하여 설계해야 합니다 [컨텐츠 조각 경로 또는 ID](#url-expressions) 앱 경험에서 미리 볼 컨텐츠 조각을 선택하려면 다음을 수행하십시오.

## 미리 보기 URL

미리 보기 URL, 사용 [URL 표현식](#url-expressions)은 컨텐츠 조각 모델의 속성에 설정됩니다.

![컨텐츠 조각 모델 미리 보기 URL](./assets/preview/cf-model-preview-url.png)

1. 관리자로 AEM 작성자 서비스에 로그인합니다
1. 다음으로 이동 __도구 > 일반 > 컨텐츠 조각 모델__
1. 을(를) 선택합니다 __컨텐츠 조각 모델__ 을(를) 선택합니다. __속성__ 맨 위 작업 표시줄을 만듭니다.
1. 을 사용하여 컨텐츠 조각 모델의 미리 보기 URL을 입력합니다 [URL 표현식](#url-expressions)
   + 미리 보기 URL은 AEM 작성자 서비스에 연결하는 앱 배포를 가리켜야 합니다.

### URL 표현식

각 컨텐츠 조각 모델에는 미리 보기 URL 세트가 있을 수 있습니다. 아래 표에 나열된 URL 표현식을 사용하여 컨텐츠 조각당 미리 보기 URL을 매개 변수화할 수 있습니다. 단일 미리 보기 URL에 여러 URL 표현식을 사용할 수 있습니다.

|  | URL 표현식 | 값 |
| --------------------------------------- | ----------------------------------- | ----------- |
| 컨텐츠 조각 경로 | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| 컨텐츠 조각 ID | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| 컨텐츠 조각 변형 | `${contentFragment.variation}` | `main` |
| 컨텐츠 조각 모델 경로 | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| 컨텐츠 조각 모델 이름 | `${contentFragment.model.name}` | `adventure` |

미리 보기 URL 예:

+ 의 미리 보기 URL __모험__ 모델이 `https://preview.app.wknd.site/adventure${contentFragment.path}` 이(가) `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ 의 미리 보기 URL __문서__ 모델이 `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` 해결됨 `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## 인앱 미리 보기

구성된 컨텐츠 조각 모델을 사용하는 모든 컨텐츠 조각에는 미리 보기 단추가 있습니다. 미리 보기 단추는 컨텐츠 조각 모델의 미리 보기 URL을 열고 열려 있는 컨텐츠 조각의 값을 [URL 표현식](#url-expressions).

![미리보기 버튼](./assets/preview/preview-button.png)

앱에서 컨텐츠 조각 변경 사항을 미리 볼 때 하드 새로 고침(브라우저의 로컬 캐시 지우기)을 수행합니다.

## React 예

AEM Headless GraphQL API를 사용하여 AEM의 모험을 표시하는 간단한 React 애플리케이션인 WKND 앱을 살펴보겠습니다.

예제 코드는에서 사용할 수 있습니다. [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-app).

## URL 및 경로

컨텐츠 조각을 미리 보는 데 사용되는 URL 또는 경로는 [URL 표현식](#url-expressions). 이 WKND 앱의 미리 보기가 활성화된 버전에서는 모험 컨텐츠 조각이 를 통해 표시됩니다. `AdventureDetail` 경로에 바인딩된 구성 요소 `/adventure<CONTENT FRAGMENT PATH>`. 따라서 WKND Adventure 모델의 미리 보기 URL을 `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` 이 경로를 확인합니다.

컨텐츠 조각 미리 보기는 앱에 주소 지정 가능한 경로가 있으며 이 경로를 채울 수 있는 경우에만 작동합니다 [URL 표현식](#url-expressions) 미리 보기 가능한 방식으로 앱의 컨텐츠 조각을 렌더링합니다.

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
        <Switch>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*'>
            <AdventureDetail />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

export default App;
```

### 작성된 콘텐츠 표시

다음 `AdventureDetail` 구성 요소는 을 통해 미리 보기 URL에 삽입된 컨텐츠 조각 경로를 구문 분석합니다 `${contentFragment.path}` [URL 표현식](#url-expressions)를 설정하는 것이 좋습니다.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the content fragment path value which is the parameter used to query for the adventure's details
    
    // Add the leading '/' back on since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const path = '/' + useParams()[0];

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
