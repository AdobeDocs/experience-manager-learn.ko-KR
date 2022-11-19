---
title: AMS Dispatcher 기본 파일 레이아웃
description: 기본 Apache 및 Dispatcher 파일 레이아웃을 파악합니다.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1163'
ht-degree: 1%

---


# 기본 파일 레이아웃

[목차](./overview.md)

[&lt;- 이전: &quot;디스패처&quot;란?](./what-is-the-dispatcher.md)

이 문서에서는 AMS 표준 구성 파일 세트와 이 구성 표준의 이면에 대한 생각을 설명합니다

## 기본 Enterprise Linux 폴더 구조

AMS에서 기본 설치는 Enterprise Linux를 기본 운영 체제로 사용합니다. Apache 웹 서버를 설치할 때 기본 설치 파일 세트가 있습니다. yum 저장소에서 제공하는 기본 RPM을 설치하여 설치하는 기본 파일은 다음과 같습니다

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

설치 설계/구조를 따르고 준수하면 다음과 같은 이점이 있습니다.

- 예측 가능한 레이아웃을 쉽게 지원
- 이전에 Enterprise Linux HTTPD 설치 작업을 수행한 모든 사용자에게 자동으로 친숙합니다.
- 충돌이나 수동 조정 없이 운영 체제에서 완전히 지원하는 패치 작업 주기를 허용합니다
- 레이블이 잘못 지정된 파일 컨텍스트의 SELinux 위반을 방지합니다.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b>
Adobe Managed Services 서버 이미지에는 일반적으로 작은 운영 체제 루트 드라이브가 있습니다.  일반적으로 '/mnt'에 마운트되는 별도의 볼륨에 데이터를 넣습니다. 그런 다음 다음 기본 디렉토리의 기본값 대신 해당 볼륨을 사용합니다

`DocumentRoot`
- 기본값:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- 기본값: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

이전 디렉토리와 새 디렉토리는 혼동하지 않도록 원래 마운트 지점에 다시 매핑됩니다.
별도의 볼륨을 사용하는 것은 중요하지 않지만 메모할 가치가 있습니다
</div>

## AMS 추가 기능

AMS는 Apache Web Server의 기본 설치에 추가됩니다.

### 문서 루트

AMS 기본 문서 루트:
- 작성자:
   - `/mnt/var/www/author/`
- 게시:
   - `/mnt/var/www/html/`
- Catch-All 및 상태 점검 유지 관리
   - `/mnt/var/www/default/`

### 스테이징 및 활성화된 VirtualHost 디렉토리

다음 디렉토리에서는 파일에 대해 작업할 수 있는 스테이징 영역이 있는 구성 파일을 작성하고 준비가 되었을 때만 활성화할 수 있습니다.
- `/etc/httpd/conf.d/available_vhosts/`
   - 이 폴더는 `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - 를 사용할 준비가 되면 `.vhost` 파일, 내부 `available_vhosts` 폴더 symlink는 `enabled_vhosts` directory

### 추가 `conf.d` 디렉토리

Apache 구성에서 일반적인 추가 항목이 있으며 Adobe에서는 이러한 파일을 한 디렉토리에 모두 포함하지 않고 명확하게 구분할 수 있도록 하위 디렉토리를 만들었습니다.

#### Rewrites 디렉토리

이 디렉토리는 `_rewrite.rules` Apache 웹 서버를 사용하는 일반적인 RewriteRuleSync를 포함하는 파일을 만듭니다. [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) 모듈

- `/etc/httpd/conf.d/rewrites/`

#### Whitelists 디렉토리

이 디렉토리는 `_whitelist.rules` 일반적인 `IP Allow` 또는 `Require IP`Apache 웹 서버를 사용하는 구문 [액세스 제어](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### Variables 디렉토리

이 디렉토리는 `.vars` 구성 파일에서 사용할 수 있는 변수가 포함된 파일을 만듭니다

- `/etc/httpd/conf.d/variables/`

### Dispatcher 모듈별 구성 디렉토리

Apache Web Server는 확장성이 뛰어나고 모듈에 많은 구성 파일이 있을 때 기본 구성 디렉토리를 클러스터링하는 대신 설치 기본 디렉토리 아래에 고유한 구성 디렉토리를 만드는 것이 가장 좋습니다.

Adobe에서는 모범 사례를 따르며 자체 솔루션을 만들었습니다

#### 모듈 구성 파일 디렉토리

- `/etc/httpd/conf.dispatcher.d/`

#### 스테이징 및 활성화된 팜

다음 디렉토리에서는 파일에 대해 작업할 수 있는 스테이징 영역이 있는 구성 파일을 작성하고 준비가 되었을 때만 활성화할 수 있습니다.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 이 폴더는 모든 `/myfarm {` 라는 파일 `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 팜 파일을 사용할 준비가 되면 available_farms 폴더 내부에 enabled_farms 디렉토리에 대한 상대 경로를 사용하여 symlink로 연결합니다

### 추가 `conf.dispatcher.d` 디렉토리

Dispatcher 팜 파일 구성의 하위 섹션인 추가 항목이 있으며 한 디렉토리에 모든 파일을 포함하지 않고 파일을 깔끔하게 구분할 수 있도록 해주는 하위 디렉토리를 만들었습니다

#### 캐시 디렉토리

이 디렉토리에는 `_cache.any`, `_invalidate.any` 모듈이 무효화 규칙 구문과 AEM에서 가져오는 캐싱 요소를 처리하는 방법에 대한 규칙을 포함하는 파일을 만듭니다.  이 섹션에 대한 자세한 내용은 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 클라이언트 헤더 디렉토리

이 디렉토리는 `_clientheaders.any` 요청이 들어올 때 AEM에 전달할 클라이언트 헤더 목록이 들어 있는 파일을 만듭니다.  이 섹션에 대한 자세한 내용은 [여기](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-http-headers-to-pass-through-clientheaders)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 필터 디렉토리

이 디렉토리는 `_filters.any` 모든 필터 규칙이 포함된 파일을 만들어 Dispatcher를 통한 트래픽을 차단하거나 AEM에 도달하도록 허용합니다

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Renders 디렉토리

이 디렉토리는 `_renders.any` dispatcher가 컨텐츠를 사용하는 각 백엔드 서버에 대한 연결 세부 정보를 포함하는 파일을 만듭니다

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts 디렉토리

이 디렉토리는 `_vhosts.any` 특정 팜과 특정 백 엔드 서버에 일치시킬 도메인 이름 및 경로 목록을 포함하는 파일을 만드는 경우

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## 전체 폴더 구조

AMS는 네임스페이스 문제/충돌 및 혼동을 방지하기 위해 사용자 지정 파일 확장자를 사용하여 각 파일을 구조화했습니다.

다음은 AMS 기본 배포의 표준 파일 세트 예입니다.

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## 이상적 유지

Enterprise Linux에는 Apache 웹 서버 패키지(httpd)에 대한 패치 주기가 있습니다.

패치된 보안 수정 사항이나 구성 개선 사항이 RPM / Yum 명령을 통해 적용된 경우 변경된 파일의 맨 위에 수정 사항이 적용되지 않으므로 사용자가 더 쉽게 변경할 수 있는 기본 파일은 더 적게 설치됩니다.

대신 `.rpmnew` 파일 옆에 있습니다.  이는 원하는 일부 변경 사항을 누락하고 구성 폴더에 더 많은 가비지를 만들었다는 것을 의미합니다.

즉, 업데이트 설치 중 RPM은 `httpd.conf` 만약 `unaltered` 상태 지정 *replace* 파일을 통해 중요한 업데이트를 확인할 수 있습니다.  만약 `httpd.conf` is `altered` 그리고 *교체 안 함* 대신 파일을 만들고 `httpd.conf.rpmnew` 서비스 시작 시 적용되지 않는 파일에 원하는 많은 수정 사항이 있습니다.

Enterprise Linux가 이 사용 사례를 더 잘 처리하도록 제대로 설정되었습니다.  기본 설정을 확장하거나 대체할 수 있는 영역을 제공합니다.  httpd의 기본 설치 내에서 파일을 찾을 수 있습니다 `/etc/httpd/conf/httpd.conf`에는 와 같은 구문이 있습니다.

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

아이디어는 Apache가 새 파일을 에 추가할 때 모듈과 구성을 확장하기를 원한다는 것입니다 `/etc/httpd/conf.d/` 및 `/etc/httpd/conf.modules.d/` 파일 확장명이 인 디렉토리 `.conf`

Apache에 Dispatcher 모듈을 추가할 때의 완벽한 예로서 모듈을 만듭니다 `.so` 파일 위치 ` /etc/httpd/modules/` 그런 다음 파일에 를 추가하여 포함시킵니다. `/etc/httpd/conf.modules.d/02-dispatcher.conf` 을 사용하여 모듈을 로드합니다. `.so` 파일

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>알림:</b>
Apache에서 이미 제공한 기존 파일은 수정하지 않았습니다.  대신, 이동하려는 디렉토리에 우리의 파일을 추가했습니다.
</div><br/>

이제 파일에서 모듈을 사용합니다 <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 모듈을 초기화하고 초기 모듈별 구성 파일을 로드합니다.

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

파일과 모듈을 추가했지만 원본 파일은 변경하지 않았습니다.  따라서 원하는 기능을 사용할 수 있고, 필요한 패치 수정 사항이 누락되지 않도록 할 수 있을 뿐만 아니라 각 패키지 업그레이드와의 최고 수준의 호환성을 유지할 수 있습니다.

[다음 -> 구성 파일에 대한 설명](./explanation-config-files.md)