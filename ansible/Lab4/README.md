# Securing Sensitive Data with Ansible Vault

## Objectives

- Install and configure MySQL server using Ansible
- Create a database and user with privileges
- Secure sensitive data (DB password) using Ansible Vault
- Validate database setup

## Playbook: `playbook.yml`

```yaml
---
- name: Configure MySQL DB
  hosts: dev
  become: true
  vars_files:
    - secrets.yaml

  tasks:
    - name: Install MySQL and Python MySQL library
      apt:
        name:
          - mysql-server
          - python3-mysqldb
        state: present

    - name: Start MySQL service
      service:
        name: mysql
        state: started

    - name: Create MySQL user
      mysql_user:
        name: amr
        password: "{{ database_password }}"
        host: localhost
        priv: "*.*:ALL,GRANT"  
        state: present

    - name: Create MySQL database 'ivolve'
      mysql_db:
        name: ivolve
        state: present
        login_user: root
        login_unix_socket: /var/run/mysqld/mysqld.sock
```

## Vault File: `secrets.yaml`

```yaml
name: amr
database_password: "2002"
```

## Steps

1. **Encrypt Vault File** (if not already encrypted):

```bash
ansible-vault encrypt secrets.yaml
```

2. **Run Playbook**:

```bash
ansible-playbook -i inventory.ini playbook3.yaml --ask-become-pass --ask-vault-pass
```

3. **Verify Database**:

```bash
mysql -u ivolve_user -p -e "SHOW DATABASES;"
```

- ### Optional: To Edit the Vault Later
  
  ```bash
  ansible-vault edit secrets.yml
  ```
  
  ## Notes

- Ensure **Python MySQL module (`python3-pymysql`)** is installed for Ansible modules to work.

- Use `become: yes` for tasks requiring root privileges.

- Vault ensures sensitive info like DB passwords are not exposed in playbooks.

- Use `ansible-vault view` to read encrypted files securely.

- Ensure MySQL service is running and accessible on the managed node.

- Use `login_unix_socket: /var/run/mysqld/mysqld.sock` to let Ansible connect as MySQL root via socket (no password needed)
