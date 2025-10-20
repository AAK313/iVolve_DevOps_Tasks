# Lab 2: Automated Web Server Configuration Using Ansible Playbooks

## Overview
This lab demonstrates how to automate the configuration of a web server using Ansible playbooks.  
It covers installing Nginx, customizing the default web page, and verifying the configuration on a managed node.

---

## Step 1: Prepare Your Inventory
Ensure your inventory file (`inventory.ini`) contains the managed nodes under a group, for example:

[dev]
managed_node_ip ansible_user=user ansible_ssh_private_key_file=~/.ssh/ansible_key

yaml
Copy code

- Replace `managed_node_ip` and `user` with your managed node IP and username.

---

## Step 2: Write the Ansible Playbook
Create a file named `playbook1.yml` with the following content:

name: Configure Nginx Web Server
hosts: dev
become: yes # Run as root
tasks:

name: Install Nginx
apt:
name: nginx
state: present
update_cache: yes

name: Ensure Nginx is running and enabled
service:
name: nginx
state: started
enabled: yes

name: Customize the default web page
copy:
dest: /var/www/html/index.html
content: |
<!DOCTYPE html>
<html>
<head>
<title>Welcome to My Automated Web Server!</title>
<style>
body { font-family: Arial; background: #f4f4f4; text-align: center; padding-top: 50px; }
h1 { color: #0078D7; }
</style>
</head>
<body>
<h1>🚀 Nginx is running — configured by Ansible!</h1>
<p>This page was deployed automatically.</p>
</body>
</html>

name: Verify Nginx is serving the customized page
uri:
url: http://localhost
return_content: yes
register: webpage

name: Display the web page content (for verification)
debug:
msg: "{{ webpage.content }}"

yaml
Copy code

---

## Step 3: Run the Playbook
Execute the playbook on your managed node:

ansible-playbook -i inventory.ini playbook1.yml

yaml
Copy code

- Ansible will install Nginx, start the service, customize the web page, and verify the page content.

---

## Step 4: Verify the Web Server
- Open a browser or use `curl` on the managed node to check the web page:

curl http://managed_node_ip

yaml
Copy code

- You should see the custom HTML content defined in the playbook.

---

## Notes
- Ensure SSH access works and the inventory is correctly configured.  
- Playbooks allow repeatable and automated server configuration.  
- The `uri` module helps verify that the server is serving the correct content.

---

## References
- [Ansible Documentation](https://docs.ansible.com/)  
- [Nginx Official Documentation](https://nginx.org/en/docs/)
