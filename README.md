# ansible-role-xwiki

### Deploy xwiki with tomcat uses pragmatiker/ansible-role-tomcat
### At the Moment it has tasks to:
- [x] Downlad WAR from xwiki.org
- [x] Unpack WAR to webapps folder 

### ToDo:
- [ ] Create configs for Database and other xwiki stuf
- [ ] Create Dabase
- [ ] See if I can make it loop through multiple Instances or resort to multiple plays
 
### Example Playbook 
```
---
- name: Setup my xwiki
  hosts: server
  gather_facts: yes
  become: yes
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
