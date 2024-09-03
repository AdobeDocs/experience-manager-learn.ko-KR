---
title: HTML5 양식 제출에 대한 AEM 워크플로우 트리거 - 활용 사례 작업
description: 로컬 시스템에 샘플 자산 배포
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 90
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# 시스템에서 작동하도록 이 사용 사례를 가져오는 중

>[!NOTE]
>
>샘플 에셋이 시스템에서 작동하려면 AEM Forms 작성자 및 AEM Forms 게시 인스턴스에 대한 액세스 권한이 있는 것으로 간주됩니다.

이 사용 사례를 로컬 시스템에서 작업하려면 다음 단계를 따르십시오.

## AEM Forms 작성자 인스턴스에 다음 배포

* [MobileFormToWorkflow 번들 설치](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [HTML 5 양식의 데이터를 XDP와 병합하고 대화형 PDF를 반환하는 사용자 지정 프로필을 가져옵니다](assets/customprofile.zip).

* [서비스 사용자 번들로 개발 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
configMgr을 사용하여 Apache Sling 서비스 사용자 매퍼 서비스에 다음 항목을 추가합니다

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* [configMgr](http://localhost:4502/system/console/configMg)을 사용하여 AEM 서버 자격 증명 구성에서 폴더 이름을 지정하여 양식 제출을 다른 폴더에 저장할 수 있습니다. 폴더를 변경하는 경우 폴더에 런처를 만들어 워크플로 **ReviewSubmittedPDF**&#x200B;를 트리거해야 합니다.

![config-author](assets/author-config.png)
* [패키지 관리자를 사용하여 샘플 xdp 및 워크플로 패키지를 가져옵니다](assets/xdp-form-and-workflow.zip).


## 게시 인스턴스에 다음 자산 배포

* [MobileFormToWorkflow 번들 설치](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [configMgr](http://localhost:4503/system/console/configMgr)을 사용하여 AEM 서버 자격 증명에 제출된 데이터를 저장하려면 작성자 인스턴스의 사용자 이름/암호와 AEM 저장소의 **기존 위치**를 지정하십시오. AEM Workflow Server에 끝점의 URL을 그대로 둘 수 있습니다. 지정된 노드의 제출에서 데이터를 추출하고 저장하는 종단점입니다.
  ![publish-config](assets/publish-config.png)

* [서비스 사용자 번들로 개발 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
* [osgi 구성을 엽니다](http://localhost:4503/system/console/configMgr).
* **Apache Sling 레퍼러 필터**&#x200B;를 검색합니다. 비어 있음 확인란이 선택되어 있는지 확인합니다.
* [HTML 5 양식의 데이터를 XDP와 병합하고 대화형 PDF를 반환하는 사용자 지정 프로필을 가져옵니다](assets/customprofile.zip).


## 솔루션 테스트

* 작성자 인스턴스에 로그인
* [w9.xdp의 고급 속성을 편집합니다](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp). 제출 URL 및 렌더링 프로필이 아래와 같이 올바르게 설정되어 있는지 확인하십시오.
  ![xdp-advanced-properties](assets/mobile-form-properties.png)

* Publish w9.xdp
* 게시 인스턴스에 로그인
* [w9 양식 미리 보기](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* 여러 필드를 입력한 다음 도구 모음의 단추를 클릭하여 대화형 PDF을 다운로드합니다.
* Acrobat을 사용하여 다운로드한 PDF을 입력하고 제출 버튼을 누릅니다.
* 성공 메시지를 받아야 합니다.
* AEM 작성자 인스턴스에 관리자로 로그인
* [AEM 받은 편지함 확인](http://localhost:4502/aem/inbox)
* 제출된 PDF을 검토하려면 작업 항목이 있어야 합니다.

>[!NOTE]
>
>일부 고객은 게시 인스턴스에서 실행되는 서블릿에 PDF을 제출하는 대신 Tomcat과 같은 서블릿 컨테이너에 서블릿을 배포했습니다. 모든 것은 고객이 익숙한 토폴로지에 따라 다릅니다.이 자습서에서는 pdf 제출을 처리하기 위해 게시 인스턴스에 배포된 서블릿을 사용할 예정입니다.
