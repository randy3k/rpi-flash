# rpi-flash ⚡

`rpi-flash` is a customizable, interactive, and safe CLI wrapper around `rpi-imager --cli`. It automates cloud-init configurations (hostname, default user, password hashing, SSH keys, Wi-Fi network setup, hardware interfaces) and target disk selection, all from a single lightweight script with zero external Python dependencies.

---

## ✨ Features
- **Auto-Detect block devices**: Safely lists and selects removable USB/SD drives, protecting your primary system drive from accidental overwrite.
- **Dynamic Configuration Wizard**: Prompts you for setup options (hostname, username, password, Wi-Fi, SSH keys, SPI, I2C, etc.) and generates cloud-init configuration files on the fly.
- **On-the-fly Hashing**: Prompts for a plaintext password and safely generates SHA-512 crypt hashes using local OpenSSL to feed into shadow.
- **SSH Key Integration**: Grab local SSH public keys (`~/.ssh/*.pub`) or download keys directly from a GitHub username (`https://github.com/<username>.keys`).
- **Profile Management**: Save configuration presets to `.json` files under `~/.config/rpi-flash/profiles/` and load them instantly.
- **Official OS Catalog Integration**: Queries the official Raspberry Pi OS mirror API to automatically find, download, cache, and checksum the latest images.

---

## 🚀 Quick Start

### 1. Run the flasher interactively
To flash a drive interactively, simply run:
```bash
./rpi-flash flash
```
The wizard will guide you through:
1. Selecting a connected USB/SD device.
2. Selecting an OS image from the official catalog (or specifying a local path).
3. Customizing hostname, user credentials, SSH keys, Wi-Fi, and RPi hardware modules.
4. Prompting you to save this configuration as a profile.
5. Performing safety checks (e.g. auto-unmounting partitions) and running `rpi-imager --cli` under `sudo`.

### 2. Fully Automated (Non-Interactive) Flashing
You can bypass the wizard by providing command-line arguments:
```bash
./rpi-flash flash /dev/sdb \
  --image "Raspberry Pi OS (64-bit)" \
  --profile homelab \
  --hostname cluster-node-1 \
  --non-interactive
```

---

## 📋 Commands & Usage

### Flashing (`rpi-flash flash`)
```
usage: rpi-flash flash [-h] [-i IMAGE] [-p PROFILE] [--hostname HOSTNAME]
                       [--username USERNAME] [--password PASSWORD]
                       [--ssh-key SSH_KEYS] [--github-keys GITHUB_KEYS]
                       [--wifi-ssid WIFI_SSID] [--wifi-pass WIFI_PASS]
                       [--wifi-country WIFI_COUNTRY] [--enable-i2c]
                       [--enable-spi] [--enable-serial] [--enable-onewire]
                       [--enable-usb-gadget] [--non-interactive]
                       [device]
```

### Profile Management (`rpi-flash profile`)
- **List saved profiles**:
  ```bash
  ./rpi-flash profile list
  ```
- **Show configuration details** (sensitive fields like passwords/hashes are masked):
  ```bash
  ./rpi-flash profile show <profile_name>
  ```
- **Create a profile interactively** without flashing:
  ```bash
  ./rpi-flash profile create <profile_name>
  ```
- **Delete a profile**:
  ```bash
  ./rpi-flash profile delete <profile_name>
  ```

### Manage Images
- **List official OS catalog**:
  ```bash
  ./rpi-flash list-images
  ```
- **Download and cache an image**:
  ```bash
  ./rpi-flash download "Raspberry Pi OS (64-bit)"
  ```

---

## 🛠️ Installation & Setup

To make the script executable globally, you can create a symlink to it in your local binary path:

```bash
# Link to user local binaries directory
ln -s "$(pwd)/rpi-flash" ~/.local/bin/rpi-flash

# Ensure ~/.local/bin is in your PATH, then run from anywhere:
rpi-flash list-images
```

---

## 📦 Requirements
- **Python 3** (built-in standard library only)
- **OpenSSL** (pre-installed on Linux, used for password hashing)
- **rpi-imager** (must be installed on the host system)
