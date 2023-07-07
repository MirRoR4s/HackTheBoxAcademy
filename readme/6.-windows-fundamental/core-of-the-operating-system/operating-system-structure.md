---
description: https://academy.hackthebox.com/module/49/section/455
---

# 😆 Operating System Structure

In Windows operating systems, the root directory is \<drive\_letter>:\ (commonly C drive).&#x20;

The root directory (also known as the **boot partition（启动分区）**) is where the operating system is installed.&#x20;



Other physical and virtual drives are assigned other letters, for example, Data (E:).&#x20;



The directory structure of the boot partition is as follows:

<table><thead><tr><th width="198.5">Directory</th><th>Function</th></tr></thead><tbody><tr><td>Perflogs</td><td>Can hold Windows performance logs but is empty by default.</td></tr><tr><td>Program Files</td><td>On 32-bit systems, all 16-bit and 32-bit programs are installed here. On 64-bit systems, only 64-bit programs are installed here.</td></tr><tr><td>Program Files (x86)</td><td>32-bit and 16-bit programs are installed here on 64-bit editions of Windows.</td></tr><tr><td>ProgramData</td><td><p>This is a hidden folder that contains data that is essential for certain installed programs to run. </p><p></p><p>This data is accessible by the program no matter what user is running it.</p></td></tr><tr><td>Users</td><td>This folder contains user profiles for each user that logs onto the system and contains the two folders Public and Default.</td></tr><tr><td>Default</td><td>This is the default user profile template for all created users. Whenever a new user is added to the system, their profile is based on the Default profile.</td></tr><tr><td>Public</td><td>This folder is intended for computer users to share files and is accessible to all users by default. This folder is shared over the network by default but requires a valid network account to access.</td></tr><tr><td>AppData</td><td><p>Per user application data and settings are stored in a hidden user subfolder (i.e., cliff.moore\AppData). </p><p></p><p>Each of these folders contains three subfolders. The Roaming folder contains machine-independent data that should follow the user's profile, such as custom dictionaries. </p><p></p><p>The Local folder is specific to the computer itself and is never synchronized across the network. </p><p></p><p>LocalLow is similar to the Local folder, but it has a lower data integrity level. Therefore it can be used, for example, by a web browser set to protected or safe mode.</p></td></tr><tr><td>Windows</td><td>The majority of the files required for the Windows operating system are contained here.</td></tr><tr><td>System, System32, SysWOW64</td><td>Contains all DLLs required for the core features of Windows and the Windows API. The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path.</td></tr><tr><td>WinSxS</td><td>The Windows Component Store contains a copy of all Windows components, updates, and service packs.</td></tr></tbody></table>

### Exploring Directories Using Command Line

We can explore the file system using the [dir](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dir) command.

```bash
C:\htb> dir c:\ /a
 Volume in drive C has no label.
 Volume Serial Number is F416-77BE

 Directory of c:\

08/16/2020  10:33 AM    <DIR>          $Recycle.Bin
06/25/2020  06:25 PM    <DIR>          $WinREAgent
07/02/2020  12:55 PM             1,024 AMTAG.BIN
06/25/2020  03:38 PM    <JUNCTION>     Documents and Settings [C:\Users]
08/13/2020  06:03 PM             8,192 DumpStack.log
08/17/2020  12:11 PM             8,192 DumpStack.log.tmp
08/27/2020  10:42 AM    37,752,373,248 hiberfil.sys
08/17/2020  12:11 PM    13,421,772,800 pagefile.sys
12/07/2019  05:14 AM    <DIR>          PerfLogs
08/24/2020  10:38 AM    <DIR>          Program Files
07/09/2020  06:08 PM    <DIR>          Program Files (x86)
08/24/2020  10:41 AM    <DIR>          ProgramData
06/25/2020  03:38 PM    <DIR>          Recovery
06/25/2020  03:57 PM             2,918 RHDSetup.log
08/17/2020  12:11 PM        16,777,216 swapfile.sys
08/26/2020  02:51 PM    <DIR>          System Volume Information
08/16/2020  10:33 AM    <DIR>          Users
08/17/2020  11:38 PM    <DIR>          Windows
               7 File(s) 51,190,943,590 bytes
              13 Dir(s)  261,310,697,472 bytes free
```

The [tree](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/tree) utility is useful for graphically displaying the directory structure of a path or disk.



```bash
C:\htb> tree "c:\Program Files (x86)\VMware"
Folder PATH listing
Volume serial number is F416-77BE
C:\PROGRAM FILES (X86)\VMWARE
├───VMware VIX
│   ├───doc
│   │   ├───errors
│   │   ├───features
│   │   ├───lang
│   │   │   └───c
│   │   │       └───functions
│   │   └───types
│   ├───samples
│   └───Workstation-15.0.0
│       ├───32bit
│       └───64bit
└───VMware Workstation
    ├───env
    ├───hostd
    │   ├───coreLocale
    │   │   └───en
    │   ├───docroot
    │   │   ├───client
    │   │   └───sdk
    │   ├───extensions
    │   │   └───hostdiag
    │   │       └───locale
    │   │           └───en
    │   └───vimLocale
    │       └───en
    ├───ico
    ├───messages
    │   ├───ja
    │   └───zh_CN
    ├───OVFTool
    │   ├───env
    │   │   └───en
    │   └───schemas
    │       ├───DMTF
    │       └───vmware
    ├───Resources
    ├───tools-upgraders
    └───x64
```

The `tree` command can provide us with a large amount of information.&#x20;



The following command can be used to walk through all the files in the C drive, one screen at a time. This command can be modified to be run against any directory.



```bash
tree c:\ /f | more
```

**Questions**

Answer the question(s) below to complete this Section and earn cubes!

&#x20;RDP to with user "htb-student" and password "Academy\_WinFun!"

\+ 0  Find the non-standard directory in the C drive. Submit the contents of the flag file saved in this directory.
