---
title: 도구 모음의 다음 버튼과 이전 버튼의 간격 조정
description: 도구 모음 버튼의 공간 확보
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 59
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---

# 도구 모음 버튼의 공백 지정

AEM Forms의 도구 모음에 다음 및 이전 단추를 추가하면 기본적으로 단추가 서로 옆에 배치됩니다. 이전/뒤로 단추를 왼쪽에 둔 채 도구 모음에서 다음 단추를 맨 오른쪽 단추로 누를 수 있습니다

![도구 모음 간격](assets/toolbar-spacing.png)


## 도구 모음 스타일 지정

스타일 편집기를 사용하면 위의 사용 사례를 쉽게 수행할 수 있습니다. 도구 모음에 [이전/다음] 단추를 추가한 후에는 편집 메뉴에서 스타일 레이어를 선택했는지 확인합니다. 스타일 모드를 선택한 상태에서 도구 모음을 선택하여 스타일 속성 시트를 엽니다. Dimension 및 위치 섹션을 확장하고 모든 속성이 표시되는지 확인합니다. 다음 속성 설정
* Dimension 및 위치
   * 폭: 100%
   * 위치: 상대

변경 사항 저장

## 다음 단추 스타일 지정

[다음] 단추를 선택하고 [다음] 단추 텍스트 대신 [다음] 단추의 스타일 속성 시트를 열어야 합니다. 다음 속성 설정
* Dimension 및 위치
   * 위치: 절대 상단 1px 오른쪽 1px
* 테두리
   * 테두리 반경: 4px(위쪽, 오른쪽, 아래쪽, 왼쪽)

변경 사항 저장
