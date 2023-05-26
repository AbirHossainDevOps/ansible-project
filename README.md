# Ansible Project

## Library project deployment automation

The project requires us to make use of jenkins and ansible to deploy an web application and implement a continuous integration and continuous deployment pipeline. So for my project I used a public repository from github- [Online Library Management System PHP](https://github.com/kumarpandule/Online-Library-Management-System-PHP.git)

This project was forked for my projects use.

I made use of this project and deployed it as a LAMP stack project.

I automated the whole deployment process with the help of ansible.

The project source code required some slight modifications to work for my project. The db connection has been changed as per the following image. 2 files needed this change which are as follows, the `config.php` file in includes and admin/includes directory.

![codeChange](images/code-change.PNG)

The steps taken-

## Step 1

### Setup VM

Install 2 centos
machines as webserver that hosts the website and dbserver that hosts the database serve. Also 1 master machine that will host ansible and jenkins. The master machine is a Rocky Linux server.

## Step 2

### Setup for ansible

1.  Configure with `nmtui` to make the machines' ip static.

2.  Install package pre-requisite packages.

        `yum install vim curl wget open-vm-tools -y`

3.  Disable selinux by opening `/etc/selinux/config` change `enforcing` to `disabled`

4.  Change hostnames in `/etc/hostname`

5.  Add name resolution in `/etc/hosts`

    ```
        192.168.20.246 rocky-ansible.localdomain rocky-ansible

        192.168.20.135 webserver.localdomain webserver

        192.168.20.124 dbserver.localdomain dbserver
    ```

6.  In master machine create `ssh-keygen` add the key in the client machines in `~/.ssh/authorized_keys`.

7.  To install ansible run

    ```
        yum install epel-release -y
        yum install ansible -y
    ```

8.  In ansible config in `/etc/ansible/ansible.cfg`
    the following lines are changed as such

        ![cfg1](images/cfg1.PNG)

        ![cfg2](images/cfg2.PNG)

        ![cfg3](images/cfg3.PNG)

9.  In ansible inventory in `/etc/ansible/hosts`
    the following lines are changed as such

        ![inv](images/inv.PNG)

## Step 3

### Setup for jenkins

1. First we need to add the jenkins repo

   ```
   wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

   rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

   yum upgrade
   ```

2. Add required dependencies for the jenkins package by installing java

   ```
   sudo yum install java-11-openjdk
   ```

3. Install jenkins and reload the daemon

   ```
   yum install jenkins
   systemctl daemon-reload
   ```

4. Change jenkins defaut port using `vim /usr/lib/systemd/system/jenkins.service` as following

   ![portc](images/j-port.PNG)

5. Start the jenkins service

   ```
   systemctl start jenkins.service
   systemctl enable jenkins.service
   ```

6. Go to the the master machine ip in port 8090. For my case `192.168.20.246:8090` use the password found in `/var/lib/jenkins/secrets/initialAdminPassword` also make a new user. Add Ansible plugin.

```
Note: I have also setup jenkins reverse proxy for jenkins using nginx as per the documentation found in the jenkins website.
```

## Step 4

### Github project code modifications

The project source code required some slight modifications to work for my project. The db connection has been changed as per the following image. 2 files needed this change which are as follows, the `config.php` file in includes and admin/includes directory.

![codeChange](images/code-change.PNG)

## Step 5

### Ansible playbook

1. We will use the command `ansible-galaxy init file-name` to create roles

2. The roles created sum up to the following tree.The tree structure shows my ansible playbooks with roles implemented.

![tree](images/tree.PNG)
