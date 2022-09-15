---
title: 큰 xml 데이터 파일을 xdp 템플릿과 병합할 때 시간 초과 오류를 수정합니다
description: AEM Forms에서 큰 xml 파일을 템플릿과 병합
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# 큰 xml 데이터 파일을 xdp 템플릿에 병합하여 pdf 파일을 만들 수 있도록 하는 방법

큰 xml 데이터 파일을 xdp 템플릿과 병합할 때 로그 파일에 다음 오류가 표시될 수 있습니다

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

위의 오류를 수정하려면 다음을 수행하십시오

## 행 제한 시간 변경

* AEM 서버 중지
* 라는 폴더를 만듭니다. **설치** AEM 설치의 crx-quickstart 폴더 아래에 있습니다.
* 라는 파일을 만듭니다. **org.apache.aries.transaction.config** install 폴더에 다음 컨텐츠 aries.transaction.timeout=&quot;1200&quot;을 추가합니다. 요구 사항에 따라 시간 초과 값을 변경할 수 있습니다. 시간 초과 값은 초 단위입니다

>[!NOTE]
> org.apache.aries.transaction 구성을 만들면 [configMgr](http://localhost:4502/system/console/configMgr) 파일을 편집하는 대신


## Jacorb ORB 공급자 설정 변경

* [OSGi ConfigMgr 을 엽니다.](http://localhost:4502/system/console/configMgr)
* 검색 대상 **Jacorb ORB 공급자**
* 다음 항목을 추가합니다. jacorb.connection.client.pending_reply_timeout=600000 위의 설정은 보류 중인 회신 시간 초과(CORBA 클라이언트 시간 초과라고도 함)를 600초로 설정합니다.
* 변경 내용을 저장합니다
