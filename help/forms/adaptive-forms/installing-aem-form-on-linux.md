---
title: Linux에 AEM Forms 설치
description: AEM Forms이 Linux 설치에서 작동하도록 32비트 라이브러리를 설치하는 방법을 알아봅니다.
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
duration: 204
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 0%

---

# 32비트 버전의 공유 라이브러리 설치

AEM Forms OSGi 또는 AEM Forms j2EE가 Linux에 배포된 경우 32비트 버전의 공유 라이브러리 세트가 설치되고 사용 가능한지 확인해야 합니다.  설명은 패키지 자체에서 가져온 것입니다.

* expat(James Clark이 작성한 XML 구문 분석용 스트림 지향 XML 파서 C 라이브러리)
* fontconfig(시스템 내에서 글꼴을 찾고 애플리케이션에서 지정한 요구 사항에 따라 글꼴을 선택하도록 설계된 글꼴 구성 및 사용자 지정 라이브러리)
* 다양한 플랫폼 및 환경에 대한 고급 글꼴 지원을 제공하기 위해 개발된 Freetype(Font rendering engine). 글꼴 파일을 열고 관리할 수 있을 뿐만 아니라 개별 글리프를 효율적으로 로드, 힌트 및 렌더링할 수 있습니다. 글꼴 서버나 전체 텍스트 렌더링 라이브러리가 아닙니다.
* glibc(GNU 시스템 및 GNU/Linux 시스템뿐만 아니라 Linux를 커널로 사용하는 다른 많은 시스템용 핵심 라이브러리)
* libcurl(클라이언트측 URL 전송 라이브러리)
* libICE(클라이언트 간 교환 라이브러리)
* libicu(모든 기능을 갖춘 강력한 유니코드 및 로케일 지원을 제공하는 라이브러리 - 유니코드를 위한 국제 구성 요소). 이 라이브러리의 64비트 및 32비트 버전이 모두 필요합니다.
* libSM (X11 세션 관리 라이브러리)
* libuuid(DCE 호환 Universally Unique Identifier 라이브러리 - 로컬 시스템 외부에서 액세스할 수 있는 개체에 대한 고유 식별자를 생성하는 데 사용됨)
* libX11(X11 클라이언트측 라이브러리)
* libXau (X11 Authorization Protocol - 클라이언트의 디스플레이 액세스를 제한하는 데 유용함)
* libxcb(X 프로토콜 C 언어 바인딩 - XCB)
* libXext(X11 프로토콜에 대한 일반적인 확장 라이브러리)
* 여러 디스플레이에서 데스크탑을 확장할 수 있도록 지원하는 libXinerama(X11 확장) 이 이름은 여러 대의 프로젝터를 사용했던 와이드스크린 영화 형식인 Cinerama의 펜입니다. libXinerama는 RandR 확장과 상호 작용하는 라이브러리입니다.)
* libXrandr(Xinerama 확장 프로그램은 현재 대부분 사용되지 않으며 RandR 확장 프로그램으로 대체되었습니다.)
* libXrender(X Rendering Extension 클라이언트 라이브러리)
nss-softokn-freebl (네트워크 보안 서비스용 Freebl 라이브러리)
* zlib(범용, 무특허, 무손실 데이터 압축 라이브러리)

Red Hat Enterprise Linux 6부터 32비트 버전의 라이브러리는 파일 이름 확장명이 .686이고 64비트 버전은 .x86_64입니다. 예: expat.i686. RHEL 6 이전에는 32비트 버전의 확장명이 .i386이었습니다. 32비트 버전을 설치하기 전에 최신 64비트 버전이 설치되어 있는지 확인하십시오. 라이브러리의 64비트 버전이 설치 중인 32비트 버전보다 오래된 경우 다음과 같은 오류가 발생합니다.

0mError: 보호된 multilib 버전: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Multilib 버전 문제가 발견되었습니다.]

## 처음 설치

Red Hat Enterprise Linux에서 YellowDog Update Modifier(YUM)를 사용하여 다음과 같이 설치합니다.

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicu
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## 심볼릭 링크

또한 libcurl, libcrypto.so 및 libssl.so 심볼릭 링크를 각각 만들어 libcurl, libcrypto 및 libssl 라이브러리의 최신 32비트 버전을 가리켜야 합니다. /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## 기존 시스템에 대한 업데이트

다음과 같이 업데이트 중에 x86_64와 i686 아키텍처 간에 충돌이 발생할 수 있습니다.
오류: 거래 확인 오류:
glibc-2.28-72.el8.i686 설치에서 /lib/ld-2.28.so 파일이 glibc32-2.28-42.1.el8.x86_64 패키지의 파일과 충돌합니다.

이 문제가 발생하면 먼저 다음 경우와 같이 문제가 되는 패키지를 설치 해제합니다.
yum 제거 glibc32-2.28-42.1.el8.x86_64

이 모든 작업을 수행하려면 예를 들어 이 출력에서 명령까지 x86_64와 i686 버전이 정확히 동일해야 합니다.
yum info glibc

마지막 메타데이터 만료 확인: 0:41:33 전, 토 18 일 2020년 1월 11:37:08 AM EST.
설치된 패키지
이름 : glibc
버전 : 2.28
릴리스 : 72.el8
아키텍처 : i686
크기 : 13M
Source : glibc-2.28-72.el8.src.rpm
저장소 : @System
리포지토리에서: BaseOS
요약 : GNU libc 라이브러리
URL : http://www.gnu.org/software/glibc/
라이선스 : 예외가 있는 LGPLv2+ 및 LGPLv2+, 예외가 있는 GPLv2+ 및 GPLv2+, 예외와 BSD 및 Inner-Net 및 ISC와 Public Domain 및 GFDL
설명 : glibc 패키지는에서 사용하는 표준 라이브러리 를 포함합니다. 시스템의 여러 프로그램. 디스크 공간 및 : 메모리를 절약하고 업그레이드를 쉽게 할 수 있도록 공통 시스템 코드가 : 한 곳에 유지되며 프로그램 간에 공유됩니다. 이 특정 패키지 : 가장 중요한 공유 라이브러리 세트( 표준 C : 라이브러리 및 표준 수학 라이브러리)가 포함되어 있습니다. 이 두 라이브러리가 없으면 : Linux 시스템이 작동하지 않습니다.

이름 : glibc
버전 : 2.28
릴리스 : 72.el8
아키텍처 : x86_64
크기 : 15M
Source : glibc-2.28-72.el8.src.rpm
저장소 : @System
리포지토리에서: BaseOS
요약 : GNU libc 라이브러리
URL : http://www.gnu.org/software/glibc/
라이선스 : 예외가 있는 LGPLv2+ 및 LGPLv2+, 예외가 있는 GPLv2+ 및 GPLv2+, 예외와 BSD 및 Inner-Net 및 ISC와 Public Domain 및 GFDL
설명 : glibc 패키지는에서 사용하는 표준 라이브러리 를 포함합니다. 시스템의 여러 프로그램. 디스크 공간 및 : 메모리를 절약하고 업그레이드를 쉽게 할 수 있도록 공통 시스템 코드가 : 한 곳에 유지되며 프로그램 간에 공유됩니다. 이 특정 패키지 : 가장 중요한 공유 라이브러리 세트( 표준 C : 라이브러리 및 표준 수학 라이브러리)가 포함되어 있습니다. 이 두 라이브러리가 없으면 : Linux 시스템이 작동하지 않습니다.

## 몇 가지 유용한 yum 명령

yum 목록 설치됨
yum 검색 [part_of_package_name]
yum whats 제공 [package_name]
yum 설치 [package_name]
yum 다시 설치 [package_name]
yum 정보 [package_name]
yum deplist [package_name]
yum 제거 [package_name]
yum 확인-업데이트 [package_name]
yum 업데이트 [package_name]
