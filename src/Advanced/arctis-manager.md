---
title: Installing Arctis Manager
authors:
  - "@elegos, @aiyahhh"
tags:
  - Community
search:
  exclude: true
---

# Installing Arctis Manager

## Purpose
This guide walks you through installing [Linux-Arctis-Manager](https://github.com/elegos/Linux-Arctis-Manager) (v2.0.4+) using Distrobox. This method keeps your host system immutable while providing the manager with necessary hardware access and background services.

## 1. Create and enter the container
We will use a container named `arctis-manager`.

```bash
distrobox create -n arctis-manager --additional-packages "pipx python3-pip python3-devel libusb1-devel"
distrobox enter arctis-manager
```

## 2. Build and Install (Inside Distrobox)
Inside the arctis-manager container, install the modern Python builder uv and build the project from the develop branch.

### Install uv
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.cargo/env
```
### Clone and build
```bash
git clone https://github.com/elegos/Linux-Arctis-Manager.git -b develop
cd Linux-Arctis-Manager
uv build
```
### Install the resulting wheel
```bash
pipx install dist/*.whl --force
```
### Generate hardware rules
We generate them here, then copy them to your home folder to move to the host.
```bash
lam-cli udev write-rules --force
cp /etc/udev/rules.d/91-steelseries-arctis.rules ~/91-steelseries-arctis.rules
```
### Export the application and daemon to your host
```bash
distrobox-export --app lam-gui
distrobox-export --bin $HOME/.local/bin/lam-daemon --export-path ~/.local/bin
```
## Exit the container
```bash
exit
```

## 3. Hardware Permissions (Host Deployment)
Run these commands on your host terminal to allow the container to talk to your USB headset.
### Move the rules to the host system
```bash
sudo mv ~/91-steelseries-arctis.rules /etc/udev/rules.d/
```
### Reload the host udev daemon
```bash
sudo udevadm control --reload-rules && sudo udevadm trigger
```

## 4. Setup Background Daemon (Host)
The Manager requires `lam-daemon` to be running to communicate with the headset via D-Bus. Create a systemd user service to handle this automatically on login.
### Create the service file:
```bash
mkdir -p ~/.config/systemd/user/
nano ~/.config/systemd/user/arctis-manager.service
```
### Paste in configuration
```ini, TOML
[Unit]
Description=Linux Arctis Manager Daemon (Distrobox)
After=graphical-session.target

[Service]
ExecStart=%h/.local/bin/lam-daemon
Restart=always
RestartSec=5

[Install]
WantedBy=graphical-session.target
```
### Save and Exit
Press <kbd>Ctrl</kbd>+<kbd>O</kbd> then <kbd>Enter</kbd> to save, and <kbd>Ctrl</kbd>+<kbd>X</kbd> to exit the editor.

### Enable and start:
```bash
systemctl --user daemon-reload
systemctl --user enable --now arctis-manager.service
```
### Verification (Optional)
To verify the daemon is running and can see your device, run this on your host
```bash
distrobox enter arctis-manager -- lam-cli devices list
```

## 5. Autostart the Tray App (Optional)
To have the tray icon appear automatically on login, create a manual autostart entry on the host.
### Create the autostart folder and file
```bash
mkdir -p ~/.config/autostart/
nano ~/.config/autostart/lam-gui-tray.desktop
```
### Paste in configuration
```ini, TOML
[Desktop Entry]
Categories=Utility;Application;
Exec=/usr/bin/sh -c '$HOME/.local/bin/lam-gui --systray >/dev/null 2>&1'
Icon=arctis-manager
Name=Arctis Manager (system tray)
StartupNotify=true
Terminal=false
Type=Application
```
### Save and Exit
Press <kbd>Ctrl</kbd>+<kbd>O</kbd> then <kbd>Enter</kbd> to save, and <kbd>Ctrl</kbd>+<kbd>X</kbd> to exit.
