# Vagrant and Linux work

## Linux commands 
- Create a Directory 'mkdir name_of_the_directory'
- Go inside the Directory 'cd name_of_the_directory'
- Come out of the Directory 'cd ..' (up 1 level) or 'cd' (root directory)
- Who am I 'uname -a'
- Where am I? 'pwd'
- Create a file 'touch name_of_the_file' or 'nano file_name' you land inside the file
- Exit from nano 'ctrl x' then 'y'
- List all 'ls -a' (inc hidden files) or 'ls'
- Clear the screen 'clear'
- Print the contents of a file 'cat name_of_file'
- Delete a folder 'rm -rf name_of_the_folder'
- Delete a file 'rm filename'
- Update VM 'sudo apt-get update -y' sudo runs with perms, apt-get is an install manager, -y flag accepts all permissions. 
- Install a program 'sudo apt-get install {program}' 
- `top` shows processes
- `clear` clears the screen
 - `ps` shows processes
 - `sudo su` goes into sudo(admin) mode
 - `systemctl status` 
 - `systemctl stop` 
 - `history`
- `chmod +x filename.sh` makes file runnable
- `./filename.sh` runs executable file 

## Setting up vagrant file
- Inside directory, create file called Vagrantfile with contents:
```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64" 
  config.vm.network "private_network", ip: "192.168.10.100"
  
  end

```


## Vagrant commands
- `vagrant up` Launches the VM
- `vagrant status` Views status of the VM
- `vagrant ssh` SSH's into the VM

## VM Commands
- Update VM `sudo apt-get update -y` (sudo runs with perms, apt-get is an install manager, -y flag accepts all permissions.) 
- Install a program `sudo apt-get install {program}` 
- e.g., `sudo apt-get install nginx`


## Verify nginx 
- Open browser and navigate to 192.168.10.100
- The nginx splash page should be visible.

![The desired view](https://github.com/jackingham/DevEnvironment/blob/main/image.png?raw=true)

## Other vagrant commands
- `vagrant destroy` stops the VM and destroys config
- `vagrant suspend` stops the VM but keeps config

##Setting up provision.sh
- adding the line `config.vm.synced_folder "app", "/home/vagrant/app"` to the Vagrantfile allows the app folder to be accessed in the VM as well as host machine
- The app can be run by installing all dependancies but it is better to put these inside the provision.sh file.
 ```#!/bin/bash
sudo apt-get update -y

sudo apt-get upgrade -y

sudo  apt-get install nginx -y

sudo systemctl enable nginx

sudo apt-get install nodejs -y

sudo apt-get install npm-y

sudo apt-get install python-software-properties

curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash

sudo apt-get install nodejs -y

sudo npm install pm2 -g

npm install

cd app ```
- this fetches all the relevant dependancies and installs them
- we can make this file run when the VM boots by adding `config.vm.provision "shell", path: "app/provision.sh"` to the Vagrantfile
- the app can then be run by `cd`ing into app then using `sudo npm start` or `node app.js` (I omit these from the provision as they lock the command line)
- The website is viewable at 192.168.10.100:3000

## Reverse proxy

- A reverse proxy allows the port number to be omitted when accessing the app through a browser.
- This can be set up by `cd`ing into the `/etc/nginx/sites-available` and `sudo nano default` and replacing the file with the following:
```
server {
	listen 80;

	server_name _;

	location / {
		proxy_pass http://192.168.10.100:3000;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection 'upgrade';
		proxy_set_header Host $host;
		proxy_cache_bypass $http_upgrade;
	}
}
```
- Its best to now stop nginx and reboot the machine before rehosting the app with `node app.js` 
- The website should be visible with 192.168.10.100 without the port number.


