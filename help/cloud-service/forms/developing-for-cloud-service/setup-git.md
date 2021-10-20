---
title: Git 설치 및 설정
description: 로컬 Git 리포지토리 초기화
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Git 설치


[Git 설치](https://git-scm.com/downloads). 기본 설정을 선택하고 설치 프로세스를 완료할 수 있습니다.
명령 프롬프트에서 c:\cloudmanager\aem-banking-app type in git —version으로 이동합니다. 시스템에 설치된 GIT 버전이 표시됩니다

## 로컬 Git 리포지토리 초기화

c:\cloudmanager\aem-banking-app folder폴더에 있는지 확인하십시오.

```
git init
```

위의 명령은 프로젝트를 git 로컬 리포지토리로 초기화합니다

```
git add .
```

이렇게 하면 모든 프로젝트 파일이 Git 리포지토리에 커밋될 준비가 된 git 리포지토리에 추가됩니다

```
git commit -m "initial commit"
```

이렇게 하면 파일이 Git 리포지토리에 커밋됩니다



## 로컬 Git 리포지토리에 클라우드 관리자 리포지토리 등록

클라우드 관리자 리포지토리 액세스
![rep 정보에 액세스](assets/cloud-manager-repo.png)
클라우드 관리자 리포지토리 자격 증명 가져오기
![getCredentials](assets/cloud-manager-repo1.png)

구성 파일에 사용자 이름을 저장합니다

```java
git config --global credential.username "gbedekar-adobe-com"
```

구성 파일에 암호 저장

```java
git config --global user.password "bqwxfvxq2akawtqx3oztacb5tkx5a7"
```

(암호는 cloud manager git 리포지토리 암호입니다.)

로컬 Git 리포지토리에 클라우드 관리자 git 리포지토리를 등록합니다. 아래 명령은 다음과 같습니다 **adobe** 원격 cloud manager git 리포지토리 사용. 대신 모든 이름을 사용할 수 있었습니다. **adobe**


```java
git remote add adobe https://git.cloudmanager.adobe.com/techmarketingdemos/Program2-p24107/
```

(저장소 URL을 사용하는지 확인합니다.)

원격 저장소가 등록되어 있는지 확인합니다

```java
git remote -v
```



