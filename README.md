# Home Lab Project Setup

This repository documents the step-by-step setup of my **personal home lab environment**, designed for learning, experimentation, and app development.

---

## Table of Contents

* [Objectives](#-objectives)
* [Planned Features](#-planned-features)
* [Project Phases](#-project-phases)

  * [Phase 1: Prepare Ubuntu VM for App Development](#phase-1-prepare-ubuntu-vm-for-app-development)
  * [Phase 2: Configure VM Networking & Shared Folders](#phase-2-configure-vm-networking--shared-folders)
  * (More phases will be added as the project evolves)

---

## Objectives

* Create a safe, isolated environment to practice **system administration** and **networking**.
* Provide **backend, database, and web services** for app development.
* Configure a **Windows Server VM** for network services (DHCP, DNS, Active Directory).
* Set up a **NAS** (Network Attached Storage) for secure, remote file access.
* Document each step clearly for **reproducibility** and knowledge sharing.

---

## Planned Features

* Ubuntu VM for **backend development** (FastAPI, PostgreSQL, Docker).
* Windows Server VM for **network management**.
* Shared folders between host and VMs.
* Secure firewall (UFW), SSH access, and automated updates.
* Remote access with secure authentication.
* Progressive documentation (with screenshots) for each phase.

---

## Project Phases

The project is structured into phases, each with clear goals, commands, and proof of completion.

---

### **Phase 1: Prepare Ubuntu VM for App Development**

**Goal:**
Set up a clean Ubuntu environment with essential tools for development.

**Steps Completed:**

1. **Update the system**

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install Git**

   ```bash
   sudo apt install git -y
   ```

3. **Configure Git**

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your@email.com"
   ```

4. **Create project folder**

   ```bash
   mkdir ~/home_lab_project
   ```

5. **Install Python virtual environment tool**

   ```bash
   sudo apt install python3-venv -y
   ```

**Proof (Screenshots):**

* `system_update.png` – system update in terminal.
* `system_upgrade.png` – system upgrade in terminal.
* `git_config.png` – Git setup screenshot.

---

### **Phase 2: Configure VM Networking & Shared Folders**

**Goal:**
Configure VM networking for internet access and enable file sharing between host and VM.

**Steps Completed:**

1. **Configure networking mode**

   * VMware → VM Settings → Network Adapter → Select **Bridged**.

2. **Test internet access**

   ```bash
   ping google.com
   ```

3. **Enable shared folder in VMware**

   * Create folder on host (e.g., `C:\vm_shared`).
   * VMware → VM Settings → Options → Shared Folders → Enable → Add path.

4. **Mount shared folder in VM**

   ```bash
   vmware-hgfsclient
   sudo mkdir -p /mnt/hgfs/vm_shared
   sudo mount -t fuse.vmhgfs-fuse .host:/vm_shared /mnt/hgfs/vm_shared
   ```

5. **Set auto-mount on reboot**
   Add to `/etc/fstab`:

   ```
   .host:/vm_shared   /mnt/hgfs/vm_shared   fuse.vmhgfs-fuse   defaults   0   0
   ```

**Proof (Screenshots):**

* `ping_test.png` – successful internet test.
* `shared_folder.png` – shared folder mounted.

---

Later phases (Windows Server setup, UFW firewall, backend deployment, NAS config) will be added to this doc.

---
