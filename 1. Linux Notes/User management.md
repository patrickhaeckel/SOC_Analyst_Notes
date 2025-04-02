---
title: User management
updated: 2025-03-12 03:30:53Z
created: 2025-03-04 17:47:01Z
---

&nbsp;

| Command | Description | Example |
| --- | --- | --- |
| `sudo` | Execute command as a different user. | `sudo apt update` (Run package update as superuser) |
| `su` | Switch to another user (default: root). | `su - username` (Switch to user "username") |
| `useradd` | Create a new user. | `sudo useradd -m newuser` (Create a user with a home directory) |
| `userdel` | Delete a user account. | `sudo userdel -r olduser` (Remove user and home directory) |
| `usermod` | Modify a user account. | `sudo usermod -aG sudo username` (Add user to "sudo" group) |
| `addgroup` | Add a new group. | `sudo addgroup developers` (Create a "developers" group) |
| `delgroup` | Delete a group. | `sudo delgroup developers` (Remove the "developers" group) |
| `passwd` | Change user password. | `passwd username` (Change password for "username") |

&nbsp;

Most common options for each command and what they do:

| Command | Option | Description | Example |
| --- | --- | --- | --- |
| `sudo` | `-u` *user* | Run command as a specific user | `sudo -u username command` |
|     | `-i` | Open a login shell as root | `sudo -i` |
|     | `-s` | Run a shell as another user | `sudo -s` |
| `su` | `-` | Simulate a full login shell. Switch user | `su - username` |
|     | `-c` *command* | Run a command as another user | `su -c "ls /root" username` |
| `useradd` | `-m` | Create a home directory | `sudo useradd -m newuser` |
|     | `-s` *shell* | Specify the user’s default shell | `sudo useradd -s /bin/bash newuser` |
|     | `-G` *group* | Add the user to a specific group | `sudo useradd -G developers newuser` |
|     | `-d` *dir* | Set a custom home directory | `sudo useradd -d /custom/home newuser` |
| `userdel` | `-r` | Remove the user’s home directory | `sudo userdel -r olduser` |
| `usermod` | `-aG` *group* | Append user to a group | `sudo usermod -aG sudo username` |
|     | `-l` *newname* | Change the username | `sudo usermod -l newname oldname` |
|     | `-d` *dir* | Change the user’s home directory | `sudo usermod -d /new/home username` |
| `addgroup` | *(no options needed)* | Simply creates a new group | `sudo addgroup developers` |
| `delgroup` | *(no options needed)* | Removes a group | `sudo delgroup developers` |
| `passwd` | `-e` | Expire a user’s password (force change on next login) | `sudo passwd -e username` |
|     | `-l` | Lock a user account | `sudo passwd -l username` |
|     | `-u` | Unlock a user account | `sudo passwd -u username` |