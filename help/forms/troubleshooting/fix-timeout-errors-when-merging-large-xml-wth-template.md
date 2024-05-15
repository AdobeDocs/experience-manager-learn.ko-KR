---
title: 대용량 xml 데이터 파일을 xdp 템플릿과 병합할 때 시간 초과 오류 해결
description: AEM Forms에서 큰 xml 파일을 템플릿과 병합
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 37
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# 큰 xml 데이터 파일을 xdp 템플릿과 병합하여 pdf 파일 작성을 활성화하는 방법

큰 xml 데이터 파일을 xdp 템플릿과 병합할 때 로그 파일에 다음 오류가 표시될 수 있습니다

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

위의 오류를 해결하려면 다음을 수행하십시오

## aries 시간 제한 변경

* AEM 서버 중지
* 라는 폴더 만들기 **설치** AEM 설치의 crx-quickstart 폴더 아래에
* 라는 파일 만들기 **org.apache.aries.transaction.config** 설치 폴더 아래에 다음 content aries.transaction.timeout=&quot;1200&quot;이 표시됩니다. 요구 사항에 따라 시간 초과 값을 변경할 수 있습니다. 시간 제한 값은 초 단위입니다.

>[!NOTE]
> org.apache.aries.transaction 구성을 생성하면에 트랜잭션 시간 초과 값을 편집할 수 있습니다. [configMgr](http://localhost:4502/system/console/configMgr) 파일을 편집하는 대신


## Jacorb ORB 공급자 설정 변경

* [OSGi ConfigMgr을 엽니다.](http://localhost:4502/system/console/configMgr)
* 검색 대상 **Jacorb ORB 공급자**
* 다음 항목 jacorb.connection.client.pending_reply_timeout=600000 위의 설정은 보류 중인 응답 시간 제한(CORBA 클라이언트 시간 제한이라고도 함)을 600초로 설정합니다.
* 변경 사항 저장
