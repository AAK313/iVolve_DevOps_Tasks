
# Lab 1: Initial Ansible Configuration and Ad-Hoc Execution

## Overview
This lab demonstrates the initial setup and basic functionality of Ansible Automation Platform. It covers the installation of Ansible on the control node, SSH key setup, inventory configuration, and running ad-hoc commands on a managed node.

---

## Step 1: Install Ansible on Control Node
Install Ansible on your control node and verify the installation.

sudo apt update
sudo apt install ansible -y
ansible --version

yaml
Copy code

---

## Step 2: Generate SSH Key Pair
Create an SSH key pair for secure passwordless authentication.

ssh-keygen -t rsa -b 2048 -f ~/.ssh/ansible_key

pgsql
Copy code

- Press **Enter** to accept the default location.  
- Optionally, set a passphrase.

---

## Step 3: Copy Public Key to Managed Node
Transfer the public key to your managed node so Ansible can connect without a password.

ssh-copy-id -i ~/.ssh/ansible_key.pub user@managed_node_ip

yaml
Copy code

- Replace `user@managed_node_ip` with the managed node username and IP address.

---

## Step 4: Create Inventory File
Create an `inventory.ini` file in your repo to define the managed nodes.

[managed_nodes]
managed_node_ip ansible_user=user 

yaml
Copy code

- Replace `managed_node_ip` and `user` with the managed node IP and username.

---

## Step 5: Test Connectivity with Ping
Verify Ansible can reach the managed node using the ping module.

ansible all -i inventory.ini -m ping

yaml
Copy code

- Expected output: `pong` from the managed node.

---

## Step 6: Run Ad-Hoc Command to Check Disk Space
Execute an ad-hoc command to check disk usage on the managed node.

ansible all -i inventory.ini -m shell -a "df -h"

yaml
Copy code

- This displays disk usage information.

---

## Notes
- Ensure SSH access works before running Ansible commands.  
- Ad-hoc commands are useful for quick checks and troubleshooting before creating playbooks.  
- All commands assume the use of a private SSH key for passwordless authentication.

---

## References
- [Ansible Documentation](https://docs.ansible.com/)  
- [SSH Key Authentication](https://www.ssh.com/academy/ssh/keygen)
