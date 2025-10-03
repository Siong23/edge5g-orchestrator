# 📡 srsRAN Installation Guide

This guide explains how to install and configure **srsRAN** on Ubuntu 22.04.

---

## 1️⃣ Installing Ubuntu 22.04
- 💾 Create a bootable USB with Ubuntu 22.04.
  
  - 💽 **Base Image:** Ubuntu 22.04 (64-bit)  
    - [Ubuntu 22.04 **Cloud Image** (64-bit required)](https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64-disk-kvm.img)
    - [Ubuntu 22.04 **Server Image** (64-bit required)](http://releases.ubuntu.com/22.04/)

- 🖥️ Install on Server 1 and set up user credentials.

---

## 2️⃣ Install srsRAN
📥 Step 1: Prepare Environment
```bash
sudo apt update
sudo apt-get install cmake make gcc g++ pkg-config libfftw3-dev libmbedtls-dev libsctp-dev libyaml-cpp-dev libgtest-dev
```

📂 Step 2: Clone srsRAN Project repository
```bash
git clone https://github.com/srsRAN/srsRAN_Project.git
```

🛠️ Step 3: Build srsRAN code-base
```bash
cd srsRAN_Project
mkdir build
cd build
cmake ../
make -j $(nproc)
```

🧾 Step 4: Test srsRAN code-base
```bash
make test -j $(nproc)
```

📦 Step 5: Install srsRAN Project
```bash
sudo make install
```
