# Ansible  Learning Documentation                               (DAY -6)

### ***Objective***

The objective of this project is to automate server configuration and application deployment using Ansible as a configuration management tool.

### *Ansible overview*

Ansible is an open-source automation tool used for:

1)Configuration management

2)Application deployment

3)Infrastructure provisioning

4)Orchestration

![image.png](image.png)

### *Installation*

### ***Installing Ansible on Ubuntu***

```bash
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```

### *STEPS I FOLLOWED*

```bash
aws-server ansible_host=PUBLICIP ansible_user=ubuntu ansible_ssh_private_key_file=/home/user/ansible-k/devops.pem

```

The above code which i have done and saved in .ini file named inventory.ini

```bash
---
- name: Install Apache2 on all slave servers
  hosts: slaves
  become: yes

  tasks:

    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Apache2 package
      apt:
        name: apache2
        state: present

    - name: Start Apache2 service
      service:
        name: apache2
        state: started

    - name: Enable Apache2 on boot
      service:
        name: apache2
        enabled: yes

    - name: Create custom index page
      copy:
        dest: /var/www/html/index.html
        content: |
          <html>
            <body>
              <h1>Apache Installed via Ansible</h1>
              <h2>Server: {{ inventory_hostname }}</h2>
            </body>
          </html>

```

i done the above code and  saved in a file named apache-playbook.yml

and ran this command

```bash
ansible-playbook -i inventory.ini apache-playbook.yml
```

![Screenshot 2026-02-14 110851.png](Screenshot_2026-02-14_110851.png)

finally it is showed in public ip , which is EC2 instance that is running in background