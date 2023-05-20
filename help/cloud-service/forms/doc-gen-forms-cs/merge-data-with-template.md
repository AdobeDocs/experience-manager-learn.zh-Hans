---
title: 將資料與XDP範本合併
description: 使用必要的引數向端點發出POST要求
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 8185
thumbnail: 332439.jpg
exl-id: d144b3f6-7c7a-46a7-bc5f-1767895749d0
source-git-commit: 47d36e472719049de1346c5f0bba010c9af4e039
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# 進行POST呼叫


下一步是使用必要的引數向端點進行HTTPPOST呼叫。 範本和資料檔會以資源檔的形式提供。 產生的pdf屬性是透過請求中的選項引數指定的。embedFonts屬性用於將自訂字型嵌入產生的pdf中。[請依照此檔案將自訂字型部署至您的Forms雲端例項。](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-set-up.html?lang=en) 屬性是在options.json資源檔案中指定。 由於端點具有權杖型驗證，因此我們會在請求標頭中傳遞存取權杖。

下列程式碼用於將資料與範本合併，以產生pdf

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
