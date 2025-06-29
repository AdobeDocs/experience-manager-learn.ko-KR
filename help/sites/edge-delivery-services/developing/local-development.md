---
title: Edge Delivery Services를 위한 로컬 개발 환경 설정
description: Edge Delivery Services를 위한 로컬 개발 환경을 설정하는 방법을 알아봅니다.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 169
exl-id: 0f3e50f0-88d8-46be-be8b-0f547c3633a6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '87'
ht-degree: 100%

---

# 로컬 개발 환경 설정

Edge Delivery Services용 개발을 위한 로컬 개발 환경을 설정하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3434738/?learn=on&captions=kor)


## 비디오에 설명된 단계

1. AEM CLI 설치

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. [AEM 보일러플레이트](https://github.com/adobe/aem-boilerplate) 템플릿으로 생성된 git 저장소인 프로젝트 디렉터리로 디렉터리를 변경합니다.

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. AEM CLI를 실행하여 로컬 AEM 인스턴스를 시작합니다.

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. 웹 브라우저에서 http://localhost:3000/을 열어 AEM 웹 사이트를 확인합니다.
