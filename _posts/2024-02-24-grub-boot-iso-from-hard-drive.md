---
title: Grub&#58 Boot ISO Directly From Hard Disk
author: edc0
#date: 2024-10-08 14:10:00 +05:30
categories: [Ubuntu, Grub2, Tutorial]
tags: [grub2, iso, ubuntu, boot]
render_with_liquid: false
---

### Booting Linux ISO with Grub2

Grub2 can directly boot ISO images of many Linux distributions if configured correctly. Below is a guide for setting up Grub2 to boot from an ISO image.

#### Preparing the ISO

Ensure the ISO image is stored on a partition that doesn't have an operating system installed. For simplicity, we can create a directory inside `/boot`, assuming it resides on a separate partition.

1. Create the directory and copy your ISO image:

    ```bash
    sudo mkdir /boot/iso
    sudo cp ~/Desktop/name.iso /boot/iso
    ```

    In this example, replace `~/Desktop/name.iso` with the actual path to your ISO image.

#### Adding a Grub2 Entry

The next step is to add an entry for the ISO image in the `/etc/grub.d/40_custom` file.

1. Open the file for editing:

    ```bash
    sudo nano /etc/grub.d/40_custom
    ```

2. Add the following configuration to the file:

    ```bash
    #!/bin/sh
    echo "Adding 40_custom." >&2
    exec tail -n +4 $0

    menuentry "Kubuntu ISO" {
        set isofile="/boot/iso/ubuntu-24.04.iso"
        loopback loop (hd0,7)$isofile
        linux (loop)/casper/vmlinuz boot=casper iso-scan/filename=$isofile noprompt noeject
        initrd (loop)/casper/initrd.lz
    }
    ```

    - **Ubuntu ISO**: The label that will appear in the Grub menu.
    - **/boot/iso/ubuntu-24.04.iso**: The path to your ISO file.
    - **(hd0,7)**: The partition containing the ISO file.

3. Save and close the file.

#### Identifying the Correct Partition

To identify the correct partition for your ISO, run the following command:

```bash
sudo fdisk -l
```

This will list all the partitions. Adjust `(hd0,7)` in the Grub entry to reflect the partition where the ISO is stored. For example:
- If the ISO is in `sda1`, change `(hd0,7)` to `(hd0,1)`.
- If the ISO is in `sdb1`, change it to `(hd1,1)`.

#### Finalizing the Configuration

1. Update Grub:

    ```bash
    sudo update-grub
    ```

2. Reboot your system and select the new ISO entry from the Grub menu.

#### Installing from the ISO

If you are installing the system from the ISO, you'll need to unmount the ISO image before the installer can make changes to the disk.

1. In the live environment, open a terminal and run:

    ```bash
    sudo umount -l /isodevice
    ```

2. After this, you can proceed with the installation by double-clicking the "Install" icon on the desktop.

That's all! You should now be able to boot and install from the ISO image using Grub2.

---
> Note for long lost Netboot install (which wasn't updated after 20.04), it would require slightly different approach. 
{: .prompt-tip }

Put the downloaded files into /boot/netboot and added the following entry to the grub config:

```conf
menuentry "NetInstall" {
    linux (hd0,0)/boot/netboot/ubuntu-installer/amd64/linux
    initrd (hd0,0)/boot/netboot/ubuntu-installer/amd64/initrd.gz
}
```


Then `sudo update-grub`
