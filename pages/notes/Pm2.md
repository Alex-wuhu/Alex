---
title: PM2 Cheat Sheet
date: 2025-10-10
type: note
tags:
  - CLI
---

A quick reference guide for PM2 (Process Manager 2), a production process manager for Node.js applications.

[[toc]]

## Installation

First, you need to install PM2 globally using npm.

```bash
npm install pm2 -g
```

## Basic Process Management

| Command                            | Description                                      |
| ---------------------------------- | ------------------------------------------------ |
| `pm2 start app.js`                 | Start an application.                            |
| `pm2 start app.js --name "my-app"` | Start an application with a custom name.         |
| `pm2 start app.js --watch`         | Start an application and watch for file changes. |
| `pm2 ls` or `pm2 list`             | List all running processes.                      |
| `pm2 stop <id_or_name>`            | Stop a specific process.                         |
| `pm2 restart <id_or_name>`         | Restart a specific process.                      |
| `pm2 reload <id_or_name>`          | Reload a process with 0s downtime.               |
| `pm2 delete <id_or_name>`          | Stop and delete a process from the list.         |
| `pm2 stop all`                     | Stop all processes.                              |
| `pm2 restart all`                  | Restart all processes.                           |
| `pm2 delete all`                   | Stop and delete all processes.                   |

## Monitoring and Logging

| Command                 | Description                                |
| ----------------------- | ------------------------------------------ |
| `pm2 monit`             | Open a real-time monitoring dashboard.     |
| `pm2 show <id_or_name>` | Show detailed information about a process. |
| `pm2 logs`              | Display logs for all processes.            |
| `pm2 logs <id_or_name>` | Display logs for a specific process.       |
| `pm2 logs --lines 200`  | Display the last 200 lines of logs.        |
| `pm2 flush`             | Clear all log files.                       |

> [!NOTE]
> You can also tail log files directly. The default path is usually `~/.pm2/logs/` or `/var/log/pm2/`.
>
> ```bash
> tail -f /var/log/pm2/my-app-0.log
> ```

## Startup and Persistence

To ensure your applications restart automatically after a server reboot.

| Command         | Description                            |
| --------------- | -------------------------------------- |
| `pm2 startup`   | Generate a startup script for your OS. |
| `pm2 save`      | Save the current process list.         |
| `pm2 resurrect` | Restore previously saved processes.    |
| `pm2 unstartup` | Disable the startup script.            |

## Cluster Mode

To run your application in cluster mode and take advantage of multi-core systems.

```bash
# Start app in cluster mode, using all available CPUs
pm2 start app.js -i max

# Start app with a specific number of instances
pm2 start app.js -i 4
```
