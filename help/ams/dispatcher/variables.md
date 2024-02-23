---
title: AEM Dispatcher 구성에서 변수 사용 및 이해
description: Apache 및 Dispatcher 모듈 구성 파일에서 변수를 사용하여 다음 수준으로 이끄는 방법을 이해합니다.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 307
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---

# 변수 사용 및 이해

[목차](./overview.md)

[&lt;- 이전: 캐시 이해](./understanding-cache.md)

이 문서에서는 Apache 웹 서버 및 Dispatcher 모듈 구성 파일에서 변수의 기능을 활용할 수 있는 방법을 설명합니다.

## 변수

Apache는 변수를 지원하며 Dispatcher 모듈 버전 4.1.9부터 변수도 지원합니다!

이를 활용하여 다음과 같은 유용한 작업을 수행할 수 있습니다.

- 환경별 파일이 구성에 인라인이 아닌 압축 해제되어 개발의 구성 파일이 동일한 기능 출력으로 프로덕션에서 작동하는지 확인합니다.
- AMS가 제공하는 변경 불가능한 파일의 기능을 전환하고 로그 수준을 변경해도 변경되지 않습니다.
- 다음과 같은 변수를 기반으로 사용할 을 포함합니다. `RUNMODE` 및 `ENV_TYPE`
- 일치 `DocumentRoot`의 및 `VirtualHost` Apache 구성과 모듈 구성 사이의 DNS 이름.

## 기준선 변수 사용

AMS 기준 파일은 읽기 전용이며 변경할 수 없기 때문에 설정/해제하거나 사용하는 변수를 편집하여 구성할 수 있는 기능이 있습니다.

### 기준선 변수

AMS 기본 변수는 파일에서 선언됩니다 `/etc/httpd/conf.d/variables/ootb.vars`.  이 파일은 편집할 수 없지만 변수에 null 값이 없는지 확인하기 위해 존재합니다.  그것들은 우리가 포함하는 것보다 먼저 포함된 다음 포함됩니다. `/etc/httpd/conf.d/variables/ams_default.vars`.  이 파일을 편집하여 이러한 변수의 값을 변경하거나 파일에 동일한 변수 이름과 값을 포함할 수 있습니다.

다음은 파일 내용의 샘플입니다 `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 예제 1 - SSL 강제

위에 표시된 변수 `AUHOR_FORCE_SSL`, 또는 `PUBLISH_FORCE_SSL` 은 최종 사용자가 http 요청에 들어올 때 https로 리디렉션되도록 하는 재작성 규칙을 활성화하기 위해 1로 설정할 수 있습니다

다음은 이 토글을 사용할 수 있는 구성 파일 구문입니다.

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

다음과 같은 재작성 규칙에 포함된 는 최종 사용자 브라우저를 리디렉션하는 코드가 있지만 1로 설정되는 변수는 파일 사용 여부를 나타냅니다

### 예제 2 - 로깅 수준

변수 `DISP_LOG_LEVEL` 를 사용하여 실행 중인 구성에서 실제로 사용되는 로그 수준에 보유할 항목을 설정할 수 있습니다.

다음은 ams 기본 구성 파일에 있는 구문 예입니다.

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
| 추적 | 4 | 추적 |
| 디버그 | 3 | 디버그 |
| 정보 | 2 | 정보 |
| 경고 | 1 | 경고 |
| 오류 | 0 | 오류 |

### 예제 3 - 허용 목록

변수 `AUTHOR_WHITELIST_ENABLED` 및 `PUBLISH_WHITELIST_ENABLED` 를 1로 설정하여 IP 주소를 기반으로 최종 사용자 트래픽을 허용하거나 허용하지 않는 규칙을 포함하는 재작성 규칙을 적용할 수 있습니다.  에서 이 기능을 전환하려면 화이트리스트 규칙 파일 생성과 함께 이 기능을 포함해야 합니다.

다음은 변수가 화이트리스트 파일 및 화이트리스트 파일 예제의 포함을 활성화하는 방법에 대한 몇 가지 구문 예입니다

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

보시다시피 `sample_whitelist.rules` IP 제한을 적용하지만 변수를 토글하면 해당 변수를 `sample.vhost`

## 변수를 배치할 위치

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

이는 변경할 수 있는 사항은 아니지만 구성 파일에서 활용하는 것이 좋습니다

>[!NOTE]
>
>서비스가 시작될 때만 이 파일이 포함되기 때문입니다.  변경 사항을 적용하려면 서비스를 다시 시작해야 합니다.  다시 로드하는 것만으로는 충분하지 않지만 대신 다시 시작해야 합니다.

### 변수 파일 (`.vars`)

코드에서 제공한 사용자 지정 변수는에 있어야 합니다. `.vars` 디렉터리 내의 파일 `/etc/httpd/conf.d/variables/`

이러한 파일에는 원하는 사용자 지정 변수가 있을 수 있으며 일부 구문 예는 다음 샘플 파일에서 볼 수 있습니다

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

고유한 변수를 만들 때 파일에서 해당 내용에 따라 이름을 지정하고 설명서에 제공된 이름 지정 표준을 따릅니다 [여기](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  위의 예에서 변수 파일이 구성 파일에 사용할 변수로 다른 DNS 항목을 호스팅함을 볼 수 있습니다.

## 변수 사용

변수 파일 내에 변수를 정의했으므로 다른 구성 파일 내에서 변수를 올바르게 사용하는 방법에 대해 알아보겠습니다.

예제를 사용합니다. `.vars` 적절한 사용 사례를 보여주기 위해 위의 파일입니다.

모든 환경 기반 변수를 전체적으로 포함시키려고 합니다. 파일을 만듭니다 `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

httpd 서비스가 시작되면 AMS가 설정한 변수를 가져옵니다. `/etc/sysconfig/httpd` 및 에는 변수 세트가 있습니다. `ENV_TYPE` 및 `RUNMODE`

이 전역 `.conf` 파일이 가져오기됨 의 포함 순서가 있기 때문에 일찍 가져옵니다. `conf.d` 는 파일 이름에 000이 있으면 디렉터리의 다른 파일보다 먼저 로드됩니다.

include 문도 파일 이름에 변수를 사용합니다.  에 있는 값에 따라 실제로 로드할 파일을 변경할 수 있습니다. `ENV_TYPE` 및 `RUNMODE` 변수를 채우는 방법에 따라 페이지를 순서대로 표시합니다.

다음과 같은 경우 `ENV_TYPE` 값: `dev` 그러면 사용되는 파일은 다음과 같습니다.

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

다음과 같은 경우 `ENV_TYPE` 값: `stage` 그러면 사용되는 파일은 다음과 같습니다.

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

다음과 같은 경우 `RUNMODE` 값: `preview` 그러면 사용되는 파일은 다음과 같습니다.

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

해당 파일이 포함되면 내부에 저장된 변수 이름을 사용할 수 있습니다.

다음에서 `/etc/httpd/conf.d/available_vhosts/weretail.vhost` 개발용으로만 작동한 일반 구문을 교환할 수 있는 파일입니다.

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

dev, stage 및 prod에 작동하는 변수의 기능을 사용하는 최신 구문의 경우,

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

다음에서 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 개발용으로만 작동한 일반 구문을 교환할 수 있는 파일입니다.

```
"dev.weretail.com" 
"dev.weretail.net"
```

dev, stage 및 prod에 작동하는 변수의 기능을 사용하는 최신 구문의 경우,

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

이러한 변수는 환경당 배포된 파일이 다를 필요 없이 실행 중인 설정을 개별화하는 데 재사용률이 매우 큽니다.  기본적으로 변수를 사용하여 구성 파일을 템플릿화하고 변수를 기반으로 파일을 포함할 수 있습니다.

## 변수 값 보기

때때로 변수를 사용할 때 구성 파일에 있을 수 있는 값을 확인하기 위해 검색해야 합니다.  서버에서 다음 명령을 실행하여 해결된 변수를 볼 수 있습니다.

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

컴파일된 Apache 구성에서 변수가 어떻게 표시됩니까?

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

컴파일된 Dispatcher 구성에서 변수가 표시되는 모습:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

명령 출력에서 구성 파일과 컴파일된 출력의 변수 차이점을 확인할 수 있습니다.

예제 구성

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
```

이제 명령을 실행하여 컴파일된 출력을 확인합니다.

컴파일된 Apache 구성:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

컴파일된 디스패처 구성:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[다음 -> 플러시](./disp-flushing.md)
