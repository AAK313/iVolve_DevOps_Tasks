# Lab 1: Initial Ansible Configuration and Ad-Hoc Execution

## Overview
This lab demonstrates the initial setup and basic functionality of Ansible Automation Platform. It covers the installation of Ansible on the control node, SSH key setup, inventory configuration, and running ad-hoc commands on a managed node.

---

## Objectives
- Install and configure Ansible on the control node.
- Generate an SSH key pair for secure passwordless authentication.
- Transfer the public key to the managed node.
- Create an inventory file defining the managed node.
- Execute ad-hoc commands to verify connectivity and functionality.

---

## Prerequisites
- A control node (Linux machine) with administrative access.
- One managed node (Linux machine) accessible via SSH.
- Internet access for installing Ansible packages.

---

## Steps

### 1. Install Ansible on Control Node
```bash
sudo apt update
sudo apt install ansible -y
ansible --version

### 2. Generate SSH Key Pair
bash
Copy code
ssh-keygen -t rsa -b 2048 -f ~/.ssh/ansible_key
Press Enter to accept default location.

Optionally, set a passphrase.

### 3. Copy Public Key to Managed Node
bash
Copy code
ssh-copy-id -i ~/.ssh/ansible_key.pub user@managed_node_ip
Replace user@managed_node_ip with your managed node username and IP address.

### 4. Create Inventory File
Create inventory.ini in your repo:

ini
Copy code
[managed_nodes]
managed_node_ip ansible_user=user 
Replace managed_node_ip and user with your managed node IP and username.

### 5. Test Connectivity with Ping
bash
Copy code
ansible all -i inventory.ini -m ping
Expected output: pong from the managed node.

### 6. Run Ad-Hoc Command to Check Disk Space
bash
Copy code
ansible all -i inventory.ini -m shell -a "df -h"
Displays disk usage on the managed node.

Notes
Ensure SSH access works before running Ansible commands.

Ad-hoc commands are useful for quick checks and troubleshooting before creating playbooks.

All commands assume the use of a private SSH key for passwordless authentication.

References
Ansible Documentation
SSH Key Authentication


