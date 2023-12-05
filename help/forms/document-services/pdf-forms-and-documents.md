---
title: 다양한 유형의 PDF forms 및 문서 이해
description: PDF은 실제로 파일 형식 모음이며 이 문서에서는 양식 개발자에게 중요하고 적절한 PDF 유형에 대해 설명합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
jira: KT-7071
topic: Development
last-substantial-update: 2020-07-07T00:00:00Z
duration: 347
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---

# PDF

PDF(Portable Document Format)은 실제로 파일 형식 제품군이며, 이 문서에서는 양식 개발자에게 가장 적합한 파일 형식에 대해 자세히 설명합니다. 다양한 PDF 유형의 많은 기술 세부 사항과 표준이 진화하고 있습니다. 이러한 형식 및 사양 중 일부는 ISO(International Organization for Standardization) 표준이고 일부는 Adobe이 소유한 특정 지적 재산입니다.

이 문서에서는 다양한 유형의 PDF을 만드는 방법을 보여 줍니다. 각 항목을 사용하는 방법과 이유를 이해하는 데 도움이 됩니다. 이러한 모든 유형은 PDF 보기 및 작업을 위한 프리미어 클라이언트 도구인 Adobe Acrobat DC에서 가장 잘 작동합니다.

다음은 Acrobat DC의 PDF/A 파일 예입니다.

![Pdfa](assets/pdfa-file-in-acrobat.png)

샘플 파일은 다음과 같습니다. [여기에서 다운로드됨](assets/pdf-file-types.zip)

## XML Forms 아키텍처 PDF(XFA PDF)

Adobe은 XFA PDF 양식이라는 용어를 사용하여 AEM Forms Designer로 만든 대화형 및 동적 Forms을 나타냅니다. Designer로 만든 Forms 및 파일은 Adobe의 XML Forms 아키텍처(XFA)를 기반으로 합니다. 여러 가지 방식에서 XFA PDF 파일 형식은 기존 PDF 파일보다 HTML 파일에 더 가깝습니다. 예를 들어 다음 코드는 XFA PDF 파일에서 간단한 텍스트 개체의 모습을 보여 줍니다.

![텍스트 필드](assets/text-field.JPG)

XFA Forms은 XML을 기반으로 합니다. 이렇게 구조가 잘 되고 유연한 형식을 사용하면 AEM Forms 서버에서 디자이너 파일을 기존 PDF, PDF/A 및 HTML을 비롯한 다양한 형식으로 변환할 수 있습니다. 레이아웃 편집기의 XML 소스 탭을 선택하여 Designer에서 Forms의 전체 XML 구조를 확인할 수 있습니다. AEM Forms Designer에서 정적 및 동적 XFA Forms을 모두 만들 수 있습니다.

## 정적 PDF

정적 XFA PDF forms 레이아웃은 런타임 시 변경되지 않지만 사용자를 위해 대화형일 수 있습니다. 다음은 정적 XFA PDF forms의 몇 가지 장점입니다.

* 정적 XFA PDF forms 레이아웃은 런타임 시 변경되지 않지만 사용자를 위해 대화형일 수 있습니다.
* 정적 Forms은 Acrobat의 주석 및 마크업 도구를 지원합니다.
* 정적 Forms을 사용하면 Acrobat 주석을 가져오고 내보낼 수 있습니다.
* 정적 Forms은 AEM Forms 서버에서 수행할 수 있는 기술인 글꼴 하위 설정을 지원합니다.
* 정적 Forms은 최신 브라우저와 함께 제공되는 내장 PDF 뷰어를 사용하여 렌더링할 수 있습니다.

>[!NOTE]
>
> XDP를 Adobe 정적 PDF 양식으로 저장하여 AEM Forms Designer를 사용하여 정적 PDF을 만들 수 있습니다



### Dynamic Forms

동적 XFA PDF은 런타임 시 레이아웃을 변경할 수 있으므로 주석 달기 및 마크업 기능은 지원되지 않습니다. 그러나 동적 XFA PDF은 다음과 같은 이점을 제공합니다.

* 동적 양식은 양식의 레이아웃과 페이지 매김을 변경하는 클라이언트측 스크립트를 지원합니다. 예를 들어 Purchase Order.xdp는 동적 양식으로 저장하는 경우 무제한 양의 데이터를 수용하도록 확장되고 페이지가 지정됩니다
* 동적 양식은 런타임 시 양식의 모든 속성을 지원하는 반면, 정적 양식은 하위 집합만 지원합니다

* [정적 및 동적 pdf 양식의 차이점을 이해하려면 이 문서 를 참조하십시오](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/pdf-forms-and-documents.html#:~:text=Dynamic%20forms%20support%20all%20the,forms%20support%20only%20a%20subset)

>[!NOTE]
>
> XDP를 Adobe 동적 XML 양식으로 저장하여 AEM Forms Designer를 사용하여 동적 PDF를 만들 수 있습니다

>[!NOTE]
>
> 최신 브라우저의 기본 제공 pdf 뷰어를 사용하여 동적 양식을 렌더링할 수 없습니다.

### PDF 파일(기존 PDF)

인증된 문서는 PDF 문서 및 Forms 수신자에게 신뢰성 및 무결성을 추가로 보장합니다.

가장 널리 사용되는 PDF 형식은 기존 PDF 파일입니다. Acrobat 및 다양한 타사 도구를 사용하는 등 여러 가지 방법으로 기존 PDF 파일을 만들 수 있습니다. Acrobat에서는 다음과 같은 방법으로 기존 PDF 파일을 만들 수 있습니다. Acrobat이 설치되어 있지 않으면 컴퓨터에 이러한 옵션이 표시되지 않을 수 있습니다.

* 데스크탑 애플리케이션의 인쇄 스트림 캡처: 저작 애플리케이션의 인쇄 명령을 선택하고 Adobe PDF 프린터 아이콘을 선택합니다. 문서의 인쇄된 사본 대신 문서의 PDF 파일이 만들어집니다
* Microsoft Office 애플리케이션과 Acrobat PDFMaker 플러그인 사용: Acrobat을 설치하면 Microsoft Office 애플리케이션에 Adobe PDF 메뉴가 추가되고 Office 리본에 아이콘이 추가됩니다. 이러한 추가 기능을 사용하여 Microsoft Office에서 직접 PDF 파일을 만들 수 있습니다
* Acrobat Distiller을 사용하여 Postscript 및 Encapsulated Postscript(EPS) 파일을 PDF으로 변환: Distiller은 일반적으로 인쇄 게시 및 Postscript 형식에서 PDF 형식으로 변환해야 하는 기타 워크플로우에서 사용됩니다
* 후드에서 기존 PDF은 XFA PDF과 매우 다릅니다. XML 구조는 동일하지 않으며 파일의 인쇄 스트림을 캡처하여 생성되므로 기존 PDF은 정적 및 읽기 전용 파일입니다.

인증된 문서는 PDF 문서를 제공하고 수신자의 신뢰성과 무결성을 추가로 보장합니다.

### 아크로폼

Acroforms는 Adobe의 이전 대화형 형식 기술이며 Acrobat 버전 3으로 거슬러 올라갑니다. Adobe은 [Acrobat Forms API 참조](assets/FormsAPIReference.pdf)2003년 5월자로 결정되어 이 기술에 대한 기술적인 세부 정보를 제공합니다. Acroforms는 다음 항목의 조합입니다.

* 양식의 정적 레이아웃 및 그래픽을 정의하는 기존 PDF.
* Adobe Acrobat 프로그램의 양식 도구로 맨 위에 볼트로 고정되는 대화형 양식 필드. 이러한 양식 도구는 AEM Forms Designer에서 사용할 수 있는 기능의 작은 하위 집합입니다.

### PDF/A(아카이브 PDF)

PDF/A(Archives PDF)는 장기 보관을 향상시키는 다양한 세부 정보를 제공하는 기존 PDF의 문서 스토리지 이점을 기반으로 합니다. 기존 PDF 파일 형식은 장기 문서 저장에 많은 이점을 제공합니다. PDF의 컴팩트한 특성은 간편한 전송을 용이하게 하고 공간을 절약하며, 잘 구성된 특성을 통해 강력한 인덱싱 및 검색 기능을 사용할 수 있습니다. 기존 PDF은 메타데이터에 대한 광범위한 지원을 제공하며 PDF은 다양한 컴퓨터 환경을 지원한 오랜 역사를 가지고 있습니다.

PDF과 마찬가지로 PDF/A는 ISO 표준 사양입니다. AIIM(Association for Information and Image Management), NPES(National Printing Equipment Association), 미국 법원 행정국 등이 포함된 TASK FORCE가 개발하였다. PDF/A 사양의 목표는 장기 아카이브 형식을 제공하는 것이므로 많은 PDF 기능이 생략되어 파일이 자체 포함될 수 있습니다. 다음은 PDF/A 파일의 장기 재현성을 향상시키는 세부 항목에 대한 몇 가지 주요 사항입니다.

* 모든 콘텐츠는 파일에 포함되어야 하며 하이퍼링크, 글꼴 또는 소프트웨어 프로그램과 같은 외부 소스에 대한 종속성이 없을 수 있습니다.
* 모든 글꼴은 내장되어야 하며, 전자 문서에 대한 무제한 사용 라이선스가 있는 글꼴이어야 합니다.
* JavaScript가 허용되지 않음
* 투명도가 허용되지 않음
* 암호화가 허용되지 않음
* 오디오 및 비디오 콘텐츠는 허용되지 않습니다.
* 색상 공간은 장치 독립적 방식으로 정의되어야 합니다
* 모든 메타데이터는 특정 표준을 따라야 합니다

### PDF/A 파일 보기

샘플 파일의 두 파일은 동일한 Microsoft Word 파일에서 만들어졌습니다. 하나는 기존 PDF으로, 다른 하나는 PDF/A 파일로 만들어졌습니다. Acrobat Professional에서 다음 두 파일을 엽니다.

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

문서 모양이 동일하지만 PDF/A 파일이 열리고 맨 위에 파란색 막대가 표시되어 PDF/A 모드에서 이 문서를 보고 있음을 나타냅니다. 이 파란색 막대는 특정 유형의 PDF 파일을 열 때 표시되는 Acrobat의 문서 메시지 막대입니다.

![Pdf-img](assets/pdfa-message.png)

문서 메시지 창에는 작업을 완료하는 데 도움이 되는 지침과 버튼이 포함되어 있습니다. 색상으로 구분되어 있으며, 특수 유형의 PDF(이 PDF/A 파일 등)와 인증 및 디지털 서명된 PDF을 열면 파란색이 표시됩니다. PDF 검토에 참여하면 PDF forms의 경우 표시줄이 자주색으로 변경되고 노란색으로 변경됩니다.

>[!NOTE]
>
> 편집 사용을 클릭하면 이 문서가 PDF/A 규정 준수 대상에서 제외됩니다.
