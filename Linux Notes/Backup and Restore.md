---
title: Backup and Restore
updated: 2025-03-15 18:22:33Z
created: 2025-03-12 02:15:23Z
---

## **Rsync**

```shell-session
rsync -av /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

This command copies the entire directory (/path/to/mydirectory) to a remote host (backup_server), placing it in the /path/to/backup/directory. The archive option (-a) ensures that the original file attributes, like permissions and timestamps, are preserved, while the verbose option (-v) displays a detailed progress report of the rsync operation.

```shell-session
rsync -avz --backup --backup-dir=/path/to/backup/folder --delete /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

This `rsync` command is used to **synchronize** a directory (`/path/to/mydirectory`) to a remote backup server (`backup_server`), while also keeping backups of changed or deleted files.

### Breakdown of the command:

- **`rsync`** → The command for syncing files and directories.
- **`-avz`** → Three commonly used options:
    - `-a` (**archive mode**) → Preserves symbolic links, permissions, timestamps, and other file attributes.
    - `-v` (**verbose mode**) → Displays detailed output during execution.
    - `-z` (**compress files during transfer**) → Reduces transfer size and speed up the process.
- **`--backup`** → Enables backup of overwritten or deleted files.
- **`--backup-dir=/path/to/backup/folder`** → Specifies where backups of modified/deleted files should be stored.
- **`--delete`** → Removes files from the destination (`backup_server:/path/to/backup/directory`) that no longer exist in the source (`/path/to/mydirectory`).
- **`/path/to/mydirectory`** → The local directory you want to back up.
- **`user@backup_server:/path/to/backup/directory`** → The remote location where the files will be copied.

&nbsp;

### What Happens When You Run This Command?

1.  **Files and directories from `/path/to/mydirectory` are synchronized** to `/path/to/backup/directory` on `backup_server`.
2.  **Any deleted or modified files are backed up** in `/path/to/backup/folder` before they are removed or replaced.
3.  **Files that no longer exist in the source directory are deleted** from the destination (unless they were backed up).

This setup ensures that:

- You always have an up-to-date backup.
- Previous versions of files are saved before being overwritten or deleted.
- The `--backup-dir` stores **previous versions** of files, instead of permanently losing them.

| Scenario | **With `--backup --backup-dir`** | **Without `--backup --backup-dir`** |
| --- | --- | --- |
| **Deleted files** | Moved to `/backup/server/old_files/` before deletion | Deleted permanently from backup |
| **Modified files** | Old version moved to `/backup/server/old_files/` before replacement | Overwritten with no recovery option |
| **File recovery** | Possible (from `/backup/server/old_files/`) | Not possible (old versions lost) |

By default, rsync **does not automatically rename or timestamp older backups**, so the previous versions in `/backup/server/old_files/` might **get overwritten** each time.

If you want to **keep all versions**, you need to either:

1\. Use `--suffix=_timestamp` to prevent overwriting:

```shell-session
rsync -avz --backup --suffix=_$(date +%F_%T) --backup-dir=/backup/server/old_files/ --delete /home/user/mydirectory user@backup_server:/backup/server/main_backup/```

```

This renames backups like `file1.txt_2025-03-12_14:30:00`.

&nbsp;

2\. Organize backups by date:

```shell-session
rsync -avz --backup --backup-dir=/backup/server/old_files/$(date +%F) --delete /home/user/mydirectory user@backup_server:/backup/server/main_backup/```

```

&nbsp;

This stores old files in folders like `/backup/server/old_files/2025-03-12/`.

&nbsp;

## **Rsync - Restore Backup**

```shell-session
rsync -av user@remote_host:/path/to/backup/directory /path/to/mydirectory
```

&nbsp;

## **Rsync - Secure Transfer with SSH**

```shell-session
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

&nbsp;

## **Rsync - Test Auto Syncronization**

To enable auto-synchronization using `rsync`, you can use a combination of `cron` and `rsync` to automate the synchronization process. Scheduling the cron job to run at regular intervals ensures that the contents of the two systems are kept in sync.

We create a new script called `RSYNC_Backup.sh`, which will trigger the `rsync` command to sync our local directory with the remote one. However, because we are using a script to perform SSH for the rsync connection, we need to configure key-based authentication. This is to bypass the need to input our password when connecting with SSH.

```shell-session
sudo apt install openssh-server -y  #Install SSH Server
```

```shell-session
sudo systemctl start ssh  #start SSH Server
```

```shell-session
sudo systemctl enable ssh  #enable SSH on startup
```

```shell-session
ssh-keygen -t rsa -b 2048  #First, we generate a key pair for our user. #follow prompts, leave blank if dont want a passphrase
```

```shell-session
ssh-copy-id test@127.0.0.1  #copy our public key to the remote server. In this case the loopback address for test purposes on test user
```

**Now, we can create our script to automate the rsync backup.**

```bash
#!/bin/bash

rsync -avz -e ssh /home/pato/sshtestdir test@127.0.0.1:/home/test/backuptestdir
```

To ensure the script runs properly, we must grant the necessary permissions. It's also crucial to verify that the script is owned by the correct user, as this will limit the access to the intended user and prevent tampering by others.

```shell-session
chmod +x rsync_backup.sh
```

**After that, we can create a crontab that tells `cron` to run the script every hour at the 0th minute.**

Backup and Restore

```shell-session
cronjob -e
```

We can adjust the timing to suit our needs. To do so, the crontab needs the following content:

#### Auto-Sync - Crontab

```shell-session
0 * * * * /path/to/RSYNC_Backup.sh
```

&nbsp;

### **Step-by-Step SSH Authentication Process:**

1.  **You Run an SSH Command (or an rsync over SSH)** `ssh test@127.0.0.1`
    
    - Your system sees that the remote server (`127.0.0.1`) is listed in `~/.ssh/known_hosts`.
    - It checks if there is a **private key** (`~/.ssh/id_rsa`) corresponding to the **public key** stored on the remote server.
2.  **SSH Client Offers the Public Key**
    
    - Your SSH client says:  
        *"Hey, I have a private key. Do you recognize my public key?"*
    - The server looks inside **`~/.ssh/authorized_keys`** to check if it contains the **matching public key**.
3.  **The Server Sends a Challenge (Cryptographic Test)**
    
    - If the public key is found, the remote server **generates a challenge**—a piece of encrypted data.
    - The challenge is **encrypted with the public key** stored on the server.
4.  **Your System Uses the Private Key to Decrypt the Challenge**
    
    - Your system **does not send the private key itself** (it is never transmitted).
    - Instead, it **decrypts the challenge** using the private key stored in `~/.ssh/id_rsa`.
    - If the decrypted challenge matches the expected response, authentication succeeds.
5.  **Server Grants Access Without a Password**
    
    - Since your system **proved it owns the correct private key**, the SSH server **allows access** without asking for a password.