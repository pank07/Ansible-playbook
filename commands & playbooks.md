# Ansible commands and playbooks
- Add reposistory
```bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
  ```
- Install ansible
```bash
sudo apt install ansible
```
```bash
-Update host file of ansible
sudo vim /etc/ansible/hosts
ansible-inventory --list -y
```
```bash
[servers]
server1 ansible_host=<Public_IP>
server2 ansible_host=<Public_IP>
server3 ansible_host=<Public_IP>

[all:vars]
anisble_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/<file>
```
- Ansible Adhoc commands vs modules
ad hoc commands are great for tasks you repeat rarely.
-a is used for adhoc commands ansible all -a "df -h" -u ubuntu ansible servers -a "uptime" -ubuntu
```bash
ansible servers -a "free -h"
```
- Ansible modules are units of code that can control system resource or execute system commands
-eg.
-m is used for modules ansible all -m ping -u ubuntu
```bash
ansible servers -m ping
```
- Ansible playbooks
```bash
name: Date playbook
hosts: servers
tasks:
 - name: this will show the date
 - command: date
```

- Run ansible playbook
```bash
ansible-playbook <name_of_playbook.yml>
```
- Anisble Playbooks to install tools
```bash
-
 name: Install nginx and start it
 hosts: servers
 become: yes
 tasks:
 - name: install nginx
   apt:
     name: nginx
     state: latest

 - name: start nginx
   service:
     name: nginx
     state: started
     enabled: yes

```
- Ansible playbooks with conditional statements
```bash
name: this is will install based on os
hosts: servers
become: yes
tasks:
- name: install docker
 apt:
   name: docker.io
   state: latest
-name: install aws cli
apt:
  name: awscli
  state: latest
when: ansible_distribution== 'Debian' or ansible_distribution== 'Ubuntu'
```
- Deplying static web page by ansible playbook
```bash
-
 name:  Install nginx and server static website
 hosts: production
 become: yes
 tasks:
   - name: Inatall nginx
     apt:
       name: nginx
       state: latest

   - name: Start nginx
     service:
       name: nginx
       state: started
       enabled: yes

   - name: Deploy web page
     copy:
       src: index.html
       dest: /var/www/html
``` 
- installation_apache.yml
```bash
---
- name: Install Apache on Web Servers
  hosts: webservers
  become: true

  tasks:
    - name: Update APT package list (for Debian/Ubuntu)
      apt:
        update_cache: yes

    - name: Install Apache
      apt:
        name: apache2
        state: present

    - name: Ensure Apache is started
      service:
        name: apache2
        state: started
        enabled: true
```
