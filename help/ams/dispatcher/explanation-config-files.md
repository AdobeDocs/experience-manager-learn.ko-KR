---
title: Dispatcher 구성 파일에 대한 설명
description: 구성 파일, 이름 지정 규칙 등을 이해할 수 있습니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
duration: 379
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1688'
ht-degree: 0%

---

# 구성 파일에 대한 설명

[목차](./overview.md)

[&lt;- 이전: 기본 파일 레이아웃](./basic-file-layout.md)

이 문서에서는 Adobe Managed Services에서 프로비저닝된 표준 기본 제공 Dispatcher 서버에 배포된 각 구성 파일을 분석하고 설명합니다. 사용, 명명 규칙 등을 참조하십시오.

## 명명 규칙

Apache 웹 서버는 로 타겟팅할 때 파일의 파일 확장명이 무엇인지 실제로 신경쓰지 않습니다. `Include` 또는 `IncludeOptional` 명령문입니다.  갈등과 혼동을 없애는 이름으로 올바로 이름을 붙이면 <b>톤</b>. 사용된 이름은 파일이 적용되는 범위를 설명하므로 작업이 편리해집니다. 모든 항목의 이름이 `.conf` 이거 정말 헷갈리네요. 이름이 잘못된 파일과 확장명은 피하고 싶습니다.  다음은 일반적인 AMS 구성 Dispatcher에서 사용되는 다양한 사용자 지정 파일 확장자 및 이름 지정 규칙 목록입니다.

## conf.d/에 포함된 파일

| 파일 | 파일 대상 | 설명 |
| ---- | ---------------- | ----------- |
| 파일 이름`.conf` | `/etc/httpd/conf.d/` | 기본 Enterprise Linux 설치는 이 파일 확장명을 사용하고 폴더를 httpd.conf에 선언된 설정을 재정의하고 Apache의 전역 수준에서 추가 기능을 추가할 수 있는 위치로 포함합니다. |
| 파일 이름`.vhost` | 준비: `/etc/httpd/conf.d/available_vhosts/`<br>활성: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><b>참고:</b> .vhost 파일은 enabled_vhosts 폴더에 복사되지 않지만 available_vhosts/\*.vhost 파일의 상대 경로에 대한 심볼릭 링크를 사용합니다.</u><br><br> | \*.vhost (가상 호스트) 파일은 `<VirtualHosts>`  항목이 호스트 이름을 일치시키고 Apache가 서로 다른 규칙으로 각 도메인 트래픽을 처리할 수 있도록 합니다. 다음에서 `.vhost` 파일, 다음과 같은 기타 파일 `rewrites`, `whitelisting`, `etc` 포함됩니다. |
| 파일 이름`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` 파일 저장소 `mod_rewrite` 에 의해 명시적으로 포함되고 사용되는 규칙 `vhost` 파일 |
| 파일 이름`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` 파일은 다음 내에서 포함됩니다. `*.vhost` 파일. 화이트리스트에 IP를 허용하는 IP 정규 표현식 또는 거부 규칙이 포함되어 있습니다. IP 주소를 기반으로 가상 호스트 보기를 제한하려는 경우 이러한 파일 중 하나를 생성하여 `*.vhost` 파일 |

## conf.dispatcher.d/

| 파일 | 파일 대상 | 설명 |
| --- | --- | --- |
| 파일 이름`.any` | `/etc/httpd/conf.dispatcher.d/` | AEM Dispatcher Apache 모듈은 해당 설정의 소스를 지정합니다. `*.any` 파일. 기본 상위 포함 파일은 입니다. `conf.dispatcher.d/dispatcher.any` |
| 파일 이름`_farm.any` | 준비: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>활성: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><b>참고:</b> 이러한 팜 파일은 `enabled_farms` 폴더만 사용 `symlinks` 에 대한 상대 경로로 `available_farms/*_farm.any` 파일 <br/>`*_farm.any` 파일은 `conf.dispatcher.d/dispatcher.any` 파일. 이러한 상위 팜 파일은 각 렌더링 또는 웹 사이트 유형에 대한 모듈 동작을 제어하기 위해 존재합니다. 파일은에서 만들어집니다. `available_farms` 디렉터리 및 사용 `symlink` 대상: `enabled_farms` 디렉토리.  <br/>에서 이름별로 자동 포함됩니다. `dispatcher.any` 파일.<br/><b>기준선</b> 팜 파일 시작 문자 `000_` 먼저 로드되었는지 확인하십시오.<br><b>사용자 정의</b> 팜 파일은에서 번호 체계를 시작하여 로드해야 합니다. `100_` 를 사용하여 포함 동작을 적절히 조정합니다. |
| 파일 이름`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` 파일은 다음 내에서 포함됩니다. `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 각 팜에는 필터링해야 하는 트래픽을 변경하고 렌더러에게 전달되지 않는 일련의 규칙이 있습니다. |
| 파일 이름`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` 파일은 다음 내에서 포함됩니다. `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 이러한 파일은 요청을 처리하는 데 사용할 렌더러를 결정하기 위해 blob 일치에 의해 일치되는 호스트 이름 또는 URI 경로 목록입니다 |
| 파일 이름`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` 파일은 다음 내에서 포함됩니다. `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 이러한 파일은 캐시되는 항목과 캐시되지 않는 항목을 지정합니다. |
| 파일 이름`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 플러시 및 무효화 요청을 전송할 수 있는 IP 주소를 지정합니다. |
| 파일 이름`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 각 렌더러에 전달할 클라이언트 헤더를 지정합니다. |
| 파일 이름`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` 파일은 `conf.dispatcher.d/enabled_farms/*_farm.any` 파일. 각 렌더러에 대해 IP, 포트 및 시간 제한 설정을 지정합니다. 적절한 렌더러는 Dispatcher가 요청을 가져오기/프록시할 수 있는 AEM 시스템 또는 Livecycle Server일 수 있습니다. |

## 회피된 문제

명명 규칙을 따를 때 치명적인 결과를 초래할 수 있는 실수를 꽤 쉽게 하지 않을 수 있습니다.  몇 가지 예를 들어 보겠습니다.

### 문제 예

ExampleCo의 사이트 예제로 Dispatcher 구성 개발자가 두 개의 구성 파일을 만들었습니다.

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

다음과 같은 경우 `vhost` 파일이 실수로 `rewrites` 폴더 및 `rewrites file` 을(를) 다음에 넣습니다. `vhosts` 폴더를 삭제합니다.  이 변수는 파일 이름별로 올바르게 배포되는 것처럼 보이지만, Apache에서 *오류* 그리고 이 문제는 즉시 밝혀지지 않을 것이다.

<b>이것이 일반적으로 문제가 되는 방식</b>

다음과 같은 경우 `two files` 로 다운로드됩니다. `same` 다음 중 하나를 수행할 수 있습니다. `overwrite themselves` 또는 배포 프로세스를 악몽으로 만드는 것을 구분할 수 없게 만들 수 있습니다.

<b>파일 확장명은 동일하며 자동 포함이 쉽습니다</b>

파일 확장명은 동일하며 Apache가 사용할 자동 포함 확장명을 사용합니다 `auto include` 임의 `.conf` 대부분의 파일은 기본 폴더입니다.

<b>이것이 일반적으로 문제가 되는 방식</b>

확장자가 인 vhost 파일이 `.conf` 다음에 배치됩니다. `/etc/httpd/conf.d/` 폴더는 시도하고 Apache의 메모리에 로드하며 일반적으로 양호한 상태이지만 확장자가 인 규칙 파일을 다시 작성하는 경우 `.conf` 에 배치됩니다. `/etc/httpd/conf.d/` 폴더가 자동으로 포함되고 전역적으로 적용되어 혼란스럽고 원치 않는 결과가 발생합니다.

## 해결 방법

수행하는 작업을 기반으로 파일 이름을 자동 포함 규칙 네임스페이스에서 안전하게 지웁니다.

가상 호스트 파일인 경우 `.vhost` 를 확장으로 사용하십시오.

규칙 파일을 다시 작성한 경우 사이트 이름을 지정합니다.`_rewrite.rules` 를 접두사 및 확장자로 사용하십시오. 이 명명 규칙을 사용하면 해당 사이트의 용도와 규칙 세트를 다시 작성할 수 있습니다.

IP 화이트리스트 규칙 파일인 경우 이름을 설명으로 지정합니다.`_whitelist.rules` 를 접두사 및 확장자로 사용하십시오. 이 명명 규칙은 용도와 IP 일치 규칙 세트에 대한 설명을 제공합니다.

이러한 이름 지정 규칙을 사용하면 파일이 속해 있지 않은 자동 포함 디렉터리로 이동하는 경우 문제가 발생하지 않습니다.

예를 들어 를 사용하여 이름이 인 파일을 `.rules`, `.any`, 또는 `.vhost` 의 자동 포함 폴더에서 `/etc/httpd/conf.d/` 영향을 미치지 않을 것입니다.

배포 변경 요청에 &quot;exameco_rewrite.rules를 프로덕션 Dispatcher에 배포하십시오&quot;라고 설명되어 있는 경우 변경 사항을 배포하는 사람이 새 사이트를 추가하지 않고 파일 이름으로 표시된 대로 다시 작성 규칙을 업데이트하는 것입니다.

### 주문 포함

Enterprise Linux에 설치된 Apache 웹 서버에서 기능 및 구성을 확장할 때 알고 싶은 몇 가지 중요한 포함 주문이 있습니다

### Apache 기준선에는 다음이 포함됩니다.

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

위의 다이어그램에서 볼 수 있듯이 httpd 바이너리는 httpd.conf 파일만 구성 파일로 봅니다.  이 파일에는 다음 명령문이 포함되어 있습니다.

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### AMS 최상위 레벨 포함

표준을 적용할 때 몇 가지 파일 형식을 추가하고 을(를) 포함했습니다.

다음은 AMS 기본 디렉터리이며 최상위 수준에는 다음이 포함됩니다
![AMS 기준선에는 /etc/httpd/conf.d/enabled_vhosts/ 디렉토리의 *.vhost가 있는 파일을 포함하는 dispatcher_vhost.conf로 시작하는 항목이 포함되어 있습니다.  /etc/httpd/conf.d/enabled_vhosts/ 디렉토리의 항목은 /etc/httpd/conf.d/available_vhosts/에 있는 실제 구성 파일에 대한 심볼릭 링크입니다.](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Apache의 기준선을 작성하면 AMS가 몇 가지 추가 폴더를 만들고 최상위 수준에서에 을 포함하는 방법을 알 수 있습니다. `conf.d` 아래에 중첩된 모듈 특정 디렉터리와 폴더 `/etc/httpd/conf.dispatcher.d/`

Apache가 로드되면 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 그리고 이 파일에는 이진 파일이 포함됩니다 `/etc/httpd/modules/mod_dispatcher.so` 실행 중 상태로 전환합니다.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

의 모듈을 사용하려면 `<VirtualHost />` 구성 파일을에 드롭합니다 `/etc/httpd/conf.d/` 명명된 `dispatcher_vhost.conf` 그리고 이 파일 안에서는 모듈이 작동하는 데 필요한 기본 매개 변수 사용 설정 이 표시됩니다.

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

위에서 볼 수 있듯이 여기에는 최상위 수준이 포함됩니다 `dispatcher.any` dispatcher 모듈의 구성 파일을 가져올 파일 `/etc/httpd/conf.dispatcher.d/dispatcher.any`

이 파일의 내용에 주의하십시오.

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

최상위 수준 `dispatcher.any` 파일에는 에 있는 활성화된 모든 팜 파일이 포함되어 있습니다 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 의 파일 이름으로 `FILENAME_farm.any` 표준 명명 규칙을 따릅니다.

의 후반부에 `dispatcher_vhost.conf` 앞에서 언급한 파일에서도 include 문을 사용하여 활성화된 각 가상 호스트 파일이 활성 상태가 되도록 합니다. `/etc/httpd/conf.d/enabled_vhosts/` 파일 이름: `FILENAME.vhost` 표준 명명 규칙을 따릅니다.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

각 .vhost 파일에서 Dispatcher 모듈은 디렉터리의 기본 파일 핸들러로 초기화됩니다.  다음은 구문을 표시하는 .vhost 파일의 예입니다.

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

최상위 수준에 해결 이 포함된 후에는 언급할 가치가 있는 다른 하위 포함이 있습니다.  다음은 farms 및 vhosts 파일에 다른 하위 요소를 포함하는 방법에 대한 높은 수준의 다이어그램입니다

### AMS 가상 호스트에 다음이 포함됨

![이 그림에서는 하나의 .vhost 파일에 변수, 허용 목록의 파일이 포함되어 있고 폴더를 다시 쓰는 방법을 보여 줍니다](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

있는 경우 `.vhost` 파일 출처: `/etc/httpd/conf.d/availabled_vhosts/` 디렉터리가 `/etc/httpd/conf.d/enabled_vhosts/` 디렉터리는 실행 중인 구성에서 사용됩니다.

다음 `.vhost` 발견된 공통 부분을 기반으로 하위 포함 항목이 파일에 있습니다.  변수, 허용 목록 및 규칙 다시 작성과 같은 작업.

다음 `.vhost` 파일에는 각 파일이 포함해야 하는 위치에 따라 해당 파일에 대한 include 문이 있습니다. `.vhost` 파일.  다음은 구문 예제입니다. `.vhost` 좋은 참조로서의 파일:

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

위의 예에서 볼 수 있듯이 나중에 사용되는 이 구성 파일에 필요한 변수에 대한 가 포함되어 있습니다.

파일 내부 `/etc/httpd/conf.d/variables/weretail.vars` 정의된 변수는 다음과 같습니다.

```
Define MAIN_DOMAIN dev.weretail.com
```

다음 목록이 포함된 줄도 볼 수 있습니다. `_whitelist.rules` 다른 허용 목록 기준에 따라 이 콘텐츠를 볼 수 있는 사용자를 제한하는 파일입니다.  화이트리스트 파일 중 하나의 내용을 살펴보겠습니다 `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

일련의 재작성 규칙이 포함된 행도 볼 수 있습니다.  이제 의 내용을 살펴보겠습니다 `weretail_rewrite.rules` 파일:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS 팜에는 다음이 포함됩니다

![<FILENAME>_farms.any에 하위 .any 파일이 포함되어 팜 구성을 완료합니다.  이 그림에서는 팜에 각 최상위 섹션 파일 캐시, clientheaders, filters, renders 및 vhosts .any 파일이 포함되어 있음을 알 수 있습니다.](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

FILENAME_farm.any 파일 `/etc/httpd/conf.dispatcher.d/available_farms/` 디렉터리가 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 디렉터리는 실행 중인 구성에서 사용됩니다.

팜 파일에는 다음을 기반으로 하는 하위 포함이 있습니다. [팜의 최상위 섹션](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) 캐시, clientheaders, filter, renders 및 vhosts와 같은 기능이 있습니다.

다음 `FILENAME_farm.any` 파일에는 팜 파일에 포함해야 하는 위치에 따라 각 파일에 대한 include 문이 있습니다.  다음은 구문 예제입니다. `FILENAME_farm.any` 좋은 참조로서의 파일:

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

include 문을 사용하는 대신 모든 구문이 필요한 대신 weretail 팜에 대한 각 섹션을 볼 수 있습니다.

이러한 포함 중 몇 개의 구문을 살펴보고 각 하위 포함이 어떻게 표시되는지 파악합니다

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

보시다시피 이 팜에서 다른 팜으로 렌더링해야 하는 도메인 이름의 줄로 구분된 새 목록입니다.

다음으로 다음을 살펴보겠습니다. `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[다음 -> 캐시 이해](./understanding-cache.md)
