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

=======================================================================

- name: create second directory
  file:
    path: /opt/testdir2000
    state: directory
  tags: [create_testdir2000]

- name: create directory with mode
  file: path=/opt/testdir999 state=directory owner=root group=root mode=0777
  tags: [create_testdir999]

- name: create directory testuser
  file:
    path: /opt/testdir333/sub1/sub2/sub3
    state: directory
    owner: usertest
    group: usertest
    mode: 0777
  tags: [create_testdir333]

- name: create hamid directory
  file: path=/opt/hamid state=directory owner=root group=root mode=0777
  tags: [create_hamid]

- name: remove directory 2000
  file: path=/opt/testdir2000 state=absent
  tags: [remove_testdir2000]

- name: create file testfile
  file: path=/opt/testfile state=touch owner=root group=root mode=0644
  tags: [create_testfile]

- name: remove testfile
  file: path=/opt/testfile state=absent
  tags: [remove_testfile]

- name: create hidden file
  file: path=/home/.hamid state=touch owner=root group=root mode=0666
  tags: [create_hiddenfile]

- name: remove hiddenfile
  file: path=/home/.hamid state=absent
  tags: [remove_hiddenfile]
  
############ Copy File #################

- name: copy file
  copy: src=filetest dest=/opt backup=yes
  tags: [copyfile]

######### Template directory ##########
## template use for config file (j2 file)
- name: copy config file file.conf.j2
  template: src=file.conf.j2 dest=/opt/file.conf backup=yes
  tags: [configfilecopy]

######### Download and copy file ######
- name: Download file
  get_url: url=https://i.pinimg.com/originals/8e/c8/81/8ec8813c3202ff65698b1635c6da3e09.jpg dest=/opt/image.jpg
  tags: [get_url]

############ unzip files ###############
## this for copy zip file from source to destination and unzip the file
- name: unzip archive.gz
  unarchive: src=/root/zabbix-4.4.7.tar.gz dest=/opt
  tags: [unzipfile]

## this is for unzip file on destination
- name: unzip archive.gz
  unarchive: src=/opt/zabbix-4.4.7.tar.gz dest=/opt/ copy=no
  tags: [unzipfile2]
  
########## Create group and user ##########
- name: create test group
  group: name=test state=present
  tags: [group]

- name: create test user
  user: name=test comment="test" group=test
  tags: [user]
  
  ######### remove group and user  #########
  - name: delete test group
  group: name=test state=absent
  tags: [del_group]

- name: delete test user
  user: name=test comment="test" group=test state=absent
  tags: [del_user]
  
  ######## install and update with yum #########
  - name: install epel-release
  yum: name=epel-release state=present
  tags: [pre_install]

- name: yum update
  yum: name=* state=latest          ### update all
  tags: [yum_update]
  
  ############## FOR and LOOP in ansible #############
  - name: yum install several packages
  yum: name={{ item }} state=present
  loop:
    - vim
    - net-tools
    - ntp
  tags: [install_packages]
  
 #### create several files ####
 - name: create 4 files
  file: name=/opt/{{ item }} state=touch
  loop:
    - file1
    - file2 
    - file3 
    - file4
  tags: [create_files]
  
### create users and groups and iid with multi variable ##
- name: create g1 & g2
  group: name={{ item,name }} state=present
  loop:
    - { name: g1 }
    - { name: g2 }
  tags: [groups]

- name: create users
  user:
    name: "{{ item.name }}"
    uid: "{{ item.uid }}"
    groups: "{{ item.groups }}"
    state: present
  loop:
    - { name: testuser1,uid: 9902,groups: "g1,g2" }
    - { name: testuser2,uid: 9903,groups: g1 }
  tags: [user_uid]
