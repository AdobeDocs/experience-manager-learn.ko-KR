---
title: AMS Dispatcher 상태 검사
description: AMS는 클라우드 로드 밸런서가 AEM이 건강한지 확인하기 위해 실행되며 공용 트래픽에 대해 서비스 상태를 유지해야 하는 상태 검사 cgi-bin 스크립트를 제공합니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# AMS Dispatcher 상태 검사

[목차](./overview.md)

[&lt;- 이전: 읽기 전용 파일](./immutable-files.md)

AMS 기준 요소가 설치된 Dispatcher가 있으면 무료 Wi-Fi가 제공됩니다.  이러한 기능 중 하나는 상태 검사 스크립트 세트입니다.
이러한 스크립트를 사용하면 AEM 스택을 폴아웃하는 로드 밸런서를 통해 어떤 다리가 정상인지 파악하고 이러한 다리가 계속 작동하도록 할 수 있습니다.

![트래픽 흐름을 보여주는 애니메이션 GIF](assets/load-balancer-healthcheck/health-check.gif "상태 확인 단계")

## 기본 로드 밸런서 상태 확인

고객 트래픽이 인터넷을 통해 AEM 인스턴스에 도달하면 로드 밸런서를 통해 전달됩니다

![이미지는 로드 밸런서를 통해 인터넷에서 aem으로 이동하는 트래픽 흐름을 보여줍니다](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "로드 밸런서-트래픽 흐름")

로드 밸런서를 통해 들어오는 각 요청은 각 인스턴스로 로빈을 반올림합니다.  로드 밸런서에는 올바른 호스트에 트래픽을 전송하도록 내장 상태 확인 메커니즘이 있습니다.

기본 검사는 일반적으로 로드 밸런서에서 타겟팅된 서버가 포트 트래픽을 수신하는지(즉, TCP 80 및 443) 여부를 확인하는 포트 확인입니다

> `Note:` 이것이 효과가 있는 반면 AEM이 건강한지에 대한 실질적인 측정은 없다.  Dispatcher(Apache 웹 서버)가 작동하여 실행 중인 경우에만 테스트합니다.

## AMS 상태 검사

비정상 AEM 인스턴스를 앞세우는 건강한 디스패처에 트래픽을 보내지 않기 위해 AMS는 디스패처뿐 아니라 레그의 상태를 평가하는 몇 가지 추가 항목을 만들었습니다.

![이 그림에서는 상태 검사가 작동하기 위한 다양한 조각을 보여 줍니다](assets/load-balancer-healthcheck/health-check-pieces.png "건강 검체")

건강검진은 다음 부분으로 구성되어 있다
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5개 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

각 부분이 어떻게 설정되었는지 그리고 그 중요성까지 다루겠습니다

### AEM 패키지

AEM이 제대로 작동하는지 여부를 나타내기 위해 기본 페이지 컴파일을 수행하고 페이지를 제공해야 합니다.  Adobe Managed Services에서 테스트 페이지를 포함하는 기본 패키지를 만들었습니다.  이 페이지는 저장소가 작동되고 리소스 및 페이지 템플릿이 렌더링될 수 있는지 테스트합니다.

![이 그림에서는 CRX 패키지 관리자의 AMS 패키지를 보여 줍니다](assets/load-balancer-healthcheck/health-check-package.png "healt-check-package")

여기 페이지가 있습니다  설치 저장소 ID가 표시됩니다

![이 그림에서는 AMS 리젠트 페이지를 보여 줍니다](assets/load-balancer-healthcheck/health-check-page.png "health-check-page")

> `Note:` 페이지가 캐시 가능하지 않은지 확인합니다.  캐시된 페이지를 반환할 때마다 실제 상태를 확인하지 않습니다!

이것은 AEM이 작동 중이고 실행 중인지 확인하기 위해 테스트할 수 있는 경량 종단점입니다.

### 로드 밸런서 구성

포트 검사를 사용하는 대신 CGI-BIN 끝점을 가리키도록 로드 밸런서를 구성합니다.

![이 그림에서는 AWS 로드 밸런서 상태 확인 구성을 보여줍니다](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![이 그림에서는 Azure 로드 밸런서 상태 확인 구성을 보여줍니다](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Apache 상태 확인 가상 호스트

#### CGI-BIN 가상 호스트 `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

이것은 `<VirtualHost>` CGI-Bin 파일을 실행할 수 있도록 하는 Apache 구성 파일입니다.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin 파일은 실행할 수 있는 스크립트입니다.  이것은 취약한 공격 벡터일 수 있으며 AMS가 사용하는 이러한 스크립트는 로드 밸런서에서만 테스트할 수 있도록 공개적으로 액세스할 수 없습니다.


#### 비정상 유지 관리 가상 호스트

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

이러한 파일의 이름은 다음과 같습니다 `000_` 를 접두사로 사용합니다.  의도적으로 라이브 사이트와 동일한 도메인 이름을 사용하도록 구성되었습니다.  AEM 백 엔드 중 하나에 문제가 있음을 상태 확인에서 발견하면 이 파일을 사용하도록 설정하는 것이 좋습니다.  그런 다음 페이지가 없는 503 HTTP 응답 코드 대신 오류 페이지를 제공합니다.  그것은 정상적인 교통수단을 빼앗을 것이다 `.vhost` 파일이 로드되기 전에 로드되므로 `.vhost` 파일을 공유하는 동안 `ServerName` 또는 `ServerAlias`.  따라서 특정 도메인으로 지정된 페이지가 기본 트래픽 흐름 대신 비정상 호스트로 이동됩니다.

상태 확인 스크립트가 실행되면 현재 상태 상태를 기록합니다.  서버에 실행 중인 cronjob이 있습니다. 이 cronjob은 로그에서 정상 상태가 아닌 항목을 찾습니다.  작성자 AEM 인스턴스가 정상 상태가 아닌 것을 발견하면 symlink를 활성화합니다.

로그 항목:

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

오류를 포착하고 반응하는 크론:

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

에서 다시 로드 모드 설정을 구성하여 작성자 또는 게시된 사이트에서 이 오류 페이지를 로드할 수 있는지 여부를 제어할 수 있습니다 `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

유효한 옵션:
- 작성자
   - 기본 옵션입니다.
   - 이렇게 하면 상태가 좋지 않을 때 작성자를 위한 유지 관리 페이지가 표시됩니다
- 페이지를
   - 이 옵션은 정상 상태가 아닐 때 게시자를 위한 유지 관리 페이지를 표시합니다
- 모두
   - 이 옵션은 작성자나 게시자가 건강하지 않을 경우 또는 둘 다 위한 유지 관리 페이지를 표시합니다
- 없음
   - 이 옵션은 상태 확인의 이 기능을 건너뜁니다

를 볼 때 `VirtualHost` 이러한 설정에 대해 설정하면 각 요청이 활성화될 때 나타나는 오류 페이지와 동일한 문서가 로드되는 것을 확인할 수 있습니다.

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

응답 코드는 여전히 입니다 `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

빈 페이지 대신 이 페이지를 받게 됩니다.

![이 그림에서는 기본 유지 관리 페이지를 보여 줍니다](assets/load-balancer-healthcheck/unhealthy-page.png "비정상 페이지")

### CGI-Bin 스크립트

CSE가 로드 밸런서에서 Dispatcher를 끌어와야 할 때 동작 또는 기준을 변경하는 로드 밸런서 설정에 구성할 수 있는 스크립트는 5가지가 있습니다.

#### /bin/checkauthor

이 스크립트를 사용하면 앞쪽에 있는 모든 인스턴스를 확인하고 기록하지만, `author` AEM 인스턴스가 비정상 상태입니다.

> `Note:` 게시 AEM 인스턴스가 정상 상태가 아닌 경우 디스패처가 트래픽을 작성자 AEM 인스턴스로 보낼 수 있도록 서비스를 유지합니다

#### /bin/checkpublish(기본값)

이 스크립트를 사용하면 앞쪽에 있는 모든 인스턴스를 확인하고 기록하지만, `publish` AEM 인스턴스가 비정상 상태입니다.

> `Note:` 작성 AEM 인스턴스가 정상 상태가 아닌 경우 Dispatcher가 서비스를 유지하여 트래픽이 게시 AEM 인스턴스로 이동하도록 합니다

#### /bin/checkkether

이 스크립트를 사용하면 앞쪽에 있는 모든 인스턴스를 확인하고 기록하지만, `author` 또는 `publisher` AEM 인스턴스가 비정상 상태입니다.

> `Note:` 게시 AEM 인스턴스 또는 작성 AEM 인스턴스가 정상 상태가 아닌 경우 디스패처가 서비스 해제를 한다는 점을 기억하십시오.  그 중 하나가 건강하면 교통도 받지 못한다는 뜻입니다

#### /bin/checkbo둘 다

이 스크립트를 사용하면 앞쪽에 있는 모든 인스턴스를 확인하고 기록하지만, `author` 그리고 `publisher` AEM 인스턴스가 비정상 상태입니다.

> `Note:` 게시 AEM 인스턴스 또는 작성 AEM 인스턴스가 정상 상태가 아닌 경우 디스패처가 서비스를 끌어내지 않는다는 점을 기억하십시오.  즉, 둘 중 하나가 건강하지 않으면 계속해서 트래픽을 받고 리소스를 요청하는 사람에게 오류를 발생합니다.

#### /bin/health

이 스크립트를 사용하면 프론트팅된 모든 인스턴스를 확인하고 로깅하지만 AEM에서 오류를 반환하는지 여부에 관계없이 정상 상태로 반환됩니다.

> `Note:` 이 스크립트는 상태 검사가 원하는 대로 작동하지 않고 로드 밸런서에서 AEM 인스턴스를 유지할 수 있도록 하는 경우에 사용됩니다.

[다음 -> GIT Symlink](./git-symlinks.md)