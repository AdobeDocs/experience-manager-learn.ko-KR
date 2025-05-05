---
title: Dispatcher 구성 파일에 대한 설명
description: 구성 파일, 이름 지정 규칙 등을 이해할 수 있습니다.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
duration: 379
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1694'
ht-degree: 0%

---

# 구성 파일에 대한 설명

[목차](./overview.md)

[&lt;- 이전: 기본 파일 레이아웃](./basic-file-layout.md)

이 문서에서는 Adobe Managed Services에서 프로비저닝된 표준 내장 Dispatcher 서버에 배포된 각 구성 파일을 분석하고 설명합니다. 사용, 명명 규칙 등을 참조하십시오.

## 명명 규칙

Apache 웹 서버는 `Include` 또는 `IncludeOptional` 문으로 타깃팅할 때 파일의 파일 확장명이 무엇인지 실제로 고려하지 않습니다.  충돌과 혼동을 없애는 이름으로 이름을 올바르게 지정하면 <b>ton</b>에 도움이 됩니다. 사용된 이름은 파일이 적용되는 범위를 설명하므로 작업이 편리해집니다. 모든 항목의 이름이 `.conf`이면 너무 헷갈립니다. 이름이 잘못된 파일과 확장명은 피하고 싶습니다.  다음은 일반적인 AMS 구성 Dispatcher에서 사용되는 다양한 사용자 지정 파일 확장자 및 이름 지정 규칙 목록입니다.

## conf.d/에 포함된 파일

| 파일 | 파일 대상 | 설명 |
| ---- | ---------------- | ----------- |
| 파일 이름`.conf` | `/etc/httpd/conf.d/` | 기본 Enterprise Linux 설치는 이 파일 확장명을 사용하고 폴더를 httpd.conf에 선언된 설정을 재정의하고 Apache의 전역 수준에서 추가 기능을 추가할 수 있는 위치로 포함합니다. |
| 파일 이름`.vhost` | 준비: `/etc/httpd/conf.d/available_vhosts/`<br>활성: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><b>참고:</b> .vhost 파일은 enabled_vhosts 폴더에 복사되지 않지만 available_vhosts/\*.vhost 파일의 상대 경로에 대한 심볼릭 링크를 사용합니다</u><br><br> | \*.vhost(가상 호스트) 파일은 `<VirtualHosts>`입니다.  항목이 호스트 이름을 일치시키고 Apache가 서로 다른 규칙으로 각 도메인 트래픽을 처리할 수 있도록 합니다. `.vhost` 파일에서 `rewrites`, `whitelisting`, `etc`과(와) 같은 다른 파일이 포함됩니다. |
| 파일 이름`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` 파일은 `vhost` 파일에서 명시적으로 포함 및 사용할 `mod_rewrite` 규칙을 저장합니다. |
| 파일 이름`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules`개의 파일이 `*.vhost`개 파일 내에 포함되어 있습니다. 화이트리스트에 IP를 허용하는 IP 정규 표현식 또는 거부 규칙이 포함되어 있습니다. IP 주소를 기반으로 가상 호스트 보기를 제한하려는 경우 이러한 파일 중 하나를 생성하여 `*.vhost` 파일에서 포함합니다 |

## conf.dispatcher.d/

| 파일 | 파일 대상 | 설명 |
| --- | --- | --- |
| 파일 이름`.any` | `/etc/httpd/conf.dispatcher.d/` | AEM Dispatcher Apache 모듈은 `*.any`개 파일에서 해당 설정을 소스화합니다. 기본 상위 포함 파일은 `conf.dispatcher.d/dispatcher.any`입니다. |
| 파일 이름`_farm.any` | 준비 단계: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>활성: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><b>참고:</b> 이러한 팜 파일은 `enabled_farms` 폴더에 복사되지 않지만 `symlinks`을(를) 사용하여 `available_farms/*_farm.any` 파일의 상대 경로를 지정하십시오. <br/>`*_farm.any` 파일이 `conf.dispatcher.d/dispatcher.any` 파일 내에 포함되어 있습니다. 이러한 상위 팜 파일은 각 렌더링 또는 웹 사이트 유형에 대한 모듈 동작을 제어하기 위해 존재합니다. 파일이 `available_farms` 디렉터리에 만들어지고 `enabled_farms` 디렉터리에 `symlink`(으)로 활성화되었습니다.  <br/>`dispatcher.any` 파일의 이름별로 자동 포함됩니다.<br/><b>기준선</b> 팜 파일이 먼저 로드되었는지 확인하기 위해 `000_`(으)로 시작합니다.<br><b>사용자 지정</b> 팜 파일은 올바른 포함 동작을 위해 `100_`에서 번호 구성표를 시작한 후에 로드해야 합니다. |
| 파일 이름`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any`개의 파일이 `conf.dispatcher.d/enabled_farms/*_farm.any`개 파일 내에 포함되어 있습니다. 각 팜에는 필터링해야 하는 트래픽을 변경하고 렌더러에게 전달되지 않는 일련의 규칙이 있습니다. |
| 파일 이름`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any`개의 파일이 `conf.dispatcher.d/enabled_farms/*_farm.any`개 파일 내에 포함되어 있습니다. 이러한 파일은 요청을 처리하는 데 사용할 렌더러를 결정하기 위해 blob 일치에 의해 일치되는 호스트 이름 또는 URI 경로 목록입니다 |
| 파일 이름`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any`개의 파일이 `conf.dispatcher.d/enabled_farms/*_farm.any`개 파일 내에 포함되어 있습니다. 이러한 파일은 캐시되는 항목과 캐시되지 않는 항목을 지정합니다. |
| 파일 이름`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any`개의 파일이 `conf.dispatcher.d/enabled_farms/*_farm.any`개 파일 내에 포함되어 있습니다. 플러시 및 무효화 요청을 전송할 수 있는 IP 주소를 지정합니다. |
| 파일 이름`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any`개의 파일이 `conf.dispatcher.d/enabled_farms/*_farm.any`개 파일 내에 포함되어 있습니다. 각 렌더러에 전달할 클라이언트 헤더를 지정합니다. |
| 파일 이름`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any`개의 파일이 `conf.dispatcher.d/enabled_farms/*_farm.any`개 파일 내에 포함되어 있습니다. 각 렌더러에 대해 IP, 포트 및 시간 제한 설정을 지정합니다. 적절한 렌더러는 Dispatcher에서 요청을 페치/프록시할 수 있는 livecycle 서버 또는 AEM 시스템일 수 있습니다. |

## 회피된 문제

명명 규칙을 따를 때 치명적인 결과를 초래할 수 있는 실수를 꽤 쉽게 하지 않을 수 있습니다.  몇 가지 예를 들어 보겠습니다.

### 문제 예

ExampleCo에 대한 사이트 예제로 Dispatcher 구성 개발자에 의해 두 개의 구성 파일이 생성되었습니다.

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

`vhost` 파일이 실수로 `rewrites` 폴더에 있고 `rewrites file`이(가) `vhosts` 폴더에 있는 경우.  파일 이름별로 올바르게 배포되는 것처럼 보이지만 Apache에서 *ERROR*&#x200B;이 발생하고 문제가 즉시 확인되지 않습니다.

<b>일반적으로 문제가 되는 방법</b>

`two files`을(를) `same` 위치로 다운로드하면 `overwrite themselves`하거나 구분할 수 없게 만들어 배포 프로세스를 악몽으로 만들 수 있습니다.

<b>파일 확장명이 같고 자동 포함이 가능합니다.</b>

파일 확장명은 동일하며 Apache가 대부분의 기본 폴더에서 `.conf` 파일을 `auto include`할 자동 포함 확장명을 사용합니다.

<b>일반적으로 문제가 되는 방법</b>

확장명이 `.conf`인 vhost 파일을 `/etc/httpd/conf.d/` 폴더에 넣으면 시도하고 Apache의 메모리에 로드합니다. 일반적으로 양호한 작업이지만 확장명이 `.conf`인 rewrite 규칙 파일을 `/etc/httpd/conf.d/` 폴더에 넣으면 자동 포함되고 전체적으로 적용되어 혼동되고 원치 않는 결과가 발생합니다.

## 해결 방법

수행하는 작업을 기반으로 파일 이름을 자동 포함 규칙 네임스페이스에서 안전하게 지웁니다.

가상 호스트 파일 이름인 경우 확장명으로 `.vhost`을(를) 사용합니다.

규칙 파일을 다시 쓰는 경우 사이트 `_rewrite.rules`을(를) 접미사 및 확장자로 사용하여 이름을 지정하십시오. 이 명명 규칙을 사용하면 해당 사이트의 용도와 규칙 세트를 다시 작성할 수 있습니다.

IP 허용 목록 규칙 파일인 경우 설명 `_whitelist.rules`을(를) 접미사 및 확장명으로 지정하십시오. 이 명명 규칙은 용도와 IP 일치 규칙 세트에 대한 설명을 제공합니다.

이러한 이름 지정 규칙을 사용하면 파일이 속해 있지 않은 자동 포함 디렉터리로 이동하는 경우 문제가 발생하지 않습니다.

예를 들어 `/etc/httpd/conf.d/`의 자동 포함 폴더에 `.rules`, `.any` 또는 `.vhost`이(가) 있는 파일을 추가해도 영향을 받지 않습니다.

배포 변경 요청에 &quot;exameco_rewrite.rules를 프로덕션 Dispatcher에 배포하십시오&quot;라고 설명되어 있는 경우 변경 사항을 배포하는 사람이 새 사이트를 추가하지 않고 파일 이름으로 표시된 대로 다시 작성 규칙을 업데이트하는 것입니다.

### 주문 포함

Enterprise Linux에 설치된 Apache 웹 서버에서 기능 및 구성을 확장할 때 알고 싶은 몇 가지 중요한 포함 주문이 있습니다

### Apache 기준선에는 다음이 포함됩니다.

![Apache HTTPD 웹 서버 기준선에 포함되는 항목](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

위의 다이어그램에서 볼 수 있듯이 httpd 바이너리는 httpd.conf 파일만 구성 파일로 봅니다.  이 파일에는 다음 명령문이 포함되어 있습니다.

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### AMS 최상위 레벨 포함

표준을 적용할 때 몇 가지 파일 형식을 추가하고 을(를) 포함했습니다.

다음은 AMS 기본 디렉터리이며 최상위 수준에는 다음이 포함됩니다
![AMS 기준선에는 /etc/httpd/conf.d/enabled_vhosts/ 디렉터리에서 *.vhost가 있는 파일을 포함하는 dispatcher_vhost.conf로 시작하는 항목이 포함되어 있습니다.  /etc/httpd/conf.d/enabled_vhosts/ 디렉터리의 항목은 /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")에 있는 실제 구성 파일에 대한 심볼릭 링크입니다.

Apache의 기준선을 작성하면 AMS가 `conf.d`개 폴더와 `/etc/httpd/conf.dispatcher.d/` 아래에 중첩된 모듈별 디렉터리에 대한 일부 추가 폴더와 최상위 수준의 포함 방법을 보여줍니다.

Apache가 로드되면 `/etc/httpd/conf.modules.d/02-dispatcher.conf`을(를) 가져오고 해당 파일에는 이진 파일 `/etc/httpd/modules/mod_dispatcher.so`이(가) 실행 상태로 포함됩니다.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

`<VirtualHost />`에서 모듈을 사용하려면 구성 파일을 `/etc/httpd/conf.d/`(이름: `dispatcher_vhost.conf`)에 삭제하고 이 파일 내에서 모듈 작업에 필요한 기본 매개 변수 사용 설정을 확인합니다.

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

위에서 보듯이 Dispatcher 모듈에서 `/etc/httpd/conf.dispatcher.d/dispatcher.any`에서 구성 파일을 선택하는 데 사용할 최상위 수준 `dispatcher.any` 파일이 여기에 포함됩니다.

이 파일의 내용에 주의하십시오.

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

최상위 `dispatcher.any` 파일에는 파일 이름이 `FILENAME_farm.any`이고 표준 명명 규칙을 따르는 `/etc/httpd/conf.dispatcher.d/enabled_farms/`에 있는 활성화된 모든 팜 파일이 포함되어 있습니다.

앞에서 언급한 `dispatcher_vhost.conf` 파일의 뒷부분에서는 표준 명명 규칙을 따르는 파일 이름이 `FILENAME.vhost`인 `/etc/httpd/conf.d/enabled_vhosts/`에 있는 활성화된 각 가상 호스트 파일을 활성화하는 include 문도 수행합니다.

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

`/etc/httpd/conf.d/availabled_vhosts/` 디렉터리의 `.vhost` 파일이 `/etc/httpd/conf.d/enabled_vhosts/` 디렉터리에 심볼릭 링크되면 실행 중인 구성에서 사용됩니다.

`.vhost` 파일에 우리가 찾은 공통 부분을 기반으로 하는 하위 포함이 있습니다.  변수, 허용 목록 및 규칙 다시 작성과 같은 작업.

`.vhost` 파일에는 각 파일에 대해 `.vhost` 파일에 포함해야 하는 위치에 따라 include 문이 있습니다.  다음은 올바른 참조로서 `.vhost` 파일의 예제 구문입니다.

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

`/etc/httpd/conf.d/variables/weretail.vars` 파일 내에서 정의된 변수를 확인할 수 있습니다.

```
Define MAIN_DOMAIN dev.weretail.com
```

다른 허용 목록 조건에 따라 이 콘텐츠를 볼 수 있는 사용자를 제한하는 `_whitelist.rules`개 파일 목록이 포함된 줄도 볼 수 있습니다.  `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules` 화이트리스트 파일 중 하나의 내용을 살펴보겠습니다.

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

일련의 재작성 규칙이 포함된 행도 볼 수 있습니다.  `weretail_rewrite.rules` 파일의 내용을 살펴보겠습니다.

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS 팜에는 다음이 포함됩니다

![&lt;FILENAME>_farms.any에 하위 .any 파일이 포함되어 팜 구성을 완료합니다.  이 그림에서는 팜에 각 최상위 섹션 파일 캐시, clientheaders, filter, renders 및 vhosts .any files](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")가 포함되어 있음을 알 수 있습니다.

`/etc/httpd/conf.dispatcher.d/available_farms/` 디렉터리의 FILENAME_farm.any 파일이 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 디렉터리에 심볼릭 링크되면 실행 중인 구성에서 사용됩니다.

팜 파일에는 캐시, clientheaders, filters, renders 및 vhosts와 같은 [팜의 최상위 섹션](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#defining-farms-farms)을 기반으로 하는 하위 항목이 포함되어 있습니다.

`FILENAME_farm.any` 파일에는 팜 파일에 포함해야 하는 위치에 따라 각 파일에 대한 include 문이 있습니다.  다음은 올바른 참조로서 `FILENAME_farm.any` 파일의 예제 구문입니다.

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

다음으로 `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`을(를) 살펴보겠습니다.

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[다음 -> 캐시 이해](./understanding-cache.md)
