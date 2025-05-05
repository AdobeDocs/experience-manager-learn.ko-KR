---
title: Dynamic Media Classic 기본 워크플로우 및 Assets 미리 보기
description: 만들기(및 업로드), 작성자(및 Publish), 게재의 세 단계를 포함하는 Dynamic Media Classic의 기본 워크플로우에 대해 알아봅니다. 그런 다음 Dynamic Media Classic에서 에셋을 미리 보는 방법을 알아봅니다.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
duration: 569
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2658'
ht-degree: 0%

---

# Dynamic Media Classic 기본 워크플로우 및 Assets 미리 보기 {#main-workflow}

Dynamic Media은 만들기(및 업로드), 작성자(및 Publish) 및 게재 워크플로우 프로세스를 지원합니다. 에셋을 업로드한 다음 이미지 세트를 구축하고 최종적으로 게시하여 라이브로 만드는 것과 같은 작업을 수행합니다. 빌드 단계는 일부 워크플로우에서 선택 사항입니다. 예를 들어, 동적 크기 조정 및 이미지 확대만 수행하거나 스트리밍을 위해 비디오를 변환 및 게시하는 것이 목표라면 필요한 빌드 단계는 없습니다.

![이미지](assets/main-workflow/create-author-deliver.jpg)

Dynamic Media Classic 솔루션의 워크플로우는 세 가지 주요 단계로 구성됩니다.

1. SourceContent 만들기(및 업로드)
2. 작성자(및 Publish) Assets
3. Assets 제공

## 1단계: 만들기(및 업로드)

워크플로우의 시작입니다. 이 단계에서는 사용 중인 워크플로우에 맞는 소스 콘텐츠를 모으거나 만들어 Dynamic Media Classic에 업로드합니다. 시스템은 이미지, 비디오 및 글꼴에 대해 여러 파일 유형을 지원하지만 PDF, Adobe Illustrator 및 Adobe InDesign에 대해서도 지원합니다.

[지원되는 파일 형식](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=ko#supported-asset-file-formats)의 전체 목록을 확인하세요.

다음과 같은 여러 가지 방법으로 소스 콘텐츠를 업로드할 수 있습니다.

- 데스크탑 또는 로컬 네트워크에서 직접 액세스 [방법을 알아보세요](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=ko#upload-files-using-sps-desktop-application).
- Dynamic Media Classic FTP 서버에서 [방법을 알아보세요](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=ko#upload-files-using-via-ftp).

기본 모드는 로컬 네트워크에서 파일을 검색하고 업로드를 시작하는 데스크톱에서 입니다.

![이미지](assets/main-workflow/upload.jpg)

>[!TIP]
>
>폴더를 수동으로 추가하지 마십시오. 대신 FTP에서 업로드를 실행하고 **하위 폴더 포함** 옵션을 사용하여 Dynamic Media Classic 내에서 폴더 구조를 다시 만드십시오.

가장 중요한 두 업로드 옵션은 기본적으로 활성화되어 있습니다. 앞에서 설명한 **Publish 표시** 및 **덮어쓰기**. 덮어쓰기는 업로드되는 파일의 이름이 시스템에 이미 있는 파일과 같은 경우 새 파일이 기존 버전을 대체함을 의미합니다. 이 옵션을 선택 취소하면 파일이 업로드되지 않을 수 있습니다.

### 이미지를 업로드할 때 덮어쓰기 옵션

네 가지 변형 이미지 덮어쓰기 옵션은 전체 회사에 대해 설정할 수 있으며 종종 잘못 이해됩니다. 요컨대, 동일한 이름을 가진 에셋을 더 자주 덮어쓰거나 덮어쓰기를 덜 자주 수행하도록 규칙을 설정합니다(이 경우 새 이미지 이름은 &quot;-1&quot; 또는 &quot;-2&quot; 확장으로 변경됨).

- **같은 기본 이미지 이름/확장명으로 현재 폴더에 덮어씁니다**.
이 옵션은 교체에 가장 엄격한 규칙입니다. 대체 이미지를 원본과 동일한 폴더에 업로드하고 대체 이미지의 파일 이름 확장명이 원본과 동일해야 합니다. 이러한 요구 사항이 충족되지 않으면 복제본이 생성됩니다.

- **확장명에 상관없이 같은 기본 자산 이름으로 현재 폴더에 덮어쓰기**.
대체 이미지를 원본과 동일한 폴더에 업로드해야 하지만, 파일 이름 확장명이 원본과 다를 수 있습니다. 예를 들어 chair.tif는 chair.jpg를 대체합니다.

- **같은 기본 에셋 이름/확장명으로 모든 폴더에 덮어씁니다**.
대체 이미지의 파일 이름 확장명이 원본 이미지와 동일해야 합니다(예: chair.jpg는 chair.tif 가 아니라 chair.jpg를 대체해야 함). 그러나 대체 이미지를 원본과 다른 폴더에 업로드할 수 있습니다. 업데이트된 이미지가 새 폴더에 있습니다. 파일은 원래 위치에서 더 이상 찾을 수 없습니다.

- **확장명에 관계없이 같은 기본 자산 이름으로 모든 폴더에 덮어씁니다**.
이 옵션은 가장 포괄적인 대체 규칙입니다. 대체 이미지를 원본이 아닌 다른 폴더에 업로드하고, 파일 확장명이 다른 파일을 업로드하고, 원본 파일을 바꿀 수 있습니다. 원본 파일이 다른 폴더에 있는 경우 대체 이미지는 업로드된 새 폴더에 있습니다.

[이미지 덮어쓰기 옵션](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=ko#using-the-overwrite-images-option)에 대해 자세히 알아보세요.

필수는 아니지만, 위의 두 가지 방법 중 하나를 사용하여 업로드하는 동안 해당 특정 업로드에 대한 작업 옵션 을 지정할 수 있습니다. 예를 들어 반복 업로드를 예약하고, 업로드 시 자르기 옵션을 설정하는 등의 작업을 수행할 수 있습니다. 이러한 지표는 일부 워크플로우에 유용할 수 있으므로 자신의 워크플로우가 될 수 있는지 고려해 볼 필요가 있습니다.

[작업 옵션](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=ko#upload-options)에 대해 자세히 알아보세요.

Dynamic Media Classic은 아직 시스템에 없는 콘텐츠로는 작업할 수 없으므로 업로드는 워크플로우에서 가장 먼저 필요한 단계입니다. 업로드하는 동안 백그라운드에서 시스템은 업로드된 모든 에셋을 중앙 집중식 Dynamic Media Classic 데이터베이스에 등록하고 ID를 할당한 다음 이를 스토리지에 복사합니다. 또한 이미지 파일을 동적 크기 조정 및 확대/축소가 가능한 형식으로 변환하고 비디오 파일을 MP4 웹에 친숙한 형식으로 변환합니다.

### 개념: Dynamic Media Classic에 업로드할 때 발생하는 이미지 작업

모든 유형의 이미지를 Dynamic Media Classic에 업로드하면 피라미드 TIFF 또는 P-TIFF이라는 마스터 이미지 형식으로 변환됩니다. P TIFF은 서로 다른 레이어 대신 파일에 동일한 이미지의 여러 크기(해상도)가 포함되어 있다는 점을 제외하면 레이어 TIFF 비트맵 이미지의 형식과 유사합니다.

![이미지](assets/main-workflow/pyramid-p-tiff.png)

이미지가 변환되면 Dynamic Media Classic은 이미지의 전체 크기에 대한 &quot;스냅샷&quot;을 만들어 절반 수준으로 이미지를 확대하여 저장하고, 다시 절반 수준으로 확대하여 저장하는 식으로 원래 크기의 배수로 채워질 때까지 계속합니다. 예를 들어 2000픽셀 P TIFF의 경우 동일한 파일에 1000픽셀, 500픽셀, 250픽셀 및 125픽셀 크기(및 그 이하)가 있습니다. P TIFF 파일은 Dynamic Media Classic에서 &quot;마스터 이미지&quot;라고 하는 형식입니다.

특정 크기의 이미지를 요청하면 P-TIFF을 만들면 Dynamic Media Classic용 이미지 서버에서 다음 더 큰 크기를 빠르게 찾아 축소할 수 있습니다. 예를 들어, 2000픽셀 이미지를 업로드하고 100픽셀 이미지를 요청하면 Dynamic Media Classic은 125픽셀 버전을 찾아 2000픽셀에서 100픽셀로 확대하지 않고 100픽셀로 축소합니다. 이렇게 하면 작업이 매우 빨라집니다. 또한 이미지를 확대/축소할 때 확대/축소 뷰어에서 전체 해상도 이미지가 아닌 확대/축소되는 이미지의 타일만 요청할 수 있습니다. 마스터 이미지 형식인 P TIFF 파일이 동적 크기 조정과 확대/축소를 모두 지원하는 방식입니다.

마찬가지로 마스터 소스 비디오를 Dynamic Media Classic에 업로드할 수 있으며 Dynamic Media Classic 업로드 시 자동으로 크기를 조정하여 MP4 웹에 친숙한 형식으로 변환할 수 있습니다.

### 업로드하는 이미지의 최적 크기를 결정하기 위한 경험적 규칙

**필요한 최대 크기의 이미지를 업로드합니다.**

- 확대/축소가 필요한 경우 가장 긴 차원에서 1500-2500픽셀 범위의 고해상도 이미지를 업로드합니다. 표시할 세부 정보, 소스 이미지의 품질 및 제품 크기를 고려합니다. 예를 들어, 작은 링에 대해서는 1000픽셀 이미지를 업로드하지만 전체 실내 장면에 대해서는 3000픽셀 이미지를 업로드합니다.
- 확대/축소가 필요하지 않은 경우 표시되는 정확한 크기로 업로드합니다. 예를 들어 페이지에 배치할 로고 또는 스플래시/배너 이미지가 있는 경우 1:1 크기로 업로드한 다음 해당 크기로 정확하게 호출합니다.

**Dynamic Media Classic에 업로드하기 전에 이미지를 업샘플링하거나 확대하지 마십시오.** 예를 들어 2000픽셀 이미지로 만들기 위해 더 작은 이미지를 업샘플링하지 마십시오. 좋지 않을 거야. 업로드하기 전에 이미지를 가능한 한 완벽에 가깝게 만듭니다.

**최소 확대/축소 크기가 없지만 기본적으로 뷰어는 100%를 초과하여 확대/축소되지 않습니다.** 이미지가 너무 작으면 이미지가 전혀 확대되지 않거나 손상되지 않도록 소량만 축소됩니다.

**이미지 크기에 대한 최소값은 없지만 대용량 이미지는 업로드하지 않는 것이 좋습니다.** 거대한 이미지는 4000픽셀 이상으로 간주할 수 있습니다. 이 크기의 이미지를 업로드하면 이미지의 먼지 또는 털의 그레인과 같은 잠재적인 결함을 보여줄 수 있습니다. 이러한 이미지는 Dynamic Media Classic 서버에서 더 많은 공간을 차지하므로 계약된 스토리지 한도를 초과할 수 있습니다.

[파일 업로드](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=ko#uploading-your-files)에 대해 자세히 알아보세요.

## 2단계: 작성자(및 Publish)

콘텐츠를 만들고 업로드한 후에는 하나 이상의 하위 워크플로우를 수행하여 업로드한 자산에서 새 리치 미디어 자산을 작성합니다. 여기에는 이미지, 견본, 스핀 및 혼합 미디어 집합과 같은 다양한 유형의 집합 컬렉션과 템플릿이 모두 포함됩니다. 비디오도 포함되어 있습니다. 각 유형의 이미지 컬렉션 세트와 비디오 리치 미디어에 대한 자세한 내용은 나중에 살펴보겠습니다. 그러나 거의 모든 경우 하나 이상의 자산을 선택하고(또는 선택한 자산이 없음) 빌드할 자산 유형을 선택하는 것부터 시작합니다. 예를 들어 기본 이미지와 해당 이미지의 몇 가지 보기를 선택하고 동일한 제품의 대체 보기 컬렉션인 이미지 집합을 빌드하도록 선택할 수 있습니다.

>[!IMPORTANT]
>
>모든 에셋이 게시용으로 표시되었는지 확인합니다. 기본적으로 모든 자산은 업로드 시 게시로 자동 표시되지만, 업로드한 컨텐츠에서 새로 작성된 자산은 게시로 표시되어야 합니다.

새 자산을 빌드하면 게시 작업이 실행됩니다. 이 작업은 수동으로 수행하거나 자동으로 실행되는 게시 작업을 예약할 수 있습니다. 게시는 방정식의 비공개 Dynamic Media Classic 구의 모든 컨텐츠를 공개 게시 서버 구에 복사합니다. Dynamic Media Publish 작업 제품은 게시된 각 에셋에 대한 고유한 URL입니다.

게시하는 서버는 콘텐츠 및 워크플로의 유형에 따라 다릅니다. 예를 들어 모든 이미지는 이미지 서버로 이동하고 비디오를 FMS 서버로 스트리밍합니다. 편의상 단일 서버에 대한 단일 이벤트로 &quot;게시&quot;를 언급하겠습니다.

게시 는 게시로 표시된 모든 컨텐츠를 게시하며 컨텐츠만 게시하지 않습니다. 일반적으로 단일 관리자는 게시를 실행하는 개별 사용자가 아닌 모든 사용자를 대신하여 게시합니다. 관리자는 필요에 따라 게시하거나 자동으로 게시되는 매일, 매주 또는 10분마다 반복되는 작업을 설정할 수 있습니다. 비즈니스에 적합한 일정에 따라 Publish을 예약하십시오.

>[!TIP]
>
>게시 작업을 자동화하고 전체 Publish이 매일 오전 12시 또는 저녁 늦은 시간에 실행되도록 예약합니다.

### 개념: Dynamic Media Classic URL 이해

Dynamic Media Classic 워크플로우의 최종 제품은 에셋(이미지 세트 또는 응용 비디오 세트)을 가리키는 URL입니다. 이러한 URL은 매우 예측 가능하며 동일한 패턴을 따릅니다. 이미지들의 경우, 각각의 이미지는 P-TIFF 마스터 이미지로부터 생성된다.

다음은 두 가지 예와 함께 이미지의 URL에 대한 구문입니다.

![이미지](assets/main-workflow/dmc-url.jpg)

URL에서 물음표 왼쪽에 있는 모든 것이 특정 이미지에 대한 가상 경로이다. 물음표 오른쪽에 있는 모든 항목은 이미지 처리 방법에 대한 지침인 이미지 서버 수정자입니다. 수정자가 여러 개 있으면 앰퍼샌드로 구분됩니다.

첫 번째 예에서 이미지 &quot;Backpack_A&quot;의 가상 경로는 `http://sample.scene7.com/is/image/s7train/BackpackA`입니다. 이미지 서버 수정자는 이미지 크기를 250픽셀 너비(wid=250)로 조정하고, 크기 조정 시 선명해지는 Lanczos 보간 알고리즘(resMode=sharp2)을 사용하여 이미지를 재샘플링합니다.

두 번째 예에서는 &quot;이미지 사전 설정&quot;이라고 하는 항목을 동일한 Backpack_A 이미지에 $!(으)로 표시됩니다._template300$. 표현식 양쪽의 $ 기호는 이미지 사전 설정, 이미지 수정자 세트가 패키지화되어 이미지에 적용되고 있음을 나타냅니다.

Dynamic Media Classic URL을 결합하는 방법을 이해하면 이를 프로그래밍 방식으로 변경하는 방법과 사이트 및 백엔드 시스템에 심층적으로 통합하는 방법을 이해합니다.

### 개념: 캐싱 지연 이해

새로 업로드되고 게시된 에셋은 즉시 표시되지만 업데이트된 에셋은 10시간 캐싱 지연이 발생할 수 있습니다. 기본적으로 게시된 모든 에셋은 만료될 때까지 최소 10시간이 있습니다. 이미지를 볼 때마다 아무도 해당 이미지를 보지 않은 채 10시간이 경과할 때까지 만료되지 않는 시계를 시작하기 때문에 우리는 최소값을 말합니다. 이 10시간 기간은 에셋의 &quot;Time to Live&quot;입니다. 해당 에셋에 대한 캐시가 만료되면 업데이트된 버전을 제공할 수 있습니다.

일반적으로 이는 오류가 발생하지 않는 한 문제가 되지 않으며 이미지/에셋의 이름은 이전에 게시된 버전과 동일하지만 이미지에 문제가 있습니다. 예를 들어, 실수로 저해상도 버전을 업로드했거나 아트 감독이 이미지를 승인하지 않은 경우입니다. 이 경우, 원본 이미지를 회수하고 동일한 에셋 ID를 사용하여 새 버전으로 바꾸려고 합니다.

[업데이트해야 하는 URL에 대한 캐시를 수동으로 지우는 방법](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=ko-KR)을 알아봅니다.

>[!TIP]
>
>캐싱 지연과 관련된 문제를 방지하려면 항상 미리 작업하십시오(예: 저녁, 하루, 2주 등). 공개하기 전에 내부 당사자가 작업을 증명하기 위해 QA/수락에 대한 시간에 맞춰 빌드합니다. 전날 저녁에 작업하더라도 변경한 후 그날 저녁에 다시 게시할 수 있습니다. 아침까지 10시간이 경과하고 캐시가 올바른 이미지로 업데이트됩니다.

- [게시 작업 만들기](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html?lang=ko#creating-a-publish-job)에 대해 자세히 알아보세요.
- [게시](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html?lang=ko)에 대해 자세히 알아보세요.

## 3단계: 제공

Dynamic Media Classic 워크플로의 최종 제품은 자산을 가리키는 URL이라는 점을 기억하십시오. URL은 개별 이미지, 이미지 세트, 스핀 세트 또는 기타 이미지 세트 컬렉션이나 비디오를 가리킬 수 있습니다. 해당 URL을 가져와서 `<IMG>` 태그가 현재 사이트에서 가져온 이미지를 가리키지 않고 Dynamic Media Classic 이미지를 가리키도록 HTML을 편집하는 등의 작업을 수행해야 합니다.

게재 단계에서는 이러한 URL을 웹 사이트, 모바일 앱, 이메일 캠페인 또는 에셋을 표시할 기타 디지털 터치 포인트에 통합해야 합니다.

이미지에 대한 Dynamic Media Classic URL을 웹 사이트에 통합하는 예:

![이미지](assets/main-workflow/example-url-1.png)

빨간색으로 표시된 URL은 Dynamic Media Classic과 관련된 유일한 요소입니다.

IT 팀이나 통합 파트너는 앞장서서 코드를 작성하고 변경하여 Dynamic Media Classic URL을 사이트에 통합할 수 있습니다. Adobe은 기술, 창의적 또는 일반적인 지침을 제공하여 이러한 노력에 도움을 줄 수 있는 컨설팅 팀이 있습니다.

확대/축소 뷰어 또는 대체 보기와 확대/축소를 결합하는 뷰어와 같은 보다 복잡한 솔루션의 경우, 일반적으로 URL은 Dynamic Media Classic에 의해 호스팅되는 뷰어를 가리키며 해당 URL 내에서도 에셋 ID에 대한 참조가 됩니다.

새 팝업 창의 뷰어에서 이미지 세트를 여는 링크(빨간색)의 예:

![이미지](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>웹 사이트, 모바일 앱, 이메일 및 기타 디지털 터치포인트에 Dynamic Media Classic URL을 통합해야 합니다. Dynamic Media Classic에서 자동으로 그렇게 할 수 없습니다!

## Assets 미리 보기

업로드했거나 생성 또는 편집 중인 에셋을 미리 보고 고객이 에셋을 볼 때 원하는 대로 표시되도록 해야 할 수 있습니다. **미리 보기** 단추를 클릭하거나 자산의 축소판, **찾아보기/빌드 패널**&#x200B;의 맨 위에서 또는 **파일 > 미리 보기**&#x200B;로 이동하여 미리 보기 창에 액세스할 수 있습니다. 브라우저 창에서 이미지, 비디오 또는 이미지 세트처럼 빌드된 자산인지 여부에 관계없이 현재 패널에 있는 자산을 미리 봅니다.

### 동적 크기 미리 보기(이미지 사전 설정)

**크기** 미리 보기를 사용하여 여러 크기로 이미지를 미리 볼 수 있습니다. 사용 가능한 이미지 사전 설정 목록이 로드됩니다. 나중에 이미지 사전 설정에 대해 논의하겠지만, 특정 선명하게 하기 및 이미지 품질을 사용하여 지정된 크기로 이미지를 로드하기 위한 &quot;레시피&quot;로 생각해 보겠습니다.

### 확대/축소 미리 보기

**확대/축소** 옵션을 사용하여 포함된 다양한 확대/축소 뷰어를 기반으로 하는 미리 작성된 여러 확대/축소 사전 설정 중 하나로 이미지를 미리 볼 수도 있습니다.

[Assets 미리 보기](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html?lang=ko)에 대해 자세히 알아보세요.
