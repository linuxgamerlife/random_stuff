# Multi-Distro Test Rig Setup Guide

Fedora KDE Live USB • 1 TB NVMe • 5 Distros • Shared Games  

**Disclaimer: Use at your own risk.**

---

## Quick navigation

[1 Purpose](#1-purpose-of-this-setup)  
[2 Layout](#2-final-disk-layout)  
[3 Live USB](#3-boot-fedora-kde-live-usb)  
[4 Partition tool](#4-open-kde-partition-manager)  
[5 Confirm disk](#5-confirm-the-correct-disk)  
[6 Wipe](#6-delete-existing-partitions)  
[7 GPT](#7-create-a-gpt-partition-table)  
[8 EFI](#8-create-the-efi-system-partition)  
[9 Swap](#9-create-the-swap-partition)  
[10 Distro partitions](#10-create-the-five-distro-partitions)  
[11 Games](#11-create-the-games-partition)  
[12 Apply](#12-apply-changes-and-reboot-once)  
[13 First install](#13-install-the-first-system-initial-boot-owner)  
[14 Remaining installs](#14-install-the-remaining-four-systems)  
[15 Boot chain](#15-how-booting-works-end-to-end)  
[16 GRUB controller](#16-choose-a-primary-grub-manager)  
[17 Rebuild GRUB](#17-rebuilding-grub-on-any-system-and-why-commands-differ)  
[18 EFI boot entries](#18-firmware-boot-entries-and-efibootmgr)  
[19 Mount games](#19-mount-the-games-partition-automatically)  
[20 Steam](#20-configure-steam-to-use-the-shared-library)

---

# 1. Purpose of this setup

You are building a controlled multi-boot test platform so you can compare five distros under the same conditions.

Each distro gets:

• Identical root partition  
• Shared EFI partition  
• Shared swap partition  
• Shared games/data partition  

**What this does:**  
Creates a repeatable test rig where installs are isolated and comparisons stay fair.

---

# 2. Final disk layout

The NVMe will be split into fixed roles.

| Partition | Role |
|---------|------|
| p1 | EFI • 1 GB • FAT32 |
| p2 | SWAP • 32 GB |
| p3 | DISTRO1 • 50 GB • ext4 |
| p4 | DISTRO2 • 50 GB • ext4 |
| p5 | DISTRO3 • 50 GB • ext4 |
| p6 | DISTRO4 • 50 GB • ext4 |
| p7 | DISTRO5 • 50 GB • ext4 |
| p8 | GAMES • Remaining space • ext4 |

**What this does:**  
Prevents installers from inventing layouts and keeps tests comparable.

---

# 3. Boot Fedora KDE Live USB

Boot into the Live session.

Do NOT install yet.

Live mode runs entirely from USB and RAM.

This allows you to safely modify the disk.

**What this does:**  
Allows safe disk modification without conflicts.

---

# 4. Open KDE Partition Manager

Launch:

Application Menu → KDE Partition Manager

This tool stages disk changes and only applies them when you click Apply.

Nothing changes until Apply is pressed.

**What this does:**  
Gives direct, reliable control over partition layout.

---

# 5. Confirm the correct disk

Select:

```
/dev/nvme???
```

Verify:

• Disk size  
• Disk model  
• Correct disk  

WARNING: Deleting partitions is destructive.

If you have multiple NVMe drives, triple-check.

**What this does:**  
Prevents wiping the wrong disk.

---

# 6. Delete existing partitions

Delete ALL partitions on:

```
/dev/nvme1n1
```

Click Apply.

This removes filesystem metadata and frees the disk.

**What this does:**  
Resets disk to a clean starting point.

---

# 7. Create a GPT partition table

Create new partition table:

```
GPT
```

GPT is required for modern UEFI systems.

Supports:

• Proper boot support  
• Large disks  
• Reliable partition mapping  

**What this does:**  
Makes disk fully UEFI compatible.

---

# 8. Create the EFI System Partition

Create:

```
Size: 1 GB
Filesystem: FAT32
Label: EFI
Flag: boot
```

Do NOT use bios-grub.

UEFI firmware reads FAT32 only.

Each distro stores boot files inside:

```
/boot/efi/EFI/<distro-name>/
```

**What this does:**  
Creates shared boot location for all distros.

---

# 9. Create the SWAP partition

Create:

```
Size: 32 GB
Type: swap
```

Swap provides overflow memory.

Prevents freezes during heavy load.

Useful for:

• Gaming  
• Recording  
• Multitasking  

**What this does:**  
Improves stability.

---

# 10. Create the five distro partitions

Create five partitions:

```
Size: 50 GB each
Filesystem: ext4
Labels:
DISTRO1
DISTRO2
DISTRO3
DISTRO4
DISTRO5
```

Each partition will contain a full Linux installation.

Each distro mounts its partition as:

```
/
```

ext4 is used for consistency.

**What this does:**  
Creates five isolated OS environments.

---

# 11. Create the GAMES partition

Create final partition using remaining space:

```
Label: GAMES
Filesystem: ext4
```

Used for:

• Steam libraries  
• Downloads  
• Media  
• Benchmarks  

**What this does:**  
Preserves shared data across reinstalls.

---

# 12. Apply changes and reboot once

Click Apply.

Wait until finished.

Reboot back into Live USB once.

This ensures kernel reloads the new layout cleanly.

**What this does:**  
Finalises disk structure.

---

# 13. Install the first system (initial boot owner)

Boot installer for first distro.

Select Manual Partitioning.

Use:

```
EFI (nvme1n1p1)
Mount: /boot/efi
Format: YES

ROOT (DISTRO1)
Mount: /
Format: YES

SWAP (nvme1n1p2)
Type: swap
```

Formatting EFI is done only once.

**What this does:**  
Creates initial boot chain.

---

# 14. Install the remaining four systems

For each additional distro:

```
EFI (nvme1n1p1)
Mount: /boot/efi
Format: NO

ROOT (DISTROX)
Mount: /
Format: YES

SWAP
Type: swap
```

Never format EFI again.

Formatting EFI deletes bootloaders.

**What this does:**  
Adds distros safely.

---

# 15. How booting works end-to-end

Boot sequence:

1 Firmware starts  
2 Firmware reads EFI partition  
3 Bootloader loads  
4 GRUB menu appears  
5 Selected kernel loads  
6 Linux starts  

EFI is critical.

Breaking EFI breaks boot.

**What this does:**  
Explains boot chain.

---

# 16. Choose a primary GRUB manager

Select one distro as primary controller.

Use it to manage GRUB.

This prevents conflicts.

**What this does:**  
Keeps boot predictable.

---

# 17. Rebuilding GRUB on any system

Find grub.cfg location:

```
sudo find /boot -name grub.cfg
```

Rebuild using correct tool:

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

or

```
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

Use correct path discovered earlier.

**What this does:**  
Updates boot menu.

---

# 18. Firmware boot entries and efibootmgr

View boot entries:

```
sudo efibootmgr -v
```

Shows:

• Boot order  
• Active bootloaders  

Install if missing using package manager.

**What this does:**  
Controls firmware boot order.

---

# 19. Mount the GAMES partition automatically

Find UUID:

```
lsblk -f
```

Create mount point:

```
sudo mkdir -p /mnt/games
```

Edit fstab:

```
sudo nano /etc/fstab
```

Add:

```
UUID=XXXX /mnt/games ext4 defaults,noatime 0 2
```

Mount immediately:

```
sudo mount -a
```

**What this does:**  
Mounts shared storage automatically.

---

# 20. Configure Steam to use shared library

Inside Steam add library folder:

```
/mnt/games/SteamLibrary
```

This prevents redownloading games.

**What this does:**  
Shares games across distros.

---

# Final notes

Never format EFI after first install.

Use one primary distro to manage GRUB.

Rebuild GRUB only when necessary.

This creates a stable, repeatable multi-distro testing platform.
