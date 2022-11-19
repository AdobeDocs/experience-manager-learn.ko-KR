---
title: AEM Dispatcher 플러시
description: AEM이 Dispatcher에서 이전 캐시 파일을 무효화하는 방법을 이해합니다.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '2225'
ht-degree: 0%

---


# 디스패처 별칭 URL

[목차](./overview.md)

[&lt;- 이전: 변수 사용 및 이해](./variables.md)

이 문서에서는 플러시 발생 방법에 대한 지침을 제공하고 캐시 플러시 및 무효화를 실행하는 메커니즘을 설명합니다.


## 작동 방법

### 작업 순서

일반적인 워크플로우는 컨텐츠 작성자가 페이지를 활성화할 때, 게시자가 새 콘텐츠를 받으면 다음 다이어그램에 표시된 대로 Dispatcher에 플러시 요청을 트리거할 때 가장 잘 설명되어 있습니다.
![작성자는 Dispatcher에 플러시 요청을 전송하도록 게시자를 트리거하는 컨텐츠를 활성화합니다](assets/disp-flushing/dispatcher-flushing-order-of-events.png "dispatcher-flushing-of-events")
이 이벤트 체인화는 항목이 신규이거나 변경된 경우에만 항목을 초기화한다는 것을 강조 표시합니다.  따라서 게시자에서 변경 사항을 선택하기 전에 플러시가 발생할 수 있는 경합 조건을 방지하기 위해 캐시를 지우기 전에 게시자가 컨텐츠를 수신했는지 확인합니다.

## 복제 에이전트

작성자에는 게시자를 가리키도록 구성된 복제 에이전트가 있는데 어떤 기능이 활성화되면 파일을 전송하도록 트리거되고 모든 복제 에이전트가 게시자에게 종속됩니다.

게시자가 파일을 받으면 수신 시 이벤트를 트리거하는 Dispatcher를 가리키도록 구성된 복제 에이전트가 있습니다.  그런 다음 플러시 요청을 직렬화하고 Dispatcher에 게시합니다.

### 작성자 복제 에이전트

다음은 구성된 표준 복제 에이전트의 몇 가지 스크린샷입니다
![AEM 웹 페이지의 표준 복제 에이전트 스크린샷 /etc/replication.html](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

일반적으로 컨텐츠를 복제하는 각 게시자에 대해 작성자에 구성된 복제 에이전트가 1개 또는 2개 있습니다.

먼저 컨텐츠 활성화를에 푸시하는 표준 복제 에이전트입니다.

둘째, 역제가 있다.  이 작업은 선택 사항이며, 작성자에게 역방향 복제 활동으로 가져올 새 컨텐츠가 있는지 확인하기 위해 각 게시자 확인란을 선택하도록 설정되어 있습니다

### 게시자 복제 에이전트

다음은 구성된 표준 플러시 복제 에이전트의 스크린샷입니다
![AEM 웹 페이지의 표준 플러시 복제 에이전트 스크린샷 /etc/replication.html](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### 가상 호스트를 수신하는 디스패처 초기화 복제

Dispatcher 모듈은 POST 요청이 AEM 렌더링에 전달할 콘텐츠인 경우 또는 플러시 요청으로 직렬화된 경우 Dispatcher 처리기 자체에서 처리해야 하는 경우 알리기 위해 특정 헤더를 찾습니다.

다음은 이러한 값을 보여주는 구성 페이지의 스크린샷입니다.
![직렬화 유형이 디스패처 플러시로 표시된 기본 구성 화면 설정 탭의 그림](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

기본 설정 페이지에는 `Serialization Type` 로서의 `Dispatcher Flush` 오류 수준을 설정하고

![복제 에이전트의 전송 탭 스크린샷입니다.  플러시 요청을 게시할 URI가 표시됩니다.  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

설정 `Transport` 탭에서 `URI` 플러시 요청을 받을 Dispatcher의 IP 주소를 가리키도록 설정됩니다.  경로 `/dispatcher/invalidate.cache` 은 모듈이 플러시인지 여부를 결정하는 방법이 아닙니다. 플러시 요청임을 확인하기 위해 액세스 로그에서 볼 수 있는 명백한 종단점일 뿐입니다.  설정 `Extended` 탭에서는 디스패처 모듈에 대한 플러시 요청임을 확인하기 위해 필요한 사항을 살펴봅니다.

![복제 에이전트의 확장 탭에 대한 스크린샷입니다.  디스패처에 플러시를 지정하도록 전송된 POST 요청과 함께 전송되는 헤더를 확인합니다](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

다음 `HTTP Method` 플러시 요청의 경우 `GET` 몇 가지 특수 요청 헤더로 요청.
- CQ-Action
   - 이 변수는 요청을 기반으로 AEM 변수를 사용하며 일반적으로 이 값은 *활성화 또는 삭제*
- CQ-Handle
   - 이 변수는 요청을 기반으로 AEM 변수를 사용하며, 값은 일반적으로 예를 들어 플러시된 항목의 전체 경로입니다 `/content/dam/logo.jpg`
- CQ-Path
   - 이 변수는 요청을 기반으로 AEM 변수를 사용하며, 값은 일반적으로 플러시되는 항목의 전체 경로입니다(예: `/content/dam`
- 호스트
   - 여기에서 `Host` 헤더는 특정 타겟을 타겟팅하기 위해 스푸핑됩니다 `VirtualHost` 디스패처 Apache 웹 서버(`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`).  이 값은 `aem_flush.vhost` 파일 `ServerName` 또는 `ServerAlias`

![작성자 게시 컨텐츠에서 복제 이벤트에서 새 항목을 받으면 반응하고 트리거되는 복제 에이전트가 있음을 보여주는 표준 복제 에이전트의 화면](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

설정 `Triggers` 탭에서는 사용하는 전환된 트리거와 트리거가 무엇인지 확인할 수 있습니다

- `Ignore default`
   - 이 옵션이 활성화되므로 페이지 활성화 시 복제 에이전트가 트리거되지 않습니다.  작성자 인스턴스가 페이지를 변경하면 플러시가 트리거되는 문제입니다.  게시자이므로 해당 유형의 이벤트를 트리거하지 않을 수 있습니다.
- `On Receive`
   - 새 파일을 받으면 플러시를 트리거하려고 합니다.  따라서 작성자가 업데이트된 파일을 보내면 트리거와 Dispatcher에 플러시 요청을 보냅니다.
- `No Versioning`
   - 새 파일을 받아서 게시자가 새 버전을 생성하지 않도록 합니다.  현재 있는 파일을 대체하고 게시자 대신 버전을 계속 추적하기 위해 작성자가 사용하는 파일입니다.

이제 일반적인 플러시 요청이 어떻게 보이는지 `curl` 명령

```
$ curl \ 
-H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/dam/logo.jpg" \ 
-H "CQ-Path: /content/dam/" \ 
-H "Content-Length: 0" \  
-H "Content-Type: application/octect-stream" \ 
-H "Host: flush" \ 
http://10.43.0.32:80/dispatcher/invalidate.cache
```

이 초기화 예는 `/content/dam` 경로 업데이트 `.stat` 해당 디렉토리에 있는 파일입니다.

## 다음 `.stat` 파일

플러싱 메커니즘은 본질적으로 간단하며 우리는 이 장치의 중요성을 설명하고 싶습니다 `.stat` 캐시 파일이 생성되는 문서 루트에서 생성되는 파일입니다.

내부 `.vhost` 및 `_farm.any` 파일에서는 문서 루트 지시문을 구성하여 캐시가 있는 위치와 최종 사용자의 요청이 들어올 때 파일을 저장/제공할 위치를 지정합니다.

Dispatcher 서버에서 다음 명령을 실행하는 경우 찾기를 시작합니다 `.stat` 파일

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

다음은 캐시에 항목이 있고 Dispatcher 모듈에서 플러시 요청을 보내고 처리할 때 이 파일 구조가 표시되는 방식의 다이어그램입니다

![상태 수준이 표시된 상태](assets/disp-flushing/dispatcher-statfiles.png "dispatcher_statfiles")

### 파일 수준 시작

각 디렉토리에 `.stat` 파일이 있습니다.  플러시가 발생했음을 나타내는 표시입니다.  위의 예에서 `statfilelevel` 설정이 `3` 해당 팜 구성 파일 내부.

다음 `statfilelevel` 설정은 모듈 깊이가 트래버스되고 업데이트되는 폴더 수를 나타냅니다 `.stat` 파일.  .stat 파일이 비어 있으므로 데이터 스탬프가 있는 파일 이름 이상이며 수동으로 만들 수도 있지만 Dispatcher 서버의 명령줄에서 터치 명령을 실행합니다.

stat 파일 레벨 설정이 너무 높게 설정된 경우 각 초기화 요청은 디렉토리 트리 터치 시작 파일을 트래버스합니다.  이 경우 대용량 캐시 트리에서 주요 성능 히트가 될 수 있으며 Dispatcher의 전체 성능에 영향을 줄 수 있습니다.

이 파일 수준을 낮게 설정하면 플러시 요청이 의도한 것보다 더 많이 지워질 수 있습니다.  이렇게 되면 캐시에서 제공되는 요청이 적은 상태로 캐시가 더 자주 이탈되어 성능 문제가 발생할 수 있습니다.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

설정 `statfilelevel` 합리적인 수준으로.  폴더 구조를 보고 너무 많은 디렉터리를 트래버스하지 않고도 간결한 플러시를 허용하도록 설정되었는지 확인하십시오.   테스트하여 시스템의 성능 테스트 중에 요구 사항에 맞는지 확인합니다.

예를 들어 언어를 지원하는 사이트가 좋습니다.  일반적인 컨텐츠 트리에는 다음 디렉토리가 있습니다

`/content/brand1/en/us/`

이 예제에서는 stat 파일 레벨 4를 사용합니다.  이렇게 하면 아래에 있는 콘텐츠를 플러시할 때 보장됩니다 <b>`us`</b> 폴더가 전송되지 않으면 언어 폴더도 플러시되지 않습니다.
</div>

### 시작 파일 타임스탬프 핸드셰이크

컨텐츠에 대한 요청이 동일한 루틴으로 전달되면

1. 의 타임스탬프 `.stat` 파일은 요청된 파일의 타임스탬프와 비교됩니다
2. 만약 `.stat` 파일이 요청한 파일보다 최신 파일이며 캐시된 콘텐츠를 삭제하고 AEM에서 새 파일을 가져오고 캐시합니다.  그런 다음 컨텐츠를 제공합니다
3. 만약 `.stat` 파일이 요청된 파일보다 오래되면 파일이 새로 고침되어 컨텐츠를 제공할 수 있습니다.

### 캐시 핸드셰이크 - 예제 1

위의 예에서 컨텐츠에 대한 요청입니다 `/content/index.html`

시간 `index.html` 파일은 2019-11-01 6:21PM

가장 가까운 시간 `.stat` 파일은 2019-11-01 @ 12:22PM

위에서 읽은 내용을 이해하면 인덱스 파일이 `.stat` 파일 및 파일은 캐시에서 요청을 한 최종 사용자에게 제공됩니다

### 캐시 핸드셰이크 - 예제 2

위의 예에서 컨텐츠에 대한 요청입니다 `/content/dam/logo.jpg`

시간 `logo.jpg` 파일은 2019-10-31 @ 1:13PM

가장 가까운 시간 `.stat` 파일은 2019-11-01 @ 12:22PM

이 예제에서 알 수 있듯이 파일이 `.stat` 파일 및 가 제거되고, 캐시에서 파일을 대체하여 요청한 최종 사용자에게 제공되기 전에 새 파일을 AEM에서 가져옵니다.

## 팜 파일 설정

구성 옵션의 전체 세트에 대한 설명서는 여기에 있습니다. [https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache)

캐시 플러싱과 관련된 몇 가지 항목을 강조하려고 합니다

### 플러시 팜

두 가지 키가 있습니다 `document root` 작성자 및 게시자 트래픽의 파일을 캐시할 디렉토리입니다.  새 컨텐츠로 이러한 디렉토리를 최신 상태로 유지하려면 캐시를 플러시해야 합니다.  이러한 플러시 요청은 요청을 거부하거나 원치 않는 작업을 수행할 수 있는 일반적인 고객 트래픽 팜 구성과 얽히기를 원하지 않습니다.  대신 이 작업에 두 개의 플러시 팜을 제공합니다.

- `/etc/httpd.conf.d/available_farms/001_ams_author_flush_farm.any`
- `/etc/httpd.conf.d/available_farms/001_ams_publish_flush_farm.any`

이러한 팜 파일은 문서 루트 디렉토리를 플러시하기만 하면 됩니다.

```
/publishflushfarm {  
	/virtualhosts {
		"flush"
	}
	/cache {
		/docroot "${PUBLISH_DOCROOT}"
		/statfileslevel "${DEFAULT_STAT_LEVEL}"
		/rules {
			$include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any"
		}
		/invalidate {
			/0000 {
				/glob "*"
				/type "allow"
			}
		}
		/allowedClients {
			/0000 {
				/glob "*.*.*.*"
				/type "deny"
			}
			$include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any"
		}
	}
}
```

### 문서 루트

이 구성 항목은 팜 파일의 다음 섹션에 있습니다.

```
/myfarm { 
    /cache { 
        /docroot
```

Dispatcher가 캐시 디렉토리로 채우고 관리할 디렉토리를 지정합니다.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b>
이 디렉토리는 웹 서버가 사용하도록 구성된 도메인의 Apache 문서 루트 설정과 일치해야 합니다.

Apache 문서 루트의 하위 폴더에 있는 각 팜에 중첩된 docroot 폴더를 포함하는 것은 많은 이유로 인해 잘못된 생각입니다.
</div>

### 파일 수준 시작

이 구성 항목은 팜 파일의 다음 섹션에 있습니다.

```
/myfarm { 
    /cache { 
        /statfileslevel
```

이 설정은 `.stat` 초기화 요청이 들어올 때 파일을 생성해야 합니다.

`/statfileslevel` 문서 루트가 있는 다음 번호로 설정합니다. `/var/www/html/` 플러시할 때 다음과 같은 결과가 표시됩니다. `/content/dam/brand1/en/us/logo.jpg`

- 0 - 다음 통계 파일이 만들어집니다
   - `/var/www/html/.stat`
- 1 - 다음 통계 파일이 만들어집니다
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2 - 다음 통계 파일이 만들어집니다
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3 - 다음 상태 파일이 만들어집니다.
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4 - 다음 통계 파일이 만들어집니다.
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5 - 다음 상태 파일이 만들어집니다.
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>메모:</b>

타임스탬프 핸드셰이크가 발생하면 가장 가까운 곳을 찾습니다 `.stat` 파일.

사용 `.stat` 파일 수준 0 및 상태 파일은 `/var/www/html/.stat` 그 내용은 `/var/www/html/content/dam/brand1/en/us/` 가장 가까운 곳을 찾아 `.stat` 파일 및 최대 5개의 폴더를 트래버스하여 `.stat` 0에 있는 파일과 날짜를 비교합니다.  즉, 해당 수준의 높은 값에 한 플러시는 기본적으로 캐시된 모든 항목을 무효화합니다.
</div>

### 무효화 허용

이 구성 항목은 팜 파일의 다음 섹션에 있습니다.

```
/myfarm { 
    /cache { 
        /allowedClients {
```

이 구성 내에서 플러시 요청을 보낼 수 있는 IP 주소 목록을 배치하는 곳입니다.  플러시 요청이 Dispatcher에 들어오면 신뢰할 수 있는 IP에서 와야 합니다.  잘못 구성되었거나 신뢰할 수 없는 IP 주소로부터 플러시 요청을 보내는 경우 로그 파일에 다음 오류가 표시됩니다.

```
[Mon Nov 11 22:43:05 2019] [W] [pid 3079 (tid 139859875088128)] Flushing rejected from 10.43.0.57
```

### 무효화 규칙

이 구성 항목은 팜 파일의 다음 섹션에 있습니다.

```
/myfarm { 
    /cache { 
        /invalidate {
```

이러한 규칙은 일반적으로 초기화 요청으로 무효화할 수 있는 파일을 나타냅니다.

페이지 활성화로 중요한 파일이 무효화되지 않도록 무효화할 수 있고 수동으로 무효화해야 하는 파일을 지정하는 규칙을 재생할 수 있습니다.  다음은 html 파일만 무효화할 수 있는 구성 샘플 세트입니다.

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## 테스트 / 문제 해결

페이지를 활성화하고 페이지 활성화가 성공했다는 녹색 표시등이 나타나면 활성화한 컨텐츠가 캐시에서 플러시될 수도 있습니다.

페이지를 새로 고쳐서 이전 항목을 봅니다! what!? 초록빛이?!

몇 가지 수작업으로 플러시 프로세스를 진행하여 무엇이 잘못될 수 있는지에 대한 통찰력을 가져오겠습니다.  게시자 셸에서 curl을 사용하여 다음 초기화 요청을 실행합니다.

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "CQ-Path: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://<DISPATCHER IP ADDRESS>/dispatcher/invalidate.cache
```

테스트 초기화 요청 예

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/customer/en-us" \ 
-H "CQ-Path: /content/customer/en-us" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://169.254.196.222/dispatcher/invalidate.cache
```

Dispatcher에 요청 명령을 실행하면 로그에서 작업이 수행되고 `.stat files`.  로그 파일을 추적하면 다음 항목이 표시되어 디스패처 모듈에서 플러시 요청 히트를 확인합니다

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

이제 모듈이 선택되고 플러시 요청을 확인했으므로 이 요청이 어떻게 영향을 주는지 확인해야 합니다 `.stat` 파일.  다음 명령을 실행하고 다른 플러시를 실행할 때 타임스탬프 업데이트를 확인합니다.

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

명령에서 볼 수 있듯이 현재 타임스탬프를 출력합니다 `.stat` 파일

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

이제 플러시를 다시 실행하면 타임스탬프 업데이트를 볼 수 있습니다

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

콘텐츠 타임스탬프를 `.stat` 파일 타임스탬프

```
$ stat /mnt/var/www/html/content/customer/en-us/.stat 
  File: `.stat' 
  Size: 0           Blocks: 0          IO Block: 4096   regular empty file 
Device: ca90h/51856d    Inode: 17154125    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-13 16:22:31.000000000 -0400 
Modify: 2019-11-13 16:22:31.000000000 -0400 
Change: 2019-11-13 16:22:31.000000000 -0400 
 
$ stat /mnt/var/www/html/content/customer/en-us/logo.jpg 
File: `logo.jpg' 
  Size: 15856           Blocks: 32          IO Block: 4096   regular file 
Device: ca90h/51856d    Inode: 9175290    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-11 22:41:59.642450601 +0000 
Modify: 2019-11-11 22:41:59.642450601 +0000 
Change: 2019-11-11 22:41:59.642450601 +0000
```

타임스탬프를 보면 컨텐츠의 시간이 `.stat` 파일이 보다 최신이므로 모듈에서 캐시에서 파일을 제공하도록 하는 파일입니다. `.stat` 파일.

&quot;플러시&quot; 또는 대체되지 않는 이 파일의 타임스탬프를 업데이트한 내용이 있습니다.

[다음 -> 별칭 URL](./disp-vanity-url.md)