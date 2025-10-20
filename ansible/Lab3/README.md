# Lab 3: Structured Configuration Management with Ansible Roles

## Objectives
- Implement structured configuration management using Ansible roles.
- Create roles for installing and configuring Docker, Kubernetes CLI (`kubectl`), and Jenkins.
- Write an Ansible playbook to run the created roles.
- Verify the installation on the managed node.

---

## Roles Structure
roles/
├── docker/
│ ├── tasks/
│ │ └── main.yml
│ └── ...
├── kubectl/
│ ├── tasks/
│ │ └── main.yml
│ └── ...
└── jenkins/
├── tasks/
│ └── main.yml
└── ...

yaml
Copy code

---

## Playbook: playbook3.yml
```yaml
---
- name: Configure Docker, Kubernetes CLI, and Jenkins
  hosts: hosts
  become: yes
  roles:
    - docker
    - kubectl
    - jenkins
Inventory File: inventory.ini
csharp
Copy code
[hosts]
192.168.1.105
Run the Playbook
bash
Copy code
ansible-playbook -i inventory.ini playbook3.yml --ask-become-pass
--ask-become-pass → prompts for sudo password if key-based authentication is not configured.

Verify Configuration
Docker:

bash
Copy code
ssh user@192.168.1.105 docker --version
Kubectl:

bash
Copy code
ssh user@192.168.1.105 kubectl version --client
Jenkins: Open http://192.168.1.105:8080 in a browser.

References
Ansible Roles Documentation

Docker Official Documentation

Kubernetes Official Documentation

Jenkins Official Documentation

yaml
Copy code

