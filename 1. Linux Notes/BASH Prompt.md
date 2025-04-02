---
title: BASH Prompt
updated: 2025-03-03 01:34:33Z
created: 2025-02-20 15:27:19Z
---

**&lt;username&gt;@&lt;hostname&gt;&lt;current working directory&gt;$**

**The prompt can be customized using special characters and variables in the shell’s configuration file (`.bashrc` for the Bash shell). For example, we can use: the `\u` character to represent the current username, `\h` for the hostname, and `\w` for the current working directory**

We can look at the [bash-prompt-generator](https://bash-prompt-generator.org/) and [powerline](https://github.com/powerline/powerline), which gives us the possibility to adapt our prompt to our needs.

The `PS1` variable in Linux systems controls how your command prompt looks in the terminal.

The prompt can be customized using special characters and variables in the shell’s configuration file (`.bashrc` for the Bash shell). For example, we can use: the `\u` character to represent the current username, `\h` for the hostname, and `\w` for the current working directory.

| **Special Character** | **Description** |
| --- | --- |
| `\d` | Date (Mon Feb 6) |
| `\D{%Y-%m-%d}` | Date (YYYY-MM-DD) |
| `\H` | Full hostname |
| `\j` | Number of jobs managed by the shell |
| `\n` | Newline |
| `\r` | Carriage return |
| `\s` | Name of the shell |
| `\t` | Current time 24-hour (HH:MM:SS) |
| `\T` | Current time 12-hour (HH:MM:SS) |
| `\@` | Current time |
| `\u` | Current username |
| `\w` | Full path of the current working directory |

&nbsp;