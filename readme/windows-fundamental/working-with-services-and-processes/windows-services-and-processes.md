---
description: https://academy.hackthebox.com/module/49/section/457
---

# 🎶 Windows Services & Processes

### Windows Services

Services are a major component of the Windows operating system.&#x20;



They **allow for** the creation and management of long-running processes.&#x20;



Windows services can be started automatically at system boot without user intervention.&#x20;



These services can continue to run in the background even after the user logs out of their account on the system.



Applications can also be created to install as a service, such as a network monitoring application installed on a server.&#x20;



Services on Windows are responsible for many functions within the Windows operating system, such as networking functions, performing system **diagnostics（诊断）**, managing user credentials, controlling Windows updates, and more.



Windows services are managed via the Service Control Manager (SCM) system, accessible via the `services.msc` MMC add-in.



This **add-in（加载项）** provides a GUI interface for interacting with and managing services and displays information about each installed service.&#x20;



This information includes the service Name, Description, Status, Startup Type, and the user that the service runs under.



It is also possible to query and manage services via the command line using `sc.exe` using [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7) cmdlets such as `Get-Service`.

&#x20;&#x20;

```powershell
PS C:\htb> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl


Name                : AdobeARMservice
DisplayName         : Adobe Acrobat Update Service
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess

Name                : Appinfo
DisplayName         : Application Information
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {RpcSs, ProfSvc}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess, Win32ShareProcess
```

Service statuses can appear as Running, Stopped, or Paused, and they can be set to start manually, automatically, or on a delay at system boot.&#x20;



Services can also be shown in the state of Starting or Stopping if some action has triggered them to either start or stop.&#x20;



Windows has three categories of services: **Local Services**, **Network Services**, and **System Services**.&#x20;



Services can usually only be created, modified, and deleted by users with administrative privileges.



Misconfigurations around service permissions are a common privilege escalation vector on Windows systems.



In Windows, we have some [critical system services](https://docs.microsoft.com/en-us/windows/win32/rstmgr/critical-system-services) that cannot be stopped and restarted without a system restart.&#x20;



If we update any file or resource in use by one of these services, we must restart the system.

<table><thead><tr><th width="195">Service</th><th>Description</th></tr></thead><tbody><tr><td>smss.exe</td><td>Session Manager SubSystem. Responsible for handling sessions on the system.</td></tr><tr><td>csrss.exe</td><td>Client Server Runtime Process. The user-mode <strong>portion（一部分）</strong> of the Windows subsystem.</td></tr><tr><td>wininit.exe</td><td>Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program.</td></tr><tr><td>logonui.exe</td><td>Used for facilitating user login into a PC</td></tr><tr><td>lsass.exe</td><td>The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon（登录） service.</td></tr><tr><td>services.exe</td><td>Manages the operation of starting and stopping services.</td></tr><tr><td>winlogon.exe</td><td>Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running.</td></tr><tr><td>System</td><td>A background system process that runs the Windows kernel.</td></tr><tr><td>svchost.exe with RPCSS</td><td>Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS).</td></tr><tr><td>svchost.exe with Dcom/PnP</td><td>Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services.</td></tr></tbody></table>

This [link](https://en.wikipedia.org/wiki/List\_of\_Microsoft\_Windows\_components#Services) has a list of Windows components, including key services.

### Processes

Processes run in the background on Windows systems.&#x20;



They either run automatically as part of the Windows operating system or are started by other installed applications.



Processes associated with installed applications can often be terminated without causing a severe impact on the operating system.&#x20;



Certain processes are critical and, if terminated, will stop certain components of the operating system from running properly.&#x20;



Some examples include the Windows Logon Application, System, System Idle Process, Windows Start-Up Application, Client Server Runtime, Windows Session Manager, Service Host, and Local Security Authority Subsystem Service (LSASS) process（本地安全授予子系统服务？）.

### Local Security Authority Subsystem Service (LSASS)

`lsass.exe` is the process that is responsible for enforcing the security policy on Windows systems.&#x20;



When a user attempts to log on to the system, this process verifies their log on attempt and creates access tokens based on the user's permission levels.&#x20;



LSASS is also responsible for user account password changes.&#x20;



All events associated with this process (logon/logoff attempts, etc.) are logged within the Windows Security Log.&#x20;



LSASS is an extremely high-value target as several tools exist to extract both cleartext and hashed credentials stored in memory by this process.



### Sysinternals Tools-系统内部工具

The [SysInternals Tools suite](https://docs.microsoft.com/en-us/sysinternals) is a set of portable Windows applications that can be used to administer Windows systems (for the most part without requiring installation).&#x20;



The tools can be either downloaded from the Microsoft website or by loading them directly from an internet-accessible file share by typing `\\live.sysinternals.com\tools` into a Windows Explorer window.

For example, we can run procdump.exe directly from this share without downloading it directly to disk.

&#x20;&#x20;

```powershell
C:\htb> \\live.sysinternals.com\tools\procdump.exe -accepteula

ProcDump v9.0 - Sysinternals process dump utility
Copyright (C) 2009-2017 Mark Russinovich and Andrew Richards
Sysinternals - www.sysinternals.com

Monitors a process and writes a dump file when the process exceeds the
specified criteria or has an exception.

Capture Usage:
   procdump.exe [-mm] [-ma] [-mp] [-mc Mask] [-md Callback_DLL] [-mk]
                [-n Count]
                [-s Seconds]
                [-c|-cl CPU_Usage [-u]]
                [-m|-ml Commit_Usage]
                [-p|-pl Counter_Threshold]
                [-h]
                [-e [1 [-g] [-b]]]
                [-l]
                [-t]
                [-f  Include_Filter, ...]
                [-fx Exclude_Filter, ...]
                [-o]
                [-r [1..5] [-a]]
                [-wer]
                [-64]
                {
                 {{[-w] Process_Name | Service_Name | PID} [Dump_File | Dump_Folder]}
                |
                 {-x Dump_Folder Image_File [Argument, ...]}
                }
				
<SNIP>

```

The suite includes tools such as `Process Explorer`, an enhanced version of `Task Manager`, and `Process Monitor`, which can be used to monitor file system, registry, and network activity related to any process running on the system.&#x20;



Some additional tools are TCPView, which is used to monitor internet activity, and PSExec, which can be used to manage/connect to systems via the SMB protocol remotely.



These tools can be useful for penetration testers to, for example, discover interesting processes and possible privilege escalation paths as well as for lateral movement.

### Task Manager

Windows Task Manager is a powerful tool for managing Windows systems.&#x20;



It provides information about running processes, system performance, running services, startup programs, logged-in users/logged in user processes, and services.&#x20;



Task Manager can be opened by right-clicking on the taskbar and selecting `Task Manager`, pressing **ctrl + shift + Esc,** pressing ctrl + alt + del and selecting `Task Manager`, opening the start menu and typing `Task Manager`, **or typing `taskmgr` from a CMD or PowerShell console**.

![image](https://academy.hackthebox.com/storage/modules/49/taskmgr.png)

<table><thead><tr><th width="218">Tab</th><th>Description</th></tr></thead><tbody><tr><td>Processes tab</td><td>Shows a list of running applications and background processes along with the CPU, memory, disk, network, and power usage for each.</td></tr><tr><td>Performance tab</td><td><p>Shows graphs and data such as CPU utilization, system uptime, memory usage, disk and, networking, and GPU <strong>usage（用量）</strong>. </p><p></p><p>We can also open the <code>Resource Monitor</code>, which gives us a much more in-depth view of the current CPU, Memory, Disk, and Network resource usage.</p></td></tr></tbody></table>

![image](https://academy.hackthebox.com/storage/modules/49/resource\_monitor.png)

<table><thead><tr><th width="229">Tab</th><th>Description</th></tr></thead><tbody><tr><td>App history tab-应用历史记录</td><td>Shows resource usage for the current user account for each application for a period of time.</td></tr><tr><td>Startup tab</td><td>Shows which applications are configured to start at boot as well as the impact on the startup process.</td></tr><tr><td>Users tab</td><td>Shows logged in users and the processes/resource usage associated with their session.</td></tr><tr><td>Details tab</td><td>Shows the name, process ID (PID), status, associated username, CPU, and memory usage for each running application.</td></tr><tr><td>Services tab</td><td>Shows the name, PID, description, and status of each installed service. The Services add-in can be accessed from this tab as well.</td></tr></tbody></table>

### Process Explorer-进程管理器

[Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer) is a part of the Sysinternals tool suite.&#x20;



This tool can show which handles and DLL processes are loaded when a program runs.&#x20;



Process Explorer shows a list of currently running processes, and from there, we can see what handles the process has selected in one view or the DLLs and memory-swapped files that have been loaded in another view.&#x20;



We can also search within the tool to show which processes **tie back** to a specific handle or DLL.&#x20;



The tool can also be used to analyze parent-child process relationships to see what child processes are spawned by an application and help troubleshoot any issues such as **orphaned（孤立的）** processed that can be left behind when a process is terminated.



**Questions**

Answer the question(s) below to complete this Section and earn cubes!

Target: Click here to spawn the target system!\


Cheat Sheet[Download VPN Connection File](https://academy.hackthebox.com/vpn/key)

&#x20;RDP to with user "htb-student" and password "Academy\_WinFun!"

\+ 0  Identify one of the non-standard update services running on the host. Submit the full name of the service executable (not the DisplayName) as your answer.
