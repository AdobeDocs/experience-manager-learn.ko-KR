---
title: GIT에 Symlink 추가
description: Dispatcher 구성에서 작업할 때 symlink를 추가하는 방법 및 위치에 대한 지침입니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 0%

---


# GIT에 Symlink 추가

[목차](./overview.md)

[&lt;- 이전: Dispatcher 상태 검사](./health-check.md)

AMS에서는 Dispatcher의 소스 코드가 들어 있고 개발 및 사용자 지정을 시작할 수 있는 준비가 되어 있는 미리 채워진 GIT 리포지토리를 가져옵니다.

첫 번째 `.vhost` 파일 또는 최상위 수준 `farm.any` 파일에서 심볼 링크를 만들어야 합니다 `available_*` 디렉토리 `enabled_*` 디렉토리. 적절한 링크 유형을 사용하는 것은 Cloud Manager 파이프라인을 통한 성공적인 배포 시 중요한 것입니다. 이 페이지에서는 이 작업을 수행하는 방법을 알 수 있습니다.

## Dispatcher 원형

AEM 개발자는 일반적으로 [AEM 원형](https://github.com/adobe/aem-project-archetype)

다음은 사용된 symlink를 확인할 수 있는 소스 코드 영역의 샘플입니다.

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

예: `/etc/httpd/conf.d/available_vhosts/` 디렉토리에 준비된 잠재적 `.vhost` 실행 중인 구성에 사용할 수 있는 파일입니다.

활성화됨 `.vhost` 파일이 상대 경로로 표시됩니다. `symlinks` 내부 `/etc/httpd/conf.d/enabled_vhosts/` 디렉토리.

## symlink 만들기

Apache 웹 서버가 대상 파일을 동일한 파일로 처리하도록 파일에 대한 심볼 링크를 사용합니다.  두 디렉토리에 있는 파일을 복제하지 않습니다.  대신 한 디렉토리(심볼 링크)에서 다른 디렉토리로의 바로 가기를 사용합니다.

배포된 구성이 Linux 호스트를 대상으로 한다는 것을 인식하십시오.  대상 시스템과 호환되지 않는 symlink를 만들면 오류가 발생하고 원치 않는 결과가 발생합니다.

워크스테이션이 Linux 시스템이 아닌 경우 GIT에 커밋할 수 있도록 이러한 링크를 제대로 만드는 데 사용할 명령이 무엇인지 궁금해질 수 있습니다.

> `TIP:` Apache 웹 서버의 로컬 복사본을 설치하고 다른 설치 베이스가 있는 경우 링크가 계속 작동하므로 상대 링크를 사용하는 것이 중요합니다.  절대 경로를 사용하는 경우 워크스테이션이나 다른 시스템이 동일한 정확한 디렉터리 구조와 일치해야 합니다.

### OSX / Linux

Symlink는 이러한 운영 체제에서 기본적으로 제공되며, 이러한 링크를 만드는 방법의 몇 가지 예는 다음과 같습니다.  자주 사용하는 터미널 애플리케이션을 열고 다음 예제 명령을 사용하여 링크를 만듭니다.

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

다음은 참조에 대해 채워진 명령 예입니다.

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

다음은 를 사용하여 파일을 나열하는 경우 링크의 예입니다 `ls` 명령:

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` MS Windows(더 나은, NTFS)가 심볼 링크를 지원하는 것으로 판명되었습니다. Windows Vista!

![mklink 명령의 도움말 출력을 보여 주는 Windows 명령 프롬프트 그림](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` symlink를 만들기 위한 mklink 명령을 실행하려면 관리자 권한이 필요합니다. 관리자 계정이라도 개발자 모드가 활성화되어 있지 않은 경우 명령 프롬프트 &quot;관리자&quot;를 실행해야 합니다
> <br/>부적절한 권한:
> ![사용 권한 때문에 실패한 명령을 표시하는 Windows 명령 프롬프트 그림](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>적절한 권한:
> ![관리자로 실행된 Windows 명령 프롬프트 그림](./assets/git-symlinks/windows-mklink-properpriv.png)

다음은 링크를 만드는 명령입니다.

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


다음은 참조에 대해 채워진 명령 예입니다.

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### 개발자 모드 ( Windows 10 )

에 를 넣을 때 [개발자 모드](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), Windows 10을 사용하면 개발 중인 앱을 보다 쉽게 테스트하고, Ubuntu Bash 셸 환경을 사용하고, 다양한 개발자 중심의 설정을 변경하는 등 다양한 작업을 수행할 수 있습니다.

Microsoft은 개발자 모드에 기능을 계속 추가하는 것 같습니다. 또는 이러한 기능 중 일부가 더 광범위한 채택에 도달하면 기본적으로 활성화되고 안정적이라고 간주됩니다(예: Creators Update를 사용하면 Ubuntu Bash Shell 환경에 더 이상 개발자 모드가 필요하지 않음).

symlink는 어떻습니까? 개발자 모드 ENABLED를 사용하면 symlink를 생성할 수 있도록 높은 권한이 있는 명령 프롬프트를 실행할 필요가 없습니다. 따라서 개발자 모드가 활성화되면 모든 사용자가 symlink를 만들 수 있습니다.

> 개발자 모드를 활성화한 후 변경 사항을 적용하려면 로그오프/로그온해야 합니다.

이제 관리자로 실행하지 않고 명령이 작동하는 것을 볼 수 있습니다

![개발자 모드가 활성화된 일반 사용자로 Windows 명령 프롬프트 그림 실행](./assets/git-symlinks/windows-mklink-devmode.png)

#### 대체/프로그래밍 방식 접근 방법

특정 사용자가 심볼 링크를 만들 수 있도록 허용하는 특정 정책이 → [심볼 링크 만들기(Windows 10) - Windows 보안 | Microsoft 문서](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO:
- 이 기능은 고객이 각 장치에서 개발자 모드를 수동으로 활성화하지 않고도 조직 내 모든 개발자(즉, Active Directory)에 대한 심볼 링크 생성을 프로그래밍 방식으로 허용하는 데 활용할 수 있습니다.
- 또한 이 정책은 개발자 모드를 제공하지 않는 이전 버전의 MS Windows에서 사용할 수 있어야 합니다.

아이콘:
- 이 정책은 관리자 그룹에 속하는 사용자에게 영향을 주지 않는 것 같습니다. 관리자는 향상된 권한으로 명령 프롬프트를 실행해야 합니다. 이상해요

> 로컬/그룹 정책 변경 사항을 적용하려면 사용자 로그오프/로그온이 필요합니다.

실행 `gpedit.msc`, 필요에 따라 사용자를 추가/변경합니다. 관리자는 기본적으로 여기에 있습니다

![조정 권한을 표시하는 그룹 정책 편집기 창](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### GIT에서 Symlink 활성화

Git은 core.symlink 옵션에 따라 symlink를 처리합니다

소스: [Git - Git 구성 설명서](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*core.symlink가 false이면 심볼 링크는 링크 텍스트가 포함된 작은 일반 파일로 체크 아웃됩니다. `git-update-index[1]` 및 `git-add[1]` 기록된 형식은 일반 파일로 변경되지 않습니다. 심볼 링크를 지원하지 않는 FAT와 같은 파일 시스템에 유용합니다.
기본값은 true이며, 단, `git-clone[1]` 또는 `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` 대부분의 경우 Git은 Windows가 symlink에 좋지 않다고 가정하고 이 값을 false로 설정합니다.*

Windows에서 Git의 동작은 여기에서 잘 설명합니다. 심볼 링크 ・ git-for-windows/git Wiki ・ GitHub

> `Info`: 위에 링크된 문서에 나열된 가설은 Windows에서 AEM Developer가 설정할 수 있는 것과, 특히 NTFS와 파일 symlink와 디렉토리 symlink만 있다는 사실만으로도 문제 없는 것 같습니다

좋은 소식이 있어요 [Windows용 Git 버전 2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) 설치 관리자에 [심볼 링크 지원을 활성화하는 명시적 옵션.](https://github.com/git-for-windows/git/issues/921)

> `Warning`: core.symlink 옵션은 리포지토리를 복제하는 동안 런타임 시 제공하거나 글로벌 구성으로 저장할 수 있습니다.

![symlink 지원을 보여주는 GIT 설치 프로그램 표시](./assets/git-symlinks/windows-git-install-symlink.png)

Windows용 Git은 전역 환경 설정을 `"C:\Program Files\Git\etc\gitconfig"` . 이러한 설정은 다른 Git 데스크탑 클라이언트 앱에서 간주하지 않을 수 있습니다.
여기에는 일부 개발자들이 Git 기본 클라이언트(예: Git Cmd, Git Bash)를 사용하지 않고 일부 Git 데스크탑 앱(예: GitHub Desktop, Atlassian Sourctree)에는 시스템 또는 포함된 Git을 사용할 수 있는 설정/기본값이 다를 수 있습니다

다음은 내부에 있는 것의 샘플입니다 `gitconfig` 파일

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

위의 설명서에서 Windows가 심볼 링크를 만드는 &quot;mklink&quot; 명령을 제공한다는 것을 보았습니다.

Git Bash 환경에서 작업하는 경우 표준 Bash 명령을 대신 사용할 수 있습니다 `ln -s` 그러나 다음과 같은 특별한 지침에 따라 접두사를 추가해야 합니다.

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### 요약

Microsoft Windows OS에서 Git이 symlink를 올바르게 처리하도록 하려면(적어도 현재 AEM Dispatcher 구성 기준 요소의 범위에 대해) 다음을 수행해야 합니다.

| 항목 | 최소 버전 / 구성 | 권장 버전/구성 |
|------|---------------------------------|-------------------------------------|
| 운영 체제 | Windows Vista 이상 | Windows 10 Creator 업데이트 이상 |
| 파일 시스템 | NTFS | NTFS |
| Windows 사용자의 symlink 처리 기능 | `"Create symbolic links"` 그룹/로컬 정책 `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Windows 10 개발자 모드 사용 |
| GIT | 기본 클라이언트 버전 1.5.3 | Native Client 버전 2.10.2 이상 |
| Git 구성 | `--core.symlinks=true` 명령줄에서 git 복제 수행 옵션 | Git 전역 구성<br/>`[core]`<br/>    symlink = true <br/> 기본 Git 클라이언트 구성 경로: `C:\Program Files\Git\etc\gitconfig` <br/>Git 데스크탑 클라이언트의 표준 위치: `%HOMEPATH%\.gitconfig` |

> `Note:` 로컬 리포지토리가 이미 있는 경우 원본에서 새로 고쳐야 합니다. 새 위치에 복제하고 커밋되지 않은 로컬 변경 내용을 새로 복제된 리포지토리에 수동으로 병합할 수 있습니다.