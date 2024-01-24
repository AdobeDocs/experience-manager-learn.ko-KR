---
title: AEM 프로젝트를 cloud manager 저장소에 푸시
description: 로컬 git 저장소를 cloud manager 저장소로 푸시
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
duration: 35
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# AEM 프로젝트를 cloud manager git 저장소로 푸시

이전 단계에서 AEM 프로젝트는 AEM 인스턴스에서 만든 적응형 Forms 및 테마와 동기화되었습니다.
이제 이러한 변경 사항을 로컬 git 저장소에 추가한 다음 로컬 git 저장소를 cloud manager git 저장소에 푸시해야 합니다.
명령 프롬프트를 열고 c:\cloudmanager\aem-banking-app으로 이동합니다. 다음 명령을 실행합니다

```
git add .
```

이렇게 하면 로컬 git 저장소의 스테이지 분기에 새 파일이 추가됩니다

```
git commit -m "My First AF"
```

이렇게 하면 파일이 로컬 git 저장소의 마스터 분기에 커밋됩니다

```
git push -f bankingapp master:"MyFirstAF"
```

위의 명령에서 우리는 로컬 git 저장소에서 뱅킹 앱의 친숙한 이름으로 식별되는 cloud manager 저장소의 MyFirstAF 분기로 마스터 분기를 푸시하고 있습니다.

## 다음 단계

[개발 환경에 프로젝트 배포](./deploy-to-dev-environment.md)
