---
title: AEM Dispatcher 구성에서 변수 사용 및 이해
description: Apache 및 Dispatcher 모듈 구성 파일에서 변수를 사용하여 다음 수준으로 이동하는 방법을 이해합니다.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# 변수 사용 및 이해

[목차](./overview.md)

[&lt;- 이전: 캐시 이해](./understanding-cache.md)

이 문서에서는 Apache 웹 서버 및 Dispatcher 모듈 구성 파일에서 변수의 기능을 활용하는 방법을 설명합니다.

## 변수

Apache는 변수를 지원하며 Dispatcher 모듈의 버전 4.1.9 이후에는 변수도 지원합니다.

이를 활용하여 다음과 같은 유용한 작업을 많이 수행할 수 있습니다.

- 환경별 구성 파일이 구성에서 인라인이 아니라 추출된 모든 구성 파일이 동일한 기능 출력을 사용하여 프로덕션에서 작동하는지 확인하십시오.
- 기능을 전환하고 AMS에서 제공하고 변경할 수 없는 파일의 로그 수준을 변경합니다.
- 과 같은 변수를 기반으로 사용할 include를 변경합니다. `RUNMODE` 및 `ENV_TYPE`
- 일치 `DocumentRoot`s 및 `VirtualHost` Apache 구성과 모듈 구성 간의 DNS 이름.

## 기준 변수 사용

AMS 기준 파일이 읽기 전용이고 변경할 수 없다는 사실 때문에 사용자가 사용하는 변수를 편집하여 설정을 끄고 구성할 수도 있는 기능이 있습니다.

### 기준 변수

AMS 기본 변수는 파일에 선언됩니다 `/etc/httpd/conf.d/variables/ootb.vars`.  이 파일은 편집할 수 없지만 존재하므로 변수에 null 값이 없습니다.  그 다음은 저희가 포함한 것보다 먼저 포함됩니다 `/etc/httpd/conf.d/variables/ams_default.vars`.  해당 파일을 편집하여 이러한 변수의 값을 변경하거나 파일에 동일한 변수 이름과 값을 포함할 수 있습니다.

다음은 파일 내용의 샘플입니다 `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 예제 1 - 강제 SSL

위에 표시된 변수 `AUHOR_FORCE_SSL`, 또는 `PUBLISH_FORCE_SSL` https로 리디렉션되도록 http 요청에 들어올 때 최종 사용자를 강제하는 rewrite 규칙을 사용하도록 1로 설정할 수 있습니다.

다음은 이 전환을 사용할 수 있는 구성 파일 구문입니다.

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

rewrite 규칙을 볼 수 있듯이 에는 최종 사용자 브라우저를 리디렉션하기 위한 코드가 있지만 1로 설정되는 변수가 파일을 사용할 수 있는지 여부입니다

### 예제 2 - 로깅 수준

변수 `DISP_LOG_LEVEL` 실행 중인 구성에서 실제로 사용되는 로그 수준에 대해 원하는 항목을 설정하는 데 사용할 수 있습니다.

다음은 ams 기준 구성 파일에 있는 구문 예입니다.

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Dispatcher 로깅 수준을 높여야 하는 경우 `ams_default.vars` 변수 `DISP_LOG_LEVEL` 원하는 수준으로

예제 값은 정수 또는 단어일 수 있습니다.

| 로그 수준 | 정수 값 | 단어 값 |
| --- | --- | --- |
| Trace | 4 | trace |
| 디버그 | 3 | 디버그 |
| 정보 | 2 | 정보 |
| 경고 | 1 | 경고 |
| 오류 | 0 | 오류 |

### 예제 3 - 허용 목록

변수 `AUTHOR_WHITELIST_ENABLED` 및 `PUBLISH_WHITELIST_ENABLED` IP 주소를 기반으로 최종 사용자 트래픽을 허용 또는 허용하지 않는 규칙을 포함하는 rewrite 규칙을 참여하도록 1로 설정할 수 있습니다.  이 기능을 전환하려면 포함할 수 있도록 화이트리스트 규칙 파일 만들기와 함께 결합해야 합니다.

다음은 변수가 whitelist 파일의 includs 및 whitelist 파일 예제를 가능하게 하는 방법에 대한 몇 가지 구문 예입니다

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

볼 수 있듯이 `sample_whitelist.rules` 에서는 IP 제한을 적용하지만 변수를 전환하면 `sample.vhost`

## 변수를 넣을 위치

### 웹 서버 시작 인수

AMS는 파일 내에 있는 Apache 프로세스의 시작 인수에 서버/토폴로지 특정 변수를 넣습니다. `/etc/sysconfig/httpd`

이 파일에는 다음과 같이 미리 정의된 변수가 있습니다.

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

이러한 항목은 변경할 수 없지만 구성 파일에서 활용하는 것이 좋습니다

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

서비스가 시작될 때만 이 파일이 포함되기 때문에  변경 사항을 적용하려면 서비스를 다시 시작해야 합니다.  다시 로드만으로는 충분하지 않지만 다시 시작해야 함을 의미합니다
</div>

### 변수 파일(`.vars`)

코드에서 제공하는 사용자 지정 변수는 `.vars` 디렉토리 내의 파일 `/etc/httpd/conf.d/variables/`

이러한 파일에는 원하는 사용자 지정 변수가 있을 수 있으며 일부 구문 예는 다음 샘플 파일에서 확인할 수 있습니다

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

자신만의 변수 파일을 만들 때 컨텐츠와 설명서에 제공된 명명 표준에 따라 파일 이름을 지정하십시오 [여기](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  위의 예에서 변수 파일이 구성 파일에서 사용할 변수로 다른 DNS 항목을 호스팅함을 확인할 수 있습니다.

## 변수 사용

변수 파일 내에 변수를 정의했으므로 다른 구성 파일 내에서 변수를 올바르게 사용하는 방법을 알 수 있습니다.

이 예제를 사용합니다 `.vars` 적절한 사용 사례를 보여 주는 위의 파일.

모든 환경 기반 변수를 전체적으로 포함시켜 파일을 만들겠습니다 `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

httpd 서비스가 시작될 때 이 서비스가 AMS가 설정한 변수를 가져옵니다. `/etc/sysconfig/httpd` 및 에는 변수 세트가 있습니다. `ENV_TYPE` 및 `RUNMODE`

이 전역 `.conf` 파일의 포함 순서를 포함하므로 파일을 가져올 때 일찍 가져올 수 있습니다 `conf.d` 영숫자 로드 순서는 파일 이름에 있는 000을 의미합니다. 이 순서는 디렉토리의 다른 파일 앞에 로드된다는 것을 의미합니다.

include 구문은 파일 이름에 변수를 사용하기도 합니다.  이렇게 하면 에 있는 값에 따라 실제로 로드되는 파일을 변경할 수 있습니다 `ENV_TYPE` 및 `RUNMODE` 변수를 채우는 방법을 설명합니다.

만약 `ENV_TYPE` 값은 `dev` 그런 다음 사용할 파일은 다음과 같습니다.

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

만약 `ENV_TYPE` 값은 `stage` 그런 다음 사용할 파일은 다음과 같습니다.

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

만약 `RUNMODE` 값은 `preview` 그런 다음 사용할 파일은 다음과 같습니다.

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

해당 파일이 포함되면 내부에 저장된 변수 이름을 사용할 수 있습니다.

Adobe에서 `/etc/httpd/conf.d/available_vhosts/weretail.vhost` 파일에서는 개발용으로만 작동했던 일반 구문을 교환할 수 있습니다.

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

dev, stage 및 prod에 작동하는 변수의 기능을 사용하는 최신 구문으로,

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

Adobe에서 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 파일에서는 개발용으로만 작동했던 일반 구문을 교환할 수 있습니다.

```
"dev.weretail.com" 
"dev.weretail.net"
```

dev, stage 및 prod에 작동하는 변수의 기능을 사용하는 최신 구문으로,

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

이러한 변수는 환경마다 다른 파일을 배포하지 않고도 실행 설정을 개인화하는 데 매우 많은 재사용합니다.  기본적으로 변수를 사용하고 변수를 기반으로 파일을 포함하는 구성 파일을 템플릿으로 만들 수 있습니다.

## 변수 값 보기

변수를 사용할 때 구성 파일에 값이 있을 수 있는지 검색해야 하는 경우가 있습니다.  서버에서 다음 명령을 실행하여 확인된 변수를 보는 방법이 있습니다.

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

컴파일된 Apache 구성에서 변수가 보이는 방법:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

컴파일된 Dispatcher 구성에서 변수가 보이는 방법:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

명령 출력에서 구성 파일의 변수와 컴파일된 출력의 차이를 볼 수 있습니다.

구성 예

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

이제 명령을 실행하여 컴파일된 출력을 확인합니다

컴파일된 Apache 구성:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

컴파일된 Dispatcher 구성:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[다음 -> 플러시](./disp-flushing.md)