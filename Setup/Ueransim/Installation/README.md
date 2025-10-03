# 📡 UERANSIM Installation Guide

This guide explains how to install and configure **UERANSIM** on Ubuntu 22.04.

---

## 1️⃣ Installing Ubuntu 22.04
- 💽 **Base Image:** Ubuntu 22.04 (64-bit)  
  - [Ubuntu 22.04 **Cloud Image** (64-bit required)](https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64-disk-kvm.img)
  - [Ubuntu 22.04 **Server Image** (64-bit required)](http://releases.ubuntu.com/22.04/)

---

## 2️⃣ Install Ueransim
📥 Step 1: Prepare Environment
```bash
sudo apt update
sudo apt install -y make gcc g++ libsctp-dev lksctp-tools iproute2 cmake git
```

📂 Step 2: Clone UERANSIM
```bash
git clone https://github.com/aligungr/UERANSIM.git
```

🛠️ Step 3: Build UERANSIM
```bash
cd UERANSIM
make
```
