# LAMP Stack Setup Using Ansible Project
--------------------------------------------

Project Overview

This project demonstrates how to use Ansible to automate the setup of a LAMP (Linux, Apache, MySQL, PHP) stack on Ubuntu servers. The setup includes installing and configuring Apache, PHP, MySQL, and deploying a basic web application. Each section of the playbook is modular, using roles to separate concerns, making the playbook easier to manage and extend.

What This Project Does:

- Installs and Configures Apache: Sets up the Apache web server and ensures it is running.
- Installs and Configures MySQL: Sets up the MySQL database server with initial configuration.
- Installs and Configures PHP: Configures PHP for processing dynamic web content.
- Synchronizes Time: Uses Chrony for time synchronization across Ubuntu servers.

To get started with this project, follow these steps:

Prerequisites

1. Ansible*: Ensure Ansible is installed on your control machine.
   - Installation: [Ansible Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
2. Access to Ubuntu Servers: You need access to Ubuntu servers.
3. Passwordless SSH: Set up passwordless SSH between your control machine and the target servers.
   - Guide: [Passwordless SSH Setup](https://docs.ansible.com/ansible/latest/user_guide/connection.html#passwordless-ssh)
4. Basic Knowledge: Familiarity with Ansible, Linux, and LAMP stack concepts.

Cloning the Repository

Clone the repository to your local machine:

git clone https://github.com/Mohit722/ansible-lamp-stack-setup.git
cd lamp-setup

Configuration

1. Edit Group Variables:
   - `group_vars/all`: Define global variables applicable to all hosts.
   - `group_vars/ubuntu`: Define Ubuntu-specific variables.

2. Edit Inventory File:
   - Define your target servers in `lamp.hosts`. For example:
    
     [ubuntu]
     node01 ansible_host=172.31.40.216 ansible_user=ubuntu ansible_ssh_private_key_file=/etc/ansible/ubuntu.pem
    

Roles Directory:-
-----------------

The roles directory contains three main roles, each responsible for a specific part of the LAMP stack setup:

1. common:
Purpose: Installs and configures common packages and services across all servers.
Key Files:
 - tasks/main.yml: Contains tasks to install and configure packages, such as Chrony.
 - templates/chrony.conf.j2: Jinja2 template for Chrony configuration.
 - handlers/main.yml: Handles service restarts for the Chrony service.


2. web:
Purpose: Installs and configures the Apache web server and PHP.
Key Files:
 - tasks/main.yml: Contains tasks to install Apache, PHP, and Git. It also copies the web application files.
 - templates/index.php.j2: Jinja2 template for the index PHP file.


3. db:
Purpose: Installs and configures the MySQL database server.
Key Files:
 - tasks/main.yml: Contains tasks to install MySQL, create configuration files, and start the service.
 - templates/my.cnf.j2: Jinja2 template for MySQL configuration.
 - handlers/main.yml: Handles service restarts for MySQL.


Using the Playbooks:

1. Apply Common Configuration:
   - This step sets up Chrony for time synchronization and other common configurations.
  
   ansible-playbook lamp.yml --tags common
   

2. Configure Web Servers:
   - This step installs and configures Apache and PHP.
   
   ansible-playbook lamp.yml --tags web
   

3. Set Up MySQL:
   - This step installs and configures MySQL.
   
   ansible-playbook lamp.yml --tags db
   

Verification:

1. Check Web Server:
   - Open a web browser and navigate to `http://<your-server-ip>/index.php`.
   - You should see a page with the text "Hello, World!" and the server's hostname.

2. Verify MySQL:
   - Connect to MySQL using:
     
     mysql -u foouser -p
    
   - Check the databases and user permissions:
     
     SHOW DATABASES;
     SHOW GRANTS FOR 'foouser'@'localhost';
     

Additional Information

- Templates:
  - `roles/common/templates/chrony.conf.j2` (Chrony configuration).
  - `roles/web/templates/index.php.j2` (PHP page template).
  - `roles/db/templates/my.cnf.j2` (MySQL configuration).

- Handlers:
  - `roles/common/handlers/main.yml` (Restart Chrony service).


# License

This project is licensed under the [Mohit Tiwari GitHub Repository License](https://github.com/mohit-tiwari/lamp-setup/blob/main/LICENSE).


# Contact

For any questions or issues, please open an issue on GitHub or contact tiwarimohit722@gmail.com.

