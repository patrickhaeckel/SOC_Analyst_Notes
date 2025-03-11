---
title: Working with Files and Directories
updated: 2025-03-03 22:36:02Z
created: 2025-03-03 22:35:43Z
---

&nbsp;

**The `-p` (parents) option, allows you to create parent directories automatically.**

`mkdir -p Storage/local/user/documents`

**`tree` allows you to see the structure of the specified location  
<br/>**`tree .`

```
.
├── info.txt
└── Storage
    └── local
        └── user
            ├── documents
            └── userinfo.txt

4 directories, 2 files
```

&nbsp;

**We can use the single dot (.) to indicate that we want to start from the current directory.**

`touch ./Storage/local/user/userinfo.txt`

**With the command `mv`, we can move and also rename files and directories. The syntax for this looks like this:**

`shell-session mv <file/directory> <renamed file/directory>`  
<br/>`mv info.txt information.txt`

&nbsp;