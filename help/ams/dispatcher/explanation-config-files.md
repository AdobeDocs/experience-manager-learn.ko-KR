---
title: Dispatcher 구성 파일에 대한 설명
description: 구성 파일, 이름 지정 규칙 등을 이해합니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---


# 구성 파일에 대한 설명

[목차](./overview.md)

[&lt;- 이전: 기본 파일 레이아웃](./basic-file-layout.md)

이 문서는 Adobe Managed Services에서 프로비저닝된 표준 기본 제공 Dispatcher 서버에 배포된 각 구성 파일을 분류하고 설명합니다. 사용, 명명 규칙 등은

## 이름 지정 규칙

Apache Web Server는 실제로 파일을 로 타깃팅할 때 파일 확장자가 무엇인지에 신경 쓰지 않습니다 `Include` 또는 `IncludeOptional` 문.  충돌과 혼동을 방지하는 이름으로 적절히 이름을 지정하면 <b>톤</b>. 사용되는 이름은 파일이 적용되는 범위를 설명하며, 이 범위를 사용하면 작업이 편리해집니다. 모든 이름이 인 경우 `.conf` 정말 헷갈리네요 잘못 이름이 지정된 파일 및 확장자를 방지하려고 합니다.  다음은 일반적인 AMS 구성 Dispatcher에서 사용되는 다양한 사용자 지정 파일 확장자 및 이름 지정 규칙 목록입니다.

## conf.d/에 포함된 파일

| 파일 | 파일 대상 | 설명 |
| ---- | ---------------- | ----------- |
| 파일 이름`.conf` | `/etc/httpd/conf.d/` | 기본 Enterprise Linux 설치는 이 파일 확장자를 사용하고 폴더를 httpd.conf에 선언된 설정을 재정의할 위치로 포함하므로 Apache의 글로벌 수준에서 추가 기능을 추가할 수 있습니다. |
| 파일 이름`.vhost` | 스테이징됨: `/etc/httpd/conf.d/available_vhosts/`<br>활성: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b> .vhost 파일은 enabled_vhosts 폴더에 복사되지 않지만 available_vhosts/\*.vhost 파일의 상대 경로에 대한 symlink를 사용합니다.</div></u><br><br> | \*.vhost(가상 호스트) 파일은 `<VirtualHosts>`  호스트 이름을 일치시키고 Apache가 다른 규칙으로 각 도메인 트래픽을 처리할 수 있도록 하는 항목입니다. 에서 `.vhost` 파일, 기타 파일 `rewrites`, `whitelisting`, `etc` 포함됩니다. |
| 파일 이름`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` 파일 저장 `mod_rewrite` 에 의해 명시적으로 포함 및 소비되는 규칙 `vhost` 파일 |
| 파일 이름`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` 파일은 `*.vhost` 파일. 여기에는 IP regex 또는 허용 거부 규칙이 포함되어 있어 IP를 화이트리스트에 추가할 수 있습니다. IP 주소를 기반으로 가상 호스트 보기를 제한하려는 경우 이러한 파일 중 하나를 생성하여 `*.vhost` 파일 |

## conf.modules.d/에 포함된 파일

| 파일 | 파일 대상 | 설명 |
| --- | --- | --- |
| 파일 이름`.any` | `/etc/httpd/conf.dispatcher.d/` | AEM Dispatcher Apache 모듈은 설정을 `*.any` 파일. 기본 상위 포함 파일은 다음과 같습니다. `conf.dispatcher.d/dispatcher.any` |
| 파일 이름`_farm.any` | 스테이징됨: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>활성: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b> 이러한 팜 파일은 `enabled_farms` 폴더를 사용하지만 `symlinks` 을(를) `available_farms/*_farm.any` 파일 </div> <br/>`*_farm.any` 파일은 `conf.dispatcher.d/dispatcher.any` 파일. 이러한 부모 팜 파일은 각 렌더링 또는 웹 사이트 유형에 대한 모듈 동작을 제어하기 위해 존재합니다. 파일은 `available_farms` 디렉토리 및 `symlink` 로 `enabled_farms` 디렉토리.  <br/>그러면 의 이름으로 자동으로 포함됩니다 `dispatcher.any` 파일.<br/><b>기준선</b> 팜 파일 시작 `000_` 먼저 로드되었는지 확인합니다.<br><b>사용자 지정</b> 팜 파일은 다음에 번호 체계를 시작하여 로드해야 합니다. `100_` 적절한 포함 동작을 보장합니다. |
| 파일 이름`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 각 팜에는 필터링해야 하는 트래픽을 변경하고 렌더러로 만들지 않는 규칙 세트가 있습니다. |
| 파일 이름`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 이러한 파일은 해당 요청을 제공하는 데 사용할 렌더러를 결정하기 위해 blob 일치로 일치할 호스트 이름 또는 URI 경로 목록입니다 |
| 파일 이름`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 이러한 파일은 캐시되는 항목과 캐시되지 않는 항목을 지정합니다 |
| 파일 이름`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 플러시 및 무효화 요청을 보낼 수 있는 IP 주소를 지정합니다. |
| 파일 이름`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 각 렌더러에 전달해야 하는 클라이언트 헤더를 지정합니다. |
| 파일 이름`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 각 렌더러에 대한 IP, 포트 및 시간 제한 설정을 지정합니다. 적절한 렌더러는 Dispatcher가 요청을 가져오기/프록시할 수 있는 livecycle 서버나 AEM 시스템일 수 있습니다. |

## 문제 방지

명명 규칙을 따르면, 치명적인 결과를 초래할 수 있는 실수를 쉽게 하지 않을 수 있습니다.  몇 가지 예를 살펴보겠습니다.

### 문제 예

ExampleCo와 같은 두 개의 구성 파일이 Dispatcher 구성 개발자가 만든 사이트입니다.

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

만약 `vhost` 파일이 실수로 `rewrites` 폴더 및 `rewrites file` 에 `vhosts` 폴더를 입력합니다.  파일 이름으로 적절히 배포되는 것처럼 보이지만 Apache에서 다음을 실행합니다. *오류* 그리고 그 문제는 즉시 밝혀지지 않을 것입니다.

<b>일반적으로 문제가 되는 방식</b>

만약 `two files` 에 다운로드됩니다. `same` 다음 중 한 가지 방법을 사용할 수 있습니다. `overwrite themselves` 또는 구분할 수 없게 되어 배포 프로세스가 악몽이 됩니다.

<b>파일 확장자는 동일하고 자동 포함하기 쉽습니다</b>

파일 확장자는 동일하며 Apache에서 제공하는 자동 포함 확장을 사용합니다 `auto include` 임의 `.conf` 많은 파일에 있는 파일은 기본 폴더입니다.

<b>일반적으로 문제가 되는 방식</b>

확장자가 인 vhost 파일이 `.conf` 에 `/etc/httpd/conf.d/` 폴더를 사용하면 일반적으로 양호한 Apache의 메모리에 로드하려고 하지만, rewrite 규칙 파일이 확장자가 인 경우 `.conf` 다음 위치에 `/etc/httpd/conf.d/` 폴더를 사용하면 자동으로 포함되고 전역적으로 적용되므로 혼란스럽고 원치 않는 결과가 발생합니다.

## 해결

파일의 이름을 파일의 작업에 기반하고 자동 포함 규칙 네임스페이스에서 안전하게 해제하십시오.

가상 호스트 파일 이름인 경우 `.vhost` 를 확장 프로그램으로 사용합니다.

재작성 규칙 파일인 경우 사이트 이름을 로 지정합니다`_rewrite.rules` 는 접미사 및 확장명으로 사용됩니다. 이러한 명명 규칙을 사용하면 해당 사이트와 재작성 규칙 세트임을 알 수 있습니다.

IP 화이트리스트 규칙 파일인 경우 이름을 설명 로 지정합니다`_whitelist.rules` 는 접미사 및 확장명으로 사용됩니다. 이 명명 규칙에서는 용도를 설명하고 IP 일치 규칙 세트임을 설명합니다.

이러한 이름 지정 규칙을 사용하면 파일이 속하지 않는 자동 포함 디렉토리로 이동하는 경우 문제가 발생하지 않습니다.

예를 들어 `.rules`, `.any`, 또는 `.vhost` 의 자동 포함 폴더에서 `/etc/httpd/conf.d/` 아무 영향도 없을 거야

배포 변경 요청에 &quot;exampleco_rewrite.rules를 프로덕션 Dispatcher에 배포하십시오&quot;라고 표시된다면 변경 사항을 배포하는 사람은 자신들이 새 사이트를 추가하지 않고, 파일 이름에 지정된 대로 재작성 규칙을 업데이트하고 있음을 이미 알 수 있습니다.

### 주문 포함

Enterprise Linux에 설치된 Apache 웹 서버의 기능 및 구성을 확장할 때 알고 싶은 몇 가지 중요한 포함 주문이 있습니다

### Apache 기준 포함

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

위의 다이어그램에서 볼 수 있듯이 httpd 바이너리는 구성 파일로서 httpd.conf 파일만 찾습니다.  이 파일에는 다음 문이 포함되어 있습니다.

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### AMS 최상위 수준 포함

표준을 적용할 때 몇 가지 추가 파일 형식과 자체 include를 추가했습니다.

다음은 AMS 기준 디렉토리와 최상위 수준 include입니다.
![AMS 기준 include는 /etc/httpd/conf.d/enabled_vhosts/ 디렉토리에서 *.vhost가 있는 파일을 포함하는 dispatcher_vhost.conf로 시작됩니다.  /etc/httpd/conf.d/enabled_vhosts/ 디렉토리의 항목은 /etc/httpd/conf.d/available_vhosts/에 있는 실제 구성 파일에 대한 symlink입니다.](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Apache의 기준을 구축하여 AMS가 어떻게 추가적인 폴더와 최상위 수준 include를 만들었는지를 보여줍니다. `conf.d` 폴더 및 아래에 중첩된 모듈 특정 디렉토리 `/etc/httpd/conf.dispatcher.d/`

Apache가 로드되면 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 그리고 이 파일에는 이진 파일이 포함됩니다 `/etc/httpd/modules/mod_dispatcher.so` 실행 중인 상태로 전환됩니다.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

에서 모듈을 사용하려면 `<VirtualHost />` 구성 파일을 `/etc/httpd/conf.d/` 명명된 이름 `dispatcher_vhost.conf` 이 파일 내부에는 모듈이 작동하는 데 필요한 기본 매개 변수 설정 이 표시됩니다.

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

위에서 볼 수 있듯이 여기에는 최상위 수준이 포함됩니다 `dispatcher.any` 디스패처 모듈에서 구성 파일을 선택하도록 하는 파일입니다. `/etc/httpd/conf.dispatcher.d/dispatcher.any`

다음 파일의 내용에 주의하십시오.

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

최상위 수준 `dispatcher.any` 파일에 있는 모든 사용 가능한 팜 파일이 포함되어 있습니다. `/etc/httpd/conf.dispatcher.d/enabled_farms/` 의 파일 이름으로 `FILENAME_farm.any` 표준 명명 규칙을 따릅니다.

나중에 `dispatcher_vhost.conf` 앞서 언급한 파일에서도 사용 가능한 각 가상 호스트 파일이 `/etc/httpd/conf.d/enabled_vhosts/` 다음 파일 이름으로 `FILENAME.vhost` 표준 명명 규칙을 따릅니다.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

각 .vhost 파일에서 Dispatcher 모듈은 디렉토리의 기본 파일 처리기로 초기화됩니다.  다음은 구문을 보여주는 .vhost 파일의 예입니다.

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

최상위 수준 include가 해결되면 참조할 만한 다른 하위 include가 생깁니다.  다음은 팜 및 vhosts 파일에 다른 하위 요소가 포함되는 방법에 대한 높은 수준의 다이어그램입니다

### AMS 가상 호스트 포함

![이 그림은 하나의 .vhost 파일이 변수, 허용 목록 및 다시 작성 폴더의 파일을 포함하는 방법을 보여줍니다](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

있을 때 `.vhost` 파일 `/etc/httpd/conf.d/availabled_vhosts/` 디렉토리 `/etc/httpd/conf.d/enabled_vhosts/` 디렉토리 설정은 실행 중인 구성에서 사용됩니다.

다음 `.vhost` 파일에는 우리가 찾은 공통 부분을 기반으로 하위 include가 있습니다.  변수, 허용 목록 및 재작성 규칙과 같은 것입니다.

다음 `.vhost` 파일에는 각 파일에 포함해야 하는 위치를 기반으로 하는 include 문이 있습니다 `.vhost` 파일.  다음은 의 구문 예입니다 `.vhost` 파일 참조:

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

위의 예에서 볼 수 있듯이 나중에 사용되는 이 구성 파일에 필요한 변수에 대한 include가 있습니다.

파일 내부 `/etc/httpd/conf.d/variables/weretail.vars` 정의된 변수를 확인할 수 있습니다.

```
Define MAIN_DOMAIN dev.weretail.com
```

다음 목록을 포함하는 줄을 볼 수도 있습니다 `_whitelist.rules` 다른 화이트리스트 기준에 따라 이 컨텐츠를 볼 수 있는 사용자를 제한하는 파일입니다.  흰색 목록 파일 중 하나의 내용을 살펴보겠습니다 `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

재작성 규칙 세트를 포함하는 줄을 볼 수도 있습니다.  그 내용을 살펴봅시다 `weretail_rewrite.rules` 파일:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS 팜 포함

![<FILENAME>_farms.any에는 팜 구성을 완료하는 하위 .any 파일이 포함됩니다.  이 그림에서 팜에 각 최상위 수준 섹션 파일 캐시, clientheaders, filters, renders 및 vhosts .any 파일이 포함됩니다.](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Include")

FILENAME_farm.any 파일이 있는 경우 `/etc/httpd/conf.dispatcher.d/available_farms/` 디렉토리 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 디렉토리 설정은 실행 중인 구성에서 사용됩니다.

팜 파일은 [팜의 최상위 섹션](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#defining-farms-farms) cache, clientheaders, filters, renders 및 vhosts와 같습니다.

다음 `FILENAME_farm.any` 파일에는 팜 파일에 포함해야 하는 위치를 기반으로 각 파일에 대한 include 문이 있습니다.  다음은 의 구문 예입니다 `FILENAME_farm.any` 파일 참조:

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

볼 수 있듯이 필요한 모든 구문을 보유하는 대신 weretail farm의 각 섹션은 include 문을 대신 사용합니다.

각 하위 include가 어떤 모습인지에 대한 아이디어를 얻기 위해 이러한 몇 가지 include의 구문을 살펴보겠습니다

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

볼 수 있듯이 이 팜에서 다른 팜에 렌더링해야 하는 도메인 이름의 새 선으로 구분된 목록입니다.

다음으로, `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[다음 -> 캐시 이해](./understanding-cache.md)