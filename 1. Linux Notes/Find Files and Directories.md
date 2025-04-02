---
title: Find Files and Directories
updated: 2025-02-25 17:55:45Z
created: 2025-02-25 16:11:09Z
---

### **Commands I Used:** 

find / -type f -name \*.conf -size +25k -size -28k -newermt 2020-03-03 2>/dev/null  
find / -type f -name \*.bak 2>/dev/null | wc -l  
which xxd

### **Note:** 

| wc -l: This takes the output of the find command and pipes it to wc -l, which counts the number of lines in the output. Each line corresponds to a file, so the result will be the total number of .bak files found.

### **Which Command**

The `which` command helps you find the location of an executable file on your system. It tells you where a program (like `python`, `gcc`, or `wget`) is installed and which version would run if you executed it.

### **Find Command**

The `find` command is a powerful tool for searching files and directories based on different criteria like name, size, owner, or modification date.

#### Example Command:

`find / -type f -name "*.conf" -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null`

### **Why Do We Escape `;` in `find`?**

Since `;` is a special character in the shell, if you use it inside `find` without escaping, the shell interprets it **before** `find` runs.

For example:

`find / -type f -name "*.conf" -exec ls -al {} ;`

This **won't work** and will give an error because the shell sees `;` as a separator **before** `find` gets it.

To prevent this, we **escape it** with a backslash (`\;`):

`find / -type f -name "*.conf" -exec ls -al {} \;`

Now, `find` receives the `;` correctly as part of its `-exec` option.

### **Key Takeaways**

| Syntax | Behavior |
| --- | --- |
| `-exec command {} \;` | Runs the command **once per file** (slow for many files). |
| `-exec command {} +` | Runs the command **once for all files in batches** (faster). |

Use **`\;`** when the command **must** run individually per file.  
Use **`+`** for efficiency when working with **many files**.

&nbsp;

### **Locate Command**

Searching through the entire system for files and directories can be time-consuming, especially when performing multiple searches. The `locate` command provides a faster way to search the system. Unlike the `find` command, `locate` works by querying a local database that holds information about all existing files and directories. To keep this database up-to-date, we can use the following command:  
`sudo updatedb`  
<br/>After updating the database, searching for files with a specific extension, like `.conf`, will yield results much faster than using `find`.

`locate *.conf`  
<br/>However, the `locate` command doesn't offer as many filtering options as `find`. Therefore, it's important to consider which tool to use based on your search needs.