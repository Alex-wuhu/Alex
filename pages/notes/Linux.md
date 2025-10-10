---
title: Linux Notes
date: 2025-10-10
type: note
tags:
  - Linux
  - CLI
---

A collection of useful Linux commands and notes for various tasks.

[[toc]]

## Package Management

### Uninstalling Packages

To remove packages and their configuration files from your system.

> [!TIP] > `apt clean` removes downloaded package files, while `apt autoremove` removes packages that were automatically installed to satisfy dependencies but are now no longer needed.

```bash
# Clean up the local repository of retrieved package files
sudo apt clean && sudo apt autoremove

# Remove a package and its configuration files
sudo apt purge <package_name>
```

## Nvidia GPU

### Monitoring

To monitor the status and usage of your Nvidia GPU in real-time.

```bash
# Watch nvidia-smi, refreshing every 1 second
watch -n 1 nvidia-smi
```

### Confidential Computing

To check the confidential computing status of your Nvidia GPU.

```bash
# Check confidential computing feature status
nvidia-smi conf-compute -f

# Check if the nvidia-persistenced daemon is running
ps aux | grep nvidia-persistenced
```

## Kernel Management

### Listing Kernels

To see all installed kernel images on your system.

```bash
dpkg --list | grep linux-image
```

### Removing a Kernel

> [!WARNING]
> Be careful when removing kernels. Make sure you have a working kernel to boot into.

```bash
# Remove a specific kernel image and its configuration
sudo apt remove --purge linux-image-5.19.0-rc6-snp-host-c4daeffce56e

# Update the GRUB bootloader
sudo update-grub

# Remove any unused packages
sudo apt autoremove --purge
```

### Switching Kernels

To change the default kernel used for booting.

1.  Edit the GRUB configuration file:
    ```bash
    sudo vim /etc/default/grub
    ```
2.  Update the `GRUB_DEFAULT` entry to point to the desired kernel.
3.  Update the GRUB bootloader:
    ```bash
    sudo update-grub
    ```

## TEE (Trusted Execution Environment)

Commands related to AMD SEV and trusted execution.

```bash

# Enable the vfio-pci module

sudo modprobe vfio-pci

# Check for AMD SEV in kernel messages

sudo dmesg | grep -i sev

```

## Windows Network Drive

For information on mapping a network drive in Windows, refer to this guide:

- [Windows 映射网络驱动器](https://www.cnblogs.com/clovershell/articles/16186732.html)
