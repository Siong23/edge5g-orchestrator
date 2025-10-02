# MicroStack Installation Guide

## 1. Installing Ubuntu 22.04
- Create a bootable USB with Ubuntu 22.04.
- Install on Server 1 and set up user credentials.

## 2. Install MicroStack
- Start by installing Curl if it’s not installed using terminal.
  ```bash
  sudo apt install curl
  ```
- Install the openstack snap
  ```bash
  sudo snap install microstack --channel latest/beta
  ```

- Prepare the machine
  ```bash
  sudo microstack init --auto --control
  ```

- Get MicroStack Password
  ```bash
  sudo snap get microstack config.credentials.keystone-password
  ```

## 3. MicroStack Configuration on Server 1
### &nbsp;&nbsp;&nbsp; 3.1 Manage Image
#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3.1.1 Download the Cloud Image (Ubuntu 22.04 Cloud)
```bash
wget https://cloud-images.ubuntu.com/releases/22.04/release/ubuntu-22.04-server-cloudimg-amd64.img
```
