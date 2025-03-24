---
title: Windows에서 AEM Forms을 설치하기 위한 간소화된 단계
description: Windows에서 AEM Forms을 설치하는 빠르고 간편한 단계
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 1%

---

# Windows에서 AEM Forms을 설치하기 위한 간소화된 단계

>[!NOTE]
>
>AEM Forms을 사용하려는 경우 AEM 빠른 시작 jar를 두 번 클릭하지 마십시오.
>
>또한 AEM Forms 설치 폴더 경로에 공백이 없는지 확인합니다.
>
>예를 들어 c:\jack 및 jill\AEM Forms 폴더에 AEM Forms을 설치하지 마십시오.

>[!NOTE]
>
>AEM Forms 6.5를 설치하는 경우 다음 32비트 Microsoft Visual C++ 재배포용 파일을 설치했는지 확인하십시오.
>
>* Microsoft Visual C++ 2008 재배포 가능
>* Microsoft Visual C++ 2010 재배포 가능
>* Microsoft Visual C++ 2012 재배포 가능
>* Microsoft Visual C++ 2013 재배포 가능 항목(6.5부터)

AEM Forms을 설치하려면 [공식 설명서](https://helpx.adobe.com/kr/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html)를 따르는 것이 좋습니다. 다음 단계에 따라 Windows 환경에 AEM Forms을 설치하고 구성할 수 있습니다.

* 적절한 JDK가 설치되어 있는지 확인합니다.
   * 필요한 AEM 6.2: Oracle SE 8 JDK 1.8.x(64비트)
   * 필요한 AEM 6.3 및 AEM 6.4: Oracle SE 8 JDK 1.8.x(64비트)
   * AEM 6.5 JDK 8 또는 JDK 11이 필요합니다.
   * [공식 JDK 요구 사항](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=ko-KR)이 여기에 나열됩니다.
* JAVA_HOME이 설치된 JDK를 가리키도록 설정되어 있는지 확인합니다.
   * Windows에서 JAVA_HOME 변수를 만들려면 아래 단계를 따르십시오.
      * 내 컴퓨터 를 마우스 오른쪽 단추로 클릭하고 속성 을 선택합니다.
      * 고급 탭에서 환경 변수 를 선택하고 JAVA_HOME이라는 새 시스템 변수를 만듭니다.
      * 시스템에 설치된 JDK를 가리키도록 변수 값을 설정합니다. 예 c:\program files\java\jdk1.8.0_25

* C 드라이브에 AEMForms라는 폴더 만들기
* AEMQuickStart.Jar를 찾아 AEMForms 폴더로 이동합니다.
* 이 AEMForms 폴더에 license.properties 파일 복사
* 다음 내용으로 &quot;StartAemForms.bat&quot;라는 배치 파일을 만듭니다.
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * 여기서 AEM_6.5_Quickstart.jar는 내 AEM quickstart jar의 이름입니다.
   * jar의 이름을 임의의 이름으로 바꿀 수 있지만 배치 파일에 이름이 반영되어 있는지 확인하십시오. AEMForms 폴더에 배치 파일을 저장합니다.

* 새 명령 프롬프트를 열고 _c:\aemforms_(으)로 이동합니다.

* 명령 프롬프트에서 StartAemForms.bat 파일을 실행합니다.

* 시작의 진행 상황을 나타내는 작은 대화 상자가 표시됩니다.

* 시작이 완료되면 sling.properties 파일을 엽니다. c:\AEMForms\crx-quickstart\conf 폴더에 있습니다.

* 파일의 아래쪽을 향해 다음 2줄을 복사합니다
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* 문서 서비스가 작동하려면 다음 두 속성이 필요합니다
* sling.properties 파일을 저장합니다.
* [적절한 양식 추가 패키지 다운로드](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 패키지에 양식 추가 기능을 설치하십시오.
* 추가 기능 패키지를 설치한 후 다음 단계를 수행해야 합니다

   * **모든 번들이 활성 상태인지 확인하십시오. (AEMFD 서명 번들 제외).**
   * **모든 번들이 활성 상태가 되는 데 일반적으로 5분 이상 걸립니다.**

   * **AEMFD 서명 번들을 제외한 모든 번들이 활성 상태가 되면 시스템을 다시 시작하여 AEM Forms 설치를 완료하십시오**

## 허용 목록에 대한 sun.util.calendar 패키지

1. [브라우저 창](http://localhost:4502/system/console/configMgr)에서 Felix 웹 콘솔 열기
1. 역직렬화 방화벽 구성 검색 및 열기: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. `sun.util.calendar`을(를) `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`의 새 항목으로 추가
1. 변경 사항을 저장합니다.

축하합니다!!! 이제 시스템에 AEM Forms을 설치하고 구성했습니다.
필요에 따라 서버에서 [Reader 확장 기능](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) 또는 [PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html)을 구성할 수 있습니다
