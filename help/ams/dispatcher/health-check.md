---
title: AMS Dispatcher 상태 검사
description: AMS는 클라우드 로드 밸런서가 실행되는 상태 확인 cgi-bin 스크립트를 제공하여 AEM이 정상적이고 공개 트래픽에 대해 서비스를 유지해야 하는지 확인합니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 69b4e469-52cc-441b-b6e5-2fe7ef18da90
duration: 247
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 0%

---

# AMS Dispatcher 상태 검사

[목차](./overview.md)

[&lt;- 이전: 읽기 전용 파일](./immutable-files.md)

AMS 기준 요소가 Dispatcher를 설치했다면 몇 가지 무료 기능이 제공됩니다.  이러한 기능 중 하나는 일련의 상태 확인 스크립트입니다.
이러한 스크립트를 통해 AEM 스택 앞쪽에 있는 로드 밸런서에서 정상 작동 중인 레그를 확인하고 서비스를 유지할 수 있습니다.

![트래픽 흐름을 표시하는 애니메이션 GIF](assets/load-balancer-healthcheck/health-check.gif "상태 확인 단계")

## 기본 로드 밸런서 상태 확인

고객 트래픽이 인터넷을 통해 AEM 인스턴스에 도달하면 로드 밸런서를 통과합니다

![이 그림에서는 로드 밸런서를 통해 인터넷에서 aem으로 이동하는 트래픽 흐름을 보여 줍니다](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "로드 밸런서 트래픽 흐름")

로드 밸런서를 통해 오는 각 요청은 각 인스턴스에 라운드 로빈을 수행합니다.  로드 밸런서에는 정상적인 호스트에 트래픽을 전송할 수 있도록 내장된 상태 확인 메커니즘이 있습니다.

기본 검사는 일반적으로 로드 밸런서에서 타깃팅된 서버가 포트 트래픽이 들어올 때 수신 대기하는지 확인하는 포트 검사입니다(예: TCP 80 및 443)

> `Note:` 작동하는 동안 AEM이 정상인지 실제 측정이 없습니다.  Dispatcher(Apache 웹 서버)가 작동 및 실행 중인지 여부만 테스트합니다.

## AMS 상태 확인

비정상 AEM 인스턴스를 앞두고 있는 정상 Dispatcher로 트래픽을 보내지 않기 위해 AMS는 Dispatcher뿐만 아니라 다리의 상태를 평가하는 몇 가지 추가 기능을 만들었습니다.

![작동 중인 상태 검사에 대한 다른 부분을 보여 주는 이미지](assets/load-balancer-healthcheck/health-check-pieces.png "상태 검사 항목")

상태 검사는 다음 항목으로 구성됩니다
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

각 조각이 수행할 작업과 그 중요성에 대해 설명합니다.

### AEM 패키지

AEM이 작동하는지 표시하려면 몇 가지 기본 페이지 컴파일을 수행하고 페이지를 제공해야 합니다.  Adobe Managed Services은 테스트 페이지가 포함된 기본 패키지를 만들었습니다.  페이지는 저장소가 작동 중이고 리소스와 페이지 템플릿을 렌더링할 수 있는지 테스트합니다.

![CRX 패키지 관리자에서 AMS 패키지를 보여 주는 이미지](assets/load-balancer-healthcheck/health-check-package.png "상태 검사 패키지")

여기 페이지가 있습니다  설치 시 저장소 ID가 표시됩니다

![AMS Regent 페이지를 표시하는 이미지](assets/load-balancer-healthcheck/health-check-page.png "상태 확인 페이지")

> `Note:` 페이지를 캐싱할 수 없는지 확인합니다.  캐시된 페이지를 반환할 때마다 실제 상태를 확인하지 않습니다!

AEM이 작동 중인지 확인하기 위해 테스트할 수 있는 경량 엔드포인트입니다.

### 로드 밸런서 구성

포트 검사를 사용하는 대신 CGI-BIN 끝점을 가리키도록 로드 밸런서를 구성합니다.

![이미지는 AWS 부하 분산 장치 상태 검사 구성을 보여 줍니다](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![Azure 부하 분산 장치 상태 검사 구성을 보여 주는 이미지](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Apache 상태 검사 가상 호스트

#### CGI-BIN 가상 호스트 `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

CGI-Bin 파일을 실행할 수 있는 `<VirtualHost>` Apache 구성 파일입니다.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin 파일은 실행할 수 있는 스크립트입니다.  이는 취약한 공격 벡터가 될 수 있으며 AMS가 사용하는 이러한 스크립트는 로드 밸런서에서 테스트용으로만 공개적으로 액세스할 수 없습니다.


#### 비정상 유지 관리 가상 호스트

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

이 파일의 이름은 `000_`을(를) 접두사로 사용합니다.  라이브 사이트와 동일한 도메인 이름을 사용하도록 의도적으로 구성되었습니다.  상태 검사에서 AEM 백엔드 중 하나에 문제가 있음을 감지하면 이 파일을 사용할 수 있게 됩니다.  그런 다음 페이지 없이 503 HTTP 응답 코드 대신 오류 페이지를 제공합니다.  같은 `ServerName` 또는 `ServerAlias`을(를) 공유하는 동안 해당 `.vhost` 파일 전에 로드되기 때문에 일반 `.vhost` 파일의 트래픽을 가로채게 됩니다.  결과적으로 특정 도메인으로 향하는 페이지가 정상적인 트래픽이 통과하는 기본 페이지가 아닌 비정상 vhost로 이동합니다.

상태 검사 스크립트가 실행되면 현재 상태가 로그아웃됩니다.  1분에 한 번 서버에서 실행 중인 크론잡은(cronjob)가 로그에서 정상이 아닌 항목을 찾습니다.  작성자 AEM 인스턴스가 정상이 아닌 것을 감지하면 symlink가 활성화됩니다.

로그 항목:

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron이 오류를 감지하고 반응합니다.

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

`/var/www/cgi-bin/health_check.conf`에서 다시 로드 모드 설정을 구성하여 작성자 또는 게시된 사이트에서 이 오류 페이지를 로드할 수 있는지 여부를 제어할 수 있습니다.

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

유효한 옵션:
- 작성자
   - 기본 옵션입니다.
   - 비정상인 경우 작성자를 위한 유지 관리 페이지가 표시됩니다.
- 게시
   - 이 옵션은 게시자가 정상이 아닐 때 유지 관리 페이지를 표시합니다.
- 모두
   - 이 옵션은 작성자 또는 게시자 둘 다에 대한 유지 관리 페이지를 설정하며 상태가 정상이 아닌 경우 이 페이지를 설정합니다.
- 없음
   - 이 옵션은 상태 검사의 이 기능을 건너뜁니다.

이에 대한 `VirtualHost` 설정을 보면 설정할 때 발생하는 각 요청에 대해 오류 페이지와 동일한 문서를 로드함을 알 수 있습니다.

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

응답 코드는 여전히 `HTTP 503`입니다.

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

빈 페이지 대신 이 페이지를 받게 됩니다.

![기본 유지 관리 페이지를 표시하는 이미지](assets/load-balancer-healthcheck/unhealthy-page.png "비정상 페이지")

### CGI-Bin 스크립트

CSE가 로드 밸런서에서 Dispatcher을 가져올 때의 비헤이비어 또는 기준을 변경하는 5가지 스크립트를 로드 밸런서 설정에서 구성할 수 있습니다.

#### /bin/checkauthor

이 스크립트를 사용하면 해당 스크립트가 앞에 있는 인스턴스를 확인하고 기록하지만 `author` AEM 인스턴스가 비정상 상태인 경우에만 오류를 반환합니다

> `Note:` 게시 AEM 인스턴스가 비정상인 경우 트래픽이 작성자 AEM 인스턴스로 흐를 수 있도록 디스패처가 서비스 상태를 유지합니다

#### /bin/checkpublish(기본값)

이 스크립트를 사용하면 해당 스크립트가 앞에 있는 인스턴스를 확인하고 기록하지만 `publish` AEM 인스턴스가 비정상 상태인 경우에만 오류를 반환합니다

> `Note:` 작성자 AEM 인스턴스가 비정상인 경우 트래픽이 게시 AEM 인스턴스로 흐를 수 있도록 디스패처가 서비스 상태를 유지합니다

#### /bin/checkeither

이 스크립트를 사용하면 해당 스크립트가 앞에 있는 인스턴스를 확인하고 기록하지만 `author` 또는 `publisher` AEM 인스턴스가 비정상 상태인 경우에만 오류를 반환합니다

> `Note:` 게시 AEM 인스턴스 또는 작성자 AEM 인스턴스가 비정상인 경우 dispatcher가 서비스를 중지합니다.  그 중 한 명이 건강하다면 트래픽도 받지 못한다는 의미입니다

#### /bin/checkboth

이 스크립트를 사용하면 해당 스크립트가 앞에 있는 인스턴스를 확인하고 기록하지만 `author` 및 `publisher` AEM 인스턴스가 비정상 상태인 경우에만 오류를 반환합니다

> `Note:` 게시 AEM 인스턴스 또는 작성자 AEM 인스턴스가 비정상인 경우 dispatcher가 서비스를 종료하지 않습니다.  즉, 둘 중 하나가 비정상인 경우 트래픽을 계속 수신하고 리소스를 요청하는 사람에게 오류를 제공합니다.

#### /bin/healthy

이 스크립트를 사용하면 프론트하고 있는 인스턴스를 확인하고 기록하지만 AEM에서 오류를 반환하는지 여부에 관계없이 정상적으로 반환됩니다.

> `Note:` 상태 검사가 원하는 대로 작동하지 않고 재정의를 통해 AEM 인스턴스를 로드 밸런서에 유지할 수 있을 때 이 스크립트가 사용됩니다.

[다음 -> GIT 심볼릭 링크](./git-symlinks.md)
