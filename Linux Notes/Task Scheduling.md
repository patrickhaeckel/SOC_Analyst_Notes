---
title: Task Scheduling
updated: 2025-03-12 04:16:56Z
created: 2025-03-09 00:17:12Z
---

## Systemd

`sudo mkdir /etc/systemd/system/mytimer.timer.d` # Create a directory for the timer

`sudo vim /etc/systemd/system/mytimer.timer` # Create and edit the timer file

\[Unit\]  
Description=My Timer

\[Timer\]  
OnBootSec=3min    **\# Runs 3 minutes after boot**  
OnUnitActiveSec=1h   **\# Repeats every hour**

\[Install\]  
WantedBy=timers.target  #

`sudo vim /etc/systemd/system/mytimer.service` # Create and edit the service file

\[Unit\]  
Description=My Service

\[Service\]  
ExecStart=/full/path/to/my/script.sh # Specify the script to execute

\[Install\]  
WantedBy=multi-user.target  #Ensures it starts in a standard multi-user system.

`sudo systemctl daemon-reload` # Reload systemd configuration  
`sudo systemctl start mytimer.timer` # Start the timer manually  
`sudo systemctl enable mytimer.timer` # Enable the timer at boot

## Cron Syntax

```txt
# System Update
0 */6 * * * /path/to/update_software.sh  # Runs every 6 hours (at minute 0 of the hour)

# Execute scripts
0 0 1 * * /path/to/scripts/run_scripts.sh  # Runs at midnight on the 1st day of each month

# Cleanup DB
0 0 * * 0 /path/to/scripts/clean_database.sh  # Runs at midnight on every Sunday

# Backups
0 0 * * 7 /path/to/scripts/backup.sh  # Runs at midnight on every Sunday (same as cleanup)

# Another Example
* * * * * /home/pato/sshtestdir/rsync_backup.sh  # Runs every minute

# Run every minute
*/1 * * * * /home/pato/sshtestdir/rsync_backup.sh  # Runs every minute (equivalent to *)

```

&nbsp;

┌───────── Minute (0 - 59)  
│ ┌───────── Hour (0 - 23)  
│ │ ┌───────── Day of the month (1 - 31)  
│ │ │ ┌───────── Month (1 - 12)  
│ │ │ │ ┌───────── Day of the week (0 - 7) (Sunday = 0 or 7)  
│ │ │ │ │  
\* \* \* \* \* command-to-execute

&nbsp;**Summary of What This Crontab Does**

| Task | Schedule |
| --- | --- |
| **System Update** | Every 6 hours (midnight, 6 AM, noon, 6 PM) |
| **Execute Scripts** | 1st of every month at midnight |
| **Cleanup DB** | Every Sunday at midnight |
| **Backups** | Every Sunday at midnight |

&nbsp;

## Systemd vs. Cron

Systemd and Cron are both tools that can be used in Linux systems to schedule and automate processes. The key difference between these two tools is how they are configured. With Systemd, you need to create a timer and services script that tells the operating system when to run the tasks. On the other hand, with Cron, you need to create a `crontab` file that tells the cron daemon when to run the tasks.