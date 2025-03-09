---
title: TERMS
updated: 2025-03-03 22:56:37Z
created: 2025-03-03 22:56:27Z
---

**Inode:**

- An inode (index node) is a data structure that stores metadata about a file. This metadata includes information such as the file's size, permissions, ownership, timestamps, and location on the disk.
- Each file in a Linux file system has a unique inode number that identifies it.
- Inodes are persistent and stored on the disk. They exist even when a file is not open.

**File Descriptor:**

- A file descriptor is a non-negative integer that uniquely identifies an open file within a process. It is a handle that a process uses to interact with a file.
- File descriptors are temporary and exist only while a file is open by a process. When a process closes a file, the file descriptor is released.
- Each process has its own set of file descriptors.

&nbsp;