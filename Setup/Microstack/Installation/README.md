# 🚀 MicroStack Installation Guide

This guide explains how to install and configure **MicroStack (OpenStack)** on Ubuntu 22.04.

---

## 1️⃣ Installing Ubuntu 22.04
- 💾 Create a bootable USB with Ubuntu 22.04.
  
  - 💽 **Base Image:** Ubuntu 22.04 (64-bit)  
    - [Ubuntu 22.04 **Cloud Image** (64-bit required)](https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64-disk-kvm.img)
    - [Ubuntu 22.04 **Server Image** (64-bit required)](http://releases.ubuntu.com/22.04/)

- 🖥️ Install on Server 1 and set up user credentials.

---

## 2️⃣ Install MicroStack
- ⚙️ Step 1: Install Curl
  ```bash
  sudo apt install curl
  ```
  
- 📦 Step 2: Install the MicroStack snap
  ```bash
  sudo snap install microstack --channel latest/beta
  ```
  
- 🛠️ Step 3: Prepare the machine
  ```bash
  sudo microstack init --auto --control
  ```

 &nbsp;
 
- 🔑 Get MicroStack Password
  ```bash
  sudo snap get microstack config.credentials.keystone-password
  ```
- 🔎 Verify Horizon Dashboard
  
  - Open in browser
  ```bash
  https://<horizon_IP_address>
  ```
  > 💡 Replace `<horizon_IP_address>` with your actual Horizon IP address,  
  >    For example: `https://192.168.0.195`.
