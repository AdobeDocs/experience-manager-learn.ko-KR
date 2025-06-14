---
title: AMS Dispatcher 기본 파일 레이아웃
description: 기본 Apache 및 Dispatcher 파일 레이아웃을 이해합니다.
version: Experience Manager 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 308
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1130'
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

>[!BEGINSHADEBOX &quot;메모&quot;]

Adobe Managed Services 서버 이미지에는 일반적으로 작은 운영 체제 루트 드라이브가 있습니다.  일반적으로 `/mnt`에 탑재된 별도의 볼륨에 데이터를 넣습니다.
그런 다음 다음 다음 기본 디렉터리에 대한 기본값 대신 해당 볼륨을 사용합니다

`DocumentRoot`
- 기본값:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- 기본값: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

기존 디렉토리와 새 디렉토리가 혼동을 방지하기 위해 원래 마운트 지점에 다시 매핑된다는 점을 기억하십시오.
별도의 볼륨을 사용하는 것은 중요하지는 않지만 참고할 만한 가치가 있습니다

>[!ENDSHADEBOX]

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
   - 이 폴더는 `.vhost`(이)라는 VirtualHost/파일을 모두 호스팅합니다.
- `/etc/httpd/conf.d/enabled_vhosts/`
   - `.vhost` 파일을 사용할 준비가 되면 `available_vhosts` 폴더 내에서 `enabled_vhosts` 디렉터리의 상대 경로를 사용하여 심볼릭 링크를 만듭니다

### 추가 `conf.d`개 디렉터리

Apache 구성에서 일반적인 추가 부분이 있으며 이러한 파일을 구분하고 하나의 디렉터리에 모든 파일을 보유하지 않을 수 있는 깔끔한 방법을 위해 하위 디렉터리를 만들었습니다

#### 디렉토리 재작성

이 디렉터리에는 Apache 웹 서버 [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) 모듈을 연결하는 일반적인 RewriteRulesync가 포함된 `_rewrite.rules` 파일이 모두 포함될 수 있습니다.

- `/etc/httpd/conf.d/rewrites/`

#### 허용 목록 디렉터리

이 디렉터리에는 일반적인 `IP Allow` 또는 Apache 웹 서버 [액세스 제어](https://httpd.apache.org/docs/2.4/howto/access.html)를 사용하는 `Require IP` 구문을 포함하는 `_whitelist.rules` 파일을 모두 포함할 수 있습니다.

- `/etc/httpd/conf.d/whitelists/`

#### 변수 디렉토리

이 디렉터리에는 구성 파일에서 사용할 수 있는 변수를 포함하는 `.vars` 파일이 모두 들어 있을 수 있습니다.

- `/etc/httpd/conf.d/variables/`

### Dispatcher 모듈별 구성 디렉터리

Apache 웹 서버는 매우 확장 가능하며 모듈에 구성 파일이 많은 경우 기본 구성 디렉토리를 어지럽히지 않고 설치 기본 디렉토리 아래에 고유한 구성 디렉토리를 만드는 것이 가장 좋습니다.

우리는 모범 사례를 따르고 우리만의 것을 만들었다

#### 모듈 구성 파일 디렉터리

- `/etc/httpd/conf.dispatcher.d/`

#### 스테이징 및 활성화된 팜

다음 디렉터리를 사용하면 파일에서 작업할 수 있는 준비 영역이 있는 구성 파일을 빌드할 수 있으며 준비가 되었을 때만 활성화할 수 있습니다.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - 이 폴더는 이름이 `_farm.any`인 `/myfarm {` 파일을 모두 호스팅합니다.
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - 팜 파일을 사용할 준비가 되면 available_farms 폴더 내에 enabled_farms 디렉터리의 상대 경로를 사용하여 심볼릭 링크를 만듭니다

### 추가 `conf.dispatcher.d`개 디렉터리

Dispatcher 팜 파일 구성의 하위 섹션인 추가 부분이 있으며, 이러한 파일을 구분하고 하나의 디렉터리에 모든 파일이 없는 깔끔한 방법을 위해 하위 디렉터리를 만들었습니다

#### 캐시 디렉터리

이 디렉터리에는 모듈이 AEM에서 가져온 캐싱 요소와 무효화 규칙 구문을 처리하는 방법에 대한 규칙이 들어 있는 `_cache.any`, `_invalidate.any` 파일이 모두 포함되어 있습니다.  이 섹션에 대한 자세한 내용은 여기 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#configuring-the-dispatcher-cache-cache)를 참조하세요.

- `/etc/httpd/conf.dispatcher.d/cache/`

#### 클라이언트 헤더 디렉터리

이 디렉터리에는 요청이 들어올 때 AEM에 전달할 클라이언트 헤더 목록을 포함하는 만든 모든 `_clientheaders.any` 파일이 들어 있을 수 있습니다.  이 섹션에 대한 자세한 내용은 [여기](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko-KR)를 참조하세요.

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### 필터 디렉터리

이 디렉터리에는 사용자가 만든 모든 `_filters.any` 파일이 들어 있을 수 있으며, 이 파일에는 모든 필터 규칙이 포함되어 있어 Dispatcher을 통한 트래픽이 AEM에 도달하지 못하도록 차단하거나 허용할 수 있습니다.

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Renders 디렉터리

이 디렉터리에는 Dispatcher가 콘텐츠를 사용할 각 백 엔드 서버에 대한 연결 세부 정보를 포함하는 `_renders.any` 파일이 모두 포함될 수 있습니다.

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts 디렉토리

이 디렉터리에는 사용자가 만든 모든 `_vhosts.any` 파일이 들어 있을 수 있으며, 여기에는 특정 팜과 특정 백 엔드 서버의 일치하는 도메인 이름 및 경로 목록이 들어 있습니다.

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

대신 원본 옆에 `.rpmnew` 파일이 만들어집니다.  이는 원하는 일부 변경 사항을 놓치고 구성 폴더에 불필요한 정보를 더 만들게 됨을 의미합니다.

즉, 업데이트 설치 중 RPM이 `unaltered` 상태이면 `httpd.conf`을(를) 검사하고 파일을 *바꾸기*&#x200B;하면 중요한 업데이트를 받게 됩니다.  `httpd.conf`이(가) `altered`인 경우 *이(가) 파일을 바꾸지 않고* 대신 `httpd.conf.rpmnew`이라는 참조 파일을 만들며 서비스 시작 시 적용되지 않는 많은 수정 사항이 해당 파일에 있습니다.

Enterprise Linux는 이 사용 사례를 더 나은 방식으로 처리하기 위해 올바르게 설정되었습니다.  이러한 설정은 사용자가 설정한 기본값을 확장하거나 재정의할 수 있는 영역을 제공합니다.  httpd의 기본 설치 내에서 `/etc/httpd/conf/httpd.conf` 파일을 찾을 수 있으며 다음과 같은 구문이 있습니다.

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

Apache가 파일 확장명이 `.conf`인 `/etc/httpd/conf.d/` 및 `/etc/httpd/conf.modules.d/` 디렉터리에 새 파일을 추가할 때 모듈 및 구성을 확장하기를 원한다는 것이 좋습니다

Apache에 Dispatcher 모듈을 추가할 때 완벽한 예로 ` /etc/httpd/modules/`에 모듈 `.so` 파일을 만든 다음, `/etc/httpd/conf.modules.d/02-dispatcher.conf`에 내용을 포함한 파일을 추가하여 모듈 `.so` 파일을 로드해야 합니다

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

>[!NOTE]
>
>Apache가 제공한 기존 파일을 수정하지 않았습니다. 대신, 우리는 그들이 가고자 하는 디렉터리에 우리의 것을 추가했을 뿐이다.

이제 모듈을 초기화하고 초기 모듈별 구성 파일을 로드하는 <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> 파일에서 모듈을 사용합니다

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

파일과 모듈을 추가했지만 원본 파일을 변경하지 않았다는 것을 다시 볼 수 있습니다.  이를 통해 원하는 기능을 제공할 수 있으며, 원하는 패치 수정 사항이 누락되지 않고 패키지의 각 업그레이드와의 최고 수준의 호환성을 유지할 수 있습니다.

[다음 -> 구성 파일에 대한 설명](./explanation-config-files.md)
