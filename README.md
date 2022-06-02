# ansible-role-xwiki

### Minimalist approach to Deploy xwiki with tomcat
### Uses pragmatiker/ansible-role-tomcat and depends on existing DB
### At the Moment it has tasks to:
- [x] Downlad WAR from xwiki.org
- [x] Unpack WAR to webapps folder 
- [x] Create configs for Database and other xwiki setup stuff
- [x] Make it loop through multiple Instances on one host. prod/dev/qs 

### ToDo:
- [ ] Create Database on demand
 
### Example Playbook 
```
---
- name: Setup my xwiki
  hosts: server
  gather_facts: yes
  become: yes
  pre_tasks:
    - name: Update apt cache.
      apt: update_cache=true cache_valid_time=600
      when: ansible_os_family == 'Debian'
    - name: Ensure unzip is installed
      package: name=unzip state=present
   vars:
     basedir: /opt/tomcat
     software: xwiki
     xwiki_version: 14.4
     stage: qs
     tomcat_version: 9.0.63
     instances:
       - instance: acme
         http_port: 8080
         shutdown_port: 8005
         db_passwd: CTS9K4XKfO
         stage: dev
       - instance: umbrellacorp
         http_port: 9080
         shutdown_port: 9005
         db_passwd: a08tg6zz81
         stage: viral
       - instance: starkindustries
         http_port: 10080
         shutdown_port: 10005
         db_passwd: E4G144kL6t
       - instance: nebula
         http_port: 11080
         shutdown_port: 11005
         db_passwd: I80BuBXzh6
  roles:
    - role: ansible-role-tomcat
    - role: ansible-role-xwiki
