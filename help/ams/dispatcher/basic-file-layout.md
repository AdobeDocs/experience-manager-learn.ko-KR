---
title: AMS Dispatcher 기본 파일 레이아웃
description: 기본 Apache 및 Dispatcher 파일 레이아웃을 이해합니다.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 373
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1132'
ht-degree: 0%

---

# 기본 파일 레이아웃

[목차](./overview.md)

[&lt;- 이전: &quot;Dispatcher&quot;란?](./what-is-the-dispatcher.md)

이 문서에서는 AMS 표준 구성 파일 세트 및 이 구성 표준의 개념에 대해 설명합니다

## 기본 Enterprise Linux 폴더 구조

AMS에서 기본 설치는 Enterprise Linux를 기본 운영 체제로 사용합니다. Apache 웹 서버를 설치할 때 기본 설치 파일 세트가 있습니다. 다음은 yum 저장소에서 제공하는 기본 RPM을 설치하여 설치하는 기본 파일입니다

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

설치 설계/구조를 준수하고 준수하면 다음과 같은 이점이 있습니다.

- 예측 가능한 레이아웃 지원 용이
- 이전에 엔터프라이즈 Linux HTTPD 설치로 작업한 적이 있는 모든 사용자에게 자동으로 친숙함
- 운영 체제에서 완벽하게 지원되는 패치 주기를 충돌이나 수동 조정 없이 허용합니다.
- 레이블이 잘못 지정된 파일 컨텍스트의 SELinux 위반 방지

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b>
Adobe Managed Services 서버 이미지에는 일반적으로 작은 운영 체제 루트 드라이브가 있습니다.  일반적으로 '/mnt'에 마운트된 별도의 볼륨에 데이터를 넣습니다. 그런 다음 다음 다음 기본 디렉토리의 기본값 대신 해당 볼륨을 사용합니다

`DocumentRoot`
- 기본값:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- 기본값: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

기존 디렉토리와 새 디렉토리가 혼동을 방지하기 위해 원래 마운트 지점에 다시 매핑된다는 점을 기억하십시오.
별도의 볼륨을 사용하는 것은 중요하지는 않지만 참고할 만한 가치가 있습니다
</div>

## AMS 추가 기능

AMS는 Apache 웹 서버의 기본 설치에 을 추가합니다.

### 문서 루트

AMS 기본 문서 루트:
- 작성자:
   - `/mnt/var/www/author/`
- 게시:
   - `/mnt/var/www/html/`
- 다목적 및 상태 점검 유지 관리
   - `/mnt/var/www/default/`

### 스테이징 및 활성화된 VirtualHost 디렉터리

다음 디렉터리를 사용하면 파일에서 작업할 수 있는 준비 영역이 있는 구성 파일을 빌드할 수 있으며 준비가 되었을 때만 활성화할 수 있습니다.
- `/etc/httpd/conf.d/available_vhosts/`
   - 이 폴더는 호출된 모든 VirtualHost/파일을 호스팅합니다. `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - 다음을 사용할 준비가 되면 `.vhost` 파일, 파일 `available_vhosts` 폴더에 대한 상대 경로를 사용하여 링크 `enabled_vhosts` 디렉터리

### 추가 `conf.d` 디렉터리

Apache 구성에서 일반적인 추가 부분이 있으며 이러한 파일을 구분하고 하나의 디렉터리에 모든 파일을 보유하지 않을 수 있는 깔끔한 방법을 위해 하위 디렉터리를 만들었습니다

#### 디렉토리 재작성

이 디렉터리에는 `_rewrite.rules` apache 웹 서버를 연결하는 일반적인 RewriteRulesync가 포함된 파일을 만듭니다 [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) 모듈

- `/etc/httpd/conf.d/rewrites/`

#### 허용 목록 디렉터리

이 디렉터리에는 `_whitelist.rules` 일반 파일을 포함하는 파일을 만듭니다. `IP Allow` 또는 `Require IP`apache 웹 서버를 활성화하는 구문 [액세스 제어](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### 변수 디렉토리

이 디렉터리에는 `.vars` 구성 파일에서 사용할 수 있는 변수가 포함된 파일을 만듭니다

- `/etc/httpd/conf.d/variables/`

### Dispatcher 모듈별 구성 디렉터리

Apache 웹 서버는 매우 확장 가능하며 모듈에 구성 파일이 많은 경우 기본 구성 디렉토리를 어지럽히지 않고 설치 기본 디렉토리 아래에 고유한 구성 디렉토리를 만드는 것이 가장 좋습니다.

우리는 모범 사례를 따르고 우리만의 것을 만들었다

#### 모듈 구성 파일 디렉터리

- `/etc/httpd/conf.dispatcher.d/`

#### 스테이징 및 활성화된 팜

다음 디렉터리를 사용하면 파일에서 작업할 수 있는 준비 영역이 있는 구성 파일을 빌드할 수 있으며 준비가 되었을 때만 활성화할 수 있습니다.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 이 폴더는 `/myfarm {` 파일 호출됨 `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 팜 파일을 사용할 준비가 되면 available_farms 폴더 내에 enabled_farms 디렉터리의 상대 경로를 사용하여 심볼릭 링크를 만듭니다

### 추가 `conf.dispatcher.d` 디렉터리

Dispatcher 팜 파일 구성의 하위 섹션인 추가 조각이 있으며 이러한 파일을 구분하고 하나의 디렉터리에 모든 파일이 없는 깔끔한 방법을 위해 하위 디렉터리를 만들었습니다

#### 캐시 디렉터리

이 디렉터리에는 `_cache.any`, `_invalidate.any` 모듈이 AEM에서 가져온 캐싱 요소와 무효화 규칙 구문을 처리하는 방법에 대한 규칙이 들어 있는 파일을 만듭니다.  이 섹션에 대한 자세한 내용은 여기 를 참조하십시오 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 클라이언트 헤더 디렉터리

이 디렉터리에는 `_clientheaders.any` 요청이 들어올 때 AEM에 전달할 클라이언트 헤더 목록이 들어 있는 파일을 만듭니다.  이 섹션에 대한 자세한 내용은 다음과 같습니다 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko-KR)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 필터 디렉터리

이 디렉터리에는 `_filters.any` Dispatcher를 통한 트래픽이 AEM에 도달하도록 차단하거나 허용하는 모든 필터 규칙을 포함하는 파일을 만듭니다

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Renders 디렉터리

이 디렉터리에는 `_renders.any` dispatcher가 콘텐츠를 사용할 각 백엔드 서버에 대한 연결 세부 정보가 포함된 파일을 만듭니다.

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts 디렉토리

이 디렉터리에는 `_vhosts.any` 특정 팜과 특정 백 엔드 서버의 일치하는 도메인 이름 및 경로 목록이 들어 있는 파일을 만듭니다

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## 전체 폴더 구조

AMS는 네임스페이스 문제/충돌 및 혼란을 방지하기 위해 사용자 지정 파일 확장자와 함께 각 파일을 구조화했습니다.

다음은 AMS 기본 배포의 표준 파일 세트의 예입니다.

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

## 이상적인 유지

Enterprise Linux에는 Apache 웹 서버 패키지(httpd)에 대한 패치 주기가 있습니다.

RPM / Yum 명령을 통해 적용된 보안 수정 사항이나 구성 개선 사항이 있는 경우 변경된 파일의 상단에 수정 사항이 적용되지 않기 때문에 설치 횟수가 적은 기본 파일은 더 잘 변경됩니다.

대신 `.rpmnew` 원본 옆에 있는 파일입니다.  이는 원하는 일부 변경 사항을 놓치고 구성 폴더에 불필요한 정보를 더 만들게 됨을 의미합니다.

즉, 업데이트 설치 중 RPM은 `httpd.conf` 다음 위치에 있을 경우 `unaltered` 명시합니다. *replace* 파일을 클릭하면 중요한 업데이트가 제공됩니다.  다음과 같은 경우 `httpd.conf` 다음과 같음 `altered` then it *이(가) 을(를) 대체하지 않음* 이 파일을 만들면 대신 라는 참조 파일이 만들어집니다. `httpd.conf.rpmnew` 또한 서비스 시작 시 적용되지 않는 파일에 많은 수정 사항이 있습니다.

Enterprise Linux는 이 사용 사례를 더 나은 방식으로 처리하기 위해 올바르게 설정되었습니다.  이러한 설정은 사용자가 설정한 기본값을 확장하거나 재정의할 수 있는 영역을 제공합니다.  httpd의 기본 설치 내에서 파일을 찾을 수 있습니다. `/etc/httpd/conf/httpd.conf`및 에는 다음과 같은 구문이 있습니다.

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

Apache가 새 파일을에 추가할 때 모듈과 구성을 확장하기를 원한다는 개념입니다. `/etc/httpd/conf.d/` 및 `/etc/httpd/conf.modules.d/` 파일 확장명이 인 디렉토리 `.conf`

완벽한 예로 Apache에 Dispatcher 모듈을 추가하면 모듈이 만들어집니다 `.so` 파일 위치 ` /etc/httpd/modules/` 그런 다음 파일을 추가하여 포함시킵니다. `/etc/httpd/conf.modules.d/02-dispatcher.conf` 모듈을 로드할 내용 포함 `.so` 파일

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>알림:</b>
apache가 제공한 기존 파일을 수정하지 않았습니다.  대신 이동해야 할 디렉터리에 Ours를 추가했습니다.
</div><br/>

이제 파일에서 모듈을 사용합니다. <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 모듈을 초기화하고 초기 모듈별 구성 파일을 로드합니다.

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

파일과 모듈을 추가했지만 원본 파일을 변경하지 않았다는 것을 다시 볼 수 있습니다.  이를 통해 원하는 기능을 제공할 수 있으며, 원하는 패치 수정 사항이 누락되지 않고 패키지의 각 업그레이드와의 최고 수준의 호환성을 유지할 수 있습니다.

[다음 -> 구성 파일에 대한 설명](./explanation-config-files.md)
