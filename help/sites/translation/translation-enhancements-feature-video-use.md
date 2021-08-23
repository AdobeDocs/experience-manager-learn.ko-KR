---
title: AEM의 번역 개선 사항
description: AEM의 강력한 번역 프레임워크를 사용하면 지원되는 번역 공급업체에서 AEM 컨텐츠를 원활하게 변환할 수 있습니다. 최신 개선 사항에 대해 알아봅니다.
version: 6.3, 6.4, 6.5
topic: 로컬라이제이션
feature: 다중 사이트 관리자, 언어 사본
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 2%

---


# 다중 사이트 관리자를 사용한 번역 개선 사항 {#translation-enhancements}

AEM의 강력한 번역 프레임워크를 사용하면 지원되는 번역 공급업체에서 AEM 컨텐츠를 원활하게 변환할 수 있습니다.

## AEM 6.5의 번역 개선 사항

>[!VIDEO](https://video.tv.adobe.com/v/27405?quality=9&learn=on)

AEM 6.5 번역 개선 사항은 다음과 같습니다.

**번역 작업 자동 승인**: 번역 작업의 승인 플래그는 이진 속성입니다. 기본 검토 및 승인 워크플로우를 구동하거나 와 통합하지 않습니다. 번역 작업의 단계 수를 최소화하려면 기본적으로 번역 프로젝트의 [!UICONTROL 고급 속성]에서 &quot;자동으로 승인&quot;으로 설정됩니다. 조직에서 번역 작업에 대한 승인이 필요한 경우, 번역 프로젝트의 [!UICONTROL 고급 속성]에서 &quot;자동 승인&quot; 옵션을 선택 취소할 수 있습니다.

**번역 실행** 자동 삭제: 이제 팩트 후에 론치 관리에서 번역 실행을 수동으로 삭제하는 대신, 번역 론치가 승격되면 자동으로 삭제할 수 있습니다.

**JSON 형식으로 번역 개체 내보내기**: AEM 6.4 및 이전 버전은 번역 객체의 XML 및 XLIFF 형식을 지원합니다. 이제 시스템 콘솔 [!UICONTROL 구성 관리자]를 사용하여 내보내기 형식을 JSON 형식으로 구성할 수 있습니다. [!UICONTROL 번역 플랫폼 구성]을 찾은 다음 내보내기 형식을 JSON으로 선택할 수 있습니다.

**TMS(번역 메모리)에서 번역된 AEM 콘텐츠를 업데이트합니다**. AEM에 액세스할 수 없는 로컬 작성자는 이미 AEM에 수집된 번역된 컨텐츠를 TM(TMS의 번역 메모리)에서 직접 업데이트하고, TMS에서 AEM으로 번역 작업을 다시 전송하여 AEM의 번역을 업데이트할 수 있습니다

## AEM 6.4의 번역 개선 사항

>[!VIDEO](https://video.tv.adobe.com/v/21309?quality=9&learn=on)

작성자는 이제 Sites 관리자나 프로젝트 관리자로부터 직접 다국어 번역 프로젝트를 빠르고 쉽게 만들 수 있고, 론치를 자동으로 홍보할 수 있도록 이러한 프로젝트를 설정하고, 자동화를 위한 일정을 설정할 수 있습니다.

## 추가 리소스 {#additional-resources}

* [다국어 사이트의 컨텐츠 번역](https://helpx.adobe.com/kr/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [번역 우수 사례](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)