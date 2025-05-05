---
title: AEM Forms이 포함된 Acroforms
description: Acroforms와 AEM Forms 통합의 1부. Acroform을 사용하여 적응형 양식을 만들고 데이터를 병합하여 PDF을 가져올 수 있습니다.
feature: adaptive-forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 144
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# Acroform 만들기

Acroforms는 Acrobat을 사용하여 만든 양식입니다. Acrobat을 사용하여 처음부터 새 양식을 만들거나 Microsoft Word에서 만든 기존 양식을 가져와서 Acrobat을 사용하여 Acroform으로 변환할 수 있습니다. Microsoft Word에서 만든 양식을 Acroform으로 변환하려면 다음 단계를 수행해야 합니다.

* Acrobat을 사용하여 Word 문서 열기
* Acrobat 양식 준비 도구를 사용하여 양식의 양식 필드를 식별합니다.
* pdf를 저장합니다. 파일 이름에 공백이 없는지 확인하십시오.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>Acrobat Sign을 사용하여 서명할 입력 가능한 양식을 전송하려면 해당 필드에 이름을 지정하십시오. 예를 들어 필드 이름을 **`Sig_es_:signer1:signature`**&#x200B;로 지정할 수 있습니다. Acrobat Sign이 이해하는 구문입니다.

>[!NOTE]
>
>XFA 기반 문서를 보내는 경우 문서를 병합해야 하며 Acrobat Sign 서명 태그가 문서에 정적 텍스트로 표시되어야 합니다.

[Acrobat Sign 텍스트 태그 문서](https://helpx.adobe.com/kr/sign/using/text-tag.html)

>[!NOTE]
>
>acroform 파일 이름에 공백이 없는지 확인합니다. 현재 샘플 코드에서는 공백을 처리하지 않습니다.
>
>양식 필드 이름에는 다음만 포함될 수 있습니다.
>
>* 단일 공간
>* 단일 밑줄
>* 영숫자 문자
