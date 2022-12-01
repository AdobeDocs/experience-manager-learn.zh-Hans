---
title: 正确将符号链接添加到GIT
description: 有关在处理Dispatcher配置时如何以及在何处添加符号链接的说明。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 0%

---


# 将符号链接添加到GIT

[目录](./overview.md)

[&lt; — 上一个：调度程序运行状况检查](./health-check.md)

在AMS中，您将获得一个预填充的GIT存储库，其中包含调度程序的源代码已成熟，可供您开始开发和自定义。

在您创建 `.vhost` 文件或顶级 `farm.any` 文件，您需要从 `available_*` 目录 `enabled_*` 目录访问Advertising Cloud的帮助。 使用正确的链接类型将是成功通过Cloud Manager管道部署的关键。 本页将帮助您了解如何执行此操作。

## 调度程序原型

AEM开发人员通常从 [AEM原型](https://github.com/adobe/aem-project-archetype)

以下是源代码区域的示例，您可以在其中查看使用的符号链接：

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

例如， `/etc/httpd/conf.d/available_vhosts/` 目录包含暂存的潜在值 `.vhost` 文件。

已启用 `.vhost` 文件将显示为相对路径 `symlinks` 内部 `/etc/httpd/conf.d/enabled_vhosts/` 目录访问Advertising Cloud的帮助。

## 创建符号链接

我们使用指向文件的符号链接，以便Apache Webserver将将目标文件视为同一文件。  我们不想在两个目录中复制文件。  相反，它只是一个从一个目录（符号链接）到另一个目录的快捷方式。

确认已部署的配置将针对Linux主机。  创建与目标系统不兼容的符号链接将导致失败和不需要的结果。

如果您的工作站不是Linux计算机，您可能会想知道使用哪些命令正确创建这些链接，以便它们能够将它们提交到GIT中。

> `TIP:` 使用相对链接很重要，因为如果您安装了Apache Webserver的本地副本，并且具有不同的安装库，则链接仍然有效。  如果您使用绝对路径，则工作站或其他系统必须匹配相同的目录结构。

### OSX / Linux

符号链接是这些操作系统的本机链接，以下是如何创建这些链接的一些示例。  打开您最喜爱的终端应用程序，然后使用以下示例命令创建链接：

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

以下是引用的填充命令示例：

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

如果您使用 `ls` 命令：

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` MS Windows（更好，NTFS）支持符号链接，因为……Windows Vista!

![显示mklink命令帮助输出的Windows命令提示符图片](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` 创建符号链接的mklink命令需要管理员权限才能正确运行。 即使您是管理员帐户，也需要以管理员身份运行命令提示符，除非您启用了开发人员模式
> <br/>权限不正确：
> ![显示命令由于权限而失败的Windows命令提示符的图片](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>适当的权限：
> ![以管理员身份运行Windows命令提示符的图片](./assets/git-symlinks/windows-mklink-properpriv.png)

以下是创建链接的命令：

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


以下是引用的填充命令示例：

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### 开发人员模式(Windows 10)

何时放入 [开发人员模式](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development)，则Windows 10允许您更轻松地测试正在开发的应用程序、使用Ubuntu Bash Shell环境、更改各种以开发人员为中心的设置，以及执行其他此类操作。

Microsoft似乎会继续向开发人员模式添加功能，或者在这些功能达到更广泛的采用并被视为稳定（例如，通过Creators Update，Ubuntu Bash Shell环境不再需要开发人员模式）后，默认情况下会启用这些功能。

交响乐呢？ 启用开发人员模式后，无需运行具有提升权限的命令提示符即可创建符号链接。 因此，启用开发人员模式后，任何用户都可以创建符号链接。

> 启用开发人员模式后，用户应注销/登录，以使更改生效。

现在，您无需以管理员身份运行，即可看到该命令有效

![Windows命令提示符图片以普通用户的身份运行，并启用了开发人员模式](./assets/git-symlinks/windows-mklink-devmode.png)

#### 替代/方案方法

有一种特定的策略，允许特定用户创建符号链接→ [创建符号链接(Windows 10)- Windows安全 | Microsoft文档](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

专业人员：
- 客户可以利用此功能以编程方式允许在其组织（即Active Directory）内为所有开发人员创建符号链接，而无需在每个设备上手动启用开发人员模式。
- 此外，此策略应在未提供开发人员模式的早期MS Windows版本中可用。

图标：
- 此策略似乎对属于管理员组的用户没有影响。 管理员仍需要以提升的权限运行命令提示符。 真奇怪。

> 对本地/组策略的更改生效需要用户注销/登录。

运行 `gpedit.msc`，根据需要添加/更改用户。 默认情况下，管理员位于

![显示调整权限的组策略编辑器窗口](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### 在GIT中启用符号链接

Git根据core.symlinks选项处理符号链接

来源： [Git - git-config文档](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*如果core.symlinks为false，则符号链接将作为包含链接文本的小纯文件签出。 `git-update-index[1]` 和 `git-add[1]` 不会将记录的类型更改为常规文件。 对于FAT等不支持符号链接的文件系统非常有用。
除非 `git-clone[1]` 或 `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` 在大多数情况下，Git会假定Windows不适用于符号链接，并将其设置为false。*

Git在Windows上的行为在此处有很好的解释：符号链接· git-for-windows/git维基百科· GitHub

> `Info`:上面链接的文档中列出的假设似乎与Windows上可能的AEM Developer设置（尤其是NTFS）以及我们只有文件符号链接与目录符号链接这一事实是一致的

好消息是 [适用于Windows的Git版本2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) 安装程序具有 [用于启用符号链接支持的显式选项。](https://github.com/git-for-windows/git/issues/921)

> `Warning`:core.symlink选项可在克隆存储库时在运行时提供，或以其他方式存储为全局配置。

![显示GIT安装程序显示对符号链接的支持](./assets/git-symlinks/windows-git-install-symlink.png)

适用于Windows的Git将在 `"C:\Program Files\Git\etc\gitconfig"` . 其他Git桌面客户端应用程序可能不会考虑这些设置。
下面是相应的捕获，并非所有开发人员都会使用Git本机客户端（即Git Cmd、Git Bash），并且某些Git桌面应用程序（例如GitHub Desktop、Atlassian Sourcetree）可能具有不同的设置/默认值来使用系统或嵌入式Git

下面是内部内容的示例 `gitconfig` 文件

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Git命令行提示

在某些情况下，您可能必须创建新的符号链接（例如，添加新主机或新场）。

在上文档中，我们看到Windows提供了一个用于创建符号链接的“mklink”命令。

如果您在Git Bash环境中工作，则可以改用标准的Bash命令 `ln -s` 但它必须用特殊说明来添加前缀，如下例：

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### 摘要

要在Microsoft Windows OS上正确处理符号链接(至少对当前AEM调度程序配置基线的范围)，您将需要：

| 项目 | 最低版本/配置 | 推荐版本/配置 |
|------|---------------------------------|-------------------------------------|
| 操作系统 | Windows Vista或更高版本 | Windows 10 Creator Update或更高版本 |
| 文件系统 | NTFS | NTFS |
| 能够为Windows用户处理符号链接 | `"Create symbolic links"` 组/本地策略 `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | 启用Windows 10开发人员模式 |
| GIT | 本机客户端版本1.5.3 | 本机客户端版本2.10.2或更高版本 |
| Git配置 | `--core.symlinks=true` 从命令行执行git克隆时的选项 | Git全局配置<br/>`[core]`<br/>    symlinks = true <br/> 本机Git客户端配置路径： `C:\Program Files\Git\etc\gitconfig` <br/>Git桌面客户端的标准位置： `%HOMEPATH%\.gitconfig` |

> `Note:` 如果您已经具有本地存储库，则需要从源中刷新。 您可以克隆到新位置，并手动将未提交/未暂存的本地更改合并到新克隆的存储库中。