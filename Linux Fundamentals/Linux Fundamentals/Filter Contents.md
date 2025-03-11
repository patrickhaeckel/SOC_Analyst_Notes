---
title: 'Filter Contents '
updated: 2025-03-03 01:18:57Z
created: 2025-03-02 00:37:08Z
---

### **1\. `more`**

#### **Description:**

`more` is a command-line utility that allows users to view the content of a file one screen at a time. It is useful for navigating through large files without overwhelming the terminal.

#### **Example:**

`cat /etc/passwd | more`

### **2**\*\*. `less`\*\*

#### **Description:**

`less` is a more advanced version of `more` that allows both forward and backward navigation through a file. Unlike `more`, it does not leave the file output in the terminal after exiting.

#### **Example:**

`less /etc/passwd`

### **3\. `head`**

#### **Description:**

`head` is used to display the first few lines of a file. By default, it shows the first 10 lines.

#### **Example:**

`head /etc/passwd`

### **4\. `tail`**

#### **Description:**

`tail` displays the last few lines of a file. By default, it shows the last 10 lines.

#### **Example:**

`tail /etc/passwd`

### **5\. `sort`**

#### **Description:**

`sort` organizes the lines of a file or input alphabetically or numerically.

#### **Example:**

`cat /etc/passwd | sort`

### **6\. `grep`**

#### **Description:**

`grep` is used to search for specific patterns in a file or command output.

#### **Example:**

`cat /etc/passwd | grep "/bin/bash"`

### **7\. `cut`**

#### **Description:**

`cut` extracts specific fields or characters from a file or command output.

#### **Example:**

`cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1`

### **8\. `tr`**

#### **Description:**

`tr` (translate) replaces or deletes characters from input.

#### **Example:**

`cat /etc/passwd | grep -v "false\|nologin" | tr ":" " "`

### **9\. `column`**

#### **Description:**

`column` formats text input into columns for better readability.

#### **Example:**

`cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t`

### **10\. `awk`**

#### **Description:**

`awk` is a powerful text-processing tool used for extracting and manipulating data.

#### **Example:**

`cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'`

### **11\. `sed`**

#### **Description:**

`sed` (stream editor) is used for text substitution, deletion, and manipulation.

#### **Example:**

`cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/THB/g'`

&nbsp;

### **12\. `wc`**

#### **Description:**

`wc` (word count) counts lines, words, and characters in a file.

#### **Example:**

`cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | wc -l`

&nbsp;

&nbsp;