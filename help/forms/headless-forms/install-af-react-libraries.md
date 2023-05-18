---
title: 필요한 적응형 양식 반응 라이브러리 설치
description: React 프로젝트에 필요한 종속성을 추가합니다
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 1%

---


# 필요한 종속성 설치

React 프로젝트에서 헤드리스 적응형 양식 사용을 시작하려면 React 프로젝트에 다음 종속성을 설치합니다

* @aemforms/af-react-components
* @aemforms/af-react-renderer

다음 종속성을 포함하도록 package.json을 업데이트합니다. 0.22.41이 현재 버전이었다

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

## 프록시 설정

CORS(Cross-Origin Resource Sharing)는 웹 브라우저에서 앱이 호스팅된 도메인과는 다른 도메인으로 요청을 하도록 제한하는 보안 메커니즘입니다. CORS 오류는 다른 도메인에 호스팅된 API에서 데이터를 가져오려고 할 때 발생할 수 있습니다. 프록시를 설정하여 CORS 제한을 무시하고 React 앱에서 API에 요청을 할 수 있습니다. src 폴더의 setUpProxy.js라는 파일에서 다음 코드를 사용했습니다. **게시 인스턴스를 가리키도록 대상을 변경해야 합니다.**

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

을(를) 설치하고 추가해야 합니다 **http-proxy-middleware** 모듈을 프로젝트에 추가합니다.

## 다음 단계

[포함할 양식 가져오기](./fetch-the-form.md)