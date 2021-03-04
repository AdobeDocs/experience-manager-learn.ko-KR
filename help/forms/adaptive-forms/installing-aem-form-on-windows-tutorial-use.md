---
title: Windows에서 AEM Forms 설치를 위한 간단한 단계
seo-title: Windows에서 AEM Forms 설치를 위한 간단한 단계
description: Windows에 AEM Forms을 설치하는 빠르고 간편한 단계
seo-description: Windows에 AEM Forms을 설치하는 빠르고 간편한 단계
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: 적응형 양식
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 4%

---


# Windows에서 AEM Forms 설치를 위한 간단한 단계

>[!NOTE]
>
>AEM Forms을 사용하려면 AEM 빠른 시작 jar를 두 번 클릭하지 마십시오.
>
>또한 AEM Forms 설치 폴더 경로에 공백이 없는지 확인합니다.
>
>예를 들어 c:\jack and jill\AEM Forms folder에서 AEM Forms을 설치하지 마십시오.

>[!NOTE]
>
>AEM Forms 6.5를 설치하는 경우 다음 32비트 Microsoft Visual C++ 재배포용 파일을 설치했는지 확인하십시오.
>
>* Microsoft Visual C++ 2008 재배포 가능
>* Microsoft Visual C++ 2010 재배포 가능
>* Microsoft Visual C++ 2012 재배포 가능
>* Microsoft Visual C++ 2013 재배포 가능(6.5의 경우)


AEM Forms을 설치하려면 [공식 설명서](https://helpx.adobe.com/kr/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html)를 따르는 것이 좋습니다. Windows 환경에 AEM Forms을 설치하고 구성하려면 다음 단계를 따르십시오.

* 해당 JDK가 설치되어 있는지 확인하십시오.
   * AEM 6.2 필요:Oracle SE 8 JDK 1.8.x(64비트)
* 
   * AEM 6.3 및 AEM 6.4 필요한 경우:Oracle SE 8 JDK 1.8.x(64비트)
* AEM 6.5 JDK 8 또는 JDK 11이 필요합니다.
* [공식 JDK ](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) 요구 사항은 여기에 나와 있습니다.
* JAVA_HOME이 설치되어 있는 JDK를 가리키도록 설정되어 있는지 확인합니다.
   * 윈도우에서 JAVA_HOME 변수를 만들려면 아래 단계를 수행하십시오.
      * [내 컴퓨터]를 마우스 오른쪽 단추로 클릭하고 [속성]을 선택합니다
      * 고급 탭에서 환경 변수를 선택하고 JAVA_HOME이라는 새 시스템 변수를 만듭니다.
      * 시스템에 설치된 JDK를 가리키도록 변수 값을 설정합니다. 예: c:\program files\java\jdk1.8.0_25

* C 드라이브에 AEMForms라는 폴더를 만듭니다.
* AEMQuickStart.Jar를 찾아 AEMForms 폴더로 이동합니다.
* license.properties 파일을 이 AEMForms 폴더에 복사
* 다음 컨텐츠로 &quot;StartAemForms.bat&quot;라는 일괄 파일을 만듭니다.
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.here AEM_6.3_Quickstart.jar는 내 AEM quickstart jar의 이름입니다.
   * jar 이름을 임의의 이름으로 변경할 수 있지만 이름이 일괄 처리 파일에 반영되어 있는지 확인하십시오.배치 파일을 AEMForms 폴더에 저장합니다.

* 새 명령 프롬프트를 열고 c:\aemforms으로 이동합니다.

* 명령 프롬프트에서 StartAemForms.bat 파일을 실행합니다.

* 시작 진행률을 나타내는 작은 대화 상자가 표시됩니다.

* 시작이 완료되면 sling.properties 파일을 엽니다. c:\AEMForms\crx-quickstart\conf folder에 있습니다.

* 다음 2행을 파일 아래쪽에 복사합니다.
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bounknycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* 이 두 속성은 문서 서비스가 작동하는 데 필요합니다.
* sling.properties 파일을 저장합니다.

* [패키지 공유에 로그인](http://localhost:4502/crx/packageshare/login.html)

   * 공유를 패키지화하려면 AdobeId가 필요합니다.
   * AEM Forms 및 운영 체제 버전에 적합한 패키지에서 AEM Forms 추가 검색
   * 또는 [적절한 양식 addon 패키지](https://helpx.adobe.com/kr/aem-forms/kb/aem-forms-releases.html)
   * 패키지에 add on을 설치한 후 다음 단계를 수행해야 합니다.

      * **모든 번들이 활성 상태인지 확인합니다. (AEMFD 서명 번들 제외).**
      * **모든 번들이 활성 상태로 이동하는 데 일반적으로 5분 이상 걸립니다.**
   * **모든 번들이 활성화되면(AEMFD Signatures 번들 제외) 시스템을 다시 시작하여 AEM Forms 설치를 완료하십시오.**


* 허용 목록에 `sun.util.calendar` 패키지 추가:

   1. [브라우저 창에서 Felix 웹 콘솔 열기](http://localhost:4502/system/console/configMgr)
   2. 방화벽 구성 검색 및 열기:`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name` 아래의 새 항목으로 `sun.util.calendar`을(를) 추가합니다.
   4. 변경 사항을 저장합니다.

축하합니다!!! 이제 시스템에 AEM Forms을 설치하고 구성했습니다.
필요에 따라 서버에 [Reader 확장](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) 또는 [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html)을 구성할 수 있습니다.
