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
- `top`
- `clear` clears the screen
 - `ps`
 - `sudo su` goes into sudo(admin) mode
 - `systemctl status` 
 - `systemctl stop` 
 - `systemctl stop` 
   history
 

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

