---
title: 다양한 유형의 PDF forms 및 문서 이해
description: PDF는 파일 포맷으로 되어 있으며 이 문서에서는 양식 개발자에게 중요하며 관련이 있는 PDF 유형에 대해 설명합니다.
solution: Experience Manager, Experience Manager Forms
type: 설명서
role: 개발자
level: 초급,중급
version: 6.3,6.4,6.5
feature: 문서 서비스
kt: 7071
topic: 개발
translation-type: tm+mt
source-git-commit: 1b4512fdb047bec15d72a8278fd0ce5dfafa309f
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---


# PDF

PDF(Portable Document Format)는 실제로 파일 형식 제품군이며 이 문서에서는 양식 개발자에게 가장 적합한 파일 형식을 자세히 설명합니다. 다양한 PDF 유형의 기술적 세부 사항 및 표준은 점차 진화하고 변화하고 있습니다. 이러한 형식 및 사양 중 일부는 ISO(International Organization for Standardization) 표준이며, 일부는 Adobe이 소유하는 특정 지적 재산입니다.

이 문서에서는 다양한 유형의 PDF를 만드는 방법을 설명합니다. 각각의 기능을 사용하는 방법과 이유를 이해하는 데 도움이 됩니다. 이러한 모든 유형은 PDF를 보고 작업할 수 있는 최고의 클라이언트 도구인 Adobe Acrobat DC에서 가장 잘 작동합니다.

다음은 Acrobat DC의 PDF/A 파일 예입니다.

![Pdfa](assets/pdfa-file-in-acrobat.png)

샘플 파일은 여기에서 [다운로드할 수 있습니다](assets/pdf-file-types.zip).

## Xml Forms 아키텍처 PDF

Adobe은 AEM Forms Designer에서 만든 인터랙티브하고 다이내믹한 Forms을 참조하기 위해 PDF 양식이라는 용어를 사용합니다. Designer로 만든 Forms 및 파일은 Adobe의 XFA(XML Forms Architecture)를 기반으로 합니다. 다양한 방식으로 XFA PDF 파일 형식은 기존 PDF 파일보다 HTML 파일에 더 가깝습니다. 예를 들어 다음 코드는 XFA PDF 파일에서 Look이라는 간단한 텍스트 개체를 보여 줍니다.

![텍스트 필드](assets/text-field.JPG)

XFA Forms은 XML을 기반으로 합니다. AEM Forms Server는 구조화되고 유연한 이 포맷을 사용하여 기존의 PDF, PDF/A, HTML 등 다양한 포맷으로 디자이너 파일을 변환할 수 있습니다. 레이아웃 편집기의 XML 소스 탭을 선택하여 Designer에서 Forms의 전체 XML 구조를 볼 수 있습니다. AEM Forms Designer에서 정적 및 동적 XFA Forms을 모두 만들 수 있습니다.

## 정적 PDF

정적 XFA PDF forms 레이아웃은 런타임 시 변경되지 않지만 사용자가 상호 작용할 수 있습니다. 다음은 정적 XFA PDF forms의 몇 가지 이점입니다.

* 정적 XFA PDF forms 레이아웃은 런타임 시 변경되지 않지만 사용자가 상호 작용할 수 있습니다.
* 정적 Forms은 Acrobat의 주석 달기 및 마크업 툴을 지원합니다.
* 정적 Forms을 사용하면 Acrobat 주석을 가져오고 내보낼 수 있습니다.
* 정적 Forms은 AEM Forms Server에서 수행할 수 있는 기술인 글꼴 하위 설정을 지원합니다.
* 정적인 Forms은 최신 브라우저와 함께 제공된 내장된 PDF 뷰어를 사용하여 렌더링할 수 있습니다.

>[!NOTE]
>
> XDP를 Adobe 정적 PDF 양식으로 저장하여 AEM Forms Designer를 사용하여 정적 PDF를 만들 수 있습니다.

## PDF 포맷

PDF(Portable Document Format)는 실제로 파일 형식 제품군이며 이 문서에서는 양식 개발자에게 가장 적합한 파일 형식을 자세히 설명합니다. 다양한 PDF 유형의 기술적 세부 사항 및 표준은 점차 진화하고 변화하고 있습니다. 이러한 형식 및 사양 중 일부는 ISO(International Organization for Standardization) 표준이며, 일부는 Adobe이 소유하는 특정 지적 재산입니다.

이 문서에서는 다양한 유형의 PDF를 만드는 방법을 설명합니다. 각각의 기능을 사용하는 방법과 이유를 이해하는 데 도움이 됩니다. 이러한 모든 유형은 PDF를 보고 작업할 수 있는 최고의 클라이언트 도구인 Adobe Acrobat DC에서 가장 잘 작동합니다.

Acrobat DC에서 PDF/A 파일의 예입니다.

![pdfa](assets/pdfa-file-in-acrobat.png)

샘플 파일은 여기에서 [다운로드할 수 있습니다](assets/pdf-file-types.zip).

### XFA PDF

Adobe은 PDF 양식이라는 용어를 사용하여 AEM Forms Designer에서 만든 인터랙티브하고 다이내믹한 양식을 참조합니다. Acrobat이라는 다른 유형의 PDF 양식이 있는데, 이는 AEM Forms Designer에서 만든 PDF forms과 다릅니다. Designer로 만드는 양식과 파일은 Adobe의 XFA(XML Forms Architecture)를 기반으로 합니다. 다양한 방식으로 XFA PDF 파일 형식은 기존 PDF 파일보다 HTML 파일에 더 가깝습니다. 예를 들어 다음 코드는 XFA PDF 파일에서 간단한 텍스트 개체가 어떻게 표시되는지 보여 줍니다.

![text-field](assets/text-field.JPG)

보시다시피 XFA 양식은 XML을 기반으로 합니다. AEM Forms Server는 구조화되고 유연한 이 포맷을 사용하여 기존의 PDF, PDF/A, HTML 등 다양한 포맷으로 디자이너 파일을 변환할 수 있습니다. 레이아웃 편집기의 XML 소스 탭을 선택하여 Designer에서 양식의 전체 XML 구조를 볼 수 있습니다. AEM Forms Designer에서 정적 및 동적 XFA 양식을 모두 만들 수 있습니다.

### 정적 PDF

정적 XFA PDF forms은 런타임 시 레이아웃을 변경하지 않지만 사용자가 상호 작용할 수 있습니다. 다음은 정적 XFA PDF forms의 몇 가지 이점입니다.

* 정적 XFA PDF forms은 런타임 시 레이아웃을 변경하지 않지만 사용자가 상호 작용할 수 있습니다.
* 정적 양식은 Acrobat의 주석 달기 및 마크업 툴을 지원합니다.
* 정적 양식을 사용하여 Acrobat 주석을 가져오고 내보낼 수 있습니다.
* 정적 양식은 AEM Forms 서버에서 수행할 수 있는 기술인 글꼴 하위 설정을 지원합니다.
* 정적 양식은 최신 브라우저와 함께 제공된 내장된 pdf 뷰어를 사용하여 렌더링할 수 있습니다.

>[!NOTE]
> XDP를 Adobe 정적 PDF 양식으로 저장하여 AEM Forms Designer를 사용하여 정적 PDF를 만들 수 있습니다.

### 동적 Forms

동적 XFA PDF는 런타임 시 레이아웃을 변경할 수 있으므로 주석 달기 및 마크업 기능이 지원되지 않습니다. 그러나 동적 XFA PDF는 다음과 같은 이점을 제공합니다.

* 동적 양식은 양식의 레이아웃과 페이지 매김을 변경하는 클라이언트측 스크립트를 지원합니다. 예를 들어 Purchase Order.xdp를 동적 양식으로 저장할 경우 방대한 데이터를 수용할 수 있도록 확장 및 페이지가 확장됩니다
* 동적 양식은 런타임 시 양식의 모든 속성을 지원하는 반면 정적 양식은 하위 집합만 지원합니다

>[!NOTE]
>
> XDP를 Adobe Dynamic XML 양식으로 저장하여 AEM Forms Designer를 사용하여 동적 PDF를 만들 수 있습니다

>[!NOTE]
>
> 최신 브라우저에서 내장된 pdf 뷰어를 사용하여 동적 양식을 렌더링할 수 없습니다.

### PDF 파일(일반 PDF)

인증된 문서는 PDF 문서 및 Forms 수신자에게 문서의 신뢰성 및 무결성에 대한 보장을 제공합니다.

가장 널리 사용되는 PDF 포맷은 일반적인 PDF 파일입니다. Acrobat 및 다양한 타사 툴을 사용하는 등 일반적인 PDF 파일을 작성하는 방법에는 여러 가지가 있습니다. Acrobat은 일반적인 PDF 파일을 작성하는 다음과 같은 모든 방법을 제공합니다. Acrobat이 설치되어 있지 않은 경우 컴퓨터에 이러한 옵션이 표시되지 않을 수 있습니다.

* 데스크탑 애플리케이션의 인쇄 스트림을 캡처하여 다음을 수행합니다.제작 응용 프로그램의 [인쇄] 명령을 선택하고 Adobe PDF 프린터 아이콘을 선택합니다. 문서의 인쇄 사본이 아니라 문서의 PDF 파일을 만듭니다
* Microsoft Office 응용 프로그램에서 Acrobat PDFMaker 플러그인을 사용하여 다음을 수행합니다.Acrobat을 설치하면 Adobe PDF 메뉴가 Microsoft Office 응용 프로그램에 추가되고 Office 리본에 아이콘이 추가됩니다. 이러한 추가된 기능을 사용하여 Microsoft Office에서 바로 PDF 파일을 만들 수 있습니다
* Acrobat Distiller을 사용하여 Postscript 및 EPS(Encapsulated Postscript) 파일을 PDF로 변환:Distiller은 일반적으로 인쇄 출판 및 Postscript 포맷에서 PDF 포맷으로 변환해야 하는 기타 워크플로우에서 사용됩니다
* 백그라운드에서 기존 PDF는 XFA PDF와 매우 다릅니다. XML 구조는 동일하지 않으며 파일의 인쇄 스트림을 캡처하여 만들기 때문에 기존 PDF는 정적 읽기 전용 파일입니다.

인증된 문서는 PDF 문서 및 양식 수신자에게 문서의 신뢰성 및 무결성에 대한 추가 보장을 제공합니다.

### Acrobat

Acrobat은 Adobe의 이전 인터랙티브 양식 기술입니다.Acrobat 버전 3으로 돌아갑니다. Adobe은 이 기술에 대한 기술 세부 정보를 제공하기 위해 2003년 5월 최신 [Acrobat Forms API 참조](assets/FormsAPIReference.pdf)를 제공합니다. Acrobat은
다음 항목:

* 양식의 정적 레이아웃과 그래픽을 정의하는 일반적인 PDF입니다.
* Adobe Acrobat 프로그램의 양식 툴을 사용하여 상단에 표시되는 인터랙티브한 양식 필드 이러한 양식 도구는 AEM Forms Designer에서 사용할 수 있는 기능의 일부에 불과합니다.

### PDF/A(보관을 위한 PDF)

PDF/A(보관을 위한 PDF)는 장기 보관을 향상시켜주는 많은 구체적인 세부 정보를 제공하는 기존 PDF의 문서 저장 이점을 기반으로 합니다. 기존의 PDF 파일 포맷은 장기 문서 저장 시 다양한 이점을 제공합니다. PDF의 크기가 작아 편리하게 전송하고 공간을 편리하게 관리할 수 있으며 잘 구성된 PDF의 특성을 통해 강력한 인덱싱 및 검색 기능을 활용할 수 있습니다. 기존 PDF는 메타데이터에 대한 광범위한 지원을 제공하므로 PDF는 다양한 컴퓨터 환경을 지원하는 오랜 역사를 가지고 있습니다.

PDF와 마찬가지로 PDF/A는 ISO 표준 사양입니다. AIIM(정보 및 이미지 관리 협회), NPES(국립 인쇄 장비 협회), 그리고 미국 법원의 행정 사무소가 포함된 특별 수사대에 의해 개발되었다. PDF/A 사양의 목표는 장기 보관 포맷을 제공하는 것이므로 많은 PDF 기능이 생략되므로 파일을 직접 포함할 수 있습니다. 다음은 PDF/A 파일의 장기 재생산성을 향상시키는 사양에 대한 몇 가지 주요 정보입니다.

* 모든 컨텐츠는 파일에 포함되어 있어야 하며 하이퍼링크, 글꼴 또는 소프트웨어 프로그램과 같은 외부 소스에는 종속될 수 없습니다.
* 모든 글꼴은 임베드되어 있어야 하며 전자 문서에 대한 무제한으로 사용할 수 있는 라이선스가 있는 글꼴이어야 합니다.
* JavaScript가 허용되지 않음
* 투명도가 허용되지 않음
* 암호화가 허용되지 않음
* 오디오 및 비디오 콘텐트가 허용되지 않음
* 색상 공간은 장치 독립적 방식으로 정의해야 합니다.
* 모든 메타데이터는 특정 표준을 따라야 합니다.

### PDF/A 파일 보기

샘플 파일에 있는 2개의 파일이 동일한 Microsoft Word 파일에서 만들어졌습니다. 하나는 일반 PDF로, 다른 하나는 PDF/A 파일로 작성되었습니다. Acrobat Professional에서 다음 두 파일을 엽니다.

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

문서가 동일하게 표시되지만 PDF/A 파일은 맨 위에 파란색 막대가 표시되어 이 문서를 PDF/A 모드로 보고 있음을 나타냅니다. 이 파란색 막대는 Acrobat의 문서 메시지 막대로, 특정 유형의 PDF 파일을 열 때 표시됩니다.

![Pdf-img](assets/pdfa-message.png)

문서 메시지 모음에는 작업을 완료하는 데 도움이 되는 지침과 단추가 포함되어 있습니다. 색상이 지정되어 있으므로 PDF/A 파일과 같은 특수 유형의 PDF를 열 때 파란색으로 표시되고 인증 및 디지털 서명된 PDF를 열 수 있습니다. PDF 검토에 참여할 때 막대가 PDF forms의 경우 자주색으로, 노란색으로 변경됩니다.

>[!NOTE]
>
> [편집 활성화]를 클릭하면 이 문서를 PDF/A 규격에서 제거합니다.




