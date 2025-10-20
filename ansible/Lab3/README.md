
# Lab 3: Structured Configuration Management with Ansible Roles
## Create Inventory File
## Configure Docker&kubectl&jenkins Role
```bash
vim roles/docker/tasks/main.yml
vim roles/kubectl/tasks/main.yml
vim roles/jenkins/tasks/main.yml
```
## Create Main Playbook to Run All Roles: playbook2.yml
```bash
---
- name: Configure DevOps tools on managed node
  hosts: dev
  become: yes
  roles:
    - docker
    - kubectl
    - jenkins

```
## Run the Playbook
```bash
ansible-playbook -i inventory.ini playbook2.yml
```
## Verify Installation on Managed Node
```bash 
ssh amr@192.168.1.105 docker --version
ssh amr@192.168.1.105 kubectl version --client
ssh amr@192.168.1.105 jenkins --version

```
### References

Ansible Roles Documentation

Docker Official Documentation

Kubernetes Official Documentation

Jenkins Official Documentation
