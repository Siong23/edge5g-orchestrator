# 🚀 MicroStack Installation Guide

This guide explains how to install and configure **MicroStack (OpenStack)** on Ubuntu 22.04.

---

## 1️⃣ Installing Ubuntu 22.04
- 💽 Create a bootable USB with Ubuntu 22.04.
- 🖥️ Install on Server 1 and set up user credentials.

---

## 2️⃣ Install MicroStack
- ⚙️ Install Curl
  ```bash
  sudo apt install curl
  ```
  
- 📦 Install the OpenStack snap
  ```bash
  sudo snap install microstack --channel latest/beta
  ```
  
- 🛠️ Prepare the machine
  ```bash
  sudo microstack init --auto --control
  ```
  
- 🔑 Get MicroStack Password
  ```bash
  sudo snap get microstack config.credentials.keystone-password
  ```
