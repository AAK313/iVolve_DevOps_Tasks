# Automated Web Server Configuration Using Ansible Playbooks

## Objectives

- Automate web server installation and configuration using Ansible
- Install and enable **Nginx** on a managed node
- Deploy a custom `index.html` page
- Verify that the web server is running correctly
---
1. Playbook: `playbook1.yml`
    ```yaml
    ---
    - name: Configure Nginx Web Server
      hosts: dev
      become: yes  # Run as root
      tasks:

    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Ensure Nginx is running and enabled
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Customize the default web page
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

    - name: Verify Nginx is serving the customized page
      uri:
        url: http://localhost
        return_content: yes
      register: webpage

    - name: Display the web page content (for verification)
      debug:
        msg: "{{ webpage.content }}"





2. **Create an Inventory File** `inventory.ini`:
   ```ini
   [hosts]
   192.168.1.105
   

3. Run the Playbook:
   ```bash
   ansible-playbook -i inventory.ini playbook1.yml --ask-become-pass
   
   `--ask-become-pass` → ask for SSH password (if no key-based authentication).
   
4. **Verify Configuration**: 
   ```bash
   curl http://192.168.1.105

## References
- [Ansible Documentation](https://docs.ansible.com/)  
- [Nginx Official Documentation](https://nginx.org/en/docs/)
