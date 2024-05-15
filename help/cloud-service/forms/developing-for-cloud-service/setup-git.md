---
title: Git 설치 및 설정
description: 로컬 git 저장소 초기화
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Git 설치


[Git 설치](https://git-scm.com/downloads). 기본 설정을 선택하고 설치 프로세스를 완료할 수 있습니다.
명령 프롬프트로 이동 git —version에서 c:\cloudmanager\aem-banking-app type으로 이동합니다. 시스템에 설치된 GIT 버전이 표시됩니다

## 로컬 Git 저장소 초기화

c:\cloudmanager\aem-banking-app 폴더에 있는지 확인합니다.

```
git init
```

위의 명령은 프로젝트를 git 로컬 저장소로 초기화합니다.

```
git add .
```

이렇게 하면 git 저장소에 커밋할 준비가 된 git 저장소에 모든 프로젝트 파일이 추가됩니다

```
git commit -m "initial commit"
```

이렇게 하면 파일이 git 저장소에 커밋됩니다



## 로컬 Git 저장소에 Cloud Manager 저장소 등록

Cloud Manager 저장소 액세스
![담당자 정보 액세스](assets/cloud-manager-repo.png)
Cloud Manager 저장소 자격 증명 가져오기
![get-credentials](assets/cloud-manager-repo1.png)

구성 파일에 사용자 이름 저장

```java
git config --global credential.username "gbedekar-adobe-com"
```

구성 파일에 암호 저장

```java
git config --global user.password "XXXX"
```

(암호는 cloud manager git 저장소 암호입니다)

Cloud Manager git 저장소를 로컬 git 저장소에 등록합니다. 아래 명령은 연결됩니다 **뱅킹 앱** 원격 cloud manager git 저장소를 사용합니다. 대신 모든 이름을 사용할 수 있습니다. **뱅킹 앱**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(저장소 URL을 사용해야 합니다.)

원격 저장소가 등록되어 있는지 확인

```java
git remote -v
```

## 다음 단계

[IntelliJ에서 AEM과 AEM 프로젝트 동기화](./intellij-and-aem-sync.md)
