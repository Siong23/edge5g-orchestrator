# 🌐 OSM Installation Guide

This guide explains how to install and configure **OSM (Open Source MANO)** on Ubuntu 22.04.

---

## 1️⃣ Installing Ubuntu 22.04
- ⚙️ **Recommended Specs:** 4 CPUs, 16 GB RAM, 80 GB disk  
- 💽 **Base Image:** Ubuntu 22.04 (64-bit)  
  - [Ubuntu 22.04 **Cloud Image** (64-bit required)](https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64-disk-kvm.img)
  - [Ubuntu 22.04 **Server Image** (64-bit required)](http://releases.ubuntu.com/22.04/)

---

## 2️⃣ Install OSM (Standard Installation)
📥 Step 1: Download the OSM Installer
```bash
wget https://osm-download.etsi.org/ftp/osm-17.0-seventeen/install_osm.sh
```

🔑 Step 2: Give Execute Permission
```bash
chmod +x install_osm.sh
```

🚀 Step 3: Run the Installer
```bash
./install_osm.sh
```

&nbsp;

🔍 Verify OSM Components
```bash
kubectl get all -n osm
```
⚠️ **Note:** Make sure all the components is up and ready

&nbsp;

🔑 Retrieve OSM Password
```bash
kubectl -n osm get secret osm-secret -o jsonpath='{.data.OSM_SERVICE_PASSWORD}' | base64 --decode && echo
```
⚠️ **Note:** Default username is "admin"
