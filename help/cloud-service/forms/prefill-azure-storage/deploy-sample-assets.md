---
title: 샘플 자산 배포
description: 로컬 클라우드 준비 시스템에 샘플 자산을 배포합니다.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# 샘플 자산 배포

시스템에서 이 사용 사례를 사용하려면 로컬 클라우드 지원 시스템에 다음 자산을 배포하십시오.

* [소개 문서](./introduction.md)에 나열된 모든 필수 구성/계정을 만들었는지 확인하십시오.

* [사용자 지정 적응형 양식 템플릿 및 관련 페이지 구성 요소 설치](./assets/azure-portal-template-page-component.zip)

* [SendGrid 양식 데이터 모델을 설치](./assets/send-grid-form-data-model.zip)합니다. 이 양식 데이터 모델이 작동하려면 API 키와 주소에서 확인된 SendGrid를 제공해야 합니다. 양식 데이터 모델 편집기에서 양식 데이터 모델 테스트

* [Azure 지원 양식 데이터 모델을 설치](./assets/azure-storage-fdm.zip)합니다. 양식 데이터 모델이 작동하려면 Azure 스토리지 계정 자격 증명을 제공해야 합니다. 양식 데이터 모델 편집기에서 양식 데이터 모델을 테스트합니다.

* [샘플 적응형 양식 가져오기](./assets/credit-applications-af.zip)
* [클라이언트 라이브러리 가져오기](./assets/client-lib.zip)
* [양식을 미리 봅니다](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). 유효한 이메일을 입력하고 저장 버튼을 클릭합니다. 양식 데이터는 Azure Storage에 저장되어야 하며 저장된 양식에 대한 링크가 포함된 이메일이 지정된 이메일 주소로 전송됩니다.
