---
title: 使用Repo工具设置IntelliJ
description: 准备IntelliJ以与AEM云就绪实例同步
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8844
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# 安装Cygwin


Cygwin是一个与POSIX兼容的编程和运行时环境，在Microsoft Windows本地运行。
安装 [齐格温](https://www.cygwin.com/). 我已安装在C:\cygwin64 folder中
>[!NOTE]
> 确保与cygwin安装一起安装zip、unzip、curl、rsync包

在c:\cloudmanager文件夹下创建一个名为adoberepo的文件夹。

[安装存储库工具].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing)repo工具只是复制repo文件并将其放置在您的c:\cloudmanger\adoberepo folder中。

将以下内容添加到路径环境变量C:\cygwin64\bin;C:\CloudManager\adoberepo;

## 设置外部工具

* 启动IntelliJ
* 按Ctrl+Alt+S键以启动设置窗口。
* 选择“工具” — >“外部工具”，然后单击“+”符号，然后输入以下内容，如屏幕快照中所示。
   ![rep](assets/repo.png)
* 在“组”下拉字段中键入“repo”，并且您创建的所有命令都属于 **存储库** 群组


**Put命令**
**项目**:C:\cygwin64\bin\bash
**参数**:-l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**工作目录**:\$ProjectFileDir\$
![put-command](assets/put-command.png)

**获取命令**
**项目**:C:\cygwin64\bin\bash
**参数**:-l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**工作目录**:\$ProjectFileDir\$
![get-command](assets/get-command.png)

**状态命令**
**项目**:C:\cygwin64\bin\bash
**参数**:-l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**工作目录**:\$ProjectFileDir\$
![status-command](assets/status-command.png)

**差异命令**
**项目**:C:\cygwin64\bin\bash
**参数**:-l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**工作目录**:\$ProjectFileDir\$
![diff命令](assets/diff-command.png)

从中提取.repo文件 [repo.zip](assets/repo.zip) 并将其放在AEM项目根文件夹中。 (C:\CloudManager\aem-banking-application)。 打开.repo文件，并确保服务器和凭据设置与您的环境相匹配。
打开.gitignore文件，向文件底部添加以下内容，然后保存更改\# repo .repo

在aem-banking-application项目中选择任何项目，如ui.content和右键单击，此时您应会看到repo选项，而在repo选项下，您将看到我们之前添加的4个命令。

## 设置AEM创作实例

可以按照以下步骤在本地系统上快速设置云就绪实例。
* [下载最新的AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [下载最新的AEM Forms Addon](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* 创建以下文件夹结构c:\aemformscs\aem-sdk\author

* 从AEM SDK zip文件中提取aem-sdk-quickstart-xxxxxxx.jar文件，并将其放在c:\aemformscs\aem-sdk\author folder.Rename jar文件中，转到aem-author-p4502.jar

* 打开命令提示符，然后导航到c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui。 这将开始安装AEM。
* 使用管理员/管理员凭据登录
* 停止AEM实例
* 创建以下文件夹结构。C:\aemformscs\aem-sdk\author\crx-quickstart\install
* 将aem-forms-addon-xxxxx.far复制到安装文件夹中
* 打开命令提示符，然后导航到c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui。 这将在您的AEM实例中部署Forms Add on包。



















