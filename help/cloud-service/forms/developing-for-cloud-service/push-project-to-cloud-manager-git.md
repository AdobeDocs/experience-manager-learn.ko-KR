---
title: Cloud Manager 저장소에 AEM 프로젝트 푸시
description: 클라우드 관리자 리포지토리에 로컬 git 리포지토리 푸시
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Cloud Manager git 리포지토리에 AEM 프로젝트 푸시

이전 단계에서는 AEM 프로젝트를 AEM 인스턴스에서 만든 적응형 Forms 및 테마와 동기화했습니다.
이제 이러한 변경 사항을 로컬 Git 리포지토리에 추가한 다음 로컬 Git 리포지토리를 클라우드 관리자 git 리포지토리에 푸시해야 합니다.
명령 프롬프트를 열고 c:\cloudmanager\aem-banking-app Execute the following commands으로 이동합니다.

```
git add .
```

이렇게 하면 새 파일이 로컬 git 저장소의 스테이지 분기에 추가됩니다

```
git commit -m "My First AF"
```

이렇게 하면 로컬 Git 저장소의 마스터 분기에 파일이 커밋됩니다

```
git push -f bankingapp master:"MyFirstAF"
```

위의 명령에서는 로컬 git 리포지토리의 마스터 분기를 뱅킹앱 친숙한 이름으로 식별된 클라우드 관리자 리포지토리의 MyFirstAF 분기로 푸시합니다.
