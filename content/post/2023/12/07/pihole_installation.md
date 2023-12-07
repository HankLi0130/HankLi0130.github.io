---
title: "Pihole installation"
date: 2023-12-07
tags: ["Pi-hole", "DNS"]
draft: false
image: "/post/2023/12/07/pihole_ubuntu.png"
---

![pihole](/post/2023/12/07/pihole_ubuntu.png)

# [Pi-hole](https://pi-hole.net/)

The Pi-hole is a DNS sinkhole that protects your devices from unwanted content without installing any client-side software.

## How to deploy Pi-hole using Docker?

More detials on [docker-pi-hole](https://github.com/pi-hole/docker-pi-hole).

### Preparation

1. Ubuntu 22.04 (x86)
2. Enable `53` and `80` ports
3. Copy the scripts on the bottom and then save them to files to your machine.

### Install docker

Let's install docker first.

```shell
# update
sudo apt update

# move to the Pi-hole folder
cd pihole

# install docker via install_docker.sh
sudo ./install_docker.sh

# start Pi-hole with Docker compose
sudo docker compose up -d
```

After Docker images downloaded, you probably will see the message of port `53` has been listened, right now, we need to disable the stub resolver.

### Disable the stub resolver on Ubuntu

```shell
# disable the stub resolver
sudo ./update_systemd_resolved.sh
```

More details on [Installing on Ubuntu or Fedora](https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu-or-fedora)

### Run Pi-hole again

```shell
# start Pi-hole with Docker compose
sudo docker compose up -d
```

Use [Can You Block It](https://canyoublockit.com/) to check if ads are being blocked. 

## How to undo the deployment?

### Stop and remove Pi-hole
```shell
# remove Pi-hole with Docker compose
sudo docker compose down
```

### Enable the stub resolver on Ubuntu
```shell
# enable the stub resolver
sudo ./undo_systemd_resolved.sh
```

## Scripts

### `docker-compose.yml`

```yml
version: "3"

# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"
    environment:
      TZ: 'Asia/Taipei'
      WEBPASSWORD: 'Your Password'
    # Volumes store your data between container upgrades
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    restart: unless-stopped
```

### `install_docker.sh`

```shell
#!/bin/bash
# Set up the repository
sudo apt update

sudo apt install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### `update_systemd_resolved.sh`

```shell
# https://github.com/pi-hole/docker-pi-hole#installing-on-ubuntu-or-fedora
# Disable the stub resolver
sudo sed -r -i.orig 's/#?DNSStubListener=yes/DNSStubListener=no/g' /etc/systemd/resolved.conf

# Change the `/etc/resolv.conf` symlink to point to `/run/systemd/resolve/resolv.conf`
sudo sh -c 'rm /etc/resolv.conf && ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf'

# Restart systemd-resolved
sudo systemctl restart systemd-resolved
```

### `undo_systemd_resolved.sh`

```shell
# Enable the stub resolver
sudo sed -r -i.orig 's/DNSStubListener=no/#DNSStubListener=yes/g' /etc/systemd/resolved.conf

# Change the `/etc/resolv.conf` symlink to point to `/run/systemd/resolve/resolv.conf`
sudo sh -c 'rm /etc/resolv.conf && ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf'

# Restart systemd-resolved
sudo systemctl restart systemd-resolved
```
