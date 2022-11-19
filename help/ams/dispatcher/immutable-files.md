---
title: AMS Dispatcher 읽기 전용 또는 변경할 수 없는 파일
description: 일부 파일이 읽기 전용이거나 편집할 수 없는 이유와 원하는 기능을 변경하는 방법 이해
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 1%

---


# AMS에서 읽기 전용 또는 변경할 수 없는 파일

[목차](./overview.md)

[&lt;- 이전: 일반 로그](./common-logs.md)

## 설명

이 문서에서는 잠기고 변경되지 않을 파일과 원하는 구성 설정을 제대로 설정하는 방법에 대해 설명합니다.

AMS가 시스템을 프로비저닝하면 모든 기능이 작동하고 안전하게 사용할 수 있는 기본 구성을 롤아웃합니다.  AMS가 기능 및 보안에 대한 기본 요소로 계속 유지하려는 작업입니다.  이를 위해 일부 파일은 읽기 전용으로 표시되어 변경할 수 없습니다.

레이아웃으로 인해 해당 동작이 변경되고 필요한 변경 사항을 재정의할 수 없습니다.  이러한 파일을 변경하는 대신 원본보다 우선하는 자체 파일을 오버레이합니다.

또한 AMS가 최신 수정 사항 및 보안 개선 사항으로 Dispatcher를 패치할 때 파일이 변경되지 않는다는 사실을 확인할 수 있습니다.  그런 다음 향상된 기능을 계속 활용할 수 있고 원하는 변경 사항만 적용할 수 있습니다.
![레인에서 굴러 내려오는 공이 있는 볼링 레인을 보여 줍니다.  그 공은 화살을 가지고 있고, 그 단어는 너에게 보여 준다.  홈통 범퍼가 자라나고 그 위에 변경할 수 없는 파일들이 있다.](assets/immutable-files/bowling-file-immutability.png "볼링 파일 무변형")
위의 그림에서 보듯이, 가변 파일을 사용할 수 없기 때문에 게임을 할 수 없습니다.  그들은 단지 여러분이 공연을 해치는 것을 막고 여러분을 차선에 있게 합니다.  이 방법을 사용하면 다음과 같은 몇 가지 주요 기능을 사용할 수 있습니다.

- 사용자 지정은 고유한 안전 공간에서 처리됩니다
- 사용자 지정 변경 사항의 오버레이는 AEM의 오버레이 방법을 반영합니다
- 사용자 지정 내용을 변경하지 않고 AMS 구성 패치 작업을 수행할 수 있습니다
- 기본 설치 테스트와 사용자 지정 구성 테스트를 동시에 수행하여 문제가 사용자 지정 또는 기타 파일 중 어느 것으로 발생하는지 확인하는 데 도움이 될 수 있습니다.


다음은 Dispatcher와 함께 배포되는 일반적인 파일 목록입니다.

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
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

변경할 수 없는 파일을 확인하려면 Dispatcher에서 다음 명령을 실행하여 확인합니다.

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

다음은 변경할 수 없는 파일의 샘플 응답입니다.

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## 변경 방법

### 변수

변수를 사용하면 구성 파일 자체를 변경하지 않고 기능을 변경할 수 있습니다.  변수의 값을 조정하여 구성의 특정 요소를 조정할 수 있습니다.  파일에서 강조 표시할 수 있는 예제 중 하나 `/etc/httpd/conf.d/dispatcher_vhost.conf` 아래와 같이 표시됩니다.

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

DispatcherLogLevel 지시문에 `DISP_LOG_LEVEL` 표시되는 일반 값 대신  해당 코드 섹션 위에 변수 파일에 대한 include 문이 표시됩니다.  변수 파일 `/etc/httpd/conf.d/variables/ams_default.vars` 다음으로 우리가 보고 싶은 곳입니다.  다음은 해당 변수 파일의 내용입니다.

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

위에 현재 값이 표시됩니다. `DISP_LOG_LEVEL` 변수가 `info`.  추적하거나 디버깅하거나 선택한 숫자 값/수준을 조정하도록 조정할 수 있습니다.  이제 로그 수준을 제어하는 모든 영역이 자동으로 조정됩니다.

### 오버레이 메서드

최상위 수준의 포함 파일은 사용자 지정을 수행하는 시작점이 되므로 이를 이해하십시오.  간단한 예를 시작하기 위해 이 Dispatcher를 가리키려는 새 도메인 이름을 추가하려는 시나리오가 있습니다.  사용할 도메인 예는 we-retail.adobe.com입니다.  먼저 기존 구성 파일을 새 구성 파일에 복사하여 변경 사항을 추가할 수 있습니다.

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

기존 aem_publish.vhost 파일은 이미 작업을 수행하는 데 필요한 항목이 있으므로 복사되었으며 이미 강력한 시작을 다시 발명하고 싶지 않기 때문입니다.  이제 새 weretail.vhost 파일을 편집하고 필요한 사항을 변경합니다.

이전:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

이후:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

이제 Adobe에서 `ServerName` 및 `ServerAlias` 새 도메인 이름과 일치하고 다른 탐색 표시 헤더를 업데이트하는 것입니다.  이제 새 파일이 Apache에서 새 파일을 사용할 수 있도록 허용:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

이제 apache 웹 서버는 도메인이 트래픽을 발생시키는 항목임을 인식하지만, 디스패처 모듈에 전달할 새 도메인 이름이 있음을 알려주어야 합니다.  새 `*_vhost.any` 파일 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 그리고 해당 파일 내에 표시할 도메인 이름을 입력합니다.

```
"we-retail.adobe.com"
```

이제 새 vhost 항목 파일을 사용할 새 팜 파일을 만들어야 하며, 강력한 시작 파일을 새 파일에 복사해 보겠습니다.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

이 팜 파일에 적용해야 하는 변경 사항을 표시합니다.

이전:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

이후:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

이제 팜 이름을 업데이트했으며, 팜 이름에 을 사용하는 포함 `/virtualhosts` 팜 구성의 섹션  실행 중인 구성에서 사용할 수 있도록 이 새 팜 파일을 활성화해야 합니다.

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

이제 웹 서버 서비스를 다시 로드하고 새 도메인을 사용합니다!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

기본 구성 파일과 함께 제공된 기존 포함 및 코드를 변경하고 활용하는 데 필요한 부분만 변경했습니다.  우리는 우리가 변경해야 하는 요소에 대해서만 설명을 해야 한다.  작업을 훨씬 쉽게 만들고 더 적은 코드를 유지할 수 있도록 합니다
</div>