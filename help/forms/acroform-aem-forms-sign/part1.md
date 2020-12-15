---
title: AEM Forms을 사용한 Acrobat
seo-title: Acrobat을 사용하여 적응형 양식 데이터 병합
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---


# Acrobat 만들기

Acrobat은 Acrobat을 사용하여 만든 양식입니다. Acrobat을 사용하여 새로운 양식을 처음부터 만들거나 Microsoft Word에서 만든 기존 양식을 Acrobat으로 변환하여 Acrobat을 사용하여 Acrobat으로 변환할 수 있습니다. Microsoft Word에서 만든 양식을 Acrobat으로 변환하려면 다음 단계를 수행해야 합니다.

* Acrobat을 사용하여 Word 문서 열기
* Acrobat 양식 준비 도구를 사용하여 양식의 양식 필드를 식별합니다.
* pdf를 저장합니다. 파일 이름에 공백이 없는지 확인합니다.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Adobe Sign을 사용하여 서명할 수 있는 입력 가능한 Acrobat을 보내려면 필드에 적절한 이름을 지정하십시오. 예를 들어 필드 이름을 **Sig_es_:signer1:signature**&#x200B;로 지정할 수 있습니다. Adobe Sign이 이해하는 구문입니다.

>[!NOTE]
>
>XFA 기반 문서를 전송하는 경우 문서를 병합해야 하며 Adobe Sign 서명 태그가 문서에 정적 텍스트로 표시되어야 합니다.

[Adobe Sign 텍스트 태그 문서](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>acroform 파일 이름에 공백이 없는지 확인합니다. 현재 샘플 코드는 공백을 처리하지 않습니다.
>
>양식 필드 이름은 다음 내용만 포함할 수 있습니다.
>
>* 단일 공간
>* 단일 밑줄
>* 영숫자

