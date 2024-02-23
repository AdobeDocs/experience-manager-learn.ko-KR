---
title: AEM Dispatcher 공통 로그
description: Dispatcher의 공통 로그 항목을 살펴보고 그 의미와 해결 방법을 알아봅니다.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 252
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: tm+mt
source-wordcount: '796'
ht-degree: 0%

---

# 공통 로그

[목차](./overview.md)

[&lt;- 이전: 별칭 URL](./disp-vanity-url.md)

## 개요

표시되는 공통 로그 항목과 그 의미 및 처리 방법에 대해 설명합니다.

## GLOB 경고

샘플 로그 항목:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Dispatcher 모듈 4.2.x 이후 필터 파일에서 다음 유형의 일치 항목을 사용하지 않도록 권장하기 시작했습니다.

```
/0041 { /type "allow" /glob "* *.css *"   }
```

대신 다음과 같은 새 구문을 사용하는 것이 좋습니다.

```
/0041 { /type "allow" /url "*.css" }
```

와일드카드 선택기와 전혀 일치하지 않는 것이 좋습니다.

```
/0041 { /type "allow" /extension "css" }
```

제안된 방법 중 하나를 수행하면 로그에서 해당 오류 메시지가 제거됩니다.

## 필터 거부

>[!NOTE]
>
>로그 수준이 너무 낮게 설정된 경우에는 거부가 발생하더라도 이러한 항목이 항상 표시되는 것은 아닙니다. 필터를 정보 또는 디버그로 설정하여 필터가 요청을 거부하는지 확인할 수 있습니다.

샘플 로그 항목:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

또는:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

>[!CAUTION]
>
>Dispatcher의 규칙이 해당 요청을 필터링하도록 설정되었습니다. 이 경우 방문하려는 페이지가 의도적으로 거부되므로 이 작업을 더 이상 수행하지 않겠습니다.

로그가 다음 항목과 같이 표시되는 경우:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

우리의 디자인 파일이 `.eot` 이(가) 차단되고 있으므로 수정하겠습니다.
따라서 필터 파일을 보고 다음 줄을 추가하여 허용해야 합니다. `.eot` 다음을 통해 파일:

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

이렇게 하면 파일이 허용되고 로깅이 중지됩니다.
필터링된 내용을 보려면 로그 파일에서 이 명령을 실행할 수 있습니다.

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## 렌더링 제한 시간

소켓 시간 초과 샘플 로그 항목:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

이 문제는 팜의 렌더링 섹션에 잘못된 IP 주소가 구성된 경우 발생합니다. 이 인스턴스 또는 AEM 인스턴스가 응답하지 않거나 수신하지 않으므로 Dispatcher가 연결할 수 없습니다.

방화벽 규칙을 확인하고 AEM 인스턴스가 실행 중이며 정상인지 확인합니다.

게이트웨이 시간 초과 샘플 로그 항목:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

즉, AEM 인스턴스에 열려 있는 소켓이 있으므로 연결할 수 있으며 응답 시간이 초과됩니다. 즉, AEM 인스턴스가 너무 느리거나 비정상이며 Dispatcher가 팜의 렌더링 섹션에서 구성된 시간 초과 설정에 도달했습니다. 시간 초과 설정을 늘리거나 AEM 인스턴스가 정상이 되도록 합니다.

## 캐싱 수준

샘플 로그 항목:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

렌더링 수준에서 가져오기와 캐시에서 가져오기가 측정되었음을 의미합니다. 캐시에서 80% 이상을 달성하려면 도움말을 따라야 합니다 [여기](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

이 숫자를 가능한 한 높게 지정합니다.

>[!NOTE]
>
>팜 파일에 모든 것을 캐시하는 캐시 설정이 있더라도 너무 자주 또는 너무 공격적으로 초기화할 수 있으며, 이로 인해 캐시 적중률 비율이 낮을 수 있습니다.

## 디렉토리 누락

샘플 로그 항목:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

이 로그는 일반적으로 캐시 지우기와 항목 가져오기가 동시에 발생할 때 표시됩니다.

이 디렉토리나 문서 루트 디렉토리에 이에 대한 잘못된 권한이 있거나 apache가 필요한 새 하위 디렉토리를 만드는 것을 거부하는 잘못된 SELinux 파일 컨텍스트가 폴더에 있습니다.

권한 문제에 대해서는 문서 루트의 권한을 보고 다음과 비슷한지 확인합니다.

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

이 오류는 vanity URL을 허용하는 동적 자동 필터를 사용하도록 Dispatcher를 구성했지만 AEM 렌더러에서 패키지를 설치하여 설정을 완료하지 않은 경우 발생합니다.

이 문제를 해결하려면 AEM 인스턴스에 vanity url 기능 팩을 설치하고 익명 사용자가 준비할 수 있도록 허용하십시오. 세부 사항 [여기](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

작동 중인 별칭 URL 설정은 다음과 같습니다.

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## 팜 누락

샘플 로그 항목:

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

이 오류는에서 사용할 수 있는 모든 팜 파일의 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 다음에서 일치하는 항목을 찾을 수 없습니다. `/virtualhost` 섹션.

팜 파일은 요청이 들어 온 도메인 이름 또는 경로를 기반으로 트래픽과 일치합니다. glob 일치를 사용하며, 일치하지 않으면 팜을 제대로 구성하지 않았거나 팜에 입력한 항목에 오타가 있거나, 항목이 완전히 누락된 것입니다. 팜이 항목과 일치하지 않으면 팜 파일 스택에 포함된 마지막 팜이 기본값으로 지정됩니다. 이 예에서는 다음과 같습니다. `999_ams_publish_farm.any` publishfarm의 일반 이름으로 이름이 지정됩니다.

다음은 팜 파일의 예입니다 `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` 관련 부분을 강조하기 위해 축소했습니다.

## 다음에서 제공된 항목

샘플 로그 항목:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

콘텐츠에 대한 GET http 메서드를 통해 페이지를 가져왔습니다 `/content/we-retail/us/en.html` 24034밀리초가 걸렸습니다. 우리가 주목하고 싶었던 부분은 맨 끝에 있다 `publishfarm/0`. 타겟팅하고 일치하는 항목을 확인할 수 있습니다. `publishfarm`. 요청은 렌더링 0에서 가져왔습니다. 즉, 이 페이지는 AEM에서 요청한 다음 캐시되어야 합니다. 이제 이 페이지를 다시 요청하고 로그가 어떻게 되는지 살펴보겠습니다.

[다음 -> 읽기 전용 파일](./immutable-files.md)
