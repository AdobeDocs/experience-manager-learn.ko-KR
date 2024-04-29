---
title: AEM Assets의 메타데이터 기반 권한
description: 메타데이터 기반 권한은 폴더 구조가 아닌 에셋 메타데이터 속성에 따라 액세스를 제한하는 데 사용되는 기능입니다.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
thumbnail: xx.jpg
doc-type: Tutorial
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 03cb7ef0cf79a21ec1b96caf6c11e6f5119f777c
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 0%

---

# 메타데이터 기반 권한{#metadata-driven-permissions}

메타데이터 기반 권한은 폴더 구조가 아닌 에셋 메타데이터 속성을 기반으로 AEM Assets 작성자에 대한 액세스 제어 결정을 허용하는 데 사용되는 기능입니다. 이 기능을 사용하면 에셋 상태, 유형 또는 사용자가 정의하는 모든 사용자 지정 메타데이터 속성과 같은 특성을 평가하는 액세스 제어 정책을 정의할 수 있습니다.

예를 살펴보겠습니다. 크리에이티브가 자신의 작업을 AEM Assets에 캠페인 관련 폴더로 업로드합니다. 이는 사용이 승인되지 않은 작업 진행 중인 자산일 수 있습니다. 마케터가 이 캠페인에 대해 승인된 자산만 보도록 하려고 합니다. 메타데이터 속성을 사용하여 자산이 승인되었으며 마케터가 사용할 수 있음을 나타낼 수 있습니다.

## 작동 방법

메타데이터 기반 권한 활성화에는 &quot;상태&quot; 또는 &quot;브랜드&quot;와 같은 액세스 제한을 유도하는 에셋 메타데이터 속성 정의가 포함됩니다. 그런 다음 이러한 속성을 사용하여 특정 속성 값을 가진 에셋에 액세스할 수 있는 사용자 그룹을 지정하는 액세스 제어 항목을 만들 수 있습니다.

## 사전 요구 사항

메타데이터 기반 권한을 설정하려면 최신 버전으로 업데이트된 AEM as a Cloud Service 환경에 액세스해야 합니다.


## 개발 단계

메타데이터 기반 권한을 구현하려면 다음을 수행합니다.

1. 액세스 제어에 사용할 자산 메타데이터 속성을 결정합니다. 이 경우 라는 속성이 됩니다. `status`.
1. OSGi 구성 만들기 `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` 을 참조하십시오.
1. 다음 JSON을 생성된 파일에 붙여넣기

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. 속성 이름을 필수 값으로 바꿉니다.


제한 기반 액세스 제어 항목을 추가하기 전에 자산에 대한 권한 평가의 대상인 모든 그룹(예: &quot;기여자&quot; 또는 이와 유사한 그룹)에 대한 읽기 액세스를 먼저 거부하도록 새로운 최상위 항목을 추가해야 합니다.

1. 도구 → 보안 → 권한 화면으로 이동합니다
1. &quot;기여자&quot; 그룹(또는 모든 사용자 그룹이 속한 다른 사용자 지정 그룹)을 선택합니다
1. 화면 오른쪽 상단에 있는 &quot;ACE 추가&quot;를 클릭합니다
1. /content/dam 을 경로에 대해 선택합니다.
1. 권한에 대한 jcr:read 입력
1. 권한 유형에 대해 거부 선택
1. Restrictions에서 rep:ntNames 를 선택하고 dam:Asset 를 Restriction 값으로 입력합니다
1. 저장을 클릭합니다.

![액세스 거부](./assets/metadata-driven-permissions/deny-access.png)

이제 액세스 제어 항목을 추가하여 에셋 메타데이터 속성 값에 따라 사용자 그룹에 읽기 액세스 권한을 부여할 수 있습니다.

1. 도구 → 보안 → 권한 화면으로 이동합니다
1. 원하는 그룹 선택
1. 화면 오른쪽 상단에 있는 &quot;ACE 추가&quot;를 클릭합니다
1. Path로 /content/dam(또는 하위 폴더) 선택
1. 권한에 대한 jcr:read 입력
1. 권한 유형에 대한 허용 선택
1. 제한에서 구성된 에셋 메타데이터 속성 이름 중 하나를 선택합니다(OSGi 구성에 정의된 속성이 여기에 포함됨).
1. 제한 값 필드에 필요한 메타데이터 속성 값을 입력합니다
1. &quot;+&quot; 아이콘을 클릭하여 액세스 제어 항목에 제한 사항을 추가합니다.
1. 저장을 클릭합니다.

![액세스 허용](./assets/metadata-driven-permissions/allow-access.png)

예제 폴더에는 두 개의 자산이 있습니다.

![관리자 보기](./assets/metadata-driven-permissions/admin-view.png)

권한을 구성하고 그에 따라 에셋 메타데이터 속성을 설정하면 사용자(이 경우 마케터 사용자)에게 승인된 에셋만 표시됩니다.

![마케터 보기](./assets/metadata-driven-permissions/marketeer-view.png)

## 이점 및 고려 사항

메타데이터 기반 권한의 이점은 다음과 같습니다.

- 특정 속성에 따라 에셋 액세스를 미세하게 제어할 수 있습니다.
- 폴더 구조에서 액세스 제어 정책을 분리하여 보다 유연한 자산 구성을 허용합니다.
- 여러 메타데이터 속성을 기반으로 복잡한 액세스 제어 규칙을 정의할 수 있습니다.

>[!NOTE]
>
> 주의해야 할 사항:
> 
> - 메타데이터 속성은 문자열 같음(예: 날짜)을 사용하여 제한에 대해 평가됩니다.
> - 제한 속성에 대해 여러 값을 허용하려면 &quot;유형 선택&quot; 드롭다운에서 동일한 속성을 선택하고 새 제한 값을 입력하여 액세스 제어 항목에 추가 제한을 추가할 수 있습니다(예: `status=approved`, `status=wip`) &quot;+&quot;를 클릭하여 항목에 제한 추가
> ![다중 값 허용](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - 속성 이름이 다른 단일 액세스 제어 항목의 여러 제한 사항(예: `status=approved`, `brand=Adobe`)은 AND 조건으로 평가됩니다. 즉, 선택한 사용자 그룹에게 와 함께 에셋에 대한 읽기 액세스 권한이 부여됩니다. `status=approved AND brand=Adobe`
> ![여러 제한 허용](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - 메타데이터 속성 제한을 사용하여 새 액세스 제어 항목을 추가하면 항목(예: 제한이 있는 단일 항목)에 대한 OR 조건이 설정됩니다 `status=approved` 및 단일 항목 `brand=Adobe` 이(가) 다음으로 평가됨: `status=approved OR brand=Adobe`
> ![여러 제한 허용](./assets/metadata-driven-permissions/allow-multiple-aces.png)
