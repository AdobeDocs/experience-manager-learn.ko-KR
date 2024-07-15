---
title: XDP 템플릿과 데이터 병합
description: 필요한 매개 변수를 사용하여 끝점에 대한 POST 요청 만들기
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-8185
thumbnail: 332439.jpg
exl-id: d144b3f6-7c7a-46a7-bc5f-1767895749d0
duration: 49
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# POST 호출 만들기


다음 단계는 필요한 매개 변수를 사용하여 끝점에 대한 HTTP POST 호출을 수행하는 것입니다. 템플릿과 데이터 파일은 리소스 파일로 제공됩니다. 생성된 pdf의 속성은 요청의 옵션 매개 변수를 통해 지정됩니다. embedFonts 속성은 생성된 pdf에 사용자 정의 글꼴을 포함하는 데 사용됩니다.[이 설명서에 따라 사용자 지정 글꼴을 Forms 클라우드 인스턴스에 배포하십시오.](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-set-up.html?lang=en) 속성이 options.json 리소스 파일에 지정되어 있습니다. 따라서 끝점에는 요청 헤더에 액세스 토큰을 전달하는 토큰 기반 인증이 있습니다.

다음 코드는 데이터를 템플릿과 병합하여 pdf를 생성하는 데 사용되었습니다

```java
public class DocumentGeneration
{
        public String SAVE_LOCATION = "c:\\aspire1";
        public void mergeDataWithXdpTemplate(String postURL)
        {
                HttpPost httpPost = new HttpPost(postURL);
                CredentialUtilites cu = new CredentialUtilites();
                String accessToken = cu.getAccessToken();
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
                URL templateFile = classLoader.getResource("templates/custom_fonts.xdp");
                File xdpTemplate = new File(templateFile.getPath());
                URL url = classLoader.getResource("datafiles");
                System.out.println(url.getPath());
                File files[] = new File(url.getPath()).listFiles();
                for (int i = 0; i < files.length; i++) {
                        MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                        ContentType strContent = ContentType.create("text/plain", Charset.forName("UTF-8"));
                        builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
                        builder.addBinaryBody("data", files[i]);
                        builder.addBinaryBody("template", xdpTemplate);
                        builder.addBinaryBody("options",GetOptions.getPDFOptions().getBytes(),ContentType.APPLICATION_JSON,"options"
                        try {
                                HttpEntity entity = builder.build();
                                httpPost.setEntity(entity);
                                CloseableHttpClient httpclient = HttpClients.createDefault();
                                CloseableHttpResponse response = httpclient.execute(httpPost);
                                InputStream generatedPDF = response.getEntity().getContent();
                                byte[] bytes = IOUtils.toByteArray(generatedPDF);
                                File saveLocation = new File(SAVE_LOCATION);
                                if (!saveLocation.exists()) {
                                        saveLocation.mkdirs();
                                }
                                File outputFile = new File(SAVE_LOCATION+File.separator+files[i].getName().replace("xml", "pdf"));
                                try (FileOutputStream outputStream = new FileOutputStream(outputFile)) {
                                        outputStream.write(bytes);
                                }
                        } catch (Exception e) {
                                System.out.println("The error is " + e.getMessage());
                        }

                }
                System.out.println("Done generating " + files.length + " files");

        }

}
```
