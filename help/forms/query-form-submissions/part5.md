---
title: 로컬 서버에 샘플 배포
description: Azure 포털에 저장된 양식 제출을 쿼리하는 단계를 안내하는 다중 파트 튜토리얼입니다.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 로컬 서버에 샘플 배포

로컬 서버에서 이 사용 사례를 사용하려면 아래 단계를 따르십시오.AEM 인스턴스가 localhost, 4502 포트에서 실행 중이라고 가정합니다.

* [패키지 설치](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) 패키지 관리자를 사용합니다.

* OSGi configMgr을 사용하여 Azure 포털 자격 증명을 제공합니다.
  ![azure-portal](assets/azure-portal-config.png)
스토리지 URI가 슬래시로 끝나고 SAS 토큰이 로 시작하는지 확인합니다.
* 다음으로 이동 [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* 사용자 환경과 일치하도록 다음 3개의 데이터 소스의 인증 설정 편집
  ![data-sources](assets/fdm-data-sources.png)

* 미리 보기 및 제출 [ContactUs 양식](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [양식 제출 쿼리](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

