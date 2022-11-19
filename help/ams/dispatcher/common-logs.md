---
title: AEM Dispatcher 공통 로그
description: Dispatcher에서 제공하는 일반적인 로그 항목을 보고 이러한 항목이 무엇을 의미하는지, 어떻게 처리하는지 알아봅니다.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---


# 일반 로그

[목차](./overview.md)

[&lt;- 이전: 별칭 URL](./disp-vanity-url.md)

## 개요

일반적인 로그 항목 및 그 의미와 처리 방법에 대해 설명합니다.

## GLOB 경고

샘플 로그 항목:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Dispatcher 모듈 4.2.x부터 사람들이 필터 파일에서 다음 유형의 일치 항목을 사용하지 못하도록 하기 시작했습니다.

```
/0041 { /type "allow" /glob "* *.css *"   }
```

대신 다음과 같은 새 구문을 사용하는 것이 좋습니다.

```
/0041 { /type "allow" /url "*.css" }
```

또는 와일드카드 일치기에서 전혀 일치하지 않는 것이 좋습니다.

```
/0041 { /type "allow" /extension "css" }
```

제안된 방법 중 하나를 수행하면 로그에서 해당 오류 메시지가 제거됩니다.

## 필터 거부


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b>
로그 수준이 너무 낮게 설정된 경우 거부가 발생하는 경우에도 이러한 항목이 항상 표시되지 않습니다. 이 필드를 정보나 디버그로 설정하여 필터가 요청을 거부하는지 여부를 확인할 수 있습니다.
</div>

샘플 로그 항목:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

또는:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>주의:</b>

Dispatcher의 규칙이 해당 요청을 필터링하도록 설정되었음을 이해합니다. 이 경우 방문하려고 한 페이지가 의도적으로 거부되었으므로 이 작업을 수행하지 않을 것입니다.
</div>

로그가 다음 항목과 같은 경우:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

우리가 디자인 파일을 `.eot` 현재 차단되어 있으므로 문제를 해결하고 싶습니다.
따라서 필터 파일을 보고 다음 줄을 추가하여 `.eot` 파일스루

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

이렇게 하면 파일을 통해 진행할 수 있고 이 작업이 기록되지 않습니다.
필터링되는 내용을 보려면 로그 파일에서 이 명령을 실행할 수 있습니다.

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## 렌더링의 시간 초과

소켓 시간 제한 샘플 로그 항목:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

이 문제는 팜의 renders 섹션에 잘못된 IP 주소가 구성되어 있을 때 발생합니다. 해당 인스턴스나 AEM 인스턴스가 응답하거나 수신하는 것을 중지하여 Dispatcher가 해당 인스턴스에 연결할 수 없습니다.

방화벽 규칙을 확인하고 AEM 인스턴스가 실행 중이고 정상 상태인지 확인합니다.

게이트웨이 시간 초과 샘플 로그 항목:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

즉, AEM 인스턴스에 응답으로 도달하고 시간 초과될 수 있는 열린 소켓이 있었습니다. 즉, AEM 인스턴스가 너무 느리거나 건강하지 않으며 Dispatcher가 팜의 렌더링 섹션에 구성된 시간 제한 설정에 도달했음을 의미합니다. 시간 초과 설정을 늘리거나 AEM 인스턴스가 정상 상태가 되도록 합니다.

## 캐싱 수준

샘플 로그 항목:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

즉, 렌더링 수준과 캐시에서 가져온 가져오기가 측정됩니다. 캐시에서 80% 이상을 기록하려면 도움말을 따라야 합니다 [여기](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

이 번호를 가능한 한 높게 받으려면

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>참고:</b>
팜 파일에 캐시 설정을 사용하여 모든 항목을 캐싱하는 경우에도 너무 자주 플러싱하거나 너무 적극적으로 플러싱하면 캐시 적중률이 낮아집니다
</div>

## 디렉터리가 없습니다.

샘플 로그 항목:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

일반적으로 캐시 지우기가 동시에 발생하는 동안 항목을 가져오는 경우 표시됩니다.

또는 문서 루트 디렉토리에 대한 권한이 없거나 apache가 필요한 새 하위 디렉토리를 만들지 않는 폴더의 잘못된 SELinux 파일 컨텍스트가 있습니다.

권한 문제에 대해서는 설명서 루트의 권한을 보고 이 권한이 다음과 유사한지 확인합니다.

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## 별칭 URL을 찾을 수 없음

샘플 로그 항목:

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

이 오류는 Dispatcher에서 동적 자동 필터 허용 별칭 URL을 사용하도록 구성했지만 AEM 렌더러에 패키지를 설치하여 설정을 완료하지 않은 경우 발생합니다.

이 문제를 해결하려면 AEM 인스턴스에 별칭 url 기능 팩을 설치하고 익명 사용자가 이를 준비하도록 하십시오. 세부 사항 [여기](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

작동하는 별칭 URL은 다음과 같습니다.

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## 팜이 없습니다.

샘플 로그 항목:

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

이 오류는 모든 팜 파일에서 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 에서 일치하는 항목을 찾지 못했습니다. `/virtualhost` 섹션을 참조하십시오.

팜 파일은 요청이 들어 온 도메인 이름 또는 경로를 기반으로 하여 트래픽과 일치합니다. glob 일치를 사용하고 일치하지 않으면 팜을 제대로 구성하지 않았거나 팜에 항목을 입력하거나 항목이 완전히 누락되었습니다. 팜이 항목과 일치하지 않으면 기본적으로 포함된 팜 파일 스택에 포함된 마지막 팜으로 설정됩니다. 이 예에서 `999_ams_publish_farm.any` 이 이름은 publishfarm의 일반 이름입니다.

다음은 팜 파일의 예입니다 `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` 관련 부품을 강조하도록 축소되었습니다.

## 제공된 품목

샘플 로그 항목:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

컨텐츠의 GET http 메서드를 통해 페이지를 가져왔습니다 `/content/we-retail/us/en.html` 그리고 24034 밀리초가 걸렸습니다. 우리가 주목하고 싶었던 부분은 바로 그 끝이다 `publishfarm/0`. 타깃팅되고 과 일치하는지 확인할 수 있습니다. `publishfarm`. 요청을 렌더링 0에서 가져왔습니다. 즉, 이 페이지는 AEM에서 요청하여 캐시해야 합니다. 이제 이 페이지를 다시 요청하여 로그에 나타나는 결과를 보겠습니다.

[다음 -> 읽기 전용 파일](./immutable-files.md)