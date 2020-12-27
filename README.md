# ansible-role-xwiki

### Minimalist approach to Deploy xwiki with tomcat
### Uses pragmatiker/ansible-role-tomcat and depends on existing DB
### At the Moment it has tasks to:
- [x] Downlad WAR from xwiki.org
- [x] Unpack WAR to webapps folder 
- [x] Create configs for Database and other xwiki setup stuff

### ToDo:
- [ ] Also create Dabase on demand
- [ ] See if I can make it loop through multiple Instances on one host
 
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
    - name: Ensure unip is installed
      package: name=unzip state=present
  vars:
    basedir: /opt/tomcat
    software: xwiki
    stage: qs
    tomcat_version: 8.5.61
    xwiki_version: 11.10.12
    shutdown_port: 8005
    http_port: 8080
  roles:
    - role: ansible-role-tomcat
    - role: ansible-role-xwiki
