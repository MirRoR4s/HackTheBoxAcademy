---
description: https://academy.hackthebox.com/module/18/section/66
---

# Prompt Description

The bash prompt is easy to understand and, by default, includes information such as the user, hostname, and current working directory.&#x20;



It is a string of characters displayed on the terminal screen that indicates that the system is ready for our input.&#x20;



It typically includes information such as the current user, the computer’s hostname, and the current working directory.&#x20;



The prompt is usually displayed on a new line, and the cursor is positioned after the prompt, ready for the user to start typing a command.



It can be customized to provide useful information to the user. The format can look something like this

```bash
<username>@<hostname><current working directory>$
```

The home directory for a user is marked with a tilde <`~`> and is the default folder when we log in.

&#x20;&#x20;

```bash
<username>@<hostname>[~]$
```

The dollar sign, in this case, stands for a user. As soon as we log in as `root`, the character changes to a `hash` <`#`> and looks like this:

```bash
root@htb[/htb]#
```

For example, when we upload and run a shell on the target system, we may not see the username, hostname, and current working directory.&#x20;



This may be due to the PS1 variable in the environment not being set correctly.&#x20;



In this case, we would see the following prompts:

#### Unprivileged - User Shell Prompt

```bash
$
```

#### Privileged - Root Shell Prompt

```bash
#
```

In addition to providing basic information like the current user and working directory, we can customize to display other information in the prompt, such as the date and time, IP address, date, time, the exit status of the last command, and more.&#x20;



This is especially useful for us during our penetration tests because we can use various tools and **possibilities** like `script` or the `.bash_history` to filter and print all the commands we used and sort them by date and time.&#x20;



For example, the prompt could be set to display the full path of the current working directory instead of just the current directory name, which can also include the target’s IP address if we work organized.

The prompt can be customized using special characters and variables in the shell’s configuration file (`.bashrc` for the Bash shell).&#x20;



For example, we can use: the `\u` character to represent the current username, `\h` for the hostname, and `\w` for the current working directory.

<table data-header-hidden><thead><tr><th width="249"></th><th></th></tr></thead><tbody><tr><td><strong>Special Character</strong></td><td><strong>Description</strong></td></tr><tr><td><code>\d</code></td><td>Date (Mon Feb 6)</td></tr><tr><td><code>\D{%Y-%m-%d}</code></td><td>Date (YYYY-MM-DD)</td></tr><tr><td><code>\H</code></td><td>Full hostname</td></tr><tr><td><code>\j</code></td><td>Number of jobs managed by the shell</td></tr><tr><td></td><td>Newline</td></tr><tr><td></td><td>Carriage return</td></tr><tr><td><code>\s</code></td><td>Name of the shell</td></tr><tr><td></td><td>Current time 24-hour (HH:MM:SS)</td></tr><tr><td></td><td>Current time 12-hour (HH:MM:SS)</td></tr><tr><td><code>\@</code></td><td>Current time</td></tr><tr><td><code>\u</code></td><td>Current username</td></tr><tr><td><code>\w</code></td><td>Full path of the current working directory</td></tr></tbody></table>

Customizing the prompt can be a useful way to make your terminal experience more personalized and efficient.&#x20;



It can also be a helpful tool for troubleshooting and problem-solving, as it can provide important information about the system’s state at any given time.

In addition to customizing the prompt, we can customize their **terminal environment** with different color schemes, fonts, and other settings to make their work environment more visually appealing and easier to use.

However, we see the same as when working on the Windows GUI here.&#x20;



We are logged in as a user on a computer with a specific name, and we know which directory we are in when we navigate through our system.&#x20;



Bash prompt can also be customized and changed to our own needs.&#x20;



The adjustment of the bash prompt is outside the scope of this module.&#x20;



However, we can look at the [bashrcgenerator](http://bashrcgenerator.com/) and [powerline](https://github.com/powerline/powerline), which gives us the possibility to adapt our prompt to our needs.\
