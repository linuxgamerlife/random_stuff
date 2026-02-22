# T2Linux Setup Guide for Fedora (Intel MacBook)

This guide installs the T2Linux kernel and required firmware so that keyboard, touchpad, and Wi-Fi work correctly on Intel MacBooks with the Apple T2 chip.

Tested on: 2019 i5 MacBook Air  
Target OS: Fedora Linux

---

## Step 1: Enable the T2Linux COPR repository

This repository provides the patched kernel required for T2 Mac hardware support.

```bash
sudo dnf copr enable sharpenedblade/t2linux
```

---

## Step 2: Install the T2Linux kernel

This command replaces the standard Fedora kernel with the T2Linux kernel from the COPR repository.

```bash
sudo dnf swap --from-repo="copr:copr.fedorainfracloud.org:sharpenedblade:t2linux" kernel kernel
```

---

## Step 3: Install the T2Linux release package

This installs additional configuration and support files.

```bash
sudo dnf install t2linux-release
```

---

## Step 4: Reboot the system

After reboot, the keyboard and touchpad should work.

```bash
reboot
```

---

## Step 5: Enable Wi-Fi and Bluetooth

Follow the official guide:

https://wiki.t2linux.org/guides/wifi-bluetooth/

---

## Step 6: Install firmware (Required for Wi-Fi on some models)

Download the firmware script:

https://wiki.t2linux.org/tools/firmware.sh

Make it executable:

```bash
chmod +x firmware.sh
```

Run the script:

```bash
sudo ./firmware.sh
```

---

## Important Notes

Method 5 downloads a macOS Recovery image directly from Apple.

You must have an active internet connection using one of the following:

- Ethernet adapter
- USB tethering
- External Wi-Fi adapter

---

## Required packages

Ensure the following tools are installed:

```bash
sudo dnf install python3 rpm coreutils
```

These are used to:

- Rename firmware files
- Create installable firmware packages
- Support firmware installation process

---

## Step 7: Complete firmware installation

When running the script:

- Choose the recommended option
- Wait for completion

Then reboot:

```bash
reboot
```

---

## Result

After reboot, the following should work correctly:

- Keyboard
- Touchpad
- Wi-Fi
- Bluetooth

---

## References

T2Linux official wiki:  
https://wiki.t2linux.org/
