---
title: Edge Delivery Services을 위한 로컬 개발 환경 설정
description: Edge Delivery Services을 위한 로컬 개발 환경을 설정하는 방법.
version: 6.5, Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
source-git-commit: d17544c4f8dda03e5147a1f48dbbdae005ee9438
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---


# 로컬 개발 환경 설정

Edge Delivery Services 개발을 위한 로컬 개발 환경을 설정하는 방법.

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## 비디오에 요약된 단계

1. AEM CLI 설치

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. 디렉터리를 프로젝트 디렉터리인 로 변경합니다. 이 디렉터리는 [AEM 보일러 플레이트](https://github.com/adobe/aem-boilerplate) 템플릿.

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

1. 웹 브라우저 http://localhost:3000/ 를 열어 AEM 웹 사이트를 확인합니다.

