---
title: 소득 패널에 구성 요소 추가
description: '[수입] 패널에 테이블을 추가하겠습니다. 테이블 행을 구성하고 규칙 편집기를 사용하여 총계를 계산합니다.'
feature: 적응형 양식
version: 6.4,6.5
thumbnail: 22198.jpg
kt: 4211
topic: 개발
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 1%

---


# 소득 패널에 구성 요소 추가 {#adding-components-to-income-panel}

[수입] 패널에 테이블을 추가하겠습니다. 테이블 행을 구성하고 규칙 편집기를 사용하여 총계를 계산합니다.

**테이블 구성 요소 추가 및 구성**

>[!VIDEO](https://video.tv.adobe.com/v/22198?quality=9&learn=on)



## 소득 테이블을 동적으로 만들기 {#make-the-income-table-dynamic}

**편집 모드 인지 확인합니다. 편집 단추는 브라우저의 오른쪽 상단에 있습니다.**

* 기본적으로 적응형 양식에 표를 삽입하면 테이블이 동적이지 않으므로 런타임 시 테이블에 새 행을 추가할 수 없습니다.

* 브라우저를 새로 고칩니다.

* 컨텐츠 계층 구조에서 Row1을 선택합니다.

* 공구아이콘 아이콘을 클릭하여 등록정보 시트를 엽니다.

* 반복 설정 아래에서 최소값 및 최대값을 1과 5로 설정하고 파란색 확인 표시 아이콘을 클릭하여 변경 사항을 저장합니다. 즉, 테이블에는 최대 5개의 행이 있을 수 있습니다. 행 수가 무제한이면 최대 개수를 -1로 설정합니다.

## 규칙을 만들어 총계 계산 {#create-rule-to-calculate-grand-total}


>[!VIDEO](https://video.tv.adobe.com/v/22197?quality=9&learn=on)


