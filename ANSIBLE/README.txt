# configure ansible
create user with sudo priviledge
config /etc/hosts
config ssh
check ansible.cfp
create inventory

# Ad-hoc command syntax
ansible <hosts> -m command -a "<command>"

# sample yml file for ansible-playbook
---
- name: DEPLOY HTTPD
  hosts: web
  tasks:
  - name: INSTALL HTTPD
    yum: name=httpd state=present
  - name: START HTTPD
    service: name=httpd state=started enabled=yes
  - copy:
      content: "CENTOS WEBSITE!!!!!!"
      dest: /var/www/html/index.html
- name: DEPLOY FIREWALL
  hosts: web
  tasks:
  - name: INSTALL FIREWALL
    yum: name=firewalld state=present
  - name: START FIREWALL
    service: name=firewalld state=started enabled=yes
  - name: ALLOW SERVICE IN FIREWALL
    firewalld: service=http permanent=yes state=enabled
- name: DEPLOY APACHE
  hosts: db
  tasks:
    - name: INSTALL APACHE
      apt: name=apache2 state=present
    - name: START APACHE
      service: name=apache2 state=started enabled=yes
    - copy:
        content: "UBUNTU WEB!!!"
        dest: /var/www/html/index.html
- name: DEPLOY UFW
  hosts: db
  tasks:
  - name: INSTALL UFW
    apt: name=ufw state=latest
  - name: START UFW
    service: name=ufw state=started enabled=yes
  - name: ALLOW SSH
    ufw: rule=allow port=22 proto=tcp
  - name: ALLOW HTTP
    ufw: rule=allow port=80 proto=tcp
