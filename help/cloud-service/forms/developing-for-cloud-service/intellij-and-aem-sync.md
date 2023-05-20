---
title: 使用存放庫工具設定IntelliJ
description: 準備您的IntelliJ以與AEM cloud ready執行個體同步
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# 安裝Cygwin


Cygwin是相容於POSIX的程式設計和執行階段環境，可在Microsoft Windows上原生執行。
安裝 [Cygwin](https://www.cygwin.com/). 我已經安裝在C:\cygwin64資料夾中
>[!NOTE]
> 請務必安裝zip、unzip、curl、rsync套件與cygwin安裝

在c：\cloudmanager下建立名為adoberepo的資料夾。

[安裝存放庫工具].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing repo tool只是複製repo檔案，並將其置於c：\cloudmanger\adoberepo資料夾中。

將下列專案新增至路徑環境變數C:\cygwin64\bin；C:\CloudManager\adoberepo；

## 設定外部工具

* 啟動IntelliJ
* 按Ctrl+Alt+S鍵以啟動設定視窗。
* 選取「工具」 — >「外部工具」，然後按一下+符號並輸入下列專案，如熒幕擷取畫面所示。
   ![rep](assets/repo.png)
* 在「群組」下拉式欄位中輸入「repo」，確保您建立名為repo的群組，且您建立的所有命令都屬於 **存放庫** 群組


**Put命令**
**計畫**： C:\cygwin64\bin\bash
**引數**： -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**工作目錄**： \$ProjectFileDir\$
![put-command](assets/put-command.png)

**取得命令**
**計畫**： C:\cygwin64\bin\bash
**引數**： -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**工作目錄**： \$ProjectFileDir\$
![get-command](assets/get-command.png)

**狀態命令**
**計畫**： C:\cygwin64\bin\bash
**引數**： -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**工作目錄**： \$ProjectFileDir\$
![status-command](assets/status-command.png)

**差異命令**
**計畫**： C:\cygwin64\bin\bash
**引數**： -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**工作目錄**： \$ProjectFileDir\$
![diff-command](assets/diff-command.png)

從擷取.repo檔案 [repo.zip](assets/repo.zip) 並將其放置在您的AEM專案根資料夾中。 (C:\CloudManager\aem-banking-application)。 開啟.repo檔案，並確認伺服器和認證設定符合您的環境。
開啟.gitignore檔案，並將下列內容加入至檔案底部，然後儲存變更\# repo .repo

選取aem-banking-application專案中的任何專案（例如ui.content），然後按一下滑鼠右鍵，您應該會看到repo選項，而在repo選項下方，您會看到我們先前新增的4個命令。

## 設定AEM作者執行個體

您可以依照下列步驟操作，在本機系統上快速設定雲端就緒執行個體。
* [下載最新的AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [下載最新的AEM Forms附加元件](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* 建立下列資料夾結構c：\aemformscs\aem-sdk\author

* 從AEM SDK zip檔案中解壓縮aem-sdk-quickstart-xxxxxxx.jar檔案，並將其置於c：\aemformscs\aem-sdk\author資料夾中。將jar檔案重新命名為aem-author-p4502.jar

* 開啟命令提示字元並瀏覽至c：\aemformscs\aem-sdk\author輸入以下命令java -jar aem-author-p4502.jar -gui。 這樣會開始安裝AEM。
* 使用管理員/管理員憑證登入
* 停止AEM執行個體
* 建立下列資料夾結構。C:\aemformscs\aem-sdk\author\crx-quickstart\install
* 將aem-forms-addon-xxxxxx.far複製到安裝資料夾
* 開啟命令提示字元並瀏覽至c：\aemformscs\aem-sdk\author輸入以下命令java -jar aem-author-p4502.jar -gui。 這會在您的AEM執行個體中部署表單附加套件。
