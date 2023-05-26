# Ansible Project
## Library project deployment automation
The project requires us to make use of jenkins and ansible to deploy an web application and implement a continuous integration and continuous deployment pipeline. So for my project I used a public repository from github-[Online Library Management System PHP](https://github.com/kumarpandule/Online-Library-Management-System-PHP.git)

This project was forked for my projects use.

I made use of this project and deployed it as a LAMP stack project. So I used 2 centos 
machines as webserver that hosts the website and dbserver that hosts the database server.

I automated the whole deployment process with the help of ansible. The master machine is a Rocky Linux server which has both Ansible and Jenkins installed. 

The tree structure shows my ansible playbooks with roles implemented.
