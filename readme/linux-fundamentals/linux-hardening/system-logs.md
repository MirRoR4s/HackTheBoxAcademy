---
description: https://academy.hackthebox.com/module/18/section/2100
---

# System Logs

System logs on Linux are a set of files that contain information about the system and the activities taking place on it.&#x20;

Linux上的系统日志是一个文件集，包含了系统及系统上所发生的各种活动的信息。

These logs are important for monitoring and troubleshooting the system, as they can provide **insights（深入了解）** into system behavior, application activity, and security events.&#x20;

系统日志对于系统监视和排错来说十分重要，因为它能帮助我们深入了解如系统行为、程序行为和安全事件等信息。

These system logs can be a valuable source of information for identifying potential security weaknesses and vulnerabilities within a Linux system as well.&#x20;

系统日志也是一个有价值的信息源，我们可以利用它发现系统内潜在的安全弱点及漏洞。

By analyzing the logs on our target systems, we can gain insights into the system's behavior, network activity, and user activity and can use this information to identify any abnormal activity, such as unauthorized logins, attempted attacks, clear text credentials, or unusual file access, which could indicate a potential security breach.

通过分析系统日志，我们能够深入地了解系统的行为、网络活动和用户活动等相关的信息。利用这些信息，我们可以识别异常的活动，比如未授权登录、攻击企图、明文凭据或是异常的文件访问等等，这些异常活动暗示着我们的系统可能存在安全漏洞。

***

We, as penetration testers, can also use system logs to monitor the **effectiveness（有效性）** of our security testing activities.&#x20;

作为渗透测试者的我们，也可以利用系统日志监视安全测试活动的有效性。

By reviewing the logs after performing security testing, we can determine if our activities triggered any security events, such as intrusion detection alerts or system warnings.&#x20;

在进行安全测试后，通过查看日志，可以确定测试活动是否引发了安全事件，比如入侵检测警报或系统警告。

This information can help us refine our testing strategies and improve the overall security of the system.

这些信息可以帮助我们改进测试策略并提升系统的安全性。

***

In order to ensure the security of a Linux system, it is important to configure system logs properly.&#x20;

为了确保系统的安全性，我们需要正确地配置系统日志。

This includes setting the appropriate log levels, configuring log rotation to prevent log files from becoming too large, and ensuring that the logs are stored securely and protected from unauthorized access.&#x20;

这包括设置适当的日志等级，配置日志轮替（避免日志文件过大）、确保日志的存储安全以及保护日志免遭未授权访问。



In addition, it is important to regularly review and analyze the logs to identify potential security risks and respond to any security events in a timely manner.&#x20;

此外，我们需要定期核查和分析日志，并以此发现潜在的安全风险，同时要及时地对安全事件作出响应。

***

There are several different types of system logs on Linux, including:

Linux中有几种不同类型的系统日志，包括：

* Kernel Logs-内核日志
* System Logs-系统日志
* Authentication Logs-认证日志
* Application Logs-应用程序日志
* Security Logs-安全日志



**Kernel logs-内核日志**

These logs contain information about the system's kernel, including hardware drivers, system calls, and kernel events.&#x20;

内核日志含有系统内核的信息，包括硬件驱动器、系统调用和内核事件。

They are stored in the `/var/log/kern.log` file.&#x20;

内核日志存储在 /var/log/kern.log 文件中。

For example, kernel logs can reveal the presence of vulnerable or outdated drivers that could be targeted by attackers to gain access to the system.&#x20;

举个例子，内核日志可以帮助我们发现存在漏洞的或是过时的驱动器，这些驱动器可能会成为攻击者访问系统的目标。

They can also provide insights into **system crashes（系统崩溃）**, resource limitations, and other events that could lead to a denial of service or other security issues.&#x20;

内核日志能让我们深入了解如系统崩溃、资源限制或其他能够造成拒绝服务攻击及其他安全问题的事件。

In addition, kernel logs can help us identify **suspicious（可疑的）** system calls or other activities that could indicate the presence of malware or other malicious software on the system.&#x20;

此外，内核日志也能够帮助我们发现可疑的系统调用或是其他暗示系统中存在恶意软件的活动。

By monitoring the `/var/log/kern.log` file, we can detect any unusual behavior and take appropriate action to prevent further damage to the system.

通过监视 /`var/log/kern.log` 文件，我们能够发现异常行为并采取适当的措施阻止其进一步损害系统。

***

**System logs-系统日志**

These logs contain information about system-level events, such as service starts and stops, login attempts, and system reboots.&#x20;

系统日志包含系统级事件的信息，比如服务的启停、登录尝试和系统重启等。

They are stored in the `/var/log/syslog` file.&#x20;

系统日志存储在 `/var/log/syslog` 文件中。

By analyzing login attempts, service starts and stops, and other system-level events, we can detect any possible access or activities on the system.&#x20;

通过分析登录尝试、服务启停和其他系统级事件，我们可以识别任意可能的系统活动或系统访问。

This can help us identify any vulnerabilities that could be exploited and help us recommend security measures to mitigate these risks.&#x20;

这能够帮助我们发现任意可能会被利用的漏洞并帮助我们完善安全措施以此来消除这些影响。

In addition, we can use the `syslog` to identify potential issues that could impact the availability or performance of the system, such as failed service starts or system reboots. Here is an example of how such `syslog` file could look like:

此外，我们可以使用 `syslog` 来发现潜在的问题，这些问题可能会影响到系统的性能或可用性，比如导致服务启动失败或是系统重启。以下是 `syslog` 文件的一个示例：

**Syslog**

```bash
Feb 28 2023 15:00:01 server CRON[2715]: (root) CMD (/usr/local/bin/backup.sh)
Feb 28 2023 15:04:22 server sshd[3010]: Failed password for htb-student from 10.14.15.2 port 50223 ssh2
Feb 28 2023 15:05:02 server kernel: [  138.303596] ata3.00: exception Emask 0x0 SAct 0x0 SErr 0x0 action 0x6 frozen
Feb 28 2023 15:06:43 server apache2[2904]: 127.0.0.1 - - [28/Feb/2023:15:06:43 +0000] "GET /index.html HTTP/1.1" 200 13484 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36"
Feb 28 2023 15:07:19 server sshd[3010]: Accepted password for htb-student from 10.14.15.2 port 50223 ssh2
Feb 28 2023 15:09:54 server kernel: [  367.543975] EXT4-fs (sda1): re-mounted. Opts: errors=remount-ro
Feb 28 2023 15:12:07 server systemd[1]: Started Clean PHP session files.
```

***

**Authentication logs-认证日志**

These logs contain information about user authentication attempts, including successful and failed attempts.&#x20;

认证日志包含用户身份认证尝试的信息，包括成功的和失败的尝试。

They are stored in the `/var/log/auth.log` file. It is important to note that while the `/var/log/syslog` file may contain similar login information, the `/var/log/auth.log` file specifically focuses on user authentication attempts, making it a more valuable resource for identifying potential security threats.&#x20;

认证日志存储在 `/var/log/auth.log` 文件中。值得注意的是，尽管 `/var/log/syslog` 也包含着类似的登录信息，但是 `/var/log/auth.log` 文件主要集中于用户的身份认证尝试。所以对于发现潜在的安全威胁来说，auth.log 是一个更加有价值的资源。

Therefore, it is essential for penetration testers to review the logs stored in the `/var/log/auth.log` file to ensure that the system is secure and has not been compromised.

所以，为了确保系统是安全的，渗透测试者有必要审查一下 `/var/log/auth.log` 文件的内容。

**Auth.log**

```bash
Feb 28 2023 18:15:01 sshd[5678]: Accepted publickey for admin from 10.14.15.2 port 43210 ssh2: RSA SHA256:+KjEzN2cVhIW/5uJpVX9n5OB5zVJ92FtCZxVzzcKjw
Feb 28 2023 18:15:03 sudo:   admin : TTY=pts/1 ; PWD=/home/admin ; USER=root ; COMMAND=/bin/bash
Feb 28 2023 18:15:05 sudo:   admin : TTY=pts/1 ; PWD=/home/admin ; USER=root ; COMMAND=/usr/bin/apt-get install netcat-traditional
Feb 28 2023 18:15:08 sshd[5678]: Disconnected from 10.14.15.2 port 43210 [preauth]
Feb 28 2023 18:15:12 kernel: [  778.941871] firewall: unexpected traffic allowed on port 22
Feb 28 2023 18:15:15 auditd[9876]: Audit daemon started successfully
Feb 28 2023 18:15:18 systemd-logind[1234]: New session 4321 of user admin.
Feb 28 2023 18:15:21 CRON[2345]: pam_unix(cron:session): session opened for user root by (uid=0)
Feb 28 2023 18:15:24 CRON[2345]: pam_unix(cron:session): session closed for user root
```

In this example, we can see in the first line that a successful public key has been used for authentication for the user `admin`.&#x20;

在该例中，在第一行我们可以看到，用户 `admin` 使用公钥进行了一次成功的身份认证。

Additionally, we can see that this user is in the `sudoers` group because he can execute commands using `sudo`.&#x20;

此外，我们可以发现该用户位于`sudoers` 组，因为他可以使用`sudo` 执行命令。



The kernel message indicates that unexpected traffic was allowed on port 22, which could indicate a potential security breach.&#x20;

例中的内核信息表明有一个非预期的流量通过了22端口，这表明系统可能存在一个安全漏洞。

After that, we see that a new session was created for user "admin" by `systemd-logind` and that a `cron` session opened and closed for the user `root`.

最后，我们可以看到admin用户通过`systemd-logind` 创建了一个新的会话，然后是`root` 用户打开和关闭了一个`cron` 会话。

***

**Application logs**

These logs contain information about the activities of specific applications running on the system. They are often stored in their own files, such as `/var/log/apache2/error.log` for the Apache web server or `/var/log/mysql/error.log` for the MySQL database server. These logs are particularly important when we are targeting specific applications, such as web servers or databases, as they can provide insights into how these applications are processing and handling data. By examining these logs, we can identify potential vulnerabilities or misconfigurations. For example, access logs can be used to track requests made to a web server, while audit logs can be used to track changes made to the system or to specific files. These logs can be used to identify unauthorized access attempts, data exfiltration, or other suspicious activity.

Besides, access and audit logs are critical logs that record information about the actions of users and processes on the system. They are crucial for security and compliance purposes, and we can use them to identify potential security issues and attack vectors.

For example, `access logs` keep a record of user and process activity on the system, including login attempts, file accesses, and network connections. `Audit logs` record information about security-relevant events on the system, such as modifications to system configuration files or attempts to modify system files or settings. These logs help track potential attacks and activities or identify security breaches or other issues. An example entry in an access log file can look like the following:

**Access Log Entry**

```shell-session
2023-03-07T10:15:23+00:00 servername privileged.sh: htb-student accessed /root/hidden/api-keys.txt
```

In this log entry, we can see that the user `htb-student` used the `privileged.sh` script to access the `api-keys.txt` file in the `/root/hidden/` directory. On Linux systems, most common services have default locations for access logs:

<table data-header-hidden><thead><tr><th width="209"></th><th></th></tr></thead><tbody><tr><td><strong>Service</strong></td><td><strong>Description</strong></td></tr><tr><td><code>Apache</code></td><td>Access logs are stored in the /var/log/apache2/access.log file (or similar, depending on the distribution).</td></tr><tr><td><code>Nginx</code></td><td>Access logs are stored in the /var/log/nginx/access.log file (or similar).</td></tr><tr><td><code>OpenSSH</code></td><td>Access logs are stored in the /var/log/auth.log file on Ubuntu and in /var/log/secure on CentOS/RHEL.</td></tr><tr><td><code>MySQL</code></td><td>Access logs are stored in the /var/log/mysql/mysql.log file.</td></tr><tr><td><code>PostgreSQL</code></td><td>Access logs are stored in the /var/log/postgresql/postgresql-version-main.log file.</td></tr><tr><td><code>Systemd</code></td><td>Access logs are stored in the /var/log/journal/ directory.</td></tr></tbody></table>

**Security logs**

These security logs and their events are often recorded in a variety of log files, depending on the specific security application or tool in use.&#x20;



For example, the Fail2ban application records failed login attempts in the `/var/log/fail2ban.log` file, while the UFW firewall records activity in the `/var/log/ufw.log` file. Other security-related events, such as changes to system files or settings, may be recorded in more general system logs such as `/var/log/syslog` or `/var/log/auth.log`.&#x20;



As penetration testers, we can use log analysis tools and techniques to search for specific events or patterns of activity that may indicate a security issue and use that information to further test the system for vulnerabilities or potential attack vectors.

It is important to be familiar with the default locations for access logs and other log files on Linux systems, as this information can be useful when performing a security assessment or penetration test. By understanding how security-related events are recorded and stored, we can more effectively analyze log data and identify potential security issues.

All these logs can be accessed and analyzed using a variety of tools, including the log file viewers built into most Linux desktop environments, as well as command-line tools such as the `tail`, `grep`, and `sed` commands. Proper analysis of system logs can help identify and troubleshoot system issues, as well as detect security breaches and other events of interest.
