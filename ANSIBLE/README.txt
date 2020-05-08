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
  
### create users and groups and uid with multi variable ##
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

- name: execute file
  shell: "{{ item }}"
  args:
    chdir: /opt/
  loop:
    - ./com1
    - ./com2
    - ./com3
  tags: [comfiles]

- name: delete files
  file: path=/opt/{{ item }} state=absent
  loop:
    - com1
    - com2
    - com3
  tags: [comfiles]

- name: delete many files
  shell: rm -rf {{ item }}
  args: 
    chdir: /opt/
  loop:
    - file*
  tags: [dels]

- name: command run
  command: touch /opt/file
  tags: [command]

- name: create many file
  file: path=/home/file{{ item }} state=touch
  with_sequence: start=56 end=67 stride=2
  tags: [create_range]

- name: install mariadb
  yum: state=latest name={{ item }}
  loop:
    - mariadb-server
    - MySQL-python
  tags: [mariadb]

- name: start mariadb
  service: name=mariadb state=restarted
  tags: [db]


- name: create database
  mysql_db: name=anisadb state=present
  tags: [dbname]

- name: create user for database
  mysql_user: name=anisauser password=password host={{ item }} login_user="root" login_password="" priv=anisadb.*:ALL state=present
  loop:
    - localhost
    - 127.0.0.1
    - ::1
  tags: [anisauser]
  
  - name: flush privileges database
  command: 'mysql -ne "{{ item }}"'
  loop:
    - FLUSH PRIVILEGES
  changed_when: TRUE
  tags: [reload]

- name: download url with vars
  get_url: url={{ paths.url }}/rpm-{{ paths.major_version }}.{{ paths.minor_version }}.tar.bz2 dest=/tmp/
  tags: [get_url2]
  
####################### VARS file ################################
- name: create group with vars file
  group: name={{ item.name }} state=present
  loop: '{{ user_list }}'
  tags: [add_group]

- name: crate users with vars file
  user: name={{item.name }} comment="test" group={{item.name }}
  loop: '{{ user_list }}'
  tags: [add_user]
  
######### NOTIFY & handlers #######
##copy config file and notif to retsart
#task
- name: copy
  template: src=httpd.conf.j2 dest=/etc/httpd/conf/httpd.conf
  notify: restart apache
  tags: [restart_httpd]
#handlers
 ---
- name: restart apache
  service: name=httpd state=restarted
  tags: [restart]

################################### META ###################################
## install nginx with dependencies and meta file
## phase 1
vim roles/nginx/meta/main.yml 

---
allow_duplicates: yes
dependencies:
- { role: base }

## phase 2
vim roles/base/tasks/main.yml

- name: install epel & net-tools
  yum: name={{ item }} state=latest
  loop:
    - epel-release
    - net-tools
  tags: [base]
  
  ## phase 3
vim roles/nginx/tasks/main.yml
  
  ---
- name: install nginx
  yum: name=nginx state=latest
  tags: [nginx]

- name: copy and restart nginx
  template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf
  notify: restart nginx
  tags: [nginx]
  
  ==============
  
  ## handlers file with check notify
- name: restart mariadb
  service: name=mariadb state=restarted
  notify:
    - wait for mariadb

- name: wait for mariadb
  wait_for: path='/var/log/mariadb/mariadb.log' search_regex='started' delay=20 timeout=30

############## INCLUDE & TO DO LIST#############################
###include & to do list ##
- name: install mariadb
  include: mariadb.yml
  tags: [include]

- name: install apache
  include: apache.yml
  tags: [include]

############### WHEN #####################
# when statement
- name: register name
  yum: name=epel-release state=latest
  register: centos
  tags: [nginxx]

- name: if register centis is success then install nginx
  yum: name=nginx state=latest
  when: centos is success
  register: nginxinstall
  tags: [nginxx]

- name: if register nginxinstall is seccess copy mginx.conf to /etc/
  template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf
  when: nginxinstall is success
  notify: restart nginx
  tags: [nginxx]


# when with vars
- name: if myvar=true
  shell: echo "This centainly is myvar!" > /home/true
  when: myvar
  tags: [myvar]

- name: if myvar=false
  shell: echo "This centainly is not myvar!" > /home/false
  when: not myvar
  tags: [myvar]
  
#
- name: varname2 is not defined!!!
  shell: echo i={{ item }}>>/home/item
  loop: [ 0,2,4,6,8,10 ]
  when: item > 5
  tags: [loopvar]

## test with directory not empty and empty
- name: list files in directory
  command: ls /home/anisaa
  register: contents
  tags: [mylist]

- name: check contents for emptiness
  debug: msg="Directory is empty"
  when: contents.stdout == ""
  tags: [mylist]
  
#
- name: list files in directory
  command: ls -l /home
  register: result
  tags: [mylist2]

- name: check contents for emptiness
  debug: msg={{ result }}
  tags: [mylist2]

################ RESERVED VARIABLES ##################
## find information from host
# vim templates/test.conf.j2
{{ inventory_hostname }}
{{ ansible_hostname }}
{{ ansible_devices.sda.model }}
{{ ansible_nodename }}
{{ inventory_hostname_short }}
{{ ansible_playbook_python }}
{{ inventory_dir }}
{{ playbook_dir }}
{{ ansible_default_ipv4.network }}
{{ ansible_default_ipv4.address }}
{{ ansible_default_ipv4.netmask }}

- name: check IP
  debug: var=hostvars[inventory_hostname]['ansible_default_ipv4']['address']
  tags: [getip]
 
