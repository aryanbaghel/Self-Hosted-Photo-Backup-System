# Self-Hosted-Photo-Backup-System using Raspberry Pie

Immich on Raspberry Pi with Debian, Docker, and Tailscale
Self-host your photo and video server with privacy and convenience using Immich on a Raspberry Pi.
Features
• Full Immich deployment on Raspberry Pi
• Secure remote access using Tailscale (no port forwarding) • Lightweight setup using Docker Compose
📦 Requirements
• Raspberry Pi 4 (4GB RAM minimum recommended) • 32GB+ SD card
• Internet connection
• A Tailscale account
🪜 Setup Steps
1. ⚙ Flash and Set Up Debian on Raspberry Pi
1. Download and install Raspberry Pi Imager.
2. Select Raspberry Pi OS Lite (64-bit) or Debian Bookworm Lite.
3. Flash the image to your SD card.
4. (Optional) Enable SSH:
5. After flashing, create an empty file named ssh in the boot partition. 6. Insert the SD card into your Raspberry Pi and boot it up.
7. Connect via SSH:
       ssh pi@raspberrypi.local
Screenshot: Raspberry Pi Imager with OS selection
2. Connect Raspberry Pi to Internet via Tailscale
1. Install Tailscale:
      1
        curl -fsSL https://tailscale.com/install.sh | sh
       sudo tailscale up
2. Sign in through your browser when prompted. 3. Check Tailscale IP:
       tailscale ip -4
Screenshot: Tailscale Admin Console showing Pi online
3. Install Docker and Docker Compose
1. Install Docker:
       curl -sSL https://get.docker.com | sh
       sudo usermod -aG docker $USER
       newgrp docker
2. Install Docker Compose:
       sudo apt install docker-compose -y
Restart your Pi if you encounter permission issues
4. Install Immich via Docker Compose
1. Create a project folder:
mkdir ~/immich && cd ~/immich 2. Create a docker-compose.yml :
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
          2

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
1. Start the services:
       docker-compose up -d
Screenshot: docker ps showing Immich containers running 5. 🌍 Access Immich via Tailscale
• On any device within your Tailscale network, go to:
       http://<tailscale-pi-ip>:2283
• Log in or create an Immich account
Screenshot: Immich login page
      3

6. (Optional) Use External Storage
To store your media on an external USB drive: 1. Mount the drive:
       sudo mkdir /media/photos
       sudo mount /dev/sda1 /media/photos
2. Update the volume in docker-compose.yml : volumes:
         - /media/photos:/usr/src/app/upload
Make sure the drive is auto-mounted on reboot using
Final Notes
• Immich supports auto-upload from mobile apps. • Make sure Docker restarts on boot:
       sudo systemctl enable docker
/etc/fstab
      • Tailscale gives secure remote access with no public IP needed.
Screenshots Placeholder
• Raspberry Pi Imager
• Tailscale device list
• Docker containers running ( docker ps ) • Immich login page
License
MIT
Credits
• Immich • Tailscale • Docker
