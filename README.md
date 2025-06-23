# Self-Hosted-Photo-Backup-System using Raspberry Pie


Self-host your **photo and video server** with **privacy**, **convenience**, and **remote access** using Immich on a **Raspberry Pi**.

---

## 🚀 Features

- ✅ Full **Immich** deployment on **Raspberry Pi**
- 🔐 Secure **remote access** using **Tailscale** *(no port forwarding needed)*
- 🐳 Lightweight containerized setup using **Docker Compose**
- 💾 Optional support for **external storage** (USB drive)
- 📱 Mobile auto-upload via Immich app

---

## 📦 Requirements

- **Raspberry Pi 4** (4GB RAM minimum recommended)
- **32GB+ microSD card**
- **Internet connection**
- **Tailscale account**
- *(Optional)* USB external storage drive

---

## 🪜 Setup Steps

### 1️⃣ Flash and Set Up Debian on Raspberry Pi

1. Download and install **[Raspberry Pi Imager](https://www.raspberrypi.com/software/)**.
2. Select:
   - **Raspberry Pi OS Lite (64-bit)** or
   - **Debian Bookworm Lite**
3. Flash the image to your SD card.
4. *(Optional)* Enable SSH:
   - After flashing, create an empty file named `ssh` (no extension) in the **boot** partition.
5. Insert the SD card into your Raspberry Pi and **boot it up**.
6. Connect via SSH:
   ```bash
   ssh pi@raspberrypi.local


#!/bin/bash

echo "=== Step 1: Flash and Set Up Debian on Raspberry Pi ==="

echo ""
echo "1. Download Raspberry Pi Imager:"
echo "   👉 https://www.raspberrypi.com/software/"
echo ""

echo "2. Choose:"
echo "   - Raspberry Pi OS Lite (64-bit) OR Debian Bookworm Lite"
echo "   - Flash it to your SD card via GUI"
echo ""

read -p "➡️ Press Enter after flashing the image..."

BOOT_PART="/Volumes/boot"  # On Linux it may be /media/$USER/boot
if [ -d "$BOOT_PART" ]; then
    touch "$BOOT_PART/ssh"
    echo "✅ SSH enabled by creating $BOOT_PART/ssh"
else
    echo "⚠️ Could not detect boot partition. Please manually create a file named 'ssh' in the boot partition."
fi

echo ""
echo "Now insert the SD card into your Raspberry Pi and boot it up."
echo "To connect via SSH:"
echo "   ssh pi@raspberrypi.local"



