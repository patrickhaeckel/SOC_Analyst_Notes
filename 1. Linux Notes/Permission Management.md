---
title: Permission Management
updated: 2025-03-03 01:32:25Z
created: 2025-03-03 00:51:42Z
---

### **Understanding Linux File and Directory Permissions**

Linux uses a permission system to control access to files and directories. These permissions determine who can **read, write, or execute** a file or directory.

* * *

### **Key Permission Types**

Each file or directory has three types of permissions:

1.  **Read (`r`)** → Allows viewing the contents of a file or listing files in a directory.
2.  **Write (`w`)** → Allows modifying a file or adding/removing files in a directory.
3.  **Execute (`x`)** →
    - For files: Allows running them as a program/script.
    - For directories: Allows entering (`cd directory`) and accessing files inside.

* * *

### **How Directory Permissions Work**

#### **1\. Execute (`x`) is required to enter a directory**

Even if a user has **read (`r`)** access to a directory, they cannot access its files unless they also have **execute (`x`)** permission.

Example:

`dr--r--r-- 2 user group 4096 Feb 25 12:00 mydir/`

- Users can **see** the directory exists but **cannot enter it** (`cd mydir` will fail).

Fix:

`chmod +x mydir`

* * *

#### **2\. Execute (`x`) on a directory does not allow modifying its contents**

- Having **execute (`x`)** alone lets a user enter a directory but does **not** allow creating, deleting, or renaming files inside.
- To modify contents, the user needs **write (`w`)** permissions on the directory.

Example:

`dr-x------ 2 user group 4096 Feb 25 12:00 mydir/`

- The user **can enter (`cd mydir`)** but **cannot create/delete files** inside.

Fix:

`chmod +w mydir`

* * *

#### **3\. Executing Files Inside a Directory**

To run a file as a program (`./script.sh`), the **file itself must have execute (`x`)** permissions, even if the directory allows access.

Example:

`-rw-r--r-- 1 user group 100 Feb 25 12:00 script.sh`

- The user **cannot execute** `script.sh`, even if they have execute permissions on the directory.

Fix:

`chmod +x script.sh`

* * *

### **Understanding Linux Permission Representation**

Linux permissions are assigned to **three categories**:

1.  **Owner (User)** → The user who owns the file.
2.  **Group** → Other users in the same group.
3.  **Others** → Everyone else on the system.

Each permission is represented in a **3-character format** (`rwx` for each category).

Example:

`-rwxr--r-- 1 user group 100 Feb 25 12:00 script.sh`

- `rwx` → Owner (`user`) has full permissions.
- `r--` → Group members can only read.
- `r--` → Others can only read.

* * *

### **Setting Permissions with Octal Numbers**

Permissions can also be represented using numbers (**octal notation**):

| Permission | Symbol | Octal Value |
| --- | --- | --- |
| Read | `r` | 4   |
| Write | `w` | 2   |
| Execute | `x` | 1   |

Each permission category is represented by adding these values.

#### **Example: Setting File Permissions Using Octal Notation**

`chmod 755 script.sh`

**Breakdown of `755`:**

- **7 (Owner)** → `rwx` (4+2+1)
- **5 (Group)** → `r-x` (4+0+1)
- **5 (Others)** → `r-x` (4+0+1)

This allows the **owner to read, write, and execute**, while others can **only read and execute**.

* * *

### **Summary**

- **Execute (`x`) is required to enter directories.**
- **Execute (`x`) on a file is needed to run it as a program.**
- **Write (`w`) on a directory is needed to modify its contents.**
- **Permissions can be set using symbolic (`rwx`) or octal (`755`) notation.**

&nbsp;

### **Changing Ownership of Files and Directories with `chown`**

The `chown` command in Linux is used to change the ownership of files and directories. Ownership consists of two components:

1.  **User (Owner)** – The user who owns the file or directory.
2.  **Group** – The group associated with the file or directory.

By default, the user who creates a file or directory becomes its owner, and the group is usually the user’s primary group. However, you can modify ownership using the `chown` command.

* * *

### **Basic Syntax**

`chown <user>:<group> <file/directory>`

- `<user>` → The new owner of the file/directory.
- `<group>` → The new group of the file/directory.
- `<file/directory>` → The target file or directory whose ownership is being changed.

If you want to change only the owner or only the group, you can omit one of them:

- **Change only the owner**:
    
    `chown <user> <file/directory>`
    
- **Change only the group** (use `:` before the group name to indicate that the owner remains unchanged):
    
    `chown :<group> <file/directory>`
    

* * *

### **Examples**

#### **1\. Changing Owner and Group**

`chown root:root shell`

This command assigns ownership of the `shell` file to the user `root` and the group `root`.

#### **2\. Changing Only the Owner**

`chown alice shell`

Now, `alice` is the owner of the file, but the group remains unchanged.

#### **3\. Changing Only the Group**

`chown :developers shell`

This changes the group of the file `shell` to `developers`, while keeping the owner unchanged.

#### **4\. Changing Ownership Recursively (`-R` flag)**

To change ownership of a directory and all its contents:

`chown -R bob:staff /home/project`

This assigns `bob` as the owner and `staff` as the group for `/home/project` and all its files and subdirectories.

#### **5\. Using `chown` with Wildcards**

You can use wildcards to change ownership of multiple files:

`chown alice:users *.txt`

This changes the owner to `alice` and the group to `users` for all `.txt` files in the current directory.

#### **6\. Verifying Ownership Changes**

To confirm the new ownership, use the `ls -l` command:

`ls -l shell`

Example output:

`-rwxr-xr-- 1 alice developers 0 May 4 22:12 shell`

This shows that `alice` is now the owner and `developers` is the group.

* * *

### **Key Takeaways**

- `chown` allows changing file and directory ownership.
- You can modify both owner and group simultaneously (`chown user:group file`).
- Use `-R` to apply changes recursively to all files in a directory.
- To change only the owner: `chown user file`.
- To change only the group: `chown :group file`

&nbsp;

# **Understanding SUID, SGID, and the Sticky Bit in Linux**

Linux provides special permissions beyond standard **read (r), write (w), and execute (x)**, including **Set User ID (SUID)**, **Set Group ID (SGID)**, and the **Sticky Bit**. These permissions allow users to perform specific actions with **elevated privileges** or enforce **restrictions** in shared environments.

* * *

## **1\. SUID (Set User ID)**

### **What is SUID?**

- SUID allows a file to **execute with the permissions of its owner**, rather than the user running it.
- When a user runs an executable with **SUID set**, the program operates as if it were being run by the file’s owner.
- Typically applied to system utilities that require **root privileges** (e.g., `passwd`, which allows users to change their password while modifying a root-owned file).

### **How to Identify SUID**

- Instead of a normal **execute (`x`)** permission in the **owner** section, you'll see an **s**.
    
- Example:
    
    `ls -l /usr/bin/passwd`
    
    Output:
    
    `-rwsr-xr-x 1 root root 54256 Jan 12 12:30 /usr/bin/passwd`
    
    - The **s** in `rws` means **SUID is set**.
    - This allows **any user to execute `passwd` as root**, which is necessary to change passwords.

### **How to Set SUID**

`chmod u+s filename`

Example:

`chmod u+s myscript.shls -l myscript.sh`

Output:

`-rwsr-xr-x 1 user group 54256 Feb 25 14:00 myscript.sh`

Now, anyone running `myscript.sh` will execute it **with the file owner’s privileges**.

### **Security Risks of SUID**

- If a program with SUID has **vulnerabilities**, an attacker can exploit it to **gain unauthorized access**.
- **Never set SUID on scripts** (`.sh`) because they can be manipulated easily.

* * *

## **2\. SGID (Set Group ID)**

### **What is SGID?**

- SGID ensures that a file or directory is **executed using the permissions of its group**.
- Useful for **collaborative environments**, where multiple users need access to shared files with specific permissions.

### **How to Identify SGID**

- Instead of an **execute (`x`)** permission in the **group** section, you’ll see an **s**.
    
- Example:
    
    `ls -l /usr/bin/locate`
    
    Output:
    
    `-rwxr-sr-x 1 root locate 35192 Jan 12 12:30 /usr/bin/locate`
    
    - The **s** in `r-s` means **SGID is set**.
    - Any user running this program will have **group permissions of `locate`**.

### **How to Set SGID**

`chmod g+s filename`

Example:

`chmod g+s shared_script.shls -l shared_script.sh`

Output:

`-rwxr-sr-x 1 user group 35192 Feb 25 14:10 shared_script.sh`

Now, anyone running `shared_script.sh` will **execute it with the group’s privileges**.

### **SGID on Directories**

When applied to directories, **newly created files inherit the directory’s group** instead of the user's primary group.

Example:

`chmod g+s /shared_folderls -ld /shared_folder`

Output:

`drwxr-sr-x 2 user group 4096 Feb 25 14:15 /shared_folder`

Now, **all files created inside `/shared_folder`** will belong to `group`.

* * *

## **3\. Sticky Bit**

### **What is the Sticky Bit?**

- The **Sticky Bit** prevents users from **deleting or renaming files owned by others** in a shared directory.
- Only the **file owner, directory owner, or root** can delete or rename files.

### **Where is it Used?**

- **Public directories like `/tmp`** where multiple users can create files but shouldn’t delete each other’s data.

### **How to Identify the Sticky Bit**

- Instead of a normal **execute (`x`)** permission in the **others** section, you’ll see **t or T**.
    
- Example:
    
    `ls -ld /tmp`
    
    Output:
    
    `drwxrwxrwt 10 root root 4096 Feb 25 14:20 /tmp`
    
    - The **t** at the end means the sticky bit is **enabled**, ensuring files can only be deleted by their owners.

### **How to Set the Sticky Bit**

`chmod +t directory`

Example:

`chmod +t /shared_folderls -ld /shared_folder`

Output:

`drwxrwxrwt 2 user group 4096 Feb 25 14:25 /shared_folder`

Now, **users cannot delete each other’s files** inside `/shared_folder`.

* * *

### **Understanding `T` vs. `t` in the Sticky Bit**

- **Lowercase `t`** → Sticky bit is set, and **execute (`x`) permission exists** for others.
- **Uppercase `T`** → Sticky bit is set, but **others do NOT have execute (`x`)** permission.

Example:

`ls -ld scripts reports`

Output:

`drwxrwxr-t 3 user group 4096 Feb 25 14:30 scriptsdrwxrwxr-T 3 user group 4096 Feb 25 14:32 reports`

- **`scripts`** → Has **sticky bit (`t`) + execute (`x`)** permission, so users can enter it.
- **`reports`** → Has **sticky bit (`T`) but no execute (`x`)**, so users **cannot enter** the directory.

* * *

## **Summary**

| Feature | Symbol in `ls -l` | Applied To | Function |
| --- | --- | --- | --- |
| **SUID** | `s` (in owner) | Files | Executes as **file owner** instead of user. |
| **SGID** | `s` (in group) | Files & Directories | Files execute as **group owner**. New files in SGID directories **inherit group**. |
| **Sticky Bit** | `t` (or `T` if no execute) | Directories | Prevents users from **deleting others' files**. |

### **Commands Recap**

- **Set SUID** → `chmod u+s file`
- **Set SGID** → `chmod g+s file`
- **Set Sticky Bit** → `chmod +t directory`
- **Remove Special Permissions** → `chmod u-s file`, `chmod g-s file`, `chmod -t directory`