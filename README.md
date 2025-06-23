# Self-Hosted-Photo-Backup-System using Raspberry Pi

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
   ```

### 2️⃣ Connect Raspberry Pi to Internet via Tailscale

💡 Tailscale allows secure private networking without public IP or port forwarding.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Sign in via browser when prompted.  
Get your Pi’s Tailscale IP:
```bash
tailscale ip -4
```







