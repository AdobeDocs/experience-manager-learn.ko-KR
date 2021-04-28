---
title: Linux에서 AEM Forms 설치
description: Linux 설치 시 AEM Forms용 32비트 라이브러리를 설치하는 방법을 알아봅니다.
feature: 적응형 양식
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: 개발
role: Developer
level: Beginner
kt: 7593
translation-type: tm+mt
source-git-commit: 9583006352ca6a20a763c9d5ec7ba15c3791e897
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# 32비트 버전의 공유 라이브러리 설치

AEM FORMS. OSGi 또는 AEM Forms j2EE가 Linux에 배포될 때 32비트 버전의 공유 라이브러리를 설치하고 사용할 수 있어야 합니다.  설명은 패키지 자체에서 나온 것입니다.

* expat(James Clark이 작성한 XML 파싱용 스트림 지향 XML 파서 C 라이브러리)
* fontconfig (시스템 내에서 글꼴을 찾아 애플리케이션에서 지정한 요구 사항에 따라 해당 글꼴을 선택하기 위해 고안된 글꼴 구성 및 사용자 정의 라이브러리)
* freetype(다양한 플랫폼 및 환경에 대한 고급 글꼴 지원을 제공하기 위해 개발된 글꼴 렌더링 엔진) 글꼴 파일을 열고 관리할 수 있을 뿐만 아니라 개별 글리프를 효율적으로 로드하고 힌트 및 렌더링할 수 있습니다. 글꼴 서버나 전체 텍스트 렌더링 라이브러리가 아닙니다.)
* glibc(GNU 시스템 및 GNU/Linux 시스템을 위한 핵심 라이브러리 및 Linux를 커널로 사용하는 다른 많은 시스템)
* libcurl(클라이언트측 URL 전송 라이브러리)
* libICE(클라이언트 간 교환 라이브러리)
* libicu(강력하고 완벽한 기능을 갖춘 유니코드 및 로케일 지원을 제공하는 라이브러리 - 유니코드에 대한 국제 구성 요소). 이 라이브러리의 64비트 및 32비트 에디션 모두 필요합니다.
* libSM(X11 세션 관리 라이브러리)
* libuuid(DCE 호환 범용 고유 식별자 라이브러리 - 로컬 시스템 외부에서 액세스할 수 있는 개체의 고유 식별자를 생성하는 데 사용됨)
* libX11(X11 클라이언트측 라이브러리)
* libXau(X11 인증 프로토콜 - 디스플레이에 대한 클라이언트 액세스를 제한하는 데 유용함)
* libxcb(X 프로토콜 C 언어 바인딩 - XCB)
* libXext(X11 프로토콜에 대한 공통 확장을 위한 라이브러리)
* libXinerama(X11 확장)는 여러 디스플레이에 데스크탑 확장을 지원합니다. 이름은 여러 프로젝터를 사용하는 와이드스크린 동영상 포맷인 Cinerama에 대한 농담입니다. libXinerama는 RandR 확장 기능과 상호 작용하는 라이브러리입니다.)
* libXrandr(Xinerama 확장명은 요즘 많이 사용되지 않음 - RandR 확장으로 대체됨)
* libXrender(X 렌더링 확장 클라이언트 라이브러리)
nss-softokn-freebl(네트워크 보안 서비스용 무료 라이브러리)
* zlib(범용, 특허 없는 손실 없는 데이터 압축 라이브러리)

Red Hat Enterprise Linux 6부터 32비트 버전의 라이브러리에는 파일 이름 확장명이 .686이고 64비트 에디션의 파일 이름은 .x86_64가 됩니다. 예: expat.i686. RHEL 6 이전의 32비트 에디션에는 .i386 확장명이 있습니다. 32비트 에디션을 설치하기 전에 최신 64비트 에디션이 설치되어 있는지 확인하십시오. 64비트 버전의 라이브러리가 설치되는 32비트 버전보다 이전 버전이면 다음과 같은 오류가 표시됩니다.

0m오류:보호된 다중 라이브러리 버전:libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0m오류:다중 라이브러리 버전 문제가 발견되었습니다.]

## 최초 설치

Red Hat Enterprise Linux에서 YUM(YellowDog Update Modifier)를 사용하여 다음과 같이 설치합니다.

1. yum install exat.i686
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
13. yum livxcb.i686 설치
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## 심링크

또한 libcurl.so, libcrypto.so 및 libssl.so를 각각 최신 32비트 버전의 libcurl, libcrypto 및 libssl 라이브러리를 가리키는 symlinks를 만들어야 합니다. /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## 기존 시스템 업데이트

다음과 같이 업데이트 중에 x86_64와 i686 아키텍처 간에 충돌이 발생할 수 있습니다.
오류:트랜잭션 확인 오류:
glibc-2.28-72.el8.i686 파일의 /lib/ld-2.28-42.1.el8.x86_64 패키지의 파일 충돌

이 경우 다음과 같이 문제가 있는 패키지를 먼저 설치 해제합니다.
yum remove glibc32-2.28-42.1.el8.x86_64

이 모든 작업을 완료하면 다음과 같이 x86_64 및 i686 버전을 정확하게 동일하게 만들 수 있습니다. 예: 이 출력에서 명령까지
yum info glibc

마지막 메타데이터 만료 확인:0:41:33 전 2020년 1월 18일 오전 11:37:08
설치된 패키지
이름:글리시
버전:2.28
릴리스:72.el8
아키텍처:i686
크기 :13M
출처:glibc-2.28-72.el8.src.rpm
저장소:@System
보낸 사람:BaseOS
요약:GNU 라이브 라이브러리
URL :http://www.gnu.org/software/glibc/
라이선스:예외적인 LGPLv2+ 및 LGPLv2+, 예외적인 GPLv2+ 및 GPLv2+, BSD 및 Inner-Net, ISC 및 Public Domain, GFDL
설명:glibc 패키지에는 다음 사용자가 사용하는 표준 라이브러리가 포함되어 있습니다.시스템에 있는 여러 프로그램. 디스크 공간을 절약하려면 다음을 수행합니다.일반적인 시스템 코드는 다음과 같습니다.중앙에서 관리하고 프로그램 간에 공유할 수 있습니다. 이 특정 패키지:공유 라이브러리의 가장 중요한 세트를 포함합니다.표준 C :라이브러리 및 표준 수학 라이브러리 두 개의 라이브러리가 없는 경우, a:Linux 시스템이 작동하지 않습니다.

이름:글리시
버전:2.28
릴리스:72.el8
아키텍처:x86_64
크기 :15M
출처:glibc-2.28-72.el8.src.rpm
저장소:@System
보낸 사람:BaseOS
요약:GNU 라이브 라이브러리
URL :http://www.gnu.org/software/glibc/
라이선스:예외적인 LGPLv2+ 및 LGPLv2+, 예외적인 GPLv2+ 및 GPLv2+, BSD 및 Inner-Net, ISC 및 Public Domain, GFDL
설명:glibc 패키지에는 다음 사용자가 사용하는 표준 라이브러리가 포함되어 있습니다.시스템에 있는 여러 프로그램. 디스크 공간을 절약하려면 다음을 수행합니다.일반적인 시스템 코드는 다음과 같습니다.중앙에서 관리하고 프로그램 간에 공유할 수 있습니다. 이 특정 패키지:공유 라이브러리의 가장 중요한 세트를 포함합니다.표준 C :라이브러리 및 표준 수학 라이브러리 두 개의 라이브러리가 없는 경우, a:Linux 시스템이 작동하지 않습니다.

## 유용한 yum 명령

yum 목록이 설치되었습니다.
yum search [part_of_package_name]
yum whatproes [package_name]
yum install [package_name]
yum reinstall [package_name]
yum info [package_name]
yum 고갈목록 [package_name]
yum remove [package_name]
yum check update [package_name]
yum update [package_name]
