# Home Lab Project Setup

## Phase 1: Prepare Ubuntu VM for App Development

### Steps Completed

1. **Update Ubuntu8**
	- Command: 'sudo apt update && sudo apt upgrade -y'

2. **Install Git and configure**
	- Command: 'sudo apt install git -y'

3. **Configure Git**
	- Set name: 'git config --global user.name "Your name"
	- Set email: 'git config --global user.email "your email"

4. **Create project folder**
	- Command: mkdir ~/home_lab_project
	
5. **Install virtual Environment Tool**
	- Command: 'sudo apt install python3-venv -y'

### Screeshots
- system_update.png - Terminal showing system update
- System_upgrade.png - Terminal showing system upgrade
- python_and_git.png - Terminal showing python and git configuration

## Phase 2: Configure VM Networking & Shared folders

- Networking set to Bridged, internet tested in Ubuntu
- Shared folder 'UbuntuShared' mounted manually and auto-mounted on reboot
- Allows file exchange between Windows host and Ubuntu VM
