---
title: File Descriptors and Redirections
updated: 2025-03-09 00:48:23Z
created: 2025-03-04 01:08:44Z
---

#### Redirect STDOUT and STDERR to Separate Files

`shell-session find /etc/ -name shadow 2> stderr.txt 1> stdout.txt`

**If we want to append `STDOUT` to our existing file, we can use the double greater-than sign (`>>`).**

`shell-session find /etc/ -name passwd >> stdout.txt 2>/dev/null`

**`>> stdout.txt`**

- This appends **standard output (stdout)** (i.e., matching results) to `stdout.txt`.
- It **does not** capture **standard error (stderr),** however  `2>/dev/null` avoids error from showing in terminal effectively sending them to /dev/null/

#### Redirect STDIN

`shell-session cat < stdout.txt`

#### Redirect STDIN Stream to a File

`shell-session cat << EOF > stream.txt`

&nbsp;

### **Standard Input vs. Arguments**

In Linux, when running commands, input can come in two ways:

1.  **Standard Input (stdin)** → Data is provided interactively or from another command (e.g., using pipes `|` or redirection `<`).
2.  **Arguments** → Data is given directly as part of the command itself.

#### **Example of Arguments:**

`ls /home/user/Documents`

- Here, `/home/user/Documents` is an **argument**.
- `ls` uses this argument to determine which directory to list.

#### **Example of Standard Input:**

`echo "/home/user/Documents" | xargs ls`

- The directory path is **passed via standard input**.
- `xargs` reads from standard input and passes it as an argument to `ls`.

#### **Key Differences**

| Feature | Standard Input (stdin) | Arguments |
| --- | --- | --- |
| How Data is Given | Through pipes \` | `, redirection` <\`, or user input |
| Example Usage | \`echo "text" | cat\` |
| Flexibility | Can handle dynamic input | Works well for static input |
| Processing | Read as a stream | Parsed before execution |

&nbsp;