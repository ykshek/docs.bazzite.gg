---
title: Bazzite Installation Guide
---

# Bazzite Installation Guide

## Video Walkthrough

<link Wolfyreload's video>

## System Requirements

- Read the [**Hardware Compatibility Guide**](<link doc>) for Bazzite's system requirements.
- Secure Boot and Trusted Platform Module (TPM) are supported on most hardware, but you must [**enroll our key during or after installation**](./secure_boot.md).

### Installer Requirements

- A way to download the Bazzite ISO
  - A download manager (like [**Motrix**](https://motrix.app/)) if the direct download for the Bazzite ISO fails or is downloading too slow.
- A 16GB+ bootable medium like a Flash Drive
  - Software to flash the ISO like **Fedora Media Writer** ([**Windows/macOS**](https://github.com/FedoraQt/MediaWriter/releases) or [**Linux**](https://flathub.org/en/apps/org.fedoraproject.MediaWriter))
- A physical wired keyboard is **recommended** and **required for devices without a touchscreen**.
  - Otherwise, create a User Account with a **username** and a **user password** if you have a keyboard.

## Installation Walkthrough

### Download Bazzite

<screenshot of Bazzite's website on download page>

### Flash ISO

<screenshot of Fedora Media Writer settings>

### Booting The Installer

<screenshot of Bazzite installer first-boot black screen in GRUB>

#### Network 

<screenshot of live ISO connecting to Wi-Fi symbol on KDE image>

#### Select Language, Region, and Keyboard Layout

<screenshot installer>

#### Partitioning Options

!!! warning

    Make sure to only select the appropriate drives to avoid losing data on others, and it is best practice to safely remove any external drives before proceeding.

If you are dual-booting with Windows then read the [dualbooting guide](<link guide>).  Automatic partitioning is the only option available in the live ISO, but it should work for most people's dual-booting usecases.  If you require manual partitioning, then use the legacy ISO and follow the [manual partitioning guide](<link this>).

#### User Account Setup

<screenshot of installer>

#### Reboot

## Post-Installation

<link guide>
