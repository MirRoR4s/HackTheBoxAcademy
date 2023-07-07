---
description: https://academy.hackthebox.com/module/49/section/454
---

# Introduction to Windows

作为渗透测试者，了解各种技术是非常重要的。比如对 Windows 和 Linux 操作系统有一个比较透彻的了解就有利于我们的渗透测试。我们在渗透期间遇到的大多数操作系统，无论是本地的还是云上的，一般都是基于上述两种操作系统。所以我们要了解如何攻击和防御这些操作系统并将它们分别用作执行进一步渗透活动的平台。

#### Windows 操作系统

微软于 1985 年 11 月 20 日首次推出 Windows 操作系统。Windows 的第一个版本是一个 MS-DOS 的图形化操作系统 shell。之后更高版本的 Windows 桌面引入了 Windows 文件管理器、程序管理器和打印管理器等程序。

Windows 95 是 Windows 和 DOS 的首个完全集成，并首次提供了内置的 Internet 支持。此版本还推出了 Internet Explorer 网络浏览器。

自初始版本以来，已经发布了十几个版本的 Windows，例如 Windows XP、Vista 和 8，~~直到当前版本：Windows 10。~~ 当前最新的 Windows 版本是 Windows 11。

随着时间的推移，Microsoft（微软）为每个 Windows 桌面版提供了各个版本，以满足从休闲消费者到企业客户的不同需求。

Windows Server 于 1993 年随 Windows NT 3.1 Advanced Server 一起发布。多年来，Windows NT 经历了多次更新，添加了 Internet 信息服务 (IIS)、各种网络协议、管理向导以促进管理任务等技术。

随着 Windows 2000 的发布，Microsoft 推出了 Active Directory，最初旨在帮助系统管理员设置文件共享、数据加密、VPN 等。Windows Server 2000 还包括 Microsoft 管理控制台 (MMC) 并支持动态的磁盘卷。

Windows Server 2003 紧随其后，具有服务器角色、内置防火墙、卷影复制服务等功能。

Windows Server 2008 包括故障转移群集、Hyper-V 虚拟化软件、Server Core、事件查看器以及增强了活动目录等功能。

多年来，Microsoft 发布了更多服务器版本，包括 Server 2012、Server 2016 和最近的 Server 2019。这个最新版本增加了对 Kubernetes、Linux 容器和更多高级安全功能的支持。

随着新版本 Windows 的推出，旧版本被弃用并且不再接收 Microsoft 更新（除非在某些情况下购买了长期支持合同）。 Windows Server 2008 和 2012 的安全更新已于 2020 年 1 月 14 日终止。目前，仅支持 Server 2012 R2 及更高版本。然而，由于发现了严重的 SMBv1 漏洞（永恒之蓝），微软在过去几年中为早期版本的 Windows 发布了带外补丁。

许多版本的 Windows 现在被视为“旧版”，不再受支持。为了支持关键的应用程序或是出于运营和预算方面的考虑，各组织经常运行着各种较旧的操作系统。评估员需要了解版本之间的差异以及每个版本固有的各种错误配置和漏洞。

#### Windows 版本

以下是主要 Windows 操作系统和相关版本号的列表：

![](https://i.imgur.com/hEMiBZu.png)

我们可以使用 [Get-WmiObject](https://learn.microsoft.com/zh-cn/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) [cmdlet](https://learn.microsoft.com/zh-cn/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7.3\&viewFallbackFrom=powershell-7) 来查找有关操作系统的信息。此 cmdlet 可用于获取 WMI 类的实例或有关可用 WMI 类的信息。

![](https://i.imgur.com/7RF4usd.png)

![](https://i.imgur.com/IpH1wzb.png)

cmdlet 是什么？

![](https://i.imgur.com/agD2j4S.png)

有多种方法可以找到我们系统的版本和内部版本号。我们可以使用 `win32_OperatingSystem` 类轻松获取此信息。如下结果表明我们在 Windows 10 主机上并且内部版本号为 19041。

```bash
PS C:\htb> Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber

Version    BuildNumber
-------    -----------
10.0.19041 19041
```

其他一些可与 Get-WmiObject 一起使用的有用类是 Win32\_Process、Win32\_Service、Win32\_Bios。

Win32\_Process 用于获取进程列表，Win32\_Service 用于获取服务列表，而 Win32\_Bios 用于获取[基本输入/输出系统](https://en.wikipedia.org/wiki/BIOS) (BIOS) 信息。 BIOS 是安装在计算机主板上的固件，用于控制计算机的基本功能，例如电源管理、输入/输出接口和系统配置。我们可以使用 ComputerName 参数来获取有关远程计算机的信息。 GetWmiObject 可用于启动和停止本地和远程计算机上的服务。有关 cmdlet 的更多信息，请参见[此处](https://ss64.com/ps/get-wmiobject.html)和[此处](https://adamtheautomator.com/get-wmiobject/)。

#### 访问 Windows

**本地访问**

如果你现在正在阅读这些文字，那么你已在本地访问了某种类型的计算机，无论是智能手机、平板电脑、笔记本电脑、还是台式机。

本地访问是访问计算机（包括运行 Windows 的计算机）最常见的方式。输入可能通过键盘、触控板和/或鼠标进行，而输出来自显示屏。

Organizations with office space where employees work on a day-to-day basis build security policies and security controls around the idea that their employees are working in dedicated workspaces on computers owned by the organization.

越来越多的组织开始支持非技术和技术员工远程办公。对于从事 IT、软件开发和信息安全工作的专业技术人员来说，这并不新鲜。在任何一天，技术专业人员都可以在本地和远程访问多台机器。所以接下来让我们谈谈远程访问的概念。

**远程访问**

**远程访问指的是通过网络访问计算机**。在远程访问另一台计算机之前，需要先对计算机进行本地访问。

有无数种远程访问方法。在本模块中，我们将主要使用远程访问方法连接到 Windows 操作系统并与之交互。网络和互联网技术的进步催生了以计算机系统远程访问和远程管理为生的行业。

考虑一下 [MSP](https://www.techtarget.com/searchitchannel/definition/managed-service-provider) 和 [MSSP](https://www.gartner.com/en/information-technology/glossary/mssp-managed-security-service-provider)，这两个行业主要以远程管理其客户的计算机系统为生。此功能使他们能够集中管理、标准化所使用的技术、自动执行大量任务、启用远程工作安排，并在出现问题或出现潜在安全威胁时允许快速响应。

远程访问不仅限于 MSP 和 MSSP。拥有 IT、软件开发和/或安全团队的组织每天都使用远程访问方法来构建应用程序、管理服务器和管理员工工作站。一些最常见的远程访问技术包括但不限于：

* 虚拟专用网（VPN）
* 安全 Shell（SSH）
* 文件传输协议（FTP）
* Virtual Network Computing （VNC）
* Windows 远程管理（或是 PowerShell 远程）（WinRM）
* 远程桌面协议（RDP）

在本模块中主要使用 RDP！

**远程桌面协议（RDP）**

RDP 使用客户端/服务器架构，其中客户端应用程序用于通过启用 RDP 访问的网络指定计算机的目标 IP 地址或主机名。启用 RDP 远程访问的目标计算机被视为服务器。重要的是要注意 RDP 默认情况下在逻辑端口 3389 上侦听。请记住，IP 地址用作网络上计算机的逻辑标识符，而逻辑端口是分配给应用程序的标识符。简单来说，我们可以将网络子网视为城镇中的街道（公司网络），该子网中分配给主机的 IP 地址（如该街道上的房屋）以及逻辑端口作为可用于进入房子的窗口/大门。

一旦请求（封装在数据包中）通过其 IP 地址到达目标计算机，该请求将根据该请求中指定的端口（作为标头包含在数据包中）定向到计算机上托管的应用程序。 IP 寻址和协议封装在[网络简介模块](https://academy.hackthebox.com/module/details/34)中有更详细的介绍。从网络的角度来看，在本模块中，我们只需要了解每台计算机都分配了一个 IP 地址以通过网络进行通信，目标计算机上托管的应用程序侦听特定的逻辑端口。

我们可以使用 RDP 从运行 Linux 或 Windows 的攻击主机连接到 Windows 目标。如果我们从 Windows 主机连接到 Windows 目标，我们可以使用称为远程桌面连接 ([mstsc.exe](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/mstsc)) 的内置 RDP 客户端应用程序。查看下面的剪辑以了解基本用法：

![](https://academy.hackthebox.com/storage/modules/49/UsingRemoteDesktopConnection.gif)

为使远程连接生效，必须已在目标 Windows 系统上[允许](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access)远程访问。默认情况下，Windows 操作系统不允许远程访问。 HTB 学院团队已将我们的许多 Windows 目标配置为在通过 VPN 连接到学院实验室后允许 RDP 访问。

远程桌面连接还允许我们保存连接配置文件。这是 IT 管理员的普遍习惯，因为它可以更方便地连接到远程系统。

![](https://academy.hackthebox.com/storage/modules/49/SavingRDPConnections.gif)

作为渗透测试人员，我们可以从查找这些保存的远程桌面文件 (.rdp) 中获益。 存在许多其他远程桌面客户端应用程序，其中一些已列在这篇名为 “[远程桌面客户端](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)” 的 Microsoft 文章中。本模块不会涵盖每个远程桌面客户端应用程序。

**使用 xfreerdp**

如果使用的是 Linux 操作系统，该如何 RDP 呢？

从基于 Linux 的攻击主机，我们可以使用名为 [xfreerdp](https://linux.die.net/man/1/xfreerdp) 的工具远程访问 Windows 目标。您会注意到我们跨多个模块使用 xfreerdp，因为它易于使用、功能集、命令行实用程序和效率。查看下面的剪辑，了解 Pwnbox 的基本用法：

![](https://academy.hackthebox.com/storage/modules/49/ConnectingwithXfreerdp.gif)

请记住，我们也可以在命令行中复制并粘贴 xfreerdp 命令，因此我们不需要手动输入选项。 xfreerdp 有几个选项可供我们使用，例如驱动器重定向，以便能够将文件传输到目标主机/从目标主机传输文件，这些值得练习，我们将在 HTB Academy 的其他模块中介绍。

还存在其他 RDP 客户端，例如 [Remmina](https://remmina.org/) 和 [rdesktop](http://www.rdesktop.org/)，我们鼓励您尝试其他客户端，看看哪种最适合您。现在我们已经介绍了这些概念，让我们通过生成下面的目标并使用提供的凭据使用 RDP 连接到它来应用它们。

#### 连接到 Windows 目标

使用以下命令通过远程桌面 (RDP) 连接：

```bash
MirRoR4s@htb[/htb]$ xfreerdp /v:<targetIp> /u:htb-student /p:Password
```

> 注意：生成目标实例可能需要 1-2 分钟。

> VPN 服务器 每次“切换”时，您的连接密钥都会重新生成，您必须重新下载您的 VPN 连接文件。 切换到新 VPN 服务器时，与旧 VPN 服务器关联的所有 VM 实例都将终止。 现有的 PwnBox 实例将自动切换到新的 VPN 服务器。

**回答下列问题：**

1. 目标工作站的内置版本号是什么？
2. 工作站上安装的 Windows NT 版本是什么？比如 Windows X-大小写敏感
