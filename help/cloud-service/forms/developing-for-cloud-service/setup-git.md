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
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Install Git


[Install Git](https://git-scm.com/downloads). 기본 설정을 선택하고 설치 프로세스를 완료할 수 있습니다.
Go to your command prompt
Navigate to c:\cloudmanager\aem-banking-app
type in git --version. You should see the version of GIT that is installed on your system

## 로컬 Git 리포지토리 초기화

Make sure you are in the c:\cloudmanager\aem-banking-app folder

```
git init
```

위의 명령은 프로젝트를 git 로컬 리포지토리로 초기화합니다

```
git add .
```

This adds all the project files to the git repository ready to be committed to the git repository

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
git config --global user.password "bqwxfvxq2akawtqx3oztacb5wax5a7"
```

(암호는 cloud manager git 리포지토리 암호입니다.)

로컬 Git 리포지토리에 클라우드 관리자 git 리포지토리를 등록합니다. 아래 명령은 다음과 같습니다 **adobe** 원격 cloud manager git 리포지토리 사용. You could have used any name instead of **adobe**


```java
git remote add adobe https://git.cloudmanager.adobe.com/techmarketingdemos/Program2-p24107/
```

(저장소 URL을 사용하는지 확인합니다.)

원격 저장소가 등록되어 있는지 확인합니다

```java
git remote -v
```



