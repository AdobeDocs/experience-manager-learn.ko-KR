---
title: 투명성 및 컨텐츠 자동화 일괄 처리를 위한 Dynamic Media
description: AEM에서 Dynamic Media를 사용하여 가상 렌디션을 만들고, 투명도를 관리하고, 이미지 처리를 자동화하여 확장 가능한 컨텐츠 재사용을 가능하게 하는 방법에 대해 알아봅니다.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: a509b7c41e6adb05e6c3b791ac2e99f635973322
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 6%

---


# 투명성 및 컨텐츠 자동화 일괄 처리를 위한 Dynamic Media

AEM에서 Dynamic Media를 사용하여 가상 렌디션을 만들고, 투명도를 관리하고, 이미지 처리를 자동화하여 확장 가능한 컨텐츠 재사용을 가능하게 하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3463052/?learn=on&enablevpops&captions=kor)


## Dynamic Media 자산 예

다음은 비디오에 사용된 Dynamic Media 에셋 및 해당 URL의 예입니다.

>[!BEGINTABS]

>[!TAB 이미지 투명도 예]

다음은 비디오에 사용되는 Dynamic Media 이미지 서버 샘플 URL입니다.

| 미리 보기 | 설명 | Dynamic Media URL |
|-----------|------------------|---------|
| ![기본값](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | 기본값 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![매끄러운 배경 이미지 레이어와 합성됨](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | 매끄러운 배경 이미지 레이어와 합성됨 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![빨간색 배경](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | 빨간색 배경 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![타원 패스로 잘림](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | 타원 패스에 클리핑됨 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB 이미지 경로 예]

다음은 비디오에 사용되는 Dynamic Media 이미지 서버 샘플 URL입니다.

| 미리 보기 | 설명 | Dynamic Media URL |
|-----------|------------------|---------|
| ![80픽셀로 표준화됨(투명도 없음)](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | 80픽셀 너비로 표준화됨(투명도 없음){width="250"} | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![경로에 자르기](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | 경로에 자르기 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![경로에 클립](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | 패스에 클립 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![경로에 클립 및 경로에 자르기](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | 패스에 클립 및 패스에 자르기 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![다른 경로에 클립](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | 다른 패스에 클립 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![다른 경로에 클립하여 빨간색 배경으로 만들기](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | 다른 패스로 클립하여 빨간색 배경 만들기 | [링크](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## Dynamic Media 이미지 서버 API

* [Dynamic Media 이미지 제공 및 렌더링 API](https://experienceleague.adobe.com/ko/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Dynamic Media 스냅숏 미리 보기](https://snapshot.scene7.com/)
