# Home Lab Project Setup

This repository documents the step-by-step setup of my **personal home lab environment**, designed for learning, experimentation, and app development.

---

## Table of Contents

* [Objectives](#-objectives)
* [Planned Features](#-planned-features)
* [Project Phases](#-project-phases)

  * [Phase 1: Prepare Ubuntu VM for App Development](#phase-1-prepare-ubuntu-vm-for-app-development)
  * [Phase 2: Configure VM Networking & Shared Folders](#phase-2-configure-vm-networking--shared-folders)
  * [Phase 3: Securing Ubuntu VM with UFW (Uncomplicated Firewall)](#phase-3-securing-ubuntu-vm-with-ufw)
  * [Phase 4: Install Python & Virtual environment setup](#phase-4-install-python-and-virtual-environment-setup)
  * [Phase 5: Install & Verify PostgreSQL (Database for Backend Development)](#phase-5-install-and-verify-postgresql)
  * [Phase 6: Configure PostgreSQL to accept remote connections](#phase-6-configure-postgresql-to-accept-remote-connections)
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
   - sudo apt update -> refreshes  package list
   - sudo apt upgrade -y -> installs available updates automatically (-y answers "yes" to prompts)

2. **Install Git**

   ```bash
   sudo apt install git -y
   ```

3. **Configure Git**

   ```bash
   git init #Initializes repository in project folder
   git remote add origin <repo_url> #link repo
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

   * VMware → VM Settings → Network Adapter → Select **Bridged** (IP config from my home router)
   - This allows my VM and host PC to be in the same network so they can communicate. Given that with bridged network the VM takes its IP address from the same DHCP server as your host PC.

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

**Screenshots:**

* ![Testing internet connection](`screenshots/ping_test.png`)
* ![Shared folder mount](`shared_folder.png`)


## **Phase 3: Securing Ubuntu VM with UFW**

**Goal:**
Enable and configure a firewall (UFW) to secure my Ubuntu VM while still allowing necessary connections (like SSH for remote management)

**Steps completed**

1. **Check if UFW is installed**
   ```bash
   sudo apt list ufw
   ```

2. **Allow SSH traffic**
   ```bash
   sudo ufw allow ssh
   ```

3. **Allow HTTP/HTTPS traffic (for later web apps)**
   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   ```

3. **Enable UFW**
   ```bash
   sudo ufw enable
   ```

4. **Check status**
   ```bash
   sudo ufw status

   ```

**Proof (Screenshots):**
* `ufw_setup.png`

## **Phase 4: Install Python and Virtual Environment Setup**

**Goal**
Ensures my Ubuntu VM is properly configures with Python and virtual environment for backend development (without root). This isolates dependencies for my app backend.

**Steps completed**

1. **Verify Python installation**
   ```bash
   python3 --version
   ```

2. **Install pip (Python package manager)**
   ```bash
   sudo apt install python3-pip -y
   ```

3. **Install venv module (It allows to create iolated environments)**
   ```bash
   sudo apt install python3-venv -y
   ```

4. **Create a virtual environment inside my project folder**
   ```bash
   cd ~/home_lab_project
   python3 -m venv venv
   ```

5. **Activate the virtual environment**
   ```bash
   source venv/bin/activate
   ```

6. **Test pip inside venv**
   ```bash
    pip list
   ```

7. **Deactivate when done**
   ```bash
   deactivate
   ```

**Proof (Screenshots)**
* `python_venv_setup.png` 


### **Phase 5: Install & Verify PostgreSQL**

**Goal:**
Set up PostgreSQL on my Ubuntu VM so my backend can connect to a real database.

**Steps Completed:**

1. **Update package lists**

   ```bash
   sudo apt update 
   ```

2. **Install PostgreSQL**

   ```bash
   sudo apt install postgresql postgresql-contrib -y
   ```
   - postgresql: installs the database server
   - postgresql-contrib: adds extra tools/extensions

3. **Check PostgreSQL service status**

   ```bash
   sudo systemctl status postgresql
   ```
   - Output: active(running)

4. **Switch to the PostgreSQL default user**

   ```bash
   sudo -i -u postgres
   ```

5. **Open PostgreSQL shell**

   ```bash
   psql
   ```

6. **Create a database user for my backend app**

   Inside psql:
   ```bash
   CREATE USER <username> WITH PASSWORD '<password>';
   ```

7. **Create a database for my app**

   ```bash
   CREATE DATABASE <DB_name> OWNER <user_name>;
   ```

8. **Grant privileges**

   ```bash
   GRANT ALL PRIVILEGES ON DATABASE <DB_name> TO <user_name>;
   ```

9. **Exit psql and postgres user**

   Exit database shell:
   ```bash
   \q
   ```

   Exit postgres user:
   ```bash
   exit
   ```

10. **Test login with new user**

   ```bash
   psql -U <username> -d DB_name -h localhost
   ```
**Proof (Screenshots):**
* `postgresql_install.png`


## **Phase 6: Configure PostgreSQL to accept remote connections**

**Goal**
Enable POstgreSQL on Ubuntu VM to accept connections from my host PC and any other devices in my allowed subnet

**Steps completed**

1. Check which IP addresses PostgreSQL is accepting connections from
   ```bash
   sudo ss -plunt | grep 5432
   ```
   - Output:(Only accepts internal connections) 
   ```bash
   tcp LISTEN 0 0 127.0.0.1:5432 0.0.0.* ...
   ```
   - Command breakdown:
	- ss -> utility to view nework sockets
   	- -plunt -> shows processes(p), listening ports(l), user info(u), numeric addresses(n), TCP(t)
 
2. Edit PostgreSQL main configuration: Change listening addresses from localhost to everyone 
   ```bash  
   nano /etc/postgresql/16/main/postgresql.conf
   ````
   - Find the line: press (ctrl + W) then type:
      ```bash
      #listen_addresses = 'localhost'
      ```
   - Change to:
      ```bash
      listen_addresses = '*'
      ```
   - Save and exit -> ctrl+X, Y, Enter

3. Edit PostgreSQL authentication file: Restrict connection to allowed subnet only
   ```bash
   host all all 10.0.0.0/24 md5
   ```
   - Save and exit
   - Command breakdown:
      - host -> TCP/IP connection
      - all -> all databases
      - all -> all users
      - 10.0.0.0/24 -> only devices on a specific subnet
      - md5 -> password authentication

4. Allow PostgreSQL port through UFW firewall
   ```bash
   sudo ufw allow from 10.0.0.0/24 to any port 5432 #allow incoming connection to port 5432 from <subnet_ip> only
   sudo ufw reload #Reload UFW rules
   sudo ufw status #Check UFW rules
   ```

5. Check if psql client is installed on Windows (my host system)
   - Open PowerShell
   ```powershell
   psql --version
   ```
   - If not recognized, install PostgreSQL from their website and add PostgreSQL bin folder to PATH:
   ```makefile
   C:\Program Files\PostgreSQL\17\bin #17 is the psql version
   ```

6. Test connection to Ubuntu VM PostgreSQL
   ```PowerShell
   psql -h 10.0.0.25 -U new_db -d welker
   ```
   - -h 10.0.0.25 -> My Ubuntu IP address
   - -U new_db -> my database name
   - -d welker -> My postgreSQL username

7. Enter password set for *welker*

8. Expected Output
   ```ini
   <DB_name>=>
  ```

**Proofs (Screenshots)**
- `pg_hba_file_config.png`
- `ufw_status.png`
- `successful_host_psql_connection.png`
