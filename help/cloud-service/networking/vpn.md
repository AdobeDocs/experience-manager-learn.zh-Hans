---
title: 虚拟专用网络 (VPN)
description: 瞭解如何將AEMas a Cloud Service與您的VPN連線，以建立AEM與內部服務之間的安全通訊通道。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1318'
ht-degree: 5%

---

# 虚拟专用网络 (VPN)

瞭解如何將AEMas a Cloud Service與您的VPN連線，以建立AEM與內部服務之間的安全通訊通道。

## 什麼是虛擬私人網路？

虛擬私人網路(VPN)可讓AEMas a Cloud Service的客戶連線 **AEM環境** 在Cloud Manager計畫內到現有的、 [支援](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) VPN。 如此一來，AEMas a Cloud Service與客戶網路內的服務之間就能建立安全且受控制的連線。

Cloud Manager計畫只能有 __單一__ 網路基礎架構型別。 確定虛擬私人網路最多 [適當型別的網路基礎結構](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#general-vpn-considerations) 的AEMas a Cloud Service。

>[!NOTE]
>
>請注意，不支援將組建環境從Cloud Manager連線至VPN。 如果您必須從私人存放庫存取二進位成品，則必須使用公開網際網路上可用的URL來設定安全且受密碼保護的存放庫 [如此處所述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> 閱讀AEMas a Cloud Service [進階網路設定檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) 以取得虛擬私人網路的詳細資訊。

## 前提条件

設定虛擬私人網路時需要下列專案：

+ Adobe帳戶 [Cloud Manager企業所有者許可權](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 存取 [Cloud Manager API的驗證認證](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織ID （亦稱為IMS組織ID）
   + 使用者端ID （亦稱為API金鑰）
   + 存取權杖（亦稱為持有人權杖）
+ Cloud Manager計畫ID
+ Cloud Manager環境ID
+ 虛擬私人網路，可存取所有必要的連線引數。

如需更多詳細資訊，請觀看以下逐步解說，瞭解如何設定、設定和取得Cloud Manager API認證，以及如何使用它們進行Cloud Manager API呼叫。

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

本教學課程使用 `curl` 以進行Cloud Manager API設定。 提供的 `curl` 命令會採用Linux/macOS語法。 如果使用Windows命令提示字元，請將 `\` 換行字元 `^`.

## 為每個程式啟用虛擬私人網路

首先在AEMas a Cloud Service上啟用「虛擬私人網路」。

1. 首先，使用Cloud Manager API判斷需要進階網路的地區 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 此 `region name` 進行後續Cloud Manager API呼叫所必需。 通常會使用生產環境所在的區域。

   尋找您的AEMas a Cloud Service環境所在地區 [Cloud Manager](https://my.cloudmanager.adobe.com) 在 [環境的詳細資訊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager中顯示的區域名稱可以是 [已對應至地區代碼](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) 用於Cloud Manager API。

   __listRegions HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 使用Cloud Manager API為Cloud Manager計畫啟用虛擬私人網路 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 使用適當的 `region` 從Cloud Manager API取得的程式碼 `listRegions` 作業。

   __createNetworkInfrastructure HTTP要求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   在中定義JSON引數 `vpn-create.json` 並提供給curl，透過 `... -d @./vpn-create.json`.

   [下載範例vpn-create.json](./assets/vpn-create.json).  此檔案只是一個範例。 根據以下網址記錄的選用/必填欄位，視需要設定您的檔案： [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   等待45到60分鐘，讓Cloud Manager計畫布建網路基礎結構。

1. 檢查環境是否已完成 __虛擬私人網路__ 使用雲端管理員API進行設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 作業，使用 `id` 從上一步驟中的createNetworkInfrastructure HTTP要求傳回。

   __getNetworkInfrastructure HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   確認HTTP回應包含 __狀態__ 之 __就緒__. 如果尚未準備就緒，請每隔幾分鐘重新檢查一次狀態。

## 為每個環境設定虛擬私人網路代理

1. 啟用並設定 __虛擬私人網路__ 使用Cloud Manager API在每個AEMas a Cloud Service環境中進行設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP要求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   在中定義JSON引數 `vpn-configure.json` 並提供給curl，透過 `... -d @./vpn-configure.json`.

[下載範例vpn-configure.json](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   `nonProxyHosts` 宣告應透過預設共用IP位址範圍而不是專用輸出IP路由連線埠80或443的一組主機。 `nonProxyHosts` 當Adobe可進一步自動最佳化通過共用IP的流量時，這可能很有用。

   針對每個 `portForwards` 對應，進階網路會定義下列轉送規則：

   | Proxy主機 | Proxy連線埠 |  | 外部主機 | 外部連線埠 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署 __僅限__ 需要外部服務的HTTP/HTTPS連線，請將 `portForwards` 陣列空白，因為只有非HTTP/HTTPS請求才需要這些規則。


1. 對於每個環境，使用Cloud Manager API的 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP要求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 可使用Cloud Manager API的虛擬私人網路Proxy設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 作業。 記住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 作業，因此每次呼叫此作業時，都必須提供所有規則。

1. 現在您可以在自訂AEM程式碼和設定中使用虛擬私人網路輸出設定。

## 透過虛擬私人網路連線到外部服務

啟用「虛擬私人網路」後，AEM程式碼和設定便可使用它們透過VPN呼叫外部服務。 AEM會針對兩種風格的外部呼叫採取不同的處理方式：

1. 對外部服務的HTTP/HTTPS呼叫
   + 包含對標準80或443連線埠以外的連線埠上執行的服務發出的HTTP/HTTPS呼叫。
1. 對外部服務的非HTTP/HTTPS呼叫
   + 包含任何非HTTP呼叫，例如與Mail伺服器、SQL資料庫或在其他非HTTP/HTTPS通訊協定上執行的服務的連線。

預設允許來自標準連線埠(80/443)上AEM的HTTP/HTTPS請求，但如果未正確設定，則不會使用VPN連線，如下所述。

### HTTP/HTTPS

從AEM建立HTTP/HTTPS連線時，使用VPN時，會自動從AEM代理HTTP/HTTPS連線。 不需要其他程式碼或設定即可支援HTTP/HTTPS連線。

>[!TIP]
>
> 請參閱AEMas a Cloud Service的虛擬私人網路檔案，以瞭解 [完整的路由規則集](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### 程式碼範例

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Java™程式碼範例使用HTTP/HTTPS通訊協定，使從AEM的HTTP/HTTPS連線as a Cloud Service到外部服務。
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### 非HTTP/HTTPS連執行緒式碼範例

建立非HTTP/HTTPS連線時(例如 AEM SQL、SMTP等)，必須透過AEM提供的特殊主機名稱建立連線。

|變數名稱 |使用 | Java™程式碼 | OSGi設定 | | - | - | - | - | | `AEM_PROXY_HOST` |非HTTP/HTTPS連線的Proxy主機 | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


然後，會透過呼叫外部服務的連線。 `AEM_PROXY_HOST` 和對應的連線埠(`portForwards.portOrig`)，則AEM會路由至對應的外部主機名稱(`portForwards.name`)和連線埠(`portForwards.portDest`)。

| Proxy主機 | Proxy連線埠 |  | 外部主機 | 外部連線埠 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### 程式碼範例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連線" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL連線</a></strong></div>
      <p>
            Java™程式碼範例透過設定AEM JDBC資料來源集區來連線到外部SQL資料庫。
      </p>
    </td>
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL連線" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL連線</a></strong></div>
      <p>
            使用Java™的SQL API連線至外部SQL資料庫的Java™程式碼範例。
      </p>
    </td>
   <td>
      <a  href="./examples/email-service.md"><img alt="虚拟专用网络 (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子郵件服務</a></strong></div>
      <p>
        使用AEM連線至外部電子郵件服務的OSGi設定範例。
      </p>
    </td>
</tr></table>

### 限制透過VPN存取AEMas a Cloud Service

虛擬私人網路設定會限制VPN對AEMas a Cloud Service環境的存取。

#### 設定範例

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="套用IP允許清單" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">套用IP允許清單</a></strong></div>
      <p>
            設定IP允許清單，以便只有VPN流量可以存取AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM Publish的路徑型VPN存取限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">AEM Publish的路徑型VPN存取限制</a></strong></div>
      <p>
            AEM Publish上的特定路徑需要VPN存取。
      </p>
    </td>
   <td></td>
</tr></table>
