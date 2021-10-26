---
title: 도구 모음의 다음 및 이전 단추 간격
description: 도구 모음 단추 공간
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 96b78ff5056bd9c2be39fb2cf21b4f92863af089
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 도구 모음 단추 공간

AEM Forms의 도구 모음에 다음 및 이전 단추를 추가하면 기본적으로 단추가 서로 옆에 배치됩니다. 왼쪽에 있는 이전/뒤로 단추를 유지하면서 도구 모음에서 맨 위 오른쪽에 있는 다음 단추를 푸시할 수 있습니다

![도구 모음 간격](assets/toolbar-spacing.png)


## 도구 모음 스타일 지정

위의 사용 사례는 스타일 편집기를 사용하여 쉽게 수행할 수 있습니다. 도구 모음에 [이전/다음] 단추를 추가한 후에는 편집 메뉴에서 [스타일] 레이어를 선택했는지 확인합니다. 스타일 모드를 선택한 상태에서 도구 모음을 선택하여 스타일 속성 시트를 엽니다. Dimension 및 위치 섹션을 확장하고 모든 속성을 확인합니다. 다음 속성을 설정합니다
* Dimension 및 위치
   * 너비: 100%
   * 위치: 상대적

변경 내용을 저장합니다

## 다음 단추의 스타일 지정

Next 단추를 선택하고 다음 단추의 스타일 속성 시트(다음 단추 텍스트가 아님)를 열는지 확인합니다. 다음 속성을 설정합니다
* Dimension 및 위치
   * position: 절대 위쪽 1px 오른쪽 1px
* 테두리
   * 테두리 반경: 4px(위쪽,오른쪽,아래쪽,왼쪽)

변경 내용을 저장합니다
