# Guide-to-Install-Frappe-ERPNext-in-Windows-11-Using-WSL2

A complete Guide to Install Frappe Bench in Windows 11 Using WSL2 and install Frappe/ERPNext Application

### Pre-requisites 

- Windows 11 21H2 / Windows 10 1903 with Hyper-V Enabled
- [WSL 2.0 or greater enabled](https://learn.microsoft.com/en-us/windows/wsl/install#install-wsl-command)
- [Ubuntu Distro Installed in WSL](https://learn.microsoft.com/en-us/windows/wsl/install#change-the-default-linux-distribution-installed)

### STEP 1 Check WSL version and connect to Ubuntu

	wsl --version
	wsl -d Ubuntu

### STEP 2  Update Ubuntu

	sudo apt update -y && sudo apt upgrade -y

### STEP 3  Install git, python, and redis

	sudo apt install git python3 python3-pip python3-venv redis-server

### STEP 4  Install MariaDB

	sudo apt install software-properties-common
 	sudo apt-get update
	sudo apt-get install mariadb-server
 	sudo apt-get install mariadb-client

During this installation you'll be prompted to set the MySQL root password. If you are not prompted, you'll have to initialize the MySQL server setup yourself and change the root password. You can do that by running the command:

	sudo mysql_secure_installation

Now, edit the MariaDB configuration file.

	sudo nano /etc/mysql/my.cnf

And add this configuration

	[mysqld]
	character-set-client-handshake = FALSE
	character-set-server = utf8mb4
	collation-server = utf8mb4_unicode_ci
	
	[mysql]
	default-character-set = utf8mb4

Now, just restart the mysql service and you are good to go.

	sudo service mysql restart

### STEP 5 Install Node

	curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

After nvm is installed, you may have to close your terminal and open another one. Now run the following command to install node.

	nvm install 16 --lts

Finally, install yarn using npm

	npm install -g yarn

### STEP 6 Install wkhtmltopdf

	sudo apt-get install xvfb libfontconfig wkhtmltopdf

### STEP 7 Install Bench CLI

	pip3 install frappe-bench

Confirm the bench installation by checking version

	bench --version
 
Create your first bench folder.

	cd ~
	bench init --frappe-branch version-14 frappe-bench

After the frappe-bench folder is created, change your directory to it and run this command

	cd frappe-bench/
 	bench start

### STEP 8 Create a new site

Sitename MUST end with .localhost for trying deployments locally
MariaDB root password will be what you set in Step 4
    
	bench new-site dev.localhost
   
### STEP 9 Set bench developer mode on the new site

	bench --site dev.localhost set-config developer_mode 1
	bench --site dev.localhost clear-cache   
    
### STEP 10 Install ERPNext

	bench get-app --branch version-14 --resolve-deps erpnext
	bench --site dev.localhost install-app erpnext
 
### STEP 11 Start Frappe bench

	bench start

##### You can now login with user Administrator and the password you choose when creating the site. Your website will now be accessible at location d-code.localhost:8000
