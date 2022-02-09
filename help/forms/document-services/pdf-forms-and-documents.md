---
title: 다양한 유형의 PDF forms 및 문서 이해
description: PDF은 실제로 파일 형식 제품군이며, 이 문서에서는 양식 개발자에게 중요하고 관련된 PDF 유형을 설명합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.3,6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

Portable Document Format (PDF)은 실제로 파일 형식 제품군이며, 이 문서에서는 양식 개발자에게 가장 적합한 형식을 자세히 설명합니다. 다양한 PDF 유형의 기술적인 세부 사항과 표준이 발전하고 있습니다. 이러한 형식 및 사양 중 일부는 ISO(International Organization for Standardization) 표준이며, 일부는 Adobe이 소유한 특정 지적 재산입니다.

이 문서에서는 다양한 유형의 PDF을 만드는 방법을 보여줍니다. 각 플러그인을 사용하는 방법과 이유를 이해하는 데 도움이 됩니다. 이러한 모든 유형은 프리미어 클라이언트 도구에서 PDF(Adobe Acrobat DC)를 보고 작업할 때 가장 잘 작동합니다.

다음은 Acrobat DC의 PDF/A 파일의 예입니다.

![Pdfa](assets/pdfa-file-in-acrobat.png)

샘플 파일은 [여기에서 다운로드](assets/pdf-file-types.zip)

## XML Forms 아키텍처 PDF(XFA PDF)

Adobe은 XFA PDF 양식이라는 용어를 사용하여 AEM Forms Designer에서 만든 대화형 및 동적 Forms을 참조합니다. Designer로 만드는 Forms 및 파일은 Adobe의 XFA(XML Forms Architecture)를 기반으로 합니다. 여러 가지 방식에서 XFA PDF 파일 형식은 기존 PDF 파일보다 HTML 파일에 더 가깝습니다. 예를 들어 다음 코드는 XFA PDF 파일에서 간단한 텍스트 개체가 어떻게 표시되는지 보여 줍니다.

![텍스트 필드](assets/text-field.JPG)

XFA Forms은 XML을 기반으로 합니다. 잘 구성되어 있고 유연한 포맷의 AEM Forms Server에서 기존의 PDF, PDF/A 및 HTML을 포함하여 디자이너 파일을 다른 형식으로 변환할 수 있습니다. 레이아웃 편집기에서 XML 소스 탭을 선택하여 Designer에서 Forms의 전체 XML 구조를 볼 수 있습니다. AEM Forms Designer에서 정적 및 동적 XFA Forms을 모두 만들 수 있습니다.

## 정적 PDF

정적 XFA PDF forms 레이아웃은 런타임 시 변경되지 않지만 사용자용으로 대화형 상태일 수 있습니다. 다음은 정적 XFA PDF forms의 몇 가지 이점이 있습니다.

* 정적 XFA PDF forms 레이아웃은 런타임 시 변경되지 않지만 사용자용으로 대화형 상태일 수 있습니다.
* 정적 Forms은 Acrobat의 주석 및 마크업 도구를 지원합니다.
* 정적 Forms을 사용하여 Acrobat 주석을 가져오고 내보낼 수 있습니다.
* 정적 Forms은 AEM Forms 서버에서 수행할 수 있는 기술인 글꼴 하위 설정을 지원합니다.
* 정적 Forms은 최신 브라우저와 함께 제공되는 기본 제공 PDF 뷰어를 사용하여 렌더링할 수 있습니다.

>[!NOTE]
>
> XDP를 Adobe 정적 PDF 양식으로 저장하여 AEM Forms 디자이너를 사용하여 정적 PDF을 만들 수 있습니다



### 다이내믹 Forms

Dynamic XFA PDF은 런타임 시 레이아웃을 변경할 수 있으므로 주석 달기 및 마크업 기능이 지원되지 않습니다. 그러나 동적 XFA PDF은 다음과 같은 이점을 제공합니다.

* 동적 양식은 양식의 레이아웃 및 페이지 매김을 변경하는 클라이언트측 스크립트를 지원합니다. 예를 들어, 동적 양식으로 저장하는 경우 Purchase Order.xdp가 확장 및 게시되어 무제한 양의 데이터를 수용할 수 있습니다
* 동적 양식은 런타임 시 양식의 모든 속성을 지원하는 반면 정적 양식은 하위 집합만 지원합니다

>[!NOTE]
>
> XDP를 Adobe Dynamic XML Form으로 저장하여 AEM Forms 디자이너를 사용하여 동적 pdf를 만들 수 있습니다

>[!NOTE]
>
> 최신 브라우저의 기본 제공 pdf 뷰어를 사용하여 동적 양식을 렌더링할 수 없습니다.

### PDF 파일(기존 PDF)

Certified Document는 PDF 문서 및 Forms 수신자에게 문서의 신뢰성 및 무결성을 추가로 제공합니다.

가장 널리 사용되는 PDF 형식은 기존 PDF 파일입니다. Acrobat 및 다양한 타사 도구 사용을 포함하여 기존 PDF 파일을 만드는 방법에는 여러 가지가 있습니다. Acrobat에서는 기존 PDF 파일을 만드는 다음 모든 방법을 제공합니다. Acrobat이 설치되어 있지 않으면 컴퓨터에 이러한 옵션이 표시되지 않을 수 있습니다.

* 데스크탑 애플리케이션의 인쇄 스트림을 캡처하여 다음을 수행합니다. 작성 응용 프로그램의 인쇄 명령을 선택하고 Adobe PDF 프린터 아이콘을 선택합니다. 문서의 인쇄 사본 대신 문서의 PDF 파일을 만듭니다
* Microsoft Office 애플리케이션에서 Acrobat PDFMaker 플러그인 사용: Acrobat을 설치하면 Adobe PDF 메뉴가 Microsoft Office 응용 프로그램에 추가되고, 아이콘이 Office 리본에 추가됩니다. 이러한 추가된 기능을 사용하여 Microsoft Office에서 직접 PDF 파일을 만들 수 있습니다
* Acrobat Distiller을 사용하여 Postscript 및 Encapsulated Postscript(EPS) 파일을 PDF으로 변환: Distiller은 일반적으로 인쇄 게시 및 Postscript 포맷에서 PDF 형식으로 변환해야 하는 기타 워크플로우에서 사용됩니다
* 후드 아래에서는 기존 PDF이 XFA PDF과 매우 다릅니다. XML 구조가 동일하지 않으며 파일의 인쇄 스트림을 캡처하여 만들기 때문에 기존 PDF은 정적 및 읽기 전용 파일입니다.

Certified Document는 PDF 문서 및 양식 수신자에게 신뢰성과 무결성을 추가로 제공합니다.

### Acrobat

Acrobat은 Adobe의 이전 대화형 양식 기술입니다. 이 수정 사항은 Acrobat 버전 3. Adobe: [Acrobat Forms API 참조](assets/FormsAPIReference.pdf)은 이 기술에 대한 기술 세부 사항을 제공하기 위해 2003년 5월에 업데이트되었습니다. 곡선은 다음 항목의 조합입니다.

* 양식의 정적 레이아웃 및 그래픽을 정의하는 기존 PDF.
* Adobe Acrobat 프로그램의 양식 도구를 사용하여 맨 위에 배치되는 대화형 양식 필드입니다. 이러한 양식 도구는 AEM Forms Designer에서 사용할 수 있는 기능의 일부이며,

### PDF/A(아카이브 PDF)

PDF/A(아카이브 PDF)은 기존의 PDF이 제공하는 문서 스토리지 이점을 바탕으로 장기 아카이빙을 강화하는 구체적인 세부 정보를 제공합니다. 기존의 PDF 파일 형식은 장기 문서 스토리지에 많은 이점을 제공합니다. PDF의 컴팩트한 특성을 사용하면 전송 및 공간 구성이 용이하며, 구조가 잘 구성되어 있어 강력한 인덱싱 및 검색 기능을 사용할 수 있습니다. 기존 PDF은 메타데이터에 대한 광범위한 지원을 제공하며 PDF은 다양한 컴퓨터 환경을 지원하는 오랜 역사를 가지고 있습니다.

PDF과 마찬가지로 PDF/A는 ISO 표준 사양입니다. AIIM(Association for Information and Image Management), NPES(National Printing Equipment Association), 미국 법원 행정관서가 AIIM(National Printing Equipment Association) 등이 공동으로 개발한 AIIM(Association for Information and Image Management), NPES(National Printing Equipment Association), 미국 법원행정관서가 공동으로 개발한 제품이다. PDF/A 사양은 장기 아카이브 형식을 제공하는 것이 목적이므로 많은 PDF 기능이 생략되어 파일을 자체 포함할 수 있습니다. 다음은 PDF/A 파일의 장기 재현성을 향상시키는 사양에 대한 몇 가지 주요 점입니다.

* 모든 컨텐츠는 파일에 포함되어야 하며, 하이퍼링크, 글꼴 또는 소프트웨어 프로그램과 같은 외부 소스에 종속될 수 없습니다.
* 모든 글꼴은 반드시 포함되어야 하며 전자 문서에 무제한 사용 라이센스가 있는 글꼴이어야 합니다.
* JavaScript가 허용되지 않습니다
* 투명도가 허용되지 않습니다
* 암호화가 허용되지 않습니다
* 오디오 및 비디오 컨텐츠가 허용되지 않습니다
* 색상 공백은 장치 독립적인 방식으로 정의해야 합니다
* 모든 메타데이터는 특정 표준을 따라야 합니다

### PDF/A 파일 보기

샘플 파일에 있는 두 파일은 동일한 Microsoft Word 파일에서 만들었습니다. 하나는 기존 PDF으로, 다른 하나는 PDF/A 파일로 만들었습니다. Acrobat Professional에서 다음 두 파일을 엽니다.

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

문서가 동일하게 표시되지만 PDF/A 파일은 맨 위에 파란색 막대가 있고 PDF/A 모드에서 이 문서를 보고 있음을 나타냅니다. 이 파란색 표시줄은 Acrobat의 문서 메시지 표시줄이며, 특정 유형의 PDF 파일을 열 때 표시됩니다.

![Pdf-img](assets/pdfa-message.png)

문서 메시지 모음에는 작업을 완료하는 데 도움이 되는 지침과 단추가 있습니다. 색상으로 구분되어 있으며, 특수 유형의 PDF(이 PDF/A 파일 등)와 인증된 디지털 서명 PDF을 열면 파란색 색상이 표시됩니다. PDF 검토에 참여 중인 경우 표시줄이 PDF forms의 경우 자주색으로 변경되고, 노란색으로 변경됩니다.

>[!NOTE]
>
> 편집 활성화를 클릭하면 이 문서를 PDF/A 규정 준칙에서 가져옵니다.
