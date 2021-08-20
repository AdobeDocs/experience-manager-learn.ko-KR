---
title: Linux에 AEM Forms 설치
description: Linux 설치에서 작동하도록 AEM Forms용 32비트 라이브러리를 설치하는 방법을 알아봅니다.
feature: 적응형 양식
type: Tutorial
version: 6.4, 6.5
topic: 개발
role: Developer
level: Beginner
kt: 7593
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# 32비트 버전의 공유 라이브러리 설치

AEM FORMS. OSGi 또는 AEM Forms j2EE가 Linux에 배포된 경우 공유 라이브러리 집합의 32비트 버전이 설치 및 사용 가능한지 확인해야 합니다.  설명은 패키지 자체에서 나온 것입니다.

* expat(James Clark에서 작성한 XML 구문 분석을 위한 스트림 지향 XML 파서 C 라이브러리)
* fontconfig(시스템 내에서 글꼴을 찾아 응용 프로그램에서 지정한 요구 사항에 따라 선택할 수 있도록 설계된 글꼴 구성 및 사용자 지정 라이브러리)
* 다양한 플랫폼 및 환경에 대한 고급 글꼴 지원을 제공하기 위해 개발된 freetype(Font rendering engine) 글꼴 파일을 열고 관리할 수 있을 뿐만 아니라 개별 글리프를 효율적으로 로드, 힌트 및 렌더링할 수 있습니다. 글꼴 서버나 전체 텍스트 렌더링 라이브러리가 아닙니다.)
* glibc(GNU 시스템 및 GNU/Linux 시스템용 핵심 라이브러리 및 Linux를 커널로 사용하는 다른 많은 시스템)
* libcurl(클라이언트측 URL 전송 라이브러리)
* libICE(클라이언트 간 Exchange 라이브러리)
* libicu(강력하고 완전한 기능을 갖춘 유니코드 및 로케일 지원을 제공하는 라이브러리 - 유니코드용 국제 구성 요소). 이 라이브러리의 64비트 및 32비트 버전은 모두 필요합니다
* libSM(X11 세션 관리 라이브러리)
* libuuid(DCE 호환 Universally Unique Identifier Library - 로컬 시스템 외부에서 액세스할 수 있는 객체에 대한 고유 식별자를 생성하는 데 사용됨)
* libX11(X11 클라이언트 측 라이브러리)
* libXau(X11 Authorization Protocol - 클라이언트 액세스를 디스플레이에 제한하는 데 유용함)
* libxcb(X 프로토콜 C-언어 바인딩 - XCB)
* libXext(X11 프로토콜에 대한 일반 확장을 위한 라이브러리)
* libXinerama(X11 확장)를 통해 여러 디스플레이에서 데스크탑을 확장할 수 있습니다. 이 이름은 여러 프로젝터를 사용한 와이드스크린 영화 형식인 Cinerama에서 말장난입니다. libXinerama는 BrandR 확장과 연결하는 라이브러리입니다
* libXrandr(Xinerama 확장)은 현재 거의 사용되지 않으며, RandR 확장 기능으로 대체되었습니다.
* libXrender(X Rendering Extension 클라이언트 라이브러리)
nss-softokn-freebl(네트워크 보안 서비스용 자유 형식 라이브러리)
* zlib(범용, 무손실 데이터 압축 라이브러리)

Red Hat Enterprise Linux 6 이상에서 라이브러리의 32비트 에디션은 파일 확장명이 .686이고, 64비트 에디션은 .x86_64를 갖습니다. 예, expat.i686. RHEL 6 이전의 32비트 에디션에는 확장명이 .i386이었습니다. 32비트 버전을 설치하기 전에 최신 64비트 버전이 설치되어 있는지 확인하십시오. 64비트 버전의 라이브러리가 설치되어 있는 32비트 버전보다 오래된 경우 다음과 같은 오류가 발생합니다.

0m오류: 보호된 다중 라이브러리 버전: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Multilib 버전 문제를 발견했습니다.]

## 처음 설치

Red Hat Enterprise Linux에서 다음과 같이 Yum(YellowDog Update Modifier)를 사용하여 설치합니다.

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

## Symlink

또한 libcurl.so, libcrypto.so 및 libssl.so를 각각 최신 32비트 버전의 libcurl, libcrypto 및 libssl 라이브러리를 가리키는 symlink를 만들어야 합니다. /usr/lib/에서 파일을 찾을 수 있습니다.
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1c /usr/lib/libssl.so

## 기존 시스템 업데이트

업데이트 중에 x86_64와 i686 아키텍처 간에 다음과 같은 충돌이 발생할 수 있습니다.
오류: 트랜잭션 확인 오류:
glibc-2.28-72.el8.i686 파일의 /lib/ld-2.28.so가 패키지 glibc32-2.28-42.1.el8.x86_64 파일의 파일과 충돌합니다.

이 경우 다음과 같이 문제가 있는 패키지를 먼저 설치 해제합니다.
yum remove glibc32-2.28-42.1.el8.x86_64

모두 확인했으면 다음 출력에서 명령으로의 예를 들어 x86_64 및 i686 버전이 정확하게 동일해야 합니다.
yum info glibc

마지막 메타데이터 만료 확인: 0:41:33전, 2020년 1월 18일:37:08 오전 EST
설치된 패키지
이름 : 글라이치
버전 : 2.28
릴리스 : 72.el8
아키텍처 : i686
크기 : 13미터
소스 : glibc-2.28-72.el8.src.rpm
저장소 : @System
Repo : BaseOS
요약 : GNU libc 라이브러리
URL : http://www.gnu.org/software/glibc/
라이선스 : LGPLv2+ 및 LGPLv2+ 및 GPLv2+(예외 사항 포함) 및 BSD 및 Inner-Net 및 ISC 및 Public Domain 및 GFDL을 사용하는 GPLv2+ 및 GPLv2+
설명 : glibc 패키지에는 다음에 사용되는 표준 라이브러리가 포함되어 있습니다. 시스템의 여러 프로그램. 디스크 공간을 절약하기 위해 및 : 일반적인 시스템 코드는 다음과 같습니다. 한 곳에서 유지되며 프로그램 간에 공유됩니다. 이 특정 패키지 : 에는 다음과 같은 가장 중요한 공유 라이브러리 세트가 포함되어 있습니다. 표준 C : 라이브러리 및 표준 수학 라이브러리. 이 두 라이브러리가 없으면 : Linux 시스템이 작동하지 않습니다.

이름 : 글라이치
버전 : 2.28
릴리스 : 72.el8
아키텍처 : x86_64
크기 : 15미터
소스 : glibc-2.28-72.el8.src.rpm
저장소 : @System
Repo : BaseOS
요약 : GNU libc 라이브러리
URL : http://www.gnu.org/software/glibc/
라이선스 : LGPLv2+ 및 LGPLv2+ 및 GPLv2+(예외 사항 포함) 및 BSD 및 Inner-Net 및 ISC 및 Public Domain 및 GFDL을 사용하는 GPLv2+ 및 GPLv2+
설명 : glibc 패키지에는 다음에 사용되는 표준 라이브러리가 포함되어 있습니다. 시스템의 여러 프로그램. 디스크 공간을 절약하기 위해 및 : 일반적인 시스템 코드는 다음과 같습니다. 한 곳에서 유지되며 프로그램 간에 공유됩니다. 이 특정 패키지 : 에는 다음과 같은 가장 중요한 공유 라이브러리 세트가 포함되어 있습니다. 표준 C : 라이브러리 및 표준 수학 라이브러리. 이 두 라이브러리가 없으면 : Linux 시스템이 작동하지 않습니다.

## 몇 가지 유용한 yum 명령

yum list 설치
yum search [part_of_package_name]
yum whatoffers [package_name]
yum 설치 [package_name]
yum reinstall [package_name]
yum info [package_name]
yum 배포 [package_name]
yum 제거 [package_name]
yum check update [package_name]
yum 업데이트 [package_name]
