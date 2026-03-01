# Offline Linux Survival Scenario  
## Adding Fedora Design Suite Apps to a Fedora Games Install Using Only Bootable USB Media

Disclaimer: Use at your own risk.

## Scenario

The global internet is offline.

Infrastructure has failed. Mirrors are unreachable. Package servers cannot be contacted. There is no external network to depend on.

You are in a garage with a working PC and a small generator.

You have two USB sticks:

- USB 1: Fedora Games Lab (already installed on the machine) (7GB)
- USB 2: Fedora Design Suite (bootable USB) (4.6GB)

That is your entire software ecosystem.

The goal is simple:

Install the Fedora Design Suite applications onto your existing Fedora Games Lab system using only these two bootable USB drives.

There is no network, external repositories and no downloads.

---

## Why This Works on Fedora

Fedora is built around structured repositories and a package manager called DNF.

A bootable Fedora USB is not just an installer. It contains:

- RPM packages  
- Repository metadata  
- A defined package group layout  

Because your installed Fedora system also uses DNF, it understands this structure. That means a second Fedora bootable USB can be treated as a local software source.

You are not reinstalling the system.  
You are temporarily using the second USB as a repository.

---

## High-Level Conceptual Steps

### Step 1: Boot into the Installed Fedora Games Lab System

Start the machine normally and log in.

You now have a functioning Fedora system with DNF available. By default, it expects online repositories, but those are unreachable in this scenario.

You will replace the idea of an online repository with a physical one.

---

### Step 2: Insert the Fedora Design Suite Bootable USB

Plug in the second USB.

This USB contains a full filesystem, including the RPM packages and repository metadata used during installation.

Your system detects it as a readable storage device.

---

### Step 3: Access the USB Filesystem

From your running system, access the contents of the Design Suite USB.

You are not booting from it.

You are simply reading it like a disk that contains software packages and structured metadata.

This is critical. The metadata allows DNF to understand what packages are available and how they relate to each other.

---

### Step 4: Temporarily Add the USB as a Local Repository

Tell DNF to treat the mounted Design Suite USB as a repository source.

At this stage, nothing is installed.

You are only expanding the list of locations DNF is allowed to search for packages.

---

### Step 5: Refresh Repository Metadata

DNF reads the metadata from the USB.

It now understands:

- What design packages are available  
- What versions exist  
- What dependencies are required  

Your offline system now sees the Design Suite content as installable software.

---

### Step 6: Install the Design Suite Applications

Install either:

- The full Design Suite package group, or  
- Specific design applications you require  

DNF resolves dependencies using the packages available on the USB and installs them into your running system.

The installation process is identical to an online install. The only difference is the physical source of the packages.

---

### Step 7: Remove the Temporary Repository

Once installation completes:

- Remove the temporary repository configuration  
- Unmount the USB  
- Remove the drive  

Your Fedora Games Lab system now includes the Design Suite applications.

Fully offline. Fully functional.

---

## Why This Cannot Be Done the Same Way on Windows

Windows installation media is not structured as a reusable package repository.

It contains:

- A system image  
- Setup tools  
- Core operating system components  

Windows applications are typically distributed as standalone installers rather than through a unified repository model.

While you can store installers on a USB and run them manually, Windows installation media cannot be mounted and treated as a structured software source in the same way Fedora media can.

Fedora’s repository-based design makes this modular offline expansion possible.

---

## Summary

With only:

- One Fedora Games Lab install  
- One Fedora Design Suite bootable USB  
- No network access  

You can expand your system by reusing the second USB as a local repository.

In a disconnected world, structure matters.

Fedora’s package model allows physical media to become a portable software ecosystem.
