---
title: 이미지, 견본, 스핀 및 혼합 미디어 세트
description: Dynamic Media Classic의 가장 유용하고 강력한 기능 중 하나는 이미지, 견본, 스핀 및 혼합 미디어 세트와 같은 리치 미디어 세트를 만들 수 있도록 지원하는 것입니다. 각 리치 미디어 세트가 무엇이며 Dynamic Media Classic에서 각 유형을 만드는 방법을 알아봅니다. 그런 다음 업로드 시 리치 미디어 세트를 만드는 프로세스를 자동화하는 일괄처리 세트 사전 설정에 대해 자세히 알아보십시오.
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
duration: 280
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1359'
ht-degree: 1%

---

# 이미지, 견본, 스핀 및 혼합 미디어 세트 {#media-sets}

단일 이미지에서 벗어나 동적 크기 조정 및 확대/축소를 수행하는 Dynamic Media Classic 집합 컬렉션을 통해 더욱 풍부한 온라인 경험을 제공합니다. 자습서의 이 섹션에서는 Dynamic Media Classic에서 다음 리치 미디어 세트를 만드는 방법을 살펴봅니다.

- 이미지 집합
- 견본 집합
- 회전 집합
- 혼합 미디어 집합

또한 배치 집합 사전 설정을 사용하여 업로드를 통해 집합 생성을 자동화하는 방법에 대해서도 설명합니다.

## Set에 대해 항상 알고 싶었던 모든 것

기본 동적 크기 조정 및 확대/축소 다음으로 가장 널리 사용되는 Dynamic Media Classic 하위 제품은 세트입니다. 집합은 기본적으로 실제 이미지를 포함하지 않지만 다른 이미지 및/또는 비디오에 대한 관계 집합으로 구성된 &quot;가상&quot; 자산입니다. 세트들의 주된 매력은 그들이 &quot;바로&quot; 준비된 미니 애플리케이션이라는 것이다. 즉, 각 세트 뷰어에는 자체 논리와 인터페이스가 포함되어 있으므로 사이트에서 해당 뷰어를 호출하기만 하면 됩니다. 또한 모든 멤버 에셋과 관계를 직접 관리할 필요 없이 세트당 하나의 에셋 ID만 추적하면 됩니다.

세트를 만들 때 해당 세트는 URL에서 제공되기 전에 게시로 표시하고 게시해야 하는 별도의 자산으로 관리됩니다. 모든 멤버 에셋도 게시해야 합니다.

### 집합 유형

Dynamic Media Classic에서 만들 수 있는 네 가지 유형의 세트 (이미지, 견본, 스핀 및 혼합 미디어 세트)에 대해 알아보겠습니다.

## 이미지 집합

이것은 가장 일반적인 유형의 세트입니다. 일반적으로 동일한 항목의 대체 보기에 사용합니다. 이 이미지는 해당 이미지의 관련 썸네일을 클릭하여 뷰어에 로드하는 여러 이미지로 구성됩니다.

![이미지](assets/media-sets/image-set-1.jpg)

_이미지 집합의 예_

위 이미지 세트의 URL은 다음과 같이 표시될 수 있습니다.

![이미지](assets/media-sets/image-set-url-1.png)

- [이미지 집합 빠른 시작](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html?lang=ko)을 사용하여 이미지 집합에 대해 자세히 알아보세요.
- [이미지 집합을 만드는 방법](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html?lang=ko#creating-an-image-set)을 알아보세요.

### 견본 집합

이러한 유형의 세트는 일반적으로 동일한 항목의 색상이 지정된 뷰를 표시하는 데 사용됩니다. 이미지와 색상 견본의 쌍으로 구성됩니다.

견본과 이미지 세트의 주요 차이점은 견본 세트는 클릭 가능한 견본으로 다른 이미지를 사용하는 반면 이미지 세트는 원본 이미지의 클릭 가능한 축소판 버전을 축소하여 사용한다는 것입니다.

견본 세트는 이미지를 색상화하지 않습니다(일반적인 오해). 이미지 세트에서처럼 단순히 이미지를 교환하고 있습니다. 미니 색상 견본 이미지는 Photoshop을 사용하여 작성하거나, 각 색상을 별도로 촬영하거나, Dynamic Media Classic의 자르기 도구를 사용하여 색상 이미지 중 하나로 색상 견본을 만들 수 있습니다.

![이미지](assets/media-sets/image-set-2.jpg)

_견본 집합의 예_

위 견본 집합의 URL은 다음과 같이 나타날 수 있습니다.

![이미지](assets/media-sets/image-set_url.png)

- [견본 집합에 대한 빠른 시작](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html?lang=ko)을 사용하여 견본 집합에 대해 자세히 알아보세요.
- [견본 집합을 만드는 방법](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html?lang=ko#creating-a-swatch-set)을 알아보세요.

### 회전 집합

이 세트는 일반적으로 항목의 360도 보기를 표시하는 데 사용됩니다. 견본 세트처럼 스핀 세트는 3D 마술을 사용하지 않습니다. 실제 작업은 모든 면에서 이미지의 많은 사진을 만드는 것입니다. 뷰어는 정지 동작 애니메이션처럼 이미지 간을 전환할 수 있도록 해줍니다.

회전 세트는 단일 축을 따라 한 방향으로 회전하거나, 2D 회전 세트로 번갈아 생성된 경우 여러 축을 기준으로 회전할 수 있습니다. 예를 들어, 모든 바퀴가 지면에 있는 동안 자동차를 회전시킨 다음, 뒷바퀴에서도 &quot;뒤집기&quot;를 사용하여 회전할 수 있습니다. 올바르게 설정된 2D 스핀 세트의 경우 각 축의 행당 이미지 수는 동일해야 합니다. 즉, 두 축으로 회전하는 경우 단일 각도 스핀보다 두 배 많은 이미지가 필요합니다.

![이미지](assets/media-sets/image-set-3.png)

_회전 집합의 예_

위 회전 집합의 URL은 다음과 같이 나타날 수 있습니다.

![이미지](assets/media-sets/spin-set.png)

- [회전 집합에 대한 빠른 시작](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html?lang=ko)을 사용하여 회전 집합에 대해 자세히 알아보세요.
- 회전 집합을 [만드는 방법](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html?lang=ko#creating-a-spin-set)을 알아보세요.

## 혼합 미디어 집합

콤비네이션 세트입니다. 이전 세트를 결합하고 비디오를 단일 뷰어로 추가할 수 있습니다. 이 워크플로우에서는 먼저 구성 요소 세트를 만든 다음 혼합 미디어 세트로 조합합니다.

![이미지](assets/media-sets/image-set-4.png)

_혼합 미디어 집합의 예_

위의 혼합 미디어 세트의 URL은 다음과 같이 나타날 수 있습니다.

![이미지](assets/media-sets/image-set-url-1.png)

- [혼합 미디어 집합에 대한 빠른 시작](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html?lang=ko)이 있는 혼합 미디어 집합에 대해 자세히 알아보세요.

- [혼합 미디어 집합을 만드는 방법](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html?lang=ko#creating-a-mixed-media-set)을 알아보세요.

웹 사이트에서 확대/축소, 세트 또는 비디오용 이미지를 표시하려면 Dynamic Media Classic &quot;뷰어&quot;에서 이 이미지를 호출합니다. Dynamic Media Classic에는 견본 세트, 스핀 세트, 비디오 및 기타 많은 리치 미디어 에셋의 뷰어가 포함됩니다.

[AEM Assets 및 Dynamic Media Classic용 뷰어](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=ko)에 대해 자세히 알아보세요.

## 일괄 처리 집합 사전 설정

지금까지 Dynamic Media Classic 빌드 기능을 사용하여 수동으로 세트를 빌드하는 방법에 대해 논의했습니다. 하지만 표준화된 명명 규칙이 있는 한 일괄처리 집합 사전 설정을 사용하여 이미지 집합 및 스핀 집합 생성을 자동화할 수 있습니다.

각 사전 설정은 정의된 명명 규칙과 일치하는 이미지를 사용하여 세트를 구성하는 방법을 정의하는 고유한 이름의 자체 포함된 명령 세트입니다. 사전 설정에서 먼저 집합에 그룹화할 에셋의 이름 지정 규칙을 정의합니다. 그런 다음 이러한 이미지를 참조하도록 일괄처리 집합 사전 설정을 만들 수 있습니다.

사전 설정을 직접 만들 수는 있지만(**설정 > 응용 프로그램 설정 > 일괄처리 사전 설정**&#x200B;에서 찾을 수 있음), 컨설팅 팀이나 기술 지원 팀에서 설정하는 것이 좋습니다. 이유는 다음과 같습니다.

- 일괄처리 집합 사전 설정은 설정하기에 복잡할 수 있습니다. 일반 표현식으로 제공되며 개발자가 아닌 경우 이 구문이 낯설거나 혼란스러울 수 있습니다.
- 만들어지면 기본적으로 켜져 있습니다. &quot;실행 취소&quot; 기능은 없습니다. 수천 개의 이미지 업로드를 시작하고 사전 설정이 잘못 구성된 경우 수동으로 찾아서 삭제해야 하는 수백 또는 수천 개의 끊어진 세트가 발생할 수 있습니다.

이전에는 일괄 처리 집합 사전 설정으로 빌드하기 매우 쉬운 간단한 명명 규칙을 제안했습니다. 그러나 사전 설정은 매우 유연하므로 복잡한 이름 지정 전략을 처리할 수 있습니다. 즉, 세트에 속하는 이미지는 일반적인 이름(SKU 번호 또는 제품 ID인 경우가 많음)으로 함께 연결되어야 합니다. Dynamic Media Classic에서 사전 설정에 사용할 모든 이미지에 대한 기본 명명 규칙을 지정하거나, 각기 다른 명명 규칙을 사용하여 여러 사전 설정을 만들 수 있습니다.

일괄처리 집합 사전 설정은 업로드 시에만 적용됩니다. 이미지를 업로드한 후에는 실행할 수 없습니다. 따라서 모든 이미지 로드를 시작하기 전에 명명 규칙을 계획하고 사전 설정을 빌드하는 것이 중요합니다.

사전 설정이 생성되면 회사 관리자는 활성 또는 비활성 여부를 선택할 수 있습니다. 활성은 업로드 페이지의 **작업 옵션** 아래에 표시되지만 비활성 사전 설정은 숨겨집니다.

[일괄처리 집합 사전 설정을 만드는 방법](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=ko#creating-a-batch-set-preset)을 알아보세요.

### 업로드 시 일괄처리 집합 사전 설정 사용

일괄처리 집합 사전 설정을 만든 후 업로드 시 사용하는 방법은 다음과 같습니다.

1. **업로드**&#x200B;를 클릭하고 **데스크톱에서** 또는 **FTP를 통해**&#x200B;를 선택합니다.
2. **작업 옵션**&#x200B;을 클릭합니다.
3. **일괄처리 집합 사전 설정** 옵션을 열고 업로드에 사용할 사전 설정을 선택하거나 선택 취소합니다.
4. 업로드가 완료되면 폴더에서 완료된 세트를 찾습니다.

[일괄처리 집합 사전 설정](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=ko#batch-set-presets)에 대해 자세히 알아보세요.
