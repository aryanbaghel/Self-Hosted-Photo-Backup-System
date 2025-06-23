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
  
### 3️⃣ Install Docker and Docker Compose

💡 Use Docker to containerize the Immich deployment.  

Install Docker
```bash
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $USER
newgrp docker  # Apply group change immediately
```  
Install Docker Compose
```bash
sudo apt update
sudo apt install docker-compose -y
```

> 🔄 *Restart your Raspberry Pi if you get Docker permission errors.*

---  

  

### 4️⃣ Install Immich via Docker Compose

💡 This will deploy the full Immich stack using `docker-compose`.  
```bash
Create folder and docker-compose.yml

mkdir ~/immich && cd ~/immich

cat <<EOF > docker-compose.yml
version: "3.8"
services:
  immich-server:
    image: ghcr.io/immich-app/immich-server:release
    container_name: immich-server
    restart: always
    ports:
      - "2283:3001"
    environment:
      DB_HOSTNAME: postgres
      DB_USERNAME: postgres
      DB_PASSWORD: postgres
      DB_DATABASE_NAME: immich
    depends_on:
      - postgres
    volumes:
      - ./upload:/usr/src/app/upload

  immich-ml:
    image: ghcr.io/immich-app/immich-machine-learning:release
    container_name: immich-ml
    restart: always
    deploy:
      resources:
        limits:
          memory: 1g

  postgres:
    image: postgres:14
    container_name: immich-postgres
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: immich
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
EOF
```

Start the services
```bash
docker-compose up -d
```

Check running containers
```bash
docker ps
```


📸 *Screenshot Placeholder: `docker ps` output showing Immich containers*

---

## 5️⃣ Access Immich via Tailscale

💡 Open Immich from any device connected to your Tailscale network.


# Step 5: Access Immich web interface

# Replace with the actual IP from Step 2
http://<tailscale-pi-ip>:2283


- Log in or create an account.

📸 *Screenshot Placeholder: Immich login page*

---

## 6️⃣ (Optional) Use External USB Storage

💡 Store uploaded photos/videos on an external drive instead of SD card.


# Step 6: Mount USB drive to store media

sudo mkdir /media/photos
sudo mount /dev/sda1 /media/photos  # Replace sda1 with actual device name

# Update docker-compose.yml volume:
# Replace:
#   - ./upload:/usr/src/app/upload
# With:
#   - /media/photos:/usr/src/app/upload

# Auto-mount on reboot: edit fstab
sudo nano /etc/fstab

# Add this line (adjust fs type if not ext4):
/dev/sda1  /media/photos  ext4  defaults  0  2


📸 *Screenshot Placeholder: USB drive mounted and used for media*

---

## 📝 Final Notes

- 📱 Immich mobile app supports **automatic photo upload**.
- 🐳 Ensure Docker starts on boot:
  
  sudo systemctl enable docker
  
- 🔒 Tailscale allows **secure access** to your Pi from anywhere without needing static IP or port forwarding.

---

## 📷 Screenshots

> Add these later once you test and run the setup.

- Raspberry Pi Imager OS selection
- Tailscale showing connected Pi
- Docker containers running (`docker ps`)
- Immich login page

---

## 📄 License

**MIT**

---

## 🙌 Credits

- [Immich](https://github.com/immich-app/immich) – Photo & video management
- [Tailscale](https://tailscale.com/) – Secure private networking
- [Docker](https://www.docker.com/) – Lightweight containers







