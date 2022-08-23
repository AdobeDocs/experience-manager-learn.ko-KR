---
title: Repo Tool을 사용하여 IntelliJ 설정
description: AEM 클라우드 준비 인스턴스와 동기화하도록 IntelliJ를 준비합니다.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 3%

---

# Cygwin 설치


Cygwin은 Microsoft Windows에서 기본적으로 실행되는 POSIX 호환 프로그래밍 및 런타임 환경입니다.
설치 [Cygwin](https://www.cygwin.com/). C:\cygwin64 folder에 을 설치했습니다.
>[!NOTE]
> zip, unzip, curl, rsync 패키지를 Win 설치 시 반드시 설치하십시오

c:\cloudmanager 아래에 adoberepo 라는 폴더를 만듭니다.

[보고 도구 설치].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing 보고 도구는 리포지토리 파일을 복사하여 c:\cloudmanger\adoberepo folder에 배치하는 것에만 해당됩니다.

경로 환경 변수 C:\cygwin64\bin;C:\CloudManager\adoberepo;에 다음을 추가합니다.

## 외부 도구 설정

* IntelliJ 시작
* 설정 창을 시작하려면 Ctrl+Alt+S 키를 누릅니다.
* 도구->외부 도구 를 선택한 다음 + 기호를 클릭하고 스크린샷에 표시된 대로 다음을 입력합니다.
   ![rep](assets/repo.png)
* 그룹 드롭다운 필드에 &quot;repo&quot;를 입력하여 repo라는 그룹을 만들고 사용자가 만드는 모든 명령을 **repo** 그룹


**Put 명령**
**프로그램**: C:\cygwin64\bin\bash
**인수**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**작업 디렉토리**: \$ProjectFileDir\$
![put-command](assets/put-command.png)

**명령 가져오기**
**프로그램**: C:\cygwin64\bin\bash
**인수**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**작업 디렉토리**: \$ProjectFileDir\$
![get-command](assets/get-command.png)

**상태 명령**
**프로그램**: C:\cygwin64\bin\bash
**인수**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**작업 디렉토리**: \$ProjectFileDir\$
![status-command](assets/status-command.png)

**비교 명령**
**프로그램**: C:\cygwin64\bin\bash
**인수**: -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**작업 디렉토리**: \$ProjectFileDir\$
![diff-command](assets/diff-command.png)

에서 .repo 파일 추출 [repo.zip](assets/repo.zip) AEM 프로젝트 루트 폴더에 배치합니다. (C:\CloudManager\aem-banking-application) .repo 파일을 열고 서버와 자격 증명 설정이 환경과 일치하는지 확인합니다.
.gitignore 파일을 열고 파일의 하단에 다음 내용을 추가하고 변경 사항 \# repo .repo 를 저장합니다

ui.content와 같은 aem 뱅킹 애플리케이션 프로젝트 내에서 프로젝트를 선택하고 마우스 오른쪽 버튼을 클릭하면 repo 옵션이 표시되고 repo 옵션 아래에 이전에 추가한 4개의 명령이 표시됩니다.

## AEM 작성자 인스턴스 설정

로컬 시스템에서 클라우드 준비 인스턴스를 빠르게 설정하려면 다음 단계를 따르십시오.
* [최신 AEM SDK 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [최신 AEM Forms addon 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* 다음 폴더 구조 만들기 c:\aemformscs\aem-sdk\author

* AEM SDK zip 파일에서 aem-sdk-quickstart-xxxxxxxxx.jar 파일을 추출하여 c:\aemformscs\aem-sdk\author folder.Rename에 jar 파일의 aem-author-p4502.jar에 배치합니다

* 명령 프롬프트를 열고 c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui로 이동합니다. 그러면 AEM 설치가 시작됩니다.
* 관리자/관리자 자격 증명을 사용하여 로그인
* AEM 인스턴스를 중지합니다
* 다음 폴더 구조를 만듭니다.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* aem-forms-addon-xxxxxx.far을 설치 폴더에 복사합니다.
* 명령 프롬프트를 열고 c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui로 이동합니다. 이렇게 하면 AEM 인스턴스에 패키지에 추가 양식이 배포됩니다.
