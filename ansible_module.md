# Ansible Modules Used in shipping.yml

*Installs packages using DNF package manager.*
## 1. ansible.builtin.dnf
```yaml
- name: install maven and mysql
  ansible.builtin.dnf:
    name: "{{ item }}"
    state: installed
  loop:
    - maven
    - mysql
```

*Installs Python packages using pip.*
## 2. ansible.builtin.pip
```yaml
- name: install PyMySQL and cryptography 
  ansible.builtin.pip:
    name: "{{ item }}"
    executable: pip3.9
  loop:
    - cryptography
    - PyMySQL
```

*Creates or manages file/directory properties.*
## 3. ansible.builtin.file
```yaml
- name: create app directory
  ansible.builtin.file:
    path: /app
    state: directory
```

*Manages user accounts.*
## 4. ansible.builtin.user
```yaml
- name: create roboshop system user
  ansible.builtin.user:
    name: roboshop
    shell: /sbin/nologin
    system: true
    home: /app
```

*Downloads files from HTTP, HTTPS, or FTP.*
## 5. ansible.builtin.get_url
```yaml
- name: download shipping code
  ansible.builtin.get_url:
    url: https://roboshop-artifacts.s3.amazonaws.com/shipping-v3.zip 
    dest: /tmp/shipping.zip
```

*Unpacks archives.*
## 6. ansible.builtin.unarchive
```yaml
- name: extract shipping code
  ansible.builtin.unarchive:
    src: /tmp/shipping.zip
    dest: /app
    remote_src: yes
```

*Runs commands on remote nodes.*
## 7. ansible.builtin.command
```yaml
- name: install maven dependencies
  ansible.builtin.command: mvn clean package
  args:
    chdir: /app

- name: rename jar file
  ansible.builtin.command: mv target/shipping-1.0.jar shipping.jar
  args:
    chdir: /app
```

*Copies files to remote machines.*
## 8. ansible.builtin.copy
```yaml
- name: copy shipping service
  ansible.builtin.copy:
    src: service/shipping.service
    dest: /etc/systemd/system/shipping.service
```

*Reloads systemd manager configuration.*
## 9. ansible.builtin.systemd_service
```yaml
- name: daemon reload
  ansible.builtin.systemd_service:
    daemon_reload: true
```

*Manages services (start, stop, restart, enable, etc).*
## 10. ansible.builtin.service
```yaml
- name: enable and start shipping
  ansible.builtin.service:
    name: shipping
    state: started
    enabled: yes

- name: restart shipping
  ansible.builtin.service:
    state: restarted
    name: shipping
```

*Manages MySQL databases and imports data.*
## 11. community.mysql.mysql_db
```yaml
- name: import data
  community.mysql.mysql_db:
    name: all
    login_user: root
    login_password: RoboShop@1
    login_host: mysql.tcloudguru.in
    state: import
    target: "{{ item }}"
  loop:
    - /app/db/schema.sql
    - /app/db/app-user.sql
    - /app/db/master-data.sql
```

---
*Add more modules here as you use them in other playbooks.*