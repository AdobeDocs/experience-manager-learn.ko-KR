---
title: 빠른 설정 - AEM 헤드리스 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. AEM SDK를 설치하고 샘플 컨텐츠를 추가하고 GraphQL API를 사용하여 AEM의 컨텐츠를 사용하는 애플리케이션을 배포합니다. AEM의 옴니채널 경험 강화
sub-product: 사이트
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
translation-type: tm+mt
source-git-commit: c752106cc68774eb7e8b9fe525273bb7088d38e5
workflow-type: tm+mt
source-wordcount: '1547'
ht-degree: 1%

---


# 빠른 설정 {#setup}

이 장에서는 AEM GraphQL API를 사용하여 외부 애플리케이션이 AEM의 컨텐츠를 사용하는지 확인할 수 있도록 로컬 환경을 빠르게 설정할 수 있습니다. 튜토리얼의 이후 장은 이 설정을 구성합니다.

## 전제 조건 {#prerequisites}

다음 도구는 로컬에 설치해야 합니다.

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%4040jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 목표 {#objectives}

1. AEM SDK를 다운로드하고 설치합니다.
1. WKND 참조 사이트에서 샘플 컨텐츠를 다운로드하고 설치합니다.
1. GraphQL API를 사용하여 콘텐츠를 사용하려면 샘플 앱을 다운로드하여 설치하십시오.

## AEM SDK{#aem-sdk} 설치

이 자습서에서는 [AEM을 Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk)로 사용하여 AEM GraphQL API를 살펴봅니다. 이 섹션에서는 AEM SDK를 설치하고 작성 모드에서 실행하는 방법에 대한 빠른 안내서를 제공합니다. 로컬 개발 환경 설정에 대한 자세한 안내서는 [에서 찾을 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

>[!NOTE]
>
> 또한 Cloud Service 환경에서 AEM과 함께 자습서를 따를 수도 있습니다. 클라우드 환경 사용에 대한 추가 메모는 자습서 전체에 포함되어 있습니다.

1. **[소프트웨어 배포 포털](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM을 Cloud Service**&#x200B;로 이동하고 최신 버전의 **AEM SDK**&#x200B;을 다운로드합니다.

   ![소프트웨어 배포 포털](assets/setup/software-distribution-portal-download.png)

   >[!CAUTION]
   >
   > GraphQL 기능은 기본적으로 2021-02-04 이상의 AEM SDK에서만 활성화됩니다.

1. 다운로드 압축을 풀고 빠른 시작 jar(`aem-sdk-quickstart-XXX.jar`)를 전용 폴더(예: `~/aem-sdk/author`)에 복사합니다.
1. jar 파일의 이름을 `aem-author-p4502.jar`로 다시 지정합니다.
1. 새 터미널 창을 열고 jar 파일이 포함된 폴더로 이동합니다. 다음 명령을 실행하여 AEM 인스턴스를 설치하고 시작합니다.

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 관리자 암호를 `admin`으로 입력합니다. 모든 관리자 암호는 사용할 수 있지만 다시 구성할 필요가 없도록 로컬 개발에 기본값을 사용하는 것이 좋습니다.
1. 몇 분 후에 AEM 인스턴스가 설치를 완료하고 새 브라우저 창이 [http://localhost:4502](http://localhost:4502)에서 열립니다.
1. 사용자 이름 `admin` 및 암호 `admin`로 로그인합니다.

## 샘플 컨텐츠 설치{#wknd-site}

자습서 속도를 높이기 위해 WKND 참조 사이트의 샘플 컨텐츠를 설치합니다. WKND는 종종 AEM 트레이닝과 함께 사용되는 가상의 라이프스타일 브랜드입니다.

1. WKND 사이트용 최신 컴파일된 AEM 패키지를 다운로드합니다.[aem-guides-wknd.all-x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > AEM과 호환되는 표준 버전을 Cloud Service으로 다운로드하고 **버전이 아닌**&#x200B;을(를) 다운로드하십시오.`classic`

1. **AEM 시작** 메뉴에서 **도구** > **배포** > **패키지**&#x200B;로 이동합니다.

   ![패키지로 이동](assets/setup/navigate-to-packages.png)

1. **패키지 업로드**&#x200B;를 클릭하고 이전 단계에서 다운로드한 WKND 패키지를 선택합니다. **설치**&#x200B;를 클릭하여 패키지를 설치합니다.

1. **AEM 시작** 메뉴에서 **자산** > **파일**&#x200B;으로 이동합니다.
1. 폴더를 클릭하여 **WKND 사이트** > **영어** > **모험**&#x200B;으로 이동합니다.

   ![모험 폴더 보기](assets/setup/folder-view-adventures.png)

   WKND 브랜드로 승격된 다양한 모험들을 구성하는 모든 자산의 폴더입니다. 여기에는 이미지 및 비디오와 같은 기존 미디어 유형뿐만 아니라 **컨텐츠 조각**&#x200B;과 같은 AEM 전용 미디어가 포함됩니다.

1. **다운힐 스키잉 와이오밍** 폴더를 클릭한 다음 **다운힐 스키잉 와이오밍 콘텐츠 조각** 카드를 클릭합니다.

   ![스키 컨텐츠 조각 카드 다운로드](assets/setup/downhill-skiing-cntent-fragment.png)

1. 컨텐츠 조각 편집기 UI가 다운힐 스키 와이오밍 탐험 진행을 위해 열립니다.

   ![다운힐 스키 컨텐츠 조각](assets/setup/down-hillskiing-fragment.png)

   **제목**, **설명** 및 **활동**&#x200B;과 같은 다양한 필드가 조각을 정의하는지 확인하십시오.

   **컨텐츠** 조각은 AEM에서 컨텐츠를 관리할 수 있는 방법 중 하나입니다. 컨텐츠 조각은 텍스트, 리치 텍스트, 날짜 또는 다른 컨텐츠 조각에 대한 참조와 같은 구조화된 데이터 요소로 구성된 재사용 가능하고 프레젠테이션에 영향을 받지 않는 컨텐츠입니다. 컨텐츠 조각은 튜토리얼 후반부에서 보다 자세히 살펴봅니다.

1. **취소**&#x200B;를 클릭하여 조각을 닫습니다. 다른 폴더로 이동하여 다른 Adventure 컨텐츠를 탐색할 수 있습니다.

>[!NOTE]
>
> Cloud Service 환경을 사용하는 경우 WKND 참조 사이트와 같은 코드 베이스를 Cloud Service 환경에 배포하는 방법에 대한 설명서를 참조하십시오](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#deploying).[

## GraphQL 끝점 설치{#graphql-endpoint}

GraphQL 끝점을 구성해야 합니다. 따라서 GraphQL API가 표시되는 정확한 끝점을 결정할 때 프로젝트에 유연성을 부여할 수 있습니다. 외부 응용 프로그램에 대한 액세스 권한을 부여하려면 [CORS](#cors-config)도 필요합니다. 자습서를 더 신속하게 진행하기 위해 패키지가 미리 만들어졌습니다.

1. [aem-guides-wknd-graphql.all-1.0.0-SNAPSHOT.zip](./assets/setup/aem-guides-wknd-graphql.all-1.0.0-SNAPSHOT.zip) 패키지를 다운로드합니다.
1. **AEM 시작** 메뉴에서 **도구** > **배포** > **패키지**&#x200B;로 이동합니다.
1. **패키지 업로드**&#x200B;를 클릭하고 이전 단계에서 다운로드한 패키지를 선택합니다. **설치**&#x200B;를 클릭하여 패키지를 설치합니다.

위의 패키지에는 이후 장에 사용할 [GraphiQL 도구](https://github.com/graphql/graphiql)도 포함되어 있습니다. CORS 구성에 대한 자세한 내용은 [이(가)](#cors-config) 아래에 있을 수 있습니다.

## 샘플 앱 설치{#sample-app}

이 자습서의 목표 중 하나는 GraphQL API를 사용하여 외부 애플리케이션에서 AEM 컨텐츠를 사용하는 방법을 보여주는 것입니다. 이 자습서에서는 부분적으로 완료된 React App 예제를 사용하여 자습서를 가속화합니다. iOS, Android 또는 기타 플랫폼에 맞게 제작된 앱에도 동일한 학습 및 개념이 적용됩니다. React 앱은 불필요한 복잡성을 피하기 위해 의도적으로 간단합니다.참조 구현이 아닙니다.

1. Git을 사용하여 새 터미널 창과 복제 자습서 시작 분기:

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 선택한 IDE에서 `aem-guides-wknd-graphql/react-app/.env.development`에 있는 `.env.development` 파일을 엽니다. 파일이 다음과 같이 표시되도록 `REACT_APP_AUTHORIZATION` 줄의 주석을 해제합니다.

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/endpoint.gql
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   `React_APP_HOST_URI`이(가) 로컬 AEM 인스턴스와 일치하는지 확인합니다. 이 장에서는 반응형 앱을 AEM **작성자** 환경에 직접 연결하여 인증을 받아야 합니다. 이는 AEM 환경을 빠르게 변경하고 앱에 즉시 반영되는 것을 확인할 수 있으므로 개발 중에 일반적으로 사용됩니다.

   >[!NOTE]
   >
   > 제작 시나리오에서 앱은 AEM **게시** 환경에 연결됩니다. 자세한 내용은 튜토리얼의 후반부에서 확인하십시오.

1. `aem-guides-wknd-graphql/react-app` 폴더로 이동합니다. 앱을 설치하고 시작합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 새 브라우저 창은 [http://localhost:3000](http://localhost:3000)에서 앱을 자동으로 실행해야 합니다.

   ![반응형 시작 앱](assets/setup/react-starter-app.png)

   AEM의 현재 모험 컨텐츠 목록이 표시됩니다.

1. 모험 이미지 중 하나를 클릭하여 모험 세부 사항을 확인합니다. 모험에 대한 세부 사항을 반환하기 위해 AEM에 요청이 있습니다.

   ![모험 세부 사항 보기](assets/setup/adventure-details-view.png)

1. 브라우저의 개발자 도구를 사용하여 **네트워크** 요청을 검사합니다. **XHR** 요청을 보고 AEM용으로 구성된 GraphQL 끝점인 `/content/graphql/endpoint.gql`에 대한 여러 POST 요청을 확인합니다.

   ![GraphQL 끝점 XHR 요청](assets/setup/endpoint-gql.png)

1. 네트워크 요청을 검사하여 매개 변수와 JSON 응답을 볼 수도 있습니다. 쿼리 및 응답에 대한 이해를 높이려면 Chrome용 [GraphQL 네트워크](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm)와 같은 브라우저 확장을 설치하는 것이 도움이 될 수 있습니다.

   ![GraphQL 네트워크 확장](assets/setup/GraphQL-extension.png)

   *Chrome 확장 GraphQL 네트워크 사용*

## 컨텐츠 조각 수정

React 앱이 실행 중이니 AEM의 콘텐츠를 업데이트하고 앱에 반영된 변경 사항을 확인하십시오.

1. AEM [http://localhost:4502](http://localhost:4502)로 이동합니다.
1. **자산** > **파일** > **WKND 사이트** > **영어** > **모험**[ > **발리 서프 캠프](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)** 2/>.

   ![발리 서프 캠프 폴더](assets/setup/bali-surf-camp-folder.png)

1. **Bali Surf Camp** 컨텐츠 조각을 클릭하여 컨텐츠 조각 편집기를 엽니다.
1. 탐험 **제목** 및 **설명**&#x200B;을 수정합니다.

   ![컨텐츠 조각 수정](assets/setup/modify-content-fragment-bali.png)

1. **저장**&#x200B;을 클릭하여 변경 내용을 저장합니다.
1. [http://localhost:3000](http://localhost:3000)의 React 앱으로 다시 이동하고 새로 고쳐 변경 내용을 확인합니다.

   ![업데이트된 발리 서프 캠프 어드벤처](assets/setup/overnight-bali-surf-camp-changes.png)

## 축하합니다!{#congratulations}

이제 GraphQL을 사용하여 AEM 컨텐츠를 사용하는 외부 애플리케이션이 있습니다. 반응 앱에서 코드를 자유롭게 검사하고 기존 컨텐츠 조각 수정을 계속 시도해 볼 수 있습니다.

## 다음 단계 {#next-steps}

다음 장의 [컨텐츠 조각 모델 정의](content-fragment-models.md)에서 컨텐트를 모델링하고 **컨텐츠 조각 모델**&#x200B;으로 스키마를 작성하는 방법을 알아봅니다. 기존 모델을 검토하고 새 모델을 생성합니다. 또한 모델의 일부로 스키마를 정의하는 데 사용할 수 있는 다양한 데이터 유형에 대해서도 학습합니다.

## (보너스) CORS 구성 {#cors-config}

AEM은 기본적으로 보안을 설정하므로 크로스 원본 요청을 차단하므로 허가되지 않은 애플리케이션에서 컨텐츠를 서로 연결하고 표시할 수 없습니다.

이 자습서의 React 앱이 AEM GraphQL API 끝점과 상호 작용할 수 있도록 GraphQL 끝점 패키지에 교차 원본 리소스 공유 구성이 정의되어 있습니다.

![원본 간 리소스 공유 구성](./assets/setup/cross-origin-resource-sharing-configuration.png)

수동으로 구성하려면:

1. **도구** > **작업** > **웹 콘솔**&#x200B;에서 AEM SDK의 웹 콘솔로 이동합니다.
1. **Adobe Granite Cross-Origin Resource Sharing Policy** 행을 클릭하여 새 구성을 만듭니다.
1. 다음 필드를 업데이트하여 다른 필드는 기본값으로 남습니다.
   * 허용된 원본:`localhost:3000`
   * 허용된 원본(Regex):`.* `
   * 허용되는 경로: `/content/graphql/endpoint.gql`
   * 허용되는 메서드:`GET`, `HEAD`, `POST`
      * GraphQL에는 `POST`만 필요하지만 다른 메서드는 헤드리스 방식으로 AEM과 상호 작용할 때 유용합니다.
   * 자격 증명 지원:`Yes`
      * React 앱이 AEM 작성자 서비스에서 보호된 GraphQL 끝점과 통신할 때 필요합니다.
1. **저장**&#x200B;을 클릭합니다

이 구성을 사용하면 `localhost:3000`에서 `/content/graphql/endpoint.gql` 경로의 AEM 작성자 서비스에 대한 `POST` HTTP 요청을 허용합니다.

이 구성 및 GraphQL 끝점은 AEM 프로젝트에서 생성됩니다. [여기에서 세부 사항을 확인합니다](https://github.com/adobe/aem-guides-wknd-graphql/tree/master/aem-project).
