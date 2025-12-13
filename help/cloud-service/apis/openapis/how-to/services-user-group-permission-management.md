---
title: 제품 프로필 및 서비스 사용자 그룹 권한 관리
description: AEM as a Cloud Service에서 제품 프로필 및 서비스 사용자 그룹에 대한 권한을 관리하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17429
thumbnail: KT-17429.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 3230a8e7-6342-4497-9163-1898700f29a4
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# 제품 프로필 및 서비스 사용자 그룹 권한 관리

AEM as a Cloud Service에서 제품 프로필 및 서비스 사용자 그룹에 대한 권한을 관리하는 방법을 알아봅니다.

이 자습서에서는 다음 사항을 학습합니다.

- 제품 프로필 및 서비스와의 연결.
- 서비스 사용자 그룹의 권한을 업데이트하는 중입니다.

## 배경

AEM API를 사용하는 경우 Adobe Developer Console(또는 ADC) 프로젝트의 _자격 증명_&#x200B;에 _제품 프로필_&#x200B;을(를) 할당해야 합니다. _제품 프로필_(및 관련 서비스)은(는) 자격 증명에 대한 _권한 또는 권한 부여_&#x200B;를 제공하여 AEM 리소스에 액세스합니다. 다음 스크린샷에서는 AEM Assets 작성자 API에 대한 _자격 증명_ 및 _제품 프로필_&#x200B;을 볼 수 있습니다.

![자격 증명 및 제품 프로필](../assets/how-to/API-Credentials-Product-Profile.png)

제품 프로필이 하나 이상의 _서비스_&#x200B;와(과) 연결되어 있습니다. AEM as a Cloud Service에서 _서비스_&#x200B;는 저장소 노드에 대해 사전 정의된 ACL(액세스 제어 목록)이 있는 사용자 그룹을 나타내므로 세분화된 권한 관리가 가능합니다.

![기술 계정 사용자 제품 프로필](../assets/s2s/technical-account-user-product-profile.png)

API가 성공적으로 호출되면 ADC 프로젝트의 자격 증명을 나타내는 사용자가 제품 프로필 및 서비스 구성과 일치하는 사용자 그룹과 함께 AEM 작성자 서비스에서 만들어집니다.

![기술 계정 사용자 멤버십](../assets/s2s/technical-account-user-membership.png)

위의 시나리오에서 사용자 `1323d2...`은(는) AEM Author 서비스에서 만들어지고 사용자 그룹 `AEM Assets Collaborator Users - Service` 및 `AEM Assets Collaborator Users - author - Program XXX - Environment XXX`의 구성원입니다.

## Update Services 사용자 그룹 권한

대부분의 _서비스_&#x200B;은(는) _서비스_&#x200B;와(과) 같은 이름을 가진 AEM 인스턴스의 사용자 그룹을 통해 AEM 리소스에 _읽기_ 권한을 제공합니다.

자격 증명(기술 계정 사용자)에 AEM 리소스의 _만들기, 업데이트, 삭제_(CUD)와 같은 추가 권한이 필요한 경우가 있습니다. 이러한 경우 AEM 인스턴스에서 _서비스_ 사용자 그룹의 권한을 업데이트할 수 있습니다.

예를 들어 AEM Assets 작성자 API 호출이 GET이 아닌 요청[에 대해 ](../use-cases/invoke-api-using-oauth-s2s.md#403-error-for-non-get-requests)403 오류를 받으면 AEM 인스턴스에서 _AEM Assets Collaborator 사용자 - 서비스_ 사용자 그룹의 권한을 업데이트할 수 있습니다.

권한 사용자 인터페이스 또는 [Sling 저장소 초기화](https://sling.apache.org/documentation/bundles/repository-initialization.html) 스크립트를 사용하여 AEM 인스턴스에서 기본 사용자 그룹의 권한을 업데이트할 수 있습니다.

### 권한 사용자 인터페이스를 사용하여 권한 업데이트

권한 사용자 인터페이스를 사용하여 서비스 사용자 그룹(예: `AEM Assets Collaborator Users - Service`)의 권한을 업데이트하려면 다음 단계를 수행합니다.

- AEM 인스턴스의 **도구** > **보안** > **권한**(으)로 이동합니다.

- 서비스 사용자 그룹(예: `AEM Assets Collaborator Users - Service`)을 검색합니다.

  ![사용자 그룹 검색](../assets/how-to/search-user-group.png)

- 사용자 그룹에 새 ACE(액세스 제어 항목)를 추가하려면 **ACE 추가**&#x200B;를 클릭하십시오.

  ![ACE 추가](../assets/how-to/add-ace.png)

### 저장소 초기화 스크립트를 사용한 권한 업데이트

저장소 초기화 스크립트를 사용하여 서비스 사용자 그룹(예: `AEM Assets Collaborator Users - Service`)의 권한을 업데이트하려면 다음 단계를 수행합니다.

- 즐겨찾는 IDE에서 AEM 프로젝트를 엽니다.

- `ui.config` 모듈로 이동

- `ui.config/src/main/content/jcr_root/apps/<PROJECT-NAME>/osgiconfig/config.author`에 다음 내용이 포함된 `org.apache.sling.jcr.repoinit.RepositoryInitializer-services-group-acl-update.cfg.json` 파일을 만듭니다.

  ```json
  {
      "scripts": [
          "set ACL for \"AEM Assets Collaborator Users - Service\" (ACLOptions=ignoreMissingPrincipal)",
          "    allow jcr:read,jcr:versionManagement,crx:replicate,rep:write on /content/dam",
          "end"
      ]
  }
  ```

- 변경 사항을 커밋하고 저장소에 푸시합니다.

- [AEM 전체 스택 파이프라인](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline)을 사용하여 Cloud Manager 인스턴스에 변경 내용을 배포합니다.

- **권한** 보기를 사용하여 사용자 그룹의 권한을 확인할 수도 있습니다. AEM 인스턴스의 **도구** > **보안** > **권한**(으)로 이동합니다.

  ![권한 보기](../assets/how-to/permissions-view.png)

### 권한 확인

위의 방법 중 하나를 사용하여 권한을 업데이트한 후 에셋 메타데이터를 업데이트하라는 PATCH 요청이 이제 문제 없이 작동해야 합니다.

![PATCH 요청](../assets/how-to/patch-request.png)

## 요약

AEM as a Cloud Service에서 제품 프로필 및 서비스 사용자 그룹에 대한 권한을 관리하는 방법을 배웠습니다. 권한 사용자 인터페이스 또는 저장소 초기화 스크립트를 사용하여 AEM 인스턴스에서 서비스 사용자 그룹의 권한을 업데이트할 수 있습니다.
