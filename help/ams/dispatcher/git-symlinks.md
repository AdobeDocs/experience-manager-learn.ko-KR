---
title: GIT에 심볼릭 링크를 올바르게 추가
description: Dispatcher 구성에서 작업할 때 심볼릭 링크를 추가하는 방법과 위치에 대한 지침입니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 6e751586-e92e-482d-83ce-6fcae4c1102c
duration: 422
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1231'
ht-degree: 0%

---

# GIT에 심볼릭 링크 추가

[목차](./overview.md)

[&lt;- 이전: Dispatcher 상태 확인](./health-check.md)

AMS에서는 Dispatcher의 소스 코드가 익은 상태로 개발 및 사용자 지정을 시작할 수 있는 준비가 된 사전 채워진 GIT 저장소를 제공합니다.

첫 번째 을(를) 만든 후 `.vhost` 파일 또는 최상위 수준 `farm.any` 파일에서 심볼 링크를 만들어야 합니다. `available_*` 디렉토리 대상: `enabled_*` 디렉토리. 적절한 링크 유형을 사용하면 Cloud Manager 파이프라인을 통해 성공적으로 배포할 수 있습니다. 이 페이지는 이 작업을 수행하는 방법을 이해하는 데 도움이 됩니다.

## Dispatcher 원형

AEM 개발자는 일반적으로 다음 위치에서 프로젝트를 시작합니다. [AEM Archetype](https://github.com/adobe/aem-project-archetype)

다음은 사용된 심볼릭 링크를 볼 수 있는 소스 코드 영역의 샘플입니다.

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

예를 들어 `/etc/httpd/conf.d/available_vhosts/` 디렉터리에 준비된 가능성이 있습니다. `.vhost` 실행 중인 구성에서 사용할 수 있는 파일입니다.

활성화됨 `.vhost` 파일이 상대 경로로 표시됩니다. `symlinks` 의 내부 `/etc/httpd/conf.d/enabled_vhosts/` 디렉토리.

## 심볼릭 링크 만들기

파일에 대한 심볼 링크를 사용하므로 Apache 웹 서버는 대상 파일을 동일한 파일로 처리합니다.  두 디렉터리에 있는 파일을 복제하지 않습니다.  대신 한 디렉토리(심볼 링크)에서 다른 디렉토리로 바로 가기만 하면 됩니다.

배포된 구성이 Linux 호스트를 대상으로 합니다.  대상 시스템과 호환되지 않는 symlink를 만들면 실패와 원치 않는 결과가 발생합니다.

워크스테이션이 Linux 시스템이 아닌 경우 이러한 링크를 제대로 만드는 데 어떤 명령을 사용해야 GIT에 커밋할 수 있는지 궁금할 것입니다.

> `TIP:` Apache 웹 서버의 로컬 복사본을 설치하고 다른 설치 기반을 둔 경우 링크가 여전히 작동하므로 상대 링크를 사용하는 것이 중요합니다.  절대 경로를 사용하는 경우 워크스테이션이나 다른 시스템은 동일한 정확한 디렉토리 구조를 사용해야 합니다.

### OSX / Linux

심볼릭 링크는 이러한 운영 체제를 기반으로 하며, 이러한 링크를 만드는 방법의 몇 가지 예를 소개합니다.  즐겨 찾는 터미널 응용 프로그램을 열고 다음 예제 명령을 사용하여 링크를 만듭니다.

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

다음은 참조용으로 채워진 명령 예입니다.

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

다음은 를 사용하여 파일을 나열하는 경우의 링크 예입니다. `ls` 명령:

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` MS Windows(더 나은, NTFS)는 다음 이유로 심볼 링크를 지원합니다. Windows Vista!

![mklink 명령의 도움말 출력을 보여 주는 Windows 명령 프롬프트 그림](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` symlink를 만들기 위한 mklink 명령을 실행하려면 관리자 권한이 필요합니다. 관리자 계정이더라도 개발자 모드를 활성화하지 않은 경우 &quot;관리자로&quot; 명령 프롬프트를 실행해야 합니다
> <br/>부적절한 권한:
> ![권한으로 인해 실패한 명령을 보여 주는 Windows 명령 프롬프트 그림](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>적절한 권한:
> ![관리자로 실행된 Windows 명령 프롬프트 그림](./assets/git-symlinks/windows-mklink-properpriv.png)

다음은 링크를 만드는 명령입니다.

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


다음은 참조용으로 채워진 명령 예입니다.

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### 개발자 모드( Windows 10 )

에 넣었을 때 [개발자 모드](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), Windows 10을 사용하면 개발 중인 앱을 보다 쉽게 테스트하고, Ubuntu Bash 셸 환경을 사용하고, 다양한 개발자 중심 설정을 변경하는 등의 작업을 수행할 수 있습니다.

Microsoft은 기능을 개발자 모드에 계속 추가하거나 일부 기능이 더 널리 채택되어 안정적이라고 간주되면 기본적으로 사용할 수 있게 하는 것 같습니다(예: 크리에이터 업데이트 사용 시 Ubuntu Bash Shell 환경에서는 더 이상 개발자 모드가 필요하지 않음).

심볼릭 링크는요? 개발자 모드가 활성화되면 심볼릭 링크를 만들 수 있도록 높은 권한으로 명령 프롬프트를 실행할 필요가 없습니다. 따라서 개발자 모드가 활성화되면 모든 사용자가 심볼릭 링크를 만들 수 있습니다.

> 개발자 모드를 활성화한 후 변경 사항을 적용하려면 사용자가 로그오프/로그온해야 합니다.

이제 관리자로 를 실행하지 않고도 명령이 작동하는지 확인할 수 있습니다

![개발자 모드가 활성화된 일반 사용자로 실행되는 Windows 명령 프롬프트 그림](./assets/git-symlinks/windows-mklink-devmode.png)

#### 대안/프로그래밍 방식 접근 방식

특정 사용자가 심볼 링크를 만들 수 있도록 허용하는 특정 정책이 → [심볼 링크 만들기(Windows 10) - Windows 보안 | Microsoft 문서](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO:
- 이 기능은 고객이 각 장치에서 개발자 모드를 수동으로 활성화할 필요 없이 조직(즉, Active Directory) 내의 모든 개발자에게 심볼 링크를 만드는 것을 프로그래밍 방식으로 허용하도록 활용할 수 있습니다.
- 또한 개발자 모드를 제공하지 않는 이전 버전의 MS Windows에서 이 정책을 사용할 수 있어야 합니다.

CON:
- 이 정책은 Administrators 그룹에 속한 사용자에게는 영향을 주지 않습니다. 관리자는 높은 권한으로 명령 프롬프트를 실행해야 합니다. 이상해요

> 로컬/그룹 정책에 대한 변경 사항을 적용하려면 사용자 로그오프/로그온이 필요합니다.

실행 `gpedit.msc`, 필요에 따라 사용자를 추가/변경합니다. 관리자는 기본적으로 이 곳에 있습니다

![조정 권한을 보여 주는 그룹 정책 편집기 창](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### GIT에서 심볼릭 링크 활성화

Git은 core.symlinks 옵션에 따라 심볼릭 링크를 처리합니다

소스: [Git - git-config 설명서](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*core.symlinks가 false이면 심볼 링크는 링크 텍스트가 포함된 작은 일반 파일로 체크 아웃됩니다. `git-update-index[1]` 및 `git-add[1]` 은(는) 기록된 유형을 일반 파일로 변경하지 않습니다. 심볼 링크를 지원하지 않는 FAT와 같은 파일 시스템에 유용합니다.
기본값은 true입니다. 단, `git-clone[1]` 또는 `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` 대부분의 경우 Git은 Windows가 심볼릭 링크에 좋지 않다고 가정하고 이를 false로 설정합니다.*

Windows에서 Git의 동작은 여기에 잘 설명되어 있습니다. 심볼 링크 · git-for-windows/git Wiki · GitHub

> `Info`: 위에 링크된 설명서에 나열된 가정은 Windows(특히 NTFS)에서 AEM Developer의 설정이 가능하고 파일 심볼릭 링크와 디렉터리 심볼릭 링크만 있는 경우 문제 없는 것 같습니다

좋은 소식이 있습니다. [Windows용 Git 버전 2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) 설치 관리자에 [심볼 링크 지원을 활성화하는 명시적 옵션.](https://github.com/git-for-windows/git/issues/921)

> `Warning`: core.symlink 옵션은 저장소를 복제하는 동안 런타임에 제공하거나, 전역 구성으로 저장할 수 있습니다.

![심볼릭 링크에 대한 지원을 보여 주는 GIT 설치 관리자 표시](./assets/git-symlinks/windows-git-install-symlink.png)

Windows용 Git은에 전역 환경 설정을 저장합니다. `"C:\Program Files\Git\etc\gitconfig"` . 이러한 설정은 다른 Git 데스크탑 클라이언트 앱에서는 고려되지 않을 수 있습니다.
다음은 모든 개발자가 Git 기본 클라이언트(예: Git Cmd, Git Bash)를 사용하는 것은 아니며, 일부 Git 데스크탑 앱(예: GitHub Desktop, Atlassian Sourcetree)은 시스템 또는 임베드된 Git을 사용하기 위해 다른 설정/기본값을 가질 수 있습니다

다음은 내부에 있는 것의 샘플입니다. `gitconfig` 파일

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Git 명령줄 팁

새 심볼 링크를 만들어야 하는 시나리오가 있을 수 있습니다(예: 새 vhost 또는 새 팜 추가).

위의 설명서에서 Windows는 심볼 링크를 만들기 위한 &quot;mklink&quot; 명령을 제공합니다.

Git Bash 환경에서 작업하는 경우 대신 표준 Bash 명령을 사용할 수 있습니다 `ln -s` 그러나 여기에 예시와 같은 특수 지침이 접두사로 사용되어야 합니다.

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### 요약

Microsoft Windows OS에서 Git 처리 심볼릭 링크가 올바르게(적어도 현재 AEM Dispatcher 구성 기준의 범위에 대해) 제공되려면 다음이 필요합니다.

| 항목 | 최소 버전/구성 | 권장 버전/구성 |
|------|---------------------------------|-------------------------------------|
| 운영 체제 | Windows Vista 이상 | Windows 10 Creator Update 이상 |
| 파일 시스템 | NTFS | NTFS |
| Windows 사용자의 심볼릭 링크를 처리하는 기능 | `"Create symbolic links"` 그룹/로컬 정책 `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Windows 10 개발자 모드 사용 |
| GIT | 기본 클라이언트 버전 1.5.3 | Native Client 버전 2.10.2 이상 |
| Git 구성 | `--core.symlinks=true` 명령줄에서 git 클론을 수행할 때 옵션 | Git 글로벌 구성<br/>`[core]`<br/>    symlinks = true <br/> 기본 Git 클라이언트 구성 경로: `C:\Program Files\Git\etc\gitconfig` <br/>Git 데스크톱 클라이언트의 표준 위치: `%HOMEPATH%\.gitconfig` |

> `Note:` 이미 로컬 저장소가 있는 경우 원본에서 새로 복제해야 합니다. 새 위치에 복제하고 커밋되지 않은/준비되지 않은 로컬 변경 내용을 새로 복제된 저장소에 수동으로 병합할 수 있습니다.
