---
title: 필요한 적응형 양식 React 라이브러리 설치
description: React 프로젝트에 필요한 종속성 추가
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 필요한 종속성 설치

React 프로젝트에서 Headless 적응형 양식을 사용하려면 React 프로젝트에 다음 종속성을 설치하십시오

* @aemforms/af-react-components
* @aemforms/af-react-renderer

다음 종속성을 포함하도록 package.json을 업데이트합니다. 작성 당시 0.22.41이 현재 버전이었다

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>이 자습서의 드롭다운 목록과 카드 레이아웃은 [자료 UI 라이브러리](https://mui.com/). 시스템에서 코드가 작동하려면 적절한 자료 UI 패키지를 다운로드해야 합니다.

## 프록시 설정

CORS(원본 간 리소스 공유)는 웹 브라우저가 앱이 호스트되는 도메인과 다른 도메인에 요청을 하지 못하도록 제한하는 보안 메커니즘입니다. CORS 오류는 다른 도메인에 호스팅된 API에서 데이터를 가져오려고 할 때 발생할 수 있습니다. 프록시를 설정하면 CORS 제한을 무시하고 React 앱에서 API를 요청할 수 있습니다. src 폴더의 setUpProxy.js라는 파일에서 다음 코드를 사용했습니다. **게시 인스턴스를 가리키도록 대상을 변경해야 합니다.**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

을(를) 설치하고 추가해야 합니다. **http-proxy-middle** 모듈을 프로젝트에 추가합니다.

## 다음 단계

[포함할 양식 가져오기](./fetch-the-form.md)
