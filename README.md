# ansible-role-xwiki

### Minimalist approach to deploy multiple empty xwiki instances on tomcat & mariadb
### Depends on pragmatiker/ansible-role-tomcat
### Created directory structure
    /opt/tomcat/tomcat-xwiki-INSTANCENAME-STAGE # Tomcat Instance
    /opt/tomcat/data-xwiki-INSTANCENAME-STAGE # Persistent data dir for xWiki

### At the Moment it has tasks to:
- [x] Install MariaDB Server
- [x] Downlad WAR from xwiki.org
- [x] Create persistent data dir 
- [x] Unpack WAR to webapps folder /opt/tomcat/tomcat-xwiki-INSTANCENAME-STAGE/webapps/xwiki-INSTANCENAME-STAGE
- [x] Template WEB-INF/hibernate.cfg.xml, WEB-INF/xwiki.cfg & WEB-INF/xwiki.propterties
- [x] Download and install JDBC driver to WEB-INF/lib
- [x] Create DB and DB User

### ToDo:
- [ ] Call handler to start tomcat
- [ ] Ansible vault via include_vars
 
### Example Playbook with minimum needed vars
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
