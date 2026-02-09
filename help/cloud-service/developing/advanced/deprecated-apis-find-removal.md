---
title: AEM as a Cloud Service에서 더 이상 사용되지 않는 API 찾기 및 제거
description: AEM as a Cloud Service에서 더 이상 사용되지 않는 API를 찾고 제거하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
source-git-commit: 6c5b911d1d59573338dd1a30eb95289bc1339f19
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 3%

---


# AEM as a Cloud Service에서 더 이상 사용되지 않는 API 찾기 및 제거

AEM as a Cloud Service에서 더 이상 사용되지 않는 API를 찾고 제거하는 방법에 대해 알아봅니다.

## 개요

AEM as a Cloud Service **Action Center**&#x200B;에서 프로젝트의 _더 이상 사용되지 않는 API_&#x200B;에 대해 알려 줍니다. Cloud Manager 파이프라인을 사용하여 AEM as a Cloud Service에 최신 기능, 보안 업데이트 및 원활한 코드 배포를 가져오려면 프로젝트에서 더 이상 사용되지 않는 API를 제거하십시오.

이 자습서에서는 [AEM Analyzer Maven 플러그인](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)을 사용하여 AEM as a Cloud Service 환경에서 더 이상 사용되지 않는 API를 찾고 제거하는 방법을 알아봅니다.

## 더 이상 사용되지 않는 API를 찾는 방법

AEM as a Cloud Service 프로젝트에서 더 이상 사용되지 않는 API를 찾으려면 다음 단계를 따르십시오.

1. **최신 AEM Analyzer Maven 플러그인 사용**

   AEM 프로젝트에서 [AEM Analyzer Maven Plugin](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)의 최신 버전을 사용합니다.

   - 기본 `pom.xml`에서 플러그 인 버전은 일반적으로 선언됩니다. 버전을 최신 [릴리스 버전](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin)과(와) 비교합니다.

     ```xml
     ...
     <aemanalyser.version>1.6.14</aemanalyser.version> <!-- Latest released version as of 09-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - 플러그인은 사용 가능한 최신 AEM SDK에 대해 확인합니다. 프로젝트의 `pom.xml` 파일에서 최신 AEM SDK 버전을 사용합니다. 더 이상 사용되지 않는 API를 IDE 경고로 표시하는 데 도움이 됩니다.

     ```xml
     ...
     <aem.sdk.api>2026.2.24288.20260204T121510Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 09-Feb-2026 -->
     ...
     ```

   - `all` 모듈이 `verify` 단계에서 플러그인을 실행하는지 확인하십시오.

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **빌드 실행 및 경고 확인**

   `mvn clean install`을(를) 실행하면 분석기가 사용 중단된 API를 출력에서 **[경고]** 메시지로 보고합니다. 예:

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   빌드 성공 또는 실패에 초점을 맞출 때 이러한 메시지를 간과하기 쉽습니다.

3. **더 이상 사용되지 않는 API의 명확한 목록 가져오기**

   위의 단계 또한 동일한 정보를 제공합니다. 그러나 `verify` 모듈에서 `all` 단계를 실행하여 모든 **[WARNING]** 메시지를 한 곳에서 봅니다. 예:

   ```shell
   $ mvn clean verify -pl all
   ```

   빌드 출력의 **[WARNING]** 메시지는 프로젝트에서 더 이상 사용되지 않는 API를 나열합니다.

## 더 이상 사용되지 않는 API를 제거하는 방법

AEM 분석기는 **what**&#x200B;이(가) 더 이상 사용되지 않음을 보고하고 이를 수정하는 방법에 대해 **권장 사항**&#x200B;을 제공합니다. 그러나 아래 표를 사용하여 올바른 작업을 선택하고 자세한 내용이 필요한 경우 연결된 설명서를 따르십시오.

### 사용되지 않는 API 업데이트 관리 전략

| 분석기 경고 유형 | 표시 내용 | 권장 작업 | 참조 |
| --------------------- | ----------------- | ------------------ | --------- |
| 더 이상 사용되지 않는 AEM API | API가 AEM as a Cloud Service에서 제거됩니다. | 사용을 지원되는 공개 API로 바꾸기 | [API 제거 지침](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 더 이상 사용되지 않는 AEM 패키지 또는 클래스 | 패키지 또는 클래스는 더 이상 지원되지 않습니다. | 권장 대안을 사용하기 위한 리팩터링 코드 | [사용되지 않는 API](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| 더 이상 사용되지 않는 타사 라이브러리 | 라이브러리는 향후 SDK에서 지원되지 않습니다. | 종속성 및 리팩터링 사용 업그레이드 | [일반 지침](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 더 이상 사용되지 않는 Sling/OSGi 패턴 | 기존 주석 또는 API가 감지됨 | 최신 Sling 및 OSGi API로 마이그레이션 | [Sling/OSGi 패턴 제거](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| 제거 예정(미래 날짜) | API는 여전히 작동하지만 제거는 나중에 적용됩니다. | 파이프라인 시행 전에 정리 예약 | [릴리스 정보](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/release-notes/home) |

### 실용적인 지침

- 분석기 경고를 선택적 메시지가 아닌 **향후 파이프라인 실패**(으)로 처리합니다.
- **최신 AEM SDK**&#x200B;를 사용하여 로컬에서 더 이상 사용되지 않는 API를 수정합니다.
- 향후 AEM 업그레이드 시 문제가 발생하지 않도록 분석기 출력을 깔끔하게 유지합니다.

더 이상 사용되지 않는 API를 조기에 수정하면 프로젝트 **업그레이드에 안전하고 배포 준비가 된 상태로**&#x200B;이(가) 유지됩니다.

## 추가 리소스

- [AEM Analyzer Maven 플러그인](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [더 이상 사용되지 않는 기능 및 제거된 API](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)

