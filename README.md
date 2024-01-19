# Guide-to-Install-Frappe-ERPNext-in-Windows-11-Using-WSL2
Guide-to-Install-Frappe-ERPNext-in-Windows-11-Using-WSL2

# Guide-to-Install-Frappe-ERPNext-in-Windows-11-Using-WSL2

A complete Guide to Install Frappe Bench in Windows 11 Using WSL2 and install Frappe/ERPNext Application

### Pre-requisites 

- Windows 11 21H2 / Windows 10 1903 with Hyper-V Enabled
- [WSL 2.0 or greater enabled](https://learn.microsoft.com/en-us/windows/wsl/install#install-wsl-command)
- Ubuntu Distro Installed in WSL

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

### STEP 5

During this installation you'll be prompted to set the MySQL root password. If you are not prompted, you'll have to initialize the MySQL server setup yourself and change the root password. You can do that by running the command:
    
	sudo mysql_secure_installation

### STEP 6

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

### STEP 7 Install Node

	curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

After nvm is installed, you may have to close your terminal and open another one. Now run the following command to install node.

	nvm install 16 --lts

Finally, install yarn using npm

	npm install -g yarn

### STEP 8 Install wkhtmltopdf

	sudo apt-get install xvfb libfontconfig wkhtmltopdf

### STEP 9 Install Bench CLI

	pip3 install frappe-bench

Confirm the bench installation by checking version

	bench --version
	
	# output
	5.2.1
 
Create your first bench folder.

	cd ~
	bench init frappe-bench

After the frappe-bench folder is created, change your directory to it and run this command

	cd frappe-bench/
 	bench start

### STEP 3

   Copy example devcontainer config from 
    
    devcontainer-example folder to .devcontainer folder
    
   Copy example vscode config for devcontainer from 
    
    development/vscode-example folder to development/.vscode folder
   
### STEP 4 Install VSCode Remote Containers extension
    
    Open vscode and install 'Dev Containers' extension
    
###  STEP 5 After the extensions are installed, you can

  Open frappe_docker folder in VS Code.
  
  Launch the command, from Command Palette (Ctrl + Shift + P) Remote-Containers: Reopen in Container. You can also click in the bottom left corner to access the remote   container menu.
  
### Note: 
   if this error in running contaners try the below commnad in CMD
   
   Error starting userland proxy: listen tcp 0.0.0.0:80: bind: An attempt was made to access a socket in a way forbidden by its access permissions
	
    netsh http add iplisten ipaddress=::
                
   The development directory is ignored by git. It is mounted and available inside the container. Create all your benches (installations of bench, the tool that          manages frappe) inside this directory.
   Node v14 and v10 are installed. Check with nvm ls. Node v14 is used by default.
                
    
### STEP 6 Initilase frappe bench with frappe version 14 and Switch directory

    
    bench init --skip-redis-config-generation --frappe-branch version-14 frappe-bench
    cd frappe-bench
    
    
### STEP 7 Setup hosts
    
   We need to tell bench to use the right containers instead of localhost. Run the following commands inside the container:

    bench set-config -g db_host mariadb
    bench set-config -g redis_cache redis://redis-cache:6379
    bench set-config -g redis_queue redis://redis-queue:6379
    bench set-config -g redis_socketio redis://redis-socketio:6379
  For any reason the above commands fail, set the values in common_site_config.json manually.

    {
      "db_host": "mariadb",
      "redis_cache": "redis://redis-cache:6379",
      "redis_queue": "redis://redis-queue:6379",
      "redis_socketio": "redis://redis-socketio:6379"
    }
    
### STEP 8 Create a new site
   sitename MUST end with .localhost for trying deployments locally.
   MariaDB root password: 123
    
    bench new-site d-code.localhost --no-mariadb-socket 
    
    
### STEP 9 Set bench developer mode on the new site
    
    bench --site d-code.localhost set-config developer_mode 1
    bench --site d-code.localhost clear-cache   
    
    
### STEP 10 Install ERPNext
    
    bench get-app --branch version-14 --resolve-deps erpnext
    bench --site d-code.localhost install-app erpnext
    
    
    
    
### STEP 11 Start Frappe bench 
    
    bench start
    
  You can now login with user Administrator and the password you choose when creating the site. Your website will now be accessible at location d-code.localhost:8000
