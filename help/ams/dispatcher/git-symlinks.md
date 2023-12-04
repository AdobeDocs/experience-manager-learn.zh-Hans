---
title: 将符号链接正确添加到GIT中
description: 有关在使用Dispatcher配置时如何以及在何处添加符号链接的说明。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 6e751586-e92e-482d-83ce-6fcae4c1102c
duration: 422
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1231'
ht-degree: 0%

---

# 在GIT中添加符号链接

[目录](./overview.md)

[&lt; — 上一页： Dispatcher运行状况检查](./health-check.md)

在AMS中，您将获得一个预填充的GIT存储库，其中包含已成熟的调度程序源代码，可供您开始开发和自定义。

创建您的第一个 `.vhost` 文件或顶级 `farm.any` 文件，您需要从 `available_*` 目录到 `enabled_*` 目录。 使用正确的链接类型将是通过Cloud Manager管道成功部署的关键。 此页面将帮助您了解如何执行此操作。

## Dispatcher原型

AEM开发人员通常从以下位置启动项目： [AEM原型](https://github.com/adobe/aem-project-archetype)

以下是源代码区域示例，您可以从中看到使用的符号链接：

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

例如， `/etc/httpd/conf.d/available_vhosts/` 目录包含暂存的潜在 `.vhost` 可在运行配置中使用的文件。

已启用 `.vhost` 文件将显示为相对路径 `symlinks` 内部 `/etc/httpd/conf.d/enabled_vhosts/` 目录。

## 创建符号链接

我们使用指向文件的符号链接，因此Apache Webserver会将目标文件视为相同的文件。  我们不希望同时在这两个目录中复制文件。  而只是从一个目录（符号链接）到另一个目录的快捷方式。

识别您部署的配置将针对Linux主机。  创建与目标系统不兼容的符号链接会导致故障和不需要的结果。

如果您的工作站不是Linux计算机，您可能会想知道要使用什么命令来正确创建这些链接，以便它们可以将其提交到GIT中。

> `TIP:` 使用相对链接很重要，因为如果您安装了Apache Webserver的本地副本并具有不同的安装基础，则链接仍然有效。  如果您使用绝对路径，则您的工作站或其他系统必须匹配相同的精确目录结构。

### OSX / Linux

符号链接是这些操作系统的固有链接，下面是创建这些链接的一些示例。  打开您喜爱的终端应用程序，然后使用以下示例命令创建链接：

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

以下是供参考的填充命令示例：

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

> `Note:` 原来MS Windows（更好的是NTFS）支持符号链接，因为……Windows Vista！

![显示mklink命令的帮助输出的Windows命令提示符图片](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` 创建symlink的mklink命令需要管理员权限才能正常运行。 即使作为管理员帐户，您仍需要以管理员身份运行命令提示符，除非您已启用开发人员模式
> <br/>不正确的权限：
> ![Windows命令提示符图片，显示由于权限导致命令失败](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>适当权限：
> ![以管理员身份运行的Windows命令提示符图片](./assets/git-symlinks/windows-mklink-properpriv.png)

以下是创建链接的命令：

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


以下是供参考的填充命令示例：

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### 开发人员模式( Windows 10 )

放入时 [开发人员模式](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development)， Windows 10允许您更轻松地测试正在开发的应用程序，使用Ubuntu Bash shell环境，更改各种以开发人员为中心的设置，以及执行其他此类操作。

Microsoft似乎不断向开发人员模式添加功能，或者在达到更广泛的采用并被视为稳定时默认启用其中的某些功能（例如，使用创建者更新，Ubuntu Bash Shell环境不再需要开发人员模式）。

符号链接呢？ 启用“开发人员模式”后，无需使用提升的权限运行命令提示符即可创建符号链接。 因此，启用“开发人员模式”后，任何用户都可以创建符号链接。

> 启用“开发人员模式”后，用户应注销/登录以使更改生效。

现在，您无需以管理员身份运行命令即可看到

![Windows命令提示符，以启用开发者模式的普通用户身份运行](./assets/git-symlinks/windows-mklink-devmode.png)

#### 替代/程序化方法

有一个特定策略可允许特定用户创建符号链接→ [创建符号链接(Windows 10) - Windows安全 | Microsoft文档](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

专业版：
- 客户可以利用这一点以编程方式允许为其组织内的所有开发人员（即Active Directory）创建符号链接，而无需在每个设备上手动启用开发人员模式。
- 此外，此策略应在不提供开发人员模式的早期版本的MS Windows中可用。

CON：
- 此策略似乎对属于Administrators组的用户无效。 管理员仍需要以提升的权限运行命令提示符。 奇怪。

> 用户需要注销/登录才能使对本地/组策略的更改生效。

运行 `gpedit.msc`，根据需要添加/更改用户。 默认情况下，管理员在那里

![显示调整权限的“组策略编辑器”窗口](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### 在GIT中启用符号链接

Git根据core.symlinks选项处理符号链接

来源： [Git - git配置文档](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*如果core.symlinks为false，符号链接将作为包含链接文本的小普通文件签出。 `git-update-index[1]` 和 `git-add[1]` 不会将记录的类型更改为常规文件。 对于不支持符号链接的文件系统（如FAT）非常有用。
默认为true，但 `git-clone[1]` 或 `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` 在大多数情况下，Git会假设Windows对符号链接没有好处，并将此参数设置为false。*

Git在Windows上的行为可在此处详细解释：符号链接· git-for-windows/git Wiki · GitHub

> `Info`：上述链接文档中所列的假设似乎与AEM Developer在Windows上的可能设置无关，最明显的是NTFS，而且我们只有文件符号链接而不是目录符号链接

好消息是 [适用于Windows的Git 2.10.2版](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) 安装程序具有 [用于启用符号链接支持的显式选项。](https://github.com/git-for-windows/git/issues/921)

> `Warning`：可在克隆存储库时在运行时提供core.symlink选项，也可以作为全局配置存储。

![显示GIT安装程序显示对符号链接的支持](./assets/git-symlinks/windows-git-install-symlink.png)

Git for Windows将全局首选项存储在 `"C:\Program Files\Git\etc\gitconfig"` . 其他Git桌面客户端应用程序可能不考虑这些设置。
这里有个问题，并不是所有开发人员都会使用Git本机客户端（即Git Cmd、Git Bash），并且某些Git桌面应用程序（例如GitHub Desktop、Atlassian Sourcetree）可能具有不同的设置/默认值以使用系统或嵌入的Git

以下是CJA内部 `gitconfig` 文件

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

在有些情况下，您可能需要创建新符号链接（例如，添加新的主机或新场）。

我们在上面的文档中看到，Windows提供了“mklink”命令来创建符号链接。

如果您在Git Bash环境中工作，则可以改用标准Bash命令 `ln -s` 但前面必须加上一个特殊的指令，如下面的示例：

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### 摘要

要在Microsoft Windows操作系统上让Git正确处理符号链接(至少对于当前AEM Dispatcher配置基线的范围)，您将需要：

| 项目 | 最低版本/配置 | 建议的版本/配置 |
|------|---------------------------------|-------------------------------------|
| 操作系统 | Windows Vista或更新版本 | Windows 10创建者更新或更高版本 |
| 文件系统 | NTFS | NTFS |
| 能够为Windows用户处理符号链接 | `"Create symbolic links"` 组/本地策略 `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Windows 10开发人员模式已启用 |
| GIT | Native client版本1.5.3 | Native Client版本2.10.2或更高版本 |
| Git配置 | `--core.symlinks=true` 从命令行执行git clone时的选项 | Git全局配置<br/>`[core]`<br/>    symlinks = true <br/> 本机Git客户端配置路径： `C:\Program Files\Git\etc\gitconfig` <br/>Git桌面客户端的标准位置： `%HOMEPATH%\.gitconfig` |

> `Note:` 如果您已经拥有本地存储库，则需要从源重新克隆。 您可以克隆到一个新位置，并将未提交/未暂存的本地更改手动合并到新克隆的存储库中。
