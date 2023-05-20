---
title: 進階網路
description: 瞭解AEMas a Cloud Service的進階網路選項。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 3%

---

# 進階網路

AEMas a Cloud Service提供進階網路功能，可精確管理與AEMas a Cloud Service程式的連線與來往連線。

|  | [生产程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [沙盒程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| 支援進階網路 | ✔ | ✘ |


AEM進階網路由三個選項組成，用於管理與外部服務的連線。 Cloud Manager程式及其AEMas a Cloud Service環境一次只能使用單一型別的進階網路設定，因此請確定已選取最適合的型別。

|  | 標準連線埠上的HTTP/HTTPS | 非標準連線埠上的HTTP/HTTPS | 非HTTP/HTTPS連線 | 專用輸出IP | 「無代理主機」清單 | 連線到受VPN保護的服務 | 依IP限制AEM發佈流量 |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __無進階網路__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__彈性的連線埠輸出__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__專用輸出IP位址__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__虛擬私人網路__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


如需選取適當進階網路型別時所需考量的詳細資訊，請參閱 [進階網路檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## 進階網路教學課程

根據貴組織的需求，找出最適合的進階網路選項後，請按一下下方對應的教學課程，以取得逐步指示和程式碼範例。

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="彈性的連線埠輸出" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">彈性的連線埠輸出</a></strong></div>
      <p>
          允許非標準連線埠上的輸出AEMas a Cloud Service流量。
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="檔案專用輸出IP位址" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">專用輸出IP位址</a></strong></div>
      <p>
        從專用IP產生傳出AEMas a Cloud Service流量。
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="虚拟专用网络 (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">虚拟专用网络 (VPN)</a></strong></div>
      <p>
        保護客戶或廠商基礎架構與AEMas a Cloud Service之間的流量。
      </p>
    </td>   
  </tr>
</table>

## 程式碼範例

此集合提供針對特定使用案例運用進階網路功能所需的設定和程式碼範例。

確保適當的 [進階網路設定](#advanced-networking) 已在進行這些教學課程之前設定。

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="虚拟专用网络 (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子郵件服務</a></strong></div>
      <p>
        使用AEM連線至外部電子郵件服務的OSGi設定範例。
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Java™程式碼範例使用HTTP/HTTPS通訊協定，使從AEM的HTTP/HTTPS連線as a Cloud Service到外部服務。
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL連線" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL連線</a></strong></div>
      <p>
            Java™程式碼範例透過設定AEM JDBC資料來源集區來連線到外部SQL資料庫。
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL連線" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL連線</a></strong></div>
      <p>
            使用Java™的SQL API連線至外部SQL資料庫的Java™程式碼範例。
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="套用IP允許清單" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">套用IP允許清單</a></strong></div>
      <p>
            設定IP允許清單，以便只有VPN流量可以存取AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM Publish的路徑型VPN存取限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">AEM Publish的路徑型VPN存取限制</a></strong></div>
      <p>
            AEM Publish上的特定路徑需要VPN存取。
      </p>
    </td>
</tr>
</table>
